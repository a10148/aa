---
layout: default
title:  "strategy: Split complex tasks into simpler subtasks"
date:   2023-07-12
categories: openai best-practices
typora-root-url: ../../
parent: best-practices
---

# Strategy: Split complex tasks into simpler subtasks

## 策略：使用意图分类来识别用户查询中最相关的指令

对于需要大量独立指令集来处理不同情况的任务，首先分类查询类型并根据该分类确定所需的指令可能是有益的。这可以通过定义固定的类别并硬编码处理给定类别任务中相关的指令来实现。此过程也可以递归应用来将任务分解为一系列阶段。这种方法的优点是每个查询将只包含执行任务的下一阶段所需的指令，与使用单个查询来执行整个任务相比，这可能会导致错误率降低。这也可能导致成本降低，因为更大的提示运行成本更高（参见[定价信息](https://openai.com/pricing)）。

例如，对于客户服务应用，查询可能有用地被分类如下：

![1](/assets/images/best-practices-3/1.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-decomposition-by-intent-classification-1) 里是试一下

​	基于对客户查询的分类，可以为GPT模型提供一套更具体的指令来处理下一步。例如，假设客户需要帮助进行"故障排查"。

![2](/assets/images/best-practices-3/2.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-decomposition-by-intent-classification-2) 里是试一下

​	请注意，已指示模型发出特殊的字符串以表示对话状态的改变。这使我们能够将我们的系统转变为一个状态机，其中状态决定了注入哪些指令。通过跟踪状态、在该状态下什么指令是相关的，以及在该状态下允许什么状态转换（如果有的话），我们可以在用户体验周围设定保护性措施，这在使用较少结构化的方法时难以实现。

## 策略：对需要进行非常长对话的对话应用程序，对之前的对话进行总结或筛选

​	由于GPT具有固定的上下文长度，因此用户与助手之间的对话中，如果整个对话都包含在上下文窗口中，则不能无限期地进行。

对于这个问题有各种解决方法，其中之一是总结对话中的先前轮次。一旦输入的大小达到预定的阈值长度，就可能触发一个查询，总结对话的部分内容，而先前对话的总结可以作为系统消息的一部分包含在内。或者，可以在整个对话过程中在后台异步地总结先前的对话。

另一种解决方案是动态选择与当前查询最相关的对话的先前部分。请参见策略"[使用基于嵌入的搜索来实现高效的知识检索](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-embeddings-based-search-to-implement-efficient-knowledge-retrieval)"。

## 策略：逐段总结长文档，并递归构建完整的摘要 

​	由于GPT具有固定的上下文长度，所以不能在一个查询中用于总结比上下文长度减去生成摘要的长度更长的文本。

要总结一个非常长的文档，如一本书，我们可以使用一系列查询来总结文档的每一部分。各节的摘要可以被连接起来并总结，从而产生摘要的摘要。这个过程可以递归进行，直到整个文档被总结。如果在理解书中后面的部分时，需要使用关于早期部分的信息，那么在总结某一点的内容时，包含该点前面的文本的运行摘要可能会有用。OpenAI在使用GPT-3的变体进行的[早期研究](https://openai.com/research/summarizing-books)中已经研究过这种总结书籍的程序的有效性。

