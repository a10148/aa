---
layout: default
title:  "Completions and chatcompletions diffrents"
date:   2023-07-09
categories: openai completion
typora-root-url: ../../
parent: gpt-model
---

# completions API

​	chat completions格式可以通过使用单个用户消息构建请求，使其类似于completions格式。例如，可以使用以下chat completions提示进行

```
Translate the following English text to French: "{text}"
```

和下面格式是等效的

```
[{"role": "user", "content": 'Translate the following English text to French: "{text}"'}]
```

同样的，也可以通过格式化输入使用completions模仿chat completions。

​	这些API之间的区别主要源于它们所使用的底层GPT模型。chat completions API是我们最强大的模型（gpt-4）和最具成本效益的模型（gpt-3.5-turbo）的接口。作为参考，gpt-3.5-turbo的性能与text-davinci-003相当，但每个标记的价格只有其10%！请参阅此处的定价详情。[各模型介绍](https://platform.openai.com/docs/models)

> ```shell
> # 查看模型列表
> curl https://api.openai.com/v1/models \
>   -H "Authorization: Bearer $OPENAI_API_KEY"
> ```

## 我该选择那个模型

​	通常建议您使用gpt-4或gpt-3.5-turbo中的一种。选择使用哪种模型取决于您所处理任务的复杂性。在广泛的评估中，gpt-4通常表现更好。特别是，在仔细遵循复杂指令方面，gpt-4的能力更强。相比之下，gpt-3.5-turbo更有可能只遵循复杂多部分指令的其中一部分。与gpt-3.5-turbo相比，gpt-4不太可能虚构信息，这种行为被称为"hallucination"（幻觉）。此外，gpt-4的上下文窗口更大，最大标记数为8,192个，而gpt-3.5-turbo为4,096个。然而，gpt-3.5-turbo的输出具有更低的延迟，并且每个标记的成本要低得多。

我们建议在[playground](https://platform.openai.com/playground?mode=chat)中进行实验，以调查哪些模型在您的用途中提供了最佳的性价比。一种常见的设计模式是使用多个不同的查询类型，每种类型分派到适合处理它们的模型。

## GTP最佳实践

​	了解与GPT模型合作的最佳实践可以对应用性能产生重大影响。GPT模型所展示的故障模式以及解决或纠正这些故障模式的方法并不总是直观的。在处理GPT模型时，有一种被称为"prompt engineering"的技能，但随着该领域的发展，其范围已经超越了仅仅优化提示，而是工程化使用模型查询作为组件的系统。要了解更多信息，请阅读我们的GPT最佳实践指南，其中介绍了提高模型推理能力、减少模型幻觉等方法。您还可以在[OpenAI Cookbook](https://github.com/openai/openai-cookbook)中找到许多有用的资源，包括代码示例。

## Token的管理

​	语言模型以称为token的块来读取和写入文本。在英语中，一个token可以短至一个字符或长至一个词（例如，a或apple），而在某些语言中，token甚至可以比一个字符更短或比一个词更长。

例如，字符串"ChatGPT is great!"被编码为六个token：["Chat", "G", "PT", " is", " great", "!"]。

API调用中的token总数会影响以下方面：

您的API调用费用，因为您按token计费 您的API调用所需的时间，因为写入更多token需要更多时间 您的API调用是否有效，因为总token数必须低于模型的最大限制（gpt-3.5-turbo为4096个token） 输入和输出的token都计入这些数量。例如，如果您的API调用在消息输入中使用了10个token，并且在消息输出中收到了20个token，您将被计费为30个token。但请注意，对于某些模型，输入和输出的token的价格可能不同（请参阅定价页面获取更多信息）。

要查看API调用使用了多少个token，请检查API响应中的usage字段（例如，response\['usage'\]\['total_tokens'\]）。

像gpt-3.5-turbo和gpt-4这样的聊天模型与completions API中可用的模型使用相同的token方式，但由于其基于消息的格式，很难计算对话将使用多少个token

## 计算token数量

​	以下是一个示例函数，用于计算传递给gpt-3.5-turbo-0613的消息中的令牌数。 消息转换为令牌的确切方式可能会因模型而异。所以，当未来的模型版本发布时，这个函数返回的答案可能只是近似值。ChatML文档解释了OpenAI API如何将消息转换为令牌，这可能对你编写自己的函数有所帮助。

```python
def num_tokens_from_messages(messages, model="gpt-3.5-turbo-0613"):
  """Returns the number of tokens used by a list of messages."""
  try:
      encoding = tiktoken.encoding_for_model(model)
  except KeyError:
      encoding = tiktoken.get_encoding("cl100k_base")
  if model == "gpt-3.5-turbo-0613":  # note: future models may deviate from this
      num_tokens = 0
      for message in messages:
          num_tokens += 4  # every message follows <im_start>{role/name}\n{content}<im_end>\n
          for key, value in message.items():
              num_tokens += len(encoding.encode(value))
              if key == "name":  # if there's a name, the role is omitted
                  num_tokens += -1  # role is always required and always 1 token
      num_tokens += 2  # every reply is primed with <im_start>assistant
      return num_tokens
  else:
      raise NotImplementedError(f"""num_tokens_from_messages() is not presently implemented for model {model}.
  See https://github.com/openai/openai-python/blob/main/chatml.md for information on how messages are converted to tokens.""")
```

​	接下来，创建一条消息并将其传递给上面定义的函数，以查看令牌计数，这应该与API使用参数返回的值匹配：

```python
messages = [
  {"role": "system", "content": "You are a helpful, pattern-following assistant that translates corporate jargon into plain English."},
  {"role": "system", "name":"example_user", "content": "New synergies will help drive top-line growth."},
  {"role": "system", "name": "example_assistant", "content": "Things working well together will increase revenue."},
  {"role": "system", "name":"example_user", "content": "Let's circle back when we have more bandwidth to touch base on opportunities for increased leverage."},
  {"role": "system", "name": "example_assistant", "content": "Let's talk later when we're less busy about how to do better."},
  {"role": "user", "content": "This late pivot means we don't have time to boil the ocean for the client deliverable."},
]

model = "gpt-3.5-turbo-0613"

print(f"{num_tokens_from_messages(messages, model)} prompt tokens counted.")
# Should show ~126 total_tokens
```

​	为了确认我们上面函数生成的数字与API返回的数字相同，请创建一个新的聊天完成：

```python
# example token count from the OpenAI API
import openai


response = openai.ChatCompletion.create(
    model=model,
    messages=messages,
    temperature=0,
)

print(f'{response["usage"]["prompt_tokens"]} prompt tokens used.')
```

​	如果你想查看一个文本字符串中有多少个令牌，而不需要发起API调用，可以使用OpenAI的[tiktoken](https://github.com/openai/tiktoken) Python库。示例代码可以在OpenAI Cookbook的指南中找到，该指南讲解[如何使用tiktoken计算令牌数](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb)。

传递给API的每条消息都会消耗内容、角色和其他字段中的令牌数，再加上一些用于后台格式化的额外令牌。这可能在未来有所改变。

如果一段对话的令牌数超过了模型的最大限制（例如，gpt-3.5-turbo的限制为4096个令牌），你将不得不截断、省略或以其他方式缩小你的文本，直到它适合为止。注意，如果一条消息从消息输入中被移除，模型将完全忘记它。

需要注意的是，非常长的对话更可能收到不完整的回复。例如，一个包含4090个令牌的gpt-3.5-turbo对话，其回复将在仅剩6个令牌后被截断

## 常见问题解答

1. 为什么模型的输出结果不一致？ 默认情况下，API是非确定性的。这意味着，即使你的提示保持不变，每次调用它时你也可能得到稍微不同的完成。将温度设置为0将使输出大致确定，但仍会有少量的可变性。
2. 我应该如何设置温度参数？ 温度值较低会产生更一致的输出，而温度值较高则会生成更多样化和富有创意的结果。根据你的特定应用中对一致性和创造性的权衡需求来选择一个温度值。]
3. 最新的模型支持微调吗？ 不支持。目前，你只能微调基础的GPT-3模型（davinci、curie、babbage和ada）。关于如何使用[微调模型](https://platform.openai.com/docs/guides/fine-tuning)的更多详细信息，请参阅微调指南。
4. 你们会存储传入API的数据吗？ 截至2023年3月1日，我们会保留你的API数据30天，但不再使用通过API发送的你的数据来改进我们的模型。在我们的[数据使用政策](https://openai.com/policies/usage-policies)中了解更多。一些端点提供[零保留](https://platform.openai.com/docs/models/default-usage-policies-by-endpoint)。
5. 我如何使我的应用更安全？ 如果你想在Chat API的输出中添加一个审查层，你可以遵循我们的审查[指南](https://platform.openai.com/docs/guides/moderation)，防止违反OpenAI使用政策的内容被显示。
6. 我应该使用[ChatGPT](https://chat.openai.com/)还是API？ ChatGPT为OpenAI API中的模型提供了一个聊天接口，并包含了一系列内置功能，如集成浏览、代码执行、插件等。相比之下，使用OpenAI的API提供了更大的灵活性。
