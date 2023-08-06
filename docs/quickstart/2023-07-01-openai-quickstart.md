---
layout: default
title:  "quickstart"
date:   2023-07-01
categories: openai quickstart	
typora-root-url: ../
order: 2
parent: quickstart
---

# Quickstart

​	OpenAI已经训练出了先进的语言模型，这些模型在理解和生成文本方面表现出色。我们的API提供对这些模型的访问，并可用于解决几乎任何涉及语言处理的任务。

​	在本快速入门教程中，您将构建一个简单的示例应用程序。在此过程中，您将学习使用 API 执行任何任务所必需的关键概念和技术，包括：

- Content generation
- Summarization
- Classification, categorization, and sentiment analysis
- Data extraction
- Translation
- Many more!

## Introduction

​	completions endpoint 是 Open AI 的核心 API。它提供了一个简单而灵活、功能强大的接口。您可以将一些文本作为提示(prompt)输入，API将返回一个文本结果(completion)，尝试与您提供的提示(prompt)或上下文相匹配。

​	您可以将其视为非常高级的自动完成功能 - 该模型处理您的文本提示并尝试预测后续将出现的内容。

​	参考下图，给模型一个提示(Prompt), 模型会生成相应的结果(Completion)

![img](/assets/images/quick-start-1.png "图-1")

<center>图-1</center>

## 动手试一下

​	假设你想要创建一个宠物名称生成器，首先，您需要一个提示来明确您想要什么。让我们从一条提示(Prompt)开始。提交此提示以生成您的第一个结果。

![prompt](/assets/images/quick-start-2.png)	

<center>图-2</center>

​	很好，我们具体一些。

![prompt2](/assets/images/quick-start-3.png)

<center>图-3</center>	

​	正如您所看到的，在提示中添加一个简单的形容词会改变最终的完成结果。设计(prompt)提示本质上就是如何“编程”模型。

## 在Prompt里 添加一些样例

​	编写良好的提示对于获得良好的结果很重要，但有时这还不够。让我们尝试让您的指令变得更加复杂。

​	![start4](/assets/images/quick-start-4.png)

<center>图-4</center>

​	这个结果并不是我们想要的。这些名称非常通用，而且模型似乎没有接受我们prompt中关于马的部分的设定。让我们看看是否可以让它提出一些更相关的建议。

​	多数情况下，向模型展示并告诉模型你想要什么是有帮助的。在prompt中添加示例可以帮助模型理解方式或意思差别。我们尝试提示(prompt)中添加几个示例。

​	![start5](/assets/images/quick-start-5.png)

<center>图-5</center>	

​	这次不错！添加我们期望的输出示例有助于模型生成更符合我们期望的结果。

## 添加一些设置

​	设计完美的 Prompt 并不是获取期望结果的唯一途径。可以通过设置开控制模型的行为。其中最重要的一个设置是温度(temperature)

​	由于本章节内容都是文字或图片，无法完成交互动作，看不出来某些效果。建议各位到 [ChatGPT 官网](https://platform.openai.com/docs/quickstart/adjust-your-settings)体验一下。其实本章节 图-5 中代表的内容每次提交后，生成结果几乎都是一模一样的。这是因为模型的温度(temperature)设置为了0；

​	尝试将温度设置为 1 并重新提交相同的提示几次。

<img src="/assets/images/quick-start-6.png" alt="6-1" style="zoom:33%;" />

<img src="/assets/images/quick-start-6-1.png" style="zoom:33%;" />

<img src="/assets/images/quick-start-6-2.png" style="zoom:33%;" />

看看结果当温度设置为1时，每次提交相同的提示都会导致不同的完成结果。

请记住，模型会预测哪个文本最有可能跟随其前面的文本。温度是一个介于 0 和 1 之间的值，本质上可以让您控制模型在进行这些预测时的置信度。降低温度意味着风险更少，完成将更加准确和确定。升高温度将导致更加多样化的完成。

对于您的宠物名字生成器，您可能希望能够生成大量的名字创意。 0.6 的适中温度应该可以很好地发挥作用。



扩展阅读 - [理解Token和概率] [2023-07-02-openai-understand-token-probabilities.md](2023-07-02-openai-understand-token-probabilities.md) ()