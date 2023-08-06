---
layout: default
title:  "openai introduction"
date:   2023-07-01
categories: openai introduction
order: 1
parent: quickstart
---

# 一些概念

## GPTs

#### 什么是 ChatGPT?

> **ChatGPT**，全称**聊天生成预训练转换器**（英语：**Chat** **G**enerative **P**re-trained **T**ransformer[[2\]](https://zh.wikipedia.org/wiki/ChatGPT#cite_note-:4-2)），是[OpenAI](https://zh.wikipedia.org/wiki/OpenAI)开发的[人工智能](https://zh.wikipedia.org/wiki/人工智能)[聊天机器人](https://zh.wikipedia.org/wiki/聊天機器人)程序，于2022年11月推出。该程序使用基于[GPT-3.5](https://zh.wikipedia.org/wiki/GPT-3)、[GPT-4](https://zh.wikipedia.org/wiki/GPT-4)架构的[大型语言模型](https://zh.wikipedia.org/wiki/大型语言模型)并以[强化学习](https://zh.wikipedia.org/wiki/强化学习)训练。ChatGPT目前仍以文字方式交互，而除了可以用人类自然对话方式来交互，还可以用于甚为复杂的语言工作，包括自动生成[文本](https://zh.wikipedia.org/wiki/文本)、自动问答、自动[摘要](https://zh.wikipedia.org/wiki/摘要)等多种任务。如：在自动文本生成方面，ChatGPT可以根据输入的文本自动生成类似的文本（[剧本](https://zh.wikipedia.org/wiki/劇本)、[歌曲](https://zh.wikipedia.org/wiki/歌曲)、[企划](https://zh.wikipedia.org/wiki/企劃)等），在自动问答方面，ChatGPT可以根据输入的问题自动生成答案。还有编写和调试计算机程序的能力。[[3\]](https://zh.wikipedia.org/wiki/ChatGPT#cite_note-ChatGPT_can_write_code-3)在推广期间，所有人可以免费注册，并在登录后免费使用ChatGPT与AI机器人对话[[4\]](https://zh.wikipedia.org/wiki/ChatGPT#cite_note-4)。 -- https://zh.wikipedia.org/wiki/ChatGPT

#### 什么是GPT

> **基于转换器的生成式预训练模型**[[1\]](https://zh.wikipedia.org/wiki/基于转换器的生成式预训练模型#cite_note-:0-1)（Generative pre-trained transformers; GPT）是[OpenAI](https://zh.wikipedia.org/wiki/OpenAI)开发的一系列延伸自[Transformer模型](https://zh.wikipedia.org/wiki/Transformer模型)的[自然语言生成](https://zh.wikipedia.org/wiki/自然语言生成)（NLG）模型。它可以进行[超参数](https://zh.wikipedia.org/wiki/超参数_(机器学习))微调以完成各种[自然语言处理](https://zh.wikipedia.org/wiki/自然语言处理)任务，例如文本生成、代码生成、视频生成、文本问答、图像生成、论文写作、影视创作、科学实验设计等。基于大量语料数据上训练，以生成类似于人类自然语言的文本。其名称中的“预训练”指的是在大型文本语料库上进行的初始训练过程，其中模型学习预测文章中下一个单词，这为模型在具有限量的任务特定数据的下游任务中表现良好提供了坚实的基础。 	-- https://zh.wikipedia.org/wiki/GPT

​	按照Open AI的官方解释是: GPT模型经过训练可以理解自然语言和代码。 GPT 提供文本的输入给出相应的输出。提供的输入这里有个概念叫"prompts"。翻译过来是“提示”。

​	为什么相应输出是"prompts"(提示)，既然是是ChatGTP(聊天模型)。 为什么不能像聊天一样，一来一回的聊呢？看GPT-4给出的解释：

> 作为ChatGPT，我的目标是与用户进行对话并提供有关问题的回答或相关信息。然而，由于技术限制和设计选择，ChatGPT模型在某些情况下可能无法进行真正的对话式交互。
>
> ChatGPT模型的设计是为了处理用户的提示或问题，并生成相应的回答。在每个回合中，用户提供一个提示或问题，模型生成回答，并将其返回给用户。这种单向的提示-回答过程不同于真实的对话，其中有多个参与者之间交换消息并进行多轮交流。
>
> 限制模型只回答一次的原因有几个方面：
>
> 1. 训练数据：ChatGPT是基于单向的语言模型训练的，训练数据通常是单个句子或段落，没有包含多轮对话的上下文信息。
> 2. 输出长度：模型输出的长度是有限制的，通常限制在一定数量的标记（tokens），以避免输出过长。
> 3. 上下文维护：在多轮对话中，维护整个对话历史和上下文可以变得复杂。ChatGPT模型的设计更侧重于生成与当前提示相关的回答，而不是完全理解和保持对话的一致性。
>
> 虽然ChatGPT模型在回答问题和提供信息方面很有用，但它在模拟真实对话方面的能力有一定的局限性。针对更复杂的对话和多轮交互，可能需要采用更复杂的对话模型或设计更复杂的系统

​	ChatGPT模型可以生成回答，但它并没有真正的理解和意识。它仅仅是通过学习大量文本数据中的模式和关联来生成可能相关的回答。模型并不具备对问题进行深层次的理解、推理或推断能力，而是通过学习常见的语言模式和表达方式来生成回答

​	ChatGPT会根据你的输入，经过转码生成输入文本的向量形式是数据。GPT使用此数据，计算下一个Token出现的概率。接着新出现的token作为文本输入的一部分计算下一个Token。直到生成完整的回答或者达到限制为止。



#### GTP的历史

> |                                                              | 参数数量 |                           训练数据                           |
> | :----------------------------------------------------------: | :------: | :----------------------------------------------------------: |
> | [GPT-1](https://zh.wikipedia.org/w/index.php?title=GPT-1&action=edit&redlink=1) |  1.2亿   | BookCorpus[[8\]](https://zh.wikipedia.org/wiki/基于转换器的生成式预训练模型#cite_note-8)：是一个包含7000本未出版书籍的语料库，总大小为4.5 GB。这些书籍涵盖了各种不同的文学流派和主题。 |
> |         [GPT-2](https://zh.wikipedia.org/wiki/GPT-2)         |   15亿   | WebText：一个包含八百万个文档的语料库，总大小为40 GB。这些文本是从Reddit上投票最高的4,500万个网页中收集的，包括各种主题和来源，例如新闻、论坛、博客、维基百科和社交媒体等。 |
> |         [GPT-3](https://zh.wikipedia.org/wiki/GPT-3)         |  1750亿  | 一个总大小为570 GB的大规模文本语料库，其中包含约四千亿个标记。这些数据主要来自于CommonCrawl、WebText、英文维基百科和两个书籍语料库（Books1和Books2）。 |
> |         [GPT-4](https://zh.wikipedia.org/wiki/GPT-4)         |    ~     |                              ~                               |
>
> ​	--  https://zh.wikipedia.org/wiki/GPT

### Embeddings

> **词嵌入**（Word embedding）是[自然语言处理](https://zh.wikipedia.org/wiki/自然语言处理)（NLP）中[语言模型](https://zh.wikipedia.org/wiki/語言模型)与[表征学习](https://zh.wikipedia.org/wiki/表征学习)技术的统称。概念上而言，它是指把一个维数为所有词的数量的高维空间[嵌入](https://zh.wikipedia.org/wiki/嵌入_(数学))到一个维数低得多的连续[向量空间](https://zh.wikipedia.org/wiki/向量空间)中，每个单词或词组被映射为[实数](https://zh.wikipedia.org/wiki/实数)[域](https://zh.wikipedia.org/wiki/数域)上的向量。
>
> 词嵌入的方法包括[人工神经网络](https://zh.wikipedia.org/wiki/人工神经网络)[[1\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-1)、对词语[同现矩阵](https://zh.wikipedia.org/w/index.php?title=同现矩阵&action=edit&redlink=1)[降维](https://zh.wikipedia.org/wiki/降维)[[2\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-2)[[3\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-3)[[4\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-4)、[几率模型](https://zh.wikipedia.org/wiki/概率模型)[[5\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-5)以及单词所在上下文的显式表示等。[[6\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-6)
>
> 在底层输入中，使用词嵌入来表示词组的方法极大提升了NLP中[语法分析器](https://zh.wikipedia.org/wiki/語法分析器)[[7\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-7)和[文本情感分析](https://zh.wikipedia.org/wiki/文本情感分析)等的效果。[[8\]](https://zh.wikipedia.org/wiki/词嵌入#cite_note-8)
>
> ​	-- https://zh.wikipedia.org/wiki/%E8%AF%8D%E5%B5%8C%E5%85%A5

### Tokens

​	GPT 和 Embedding 以称为token的块的形式处理文本。token代表常见的字符序列。例如，字符串“tokenization”被分解为“token”和“ization”，而像“the”这样的短而常见的单词则被表示为单个token。请注意，在句子中，每个单词的第一个token通常以空格字符开头。查看我们的token生成器工具来测试特定字符串并查看它们如何转换为token。根据粗略的经验，1 个标记大约相当于 4 个字符或 0.75 个英文单词



### 一些资源

- Experiment in the [playground](https://platform.openai.com/playground?mode=chat)
- Read the [API reference](https://platform.openai.com/docs/api-reference)
- Visit the [help center](https://help.openai.com/en/)
- View the current API [status](https://status.openai.com/)
- Check out the [OpenAI Developer Forum](https://community.openai.com/)
- Learn about our [usage policies](https://openai.com/policies/usage-policies)