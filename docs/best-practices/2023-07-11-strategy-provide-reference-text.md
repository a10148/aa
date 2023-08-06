---
layout: default
title:  "strategy: provide reference"
date:   2023-07-11
categories: openai best-practices
typora-root-url: ../../
parent: best-practices
---

# strategy: provide reference text

## 策略：提供参考文本

### 技巧：指示模型使用参考文本来回答

​	如果我们能提供与当前查询相关的可信信息给模型，那么我们可以指示模型使用所提供的信息来编写其答案。

![1](/assets/images/best-practices-2/1.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-answer-from-retrieved-documents) 里是试一下

​	考虑到GPT的上下文窗口有限，为了应用这种策略，我们需要一种方法来动态查找与所问问题相关的信息。嵌入（[Embeddings](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)）可以用于实现高效的知识检索。查看策略"[使用基于嵌入的搜索实现高效的知识检索](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-embeddings-based-search-to-implement-efficient-knowledge-retrieval)"以获取如何实现此目标的更多细节。

## 策略：指导模型用参考文本中的引用来回答

​	如果输入已经通过相关知识进行了补充，那么要求模型通过引用提供的文档中的段落来添加引用到其答案中就变得非常简单。请注意，输出中的引用可以通过在提供的文档中进行字符串匹配来以程序化方式进行验证。

![2](/assets/images/best-practices-2/2.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-answer-with-citation) 里是试一下
