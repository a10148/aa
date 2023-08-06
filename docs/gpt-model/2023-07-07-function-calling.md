---
layout: default
title:  "function-calling"
date:   2023-07-07
categories: openai function-calling	
typora-root-url: ../../
parent: gpt-model
---

# function-calling

​	在API调用中，您可以描述要发送给gpt-3.5-turbo-0613和gpt-4-0613模型的函数，并且模型会智能地选择输出一个包含调用这些函数所需参数的JSON对象。Chat Completions API不会直接调用函数，而是模型生成的JSON可以供您在代码中调用函数。

​	最新的模型（gpt-3.5-turbo-0613和gpt-4-0613）经过微调，既能够检测何时应调用函数（根据输入），又能够以符合函数签名的JSON形式进行响应。然而，这种能力也带来潜在的风险。我们强烈建议在代表用户采取影响现实世界的行动之前，建立用户确认流程（发送电子邮件、发布内容在线、进行购买等）。这样可以确保用户在执行这些操作之前确认并授权。

> 在底层，函数按照模型训练过的语法注入到系统消息中。这意味着函数会根据模型的上下文限制进行计数，并作为输入令牌进行计费。如果遇到上下文限制，我们建议限制函数的数量或为函数参数提供的文档的长度。

函数调用使您能够更可靠地从模型中获取结构化数据，比如:

1. 要创建能够通过调用外部API（例如ChatGPT插件）来回答问题的聊天机器人。 
2. 定义像send_email(to: string, body: string)这样的函数，或者get_current_weather(location: string, unit: 'celsius' | 'fahrenheit')。 将自然语言转换为API调用。 
3. 将"Who are my top customers?"转换为get_customers(min_revenue: int, created_before: string, limit: int)并调用您的内部API。 从文本中提取结构化数据。 例如，定义一个名为extract_data(name: string, birthday: string)或sql_query(query: string)的函数。

要实现函数调用，需要按照如下步骤：

1. 通过向模型传递查询message信息和在functions参数中定义的一组函数来调用模型。
2. 模型决定是否可以调用一个函数；如果是要调用，content将是一个符合您自定义函数参数的字符串化，此字符串是JSON结构（注意：模型可能生成无效的JSON或虚构参数）。
3. 在你的代码中将字符串解析为JSON，并根据提供的参数调用您的函数（如果参数存在）。
4. 再次调用模型，将函数响应作为新消息附加到message里，模型将函数响应的内容重新生成更合适的内容返回给用户。

下面是一个完整的代码例子

```python
import openai
import json


# Example dummy function hard coded to return the same weather
# In production, this could be your backend API or an external API
def get_current_weather(location, unit="fahrenheit"):
    """Get the current weather in a given location"""
    weather_info = {
        "location": location,
        "temperature": "72",
        "unit": unit,
        "forecast": ["sunny", "windy"],
    }
    return json.dumps(weather_info)


def run_conversation():
    # Step 1: send the conversation and available functions to GPT
    messages = [{"role": "user", "content": "What's the weather like in Boston?"}]
    functions = [
        {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA",
                    },
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        }
    ]
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo-0613",
        messages=messages,
        functions=functions,
        function_call="auto",  # auto is default, but we'll be explicit
    )
    response_message = response["choices"][0]["message"]

    # Step 2: check if GPT wanted to call a function
    if response_message.get("function_call"):
        # Step 3: call the function
        # Note: the JSON response may not always be valid; be sure to handle errors
        available_functions = {
            "get_current_weather": get_current_weather,
        }  # only one function in this example, but you can have multiple
        function_name = response_message["function_call"]["name"]
        fuction_to_call = available_functions[function_name]
        function_args = json.loads(response_message["function_call"]["arguments"])
        function_response = fuction_to_call(
            location=function_args.get("location"),
            unit=function_args.get("unit"),
        )

        # Step 4: send the info on the function call and function response to GPT
        messages.append(response_message)  # extend conversation with assistant's reply
        messages.append(
            {
                "role": "function",
                "name": function_name,
                "content": function_response,
            }
        )  # extend conversation with function response
        second_response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo-0613",
            messages=messages,
        )  # get a new response from GPT where it can see the function response
        return second_response


print(run_conversation())
```

​	函数调用中的虚构输出通常可以通过系统消息来缓解。例如，如果您发现模型正在使用未提供给它的函数生成函数调用，请使用系统消息：“仅使用为您提供的函数。”，限定模型只调用你提供的函数。

​		在上面的例子中，我们将函数的响应发送回模型，并让模型决定下一步的操作。模型会生成一个面向用户的消息，告诉用户波士顿的温度，但根据查询的情况，它也可能选择再次调用一个函数

​	例如，如果您向模型提问"找到本周末波士顿的天气，预订周六两人晚餐，并更新我的日历"，并为这些查询提供相应的函数，模型可能会选择连续调用这些函数，并且只在最后生成一个面向用户的消息。

​	如果您想强制模型调用特定的函数，可以通过设置function_call: {"name": "<insert-function-name>"}来实现。您还可以通过设置function_call: "none"来强制模型生成面向用户的消息。请注意，默认行为（function_call: "auto"）是让模型自行决定是否调用函数，以及要调用哪个函数。

> 您可以在 OpenAI [说明书 ](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_with_chat_models.ipynb) 中找到更多函数调用示例

