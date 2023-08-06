---
layout: default
title:  "completions API"
date:   2023-07-08
categories: openai completions	
typora-root-url: ../../
parent: gpt-model
---

# completions API

​	Completions API与Chat Completions API具有不同的接口。输入不再是消息列表，而是一个名为prompt的自由文本字符串。

Completions API 请求示例：

```python
import openai

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Write a tagline for an ice cream shop."
)
```

Completions API 响应示例：

```python
{
  "choices": [
    {
      "finish_reason": "length",
      "index": 0,
      "logprobs": null,
      "text": "\n\n\"Let Your Sweet Tooth Run Wild at Our Creamy Ice Cream Shack"
    }
  ],
  "created": 1683130927,
  "id": "cmpl-7C9Wxi9Du4j1lQjdjhxBlO22M61LD",
  "model": "text-davinci-003",
  "object": "text_completion",
  "usage": {
    "completion_tokens": 16,
    "prompt_tokens": 10,
    "total_tokens": 26
  }
}
```

> 查看详细接口文档[Completions API](https://platform.openai.com/docs/api-reference/completions)

​	Completions API 会根据提示给出相应的返回结果，返回结果和请求中的提示一样也是由token组成的。而Completions API 可以提供与每个输出token相关的最可能标记的有限数量的对数概率。通过使用logprobs字段来控制此功能。在某些情况下，方便评估模型在输出中的置信度可能。

​	Completions API 除了标准的提示(也称为前缀式)之外还支持通过提供后缀来插入文本(Inserting text)。在撰写长篇文字、段落之间过渡、按照大纲编写或引导模型走向结尾时，会出现这种需求。这个特性同样适用于代码，可以在函数或文件的中间插入文本。

## Inserting text

​	为了说明后缀上下文对生成的文本的影响，考虑以下提示：“今天我决定做一个重大改变。”我们可以想象有很多种方式来完成这个句子。但是，如果我们现在提供故事的结尾：“我换了一个新发型，得到了很多赞美！”那么预期的完成结果就变得清晰了。

> I went to college at Boston University. After getting my degree, I decided to make a change. A big change!
> **I packed my bags and moved to the west coast of the United States.**
> Now, I can’t get enough of the Pacific Ocean!

