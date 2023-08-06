---
layout: default
title:  "strategy: write clear instructions"
date:   2023-07-10
categories: openai best-practices
typora-root-url: ../../
parent: best-practices
---

# strategy: write clear instructions

## 策略：编写清晰的指示 

### 技巧：在你的查询中包含详细信息以获取更相关的答案 

​	为了得到高度相关的回应，确保请求提供任何重要的细节或上下文信息。否则，你就把理解你的意图的工作留给了模型。

| 反例                       | 正例                                                         |
| -------------------------- | ------------------------------------------------------------ |
| 如何在Excel中加数？        | 如何在Excel中累加一行的美元金额？我希望自动为整个工作表的行进行此操作，所有的总计都在右边的名为“总计”的列中。 |
| 谁是总统？                 | 2021年墨西哥的总统是谁？选举是多久举行一次？                 |
| 编写代码计算斐波那契数列。 | 编写一个TypeScript函数来高效计算斐波那契数列。请大量注释代码，解释每一部分的作用以及为什么这样编写。 |
| 总结会议记录。             | 将会议记录总结为一段话。然后写一个markdown列表，列出发言人及他们的关键点。最后，列出发言人提出的下一步行动或行动事项（如果有的话）。 |

### 技巧：让模型采用人物角色 

系统消息可以用来指定模型在回复中所使用的人物角色。

![system/user](/assets/images/best-practices/1.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-playful-thank-you-note) 里是试一下

### 技巧：使用分隔符清楚地标明输入的不同部分 

如三引号、XML标签、章节标题等分隔符可以帮助划分需要以不同方式处理的文本部分。

![2](/assets/images/best-practices/2.png)

在官网 [ Playground ](https://platform.openai.com/playground/p/default-delimiters-1) 里是试一下

![3](/assets/images/best-practices/3.png)

在官网 [ Playground ](https://platform.openai.com/playground/p/default-delimiters-2)  里是试一下

![4](/assets/images/best-practices/4.png)

在官网 [ Playground ](https://platform.openai.com/playground/p/default-delimiters-3) 里是试一下

​	对于这种直接的任务，使用分隔符可能不会改变输出质量。然而，任务越复杂，消除任务细节的歧义性就越重要。不要让GPT为理解你的具体要求而费力。

### 技巧：明确指定完成任务所需的步骤 

有些任务最好是以一系列步骤来指定。明确地写出步骤可以让模型更容易地遵循它们。

![5](/assets/images/best-practices/5.png)

在官网 [Playground ](https://platform.openai.com/playground/p/default-step-by-step-summarize-and-translate) 试一下

### 技巧：提供示例 

提供适用于所有示例的一般性指示通常比通过示例演示任务的所有排列更高效，但在某些情况下，提供示例可能更容易。例如，如果你希望模型复制一种特定的响应用户查询的方式，这种方式很难明确描述。这被称为“少示例”提示。

![6](/assets/images/best-practices/6.png)

在官网 [Playground ](https://platform.openai.com/playground/p/default-chat-few-shot) 试一下

### 技巧：指定输出的期望长度 

​	你可以要求模型产生给定目标长度的输出。目标输出长度可以按词数、句数、段落数、项目符号点数等来指定。但请注意，指示模型生成特定数量的词汇的精度并不高。模型更能可靠地生成具有特定数量段落或项目符号点的输出。

![7](/assets/images/best-practices/7.png)

在官网 [Playground ](https://platform.openai.com/playground/p/default-summarize-text-50-words) 试一下

![8](/assets/images/best-practices/8.png)

在官网 [Playground ](https://platform.openai.com/playground/p/default-summarize-text-2-paragraphs) 试一下

![9](/assets/images/best-practices/9.png)

在官网 [Playground ](https://platform.openai.com/playground/p/default-summarize-text-3-bullet-points) 试一下
