---
layout: default
title:  "strategy: Use external tools"
date:   2023-07-13
categories: openai best-practices
typora-root-url: ../../
parent: best-practices
---

# Strategy: Use external tools

## 策略：使用基于嵌入的搜索实现高效知识检索

模型可以利用作为其输入部分提供的外部信息源。这可以帮助模型生成更有见地和最新的响应。例如，如果用户提问关于特定电影的问题，将有关电影的高质量信息（例如，演员，导演等）添加到模型的输入可能很有用。嵌入可以用来实现高效的知识检索，因此可以在运行时动态地将相关信息添加到模型输入。

文本嵌入是一种可以衡量文本字符串之间相关性的向量。相似或相关的字符串在一起会比不相关的字符串更接近。这个事实，加上存在快速向量搜索算法，意味着嵌入可以用来实现高效的知识检索。特别是，可以将文本语料库切成块，每个块都可以嵌入并存储。然后可以嵌入给定的查询，并执行向量搜索以找到与查询最相关的语料库中的文本块（即在嵌入空间中最接近的）。

[OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/examples/vector_databases/Using_vector_databases_for_embeddings_search.ipynb)中可以找到示例实现。参见策略“[指示模型使用检索到的知识回答查询](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-use-retrieved-knowledge-to-answer-queries)”，了解如何使用知识检索最大程度地减少模型编造不正确事实的可能性。

## 策略：使用代码执行进行更精确的计算或调用外部API

不能依赖GPT自己准确地进行算术或长时间的计算。在需要这种情况的情况下，可以指示模型编写和运行代码，而不是进行自己的计算。特别是，可以指示模型将要运行的代码放入指定格式，例如三重反引号。产生输出后，可以提取并运行代码。最后，如果需要，可以将代码执行引擎（即Python解释器）的输出作为模型的下一次查询的输入。

![1](/assets/images/best-practices-4/1.png)

代码执行的另一个好的使用场景是调用外部API。如果正确地指导模型使用API，它可以编写利用该API的代码。可以通过提供文档和/或显示如何使用API的代码示例来指导模型如何使用API。

![2](/assets/images/best-practices-4/2.png)

**警告：执行模型生成的代码本身并不安全，任何寻求这样做的应用都应该采取预防措施。特别是，需要一个沙箱代码执行环境来限制不受信任的代码可能造成的损害。**

## 策略：赋予模型访问特定函数的能力

Chat Completions API允许在请求中传递一系列函数描述。这使得模型能够根据提供的模式生成函数参数。API以JSON格式返回生成的函数参数，可用于执行函数调用。然后，函数调用提供的输出可以在以下请求中反馈给模型，以封闭循环。这是使用GPT模型调用外部函数的推荐方式。要了解更多信息，请查看我们的GPT引导手册中的[函数调用部分](https://platform.openai.com/docs/guides/gpt/function-calling)，以及OpenAI Cookbook中更多的[函数调用示例](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_call_functions_with_chat_models.ipynb)。
