---
layout: default
title:  "strategy: give GPTs time to think"
date:   2023-07-12
categories: openai best-practices
typora-root-url: ../../
parent: best-practices
---

# Strategy: Give GPTs time to "think"

## 策略：在模型匆忙得出结论前，指导它自己找出解决方案 

​	有时候，当我们明确指导模型在得出结论前先从基本原则进行推理，我们会得到更好的结果。比如，假设我们希望模型评估一个学生对数学问题的解答。最直观的方法就是直接问模型学生的解答是否正确。

![3](/assets/images/best-practices-3/3.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-rushing-to-a-conclusion) 里是试一下

​	但学生的解答实际上是不正确的！我们可以通过提示模型首先生成自己的解答，使模型成功地注意到这一点。

![4](/assets/images/best-practices-3/4.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-avoid-rushing-to-a-conclusion) 里是试一下

## 策略：使用内心独白或一系列查询来隐藏模型的推理过程

​	前一个策略表明，在回答特定问题之前，模型有时需要对问题进行详细推理。对于某些应用来说，模型用来得出最后答案的推理过程不适合与用户共享。例如，在辅导应用中，我们可能想鼓励学生自己找出答案，但是模型对学生解答的推理过程可能会向学生透露答案。

内心独白是一种可以用来缓解这个问题的策略。内心独白的想法是指导模型将那些需要从用户视线中隐藏的输出部分放入一种结构化的格式，使得解析它们变得容易。然后在将输出呈现给用户之前，对输出进行解析，并且只有部分输出被显示。

![5](/assets/images/best-practices-3/5.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-query-sequence-2) 里是试一下

或者，可以通过一系列查询来实现这一点，其中除最后一个以外的所有查询的输出都对最终用户隐藏。

首先，我们可以要求模型自己解决问题。由于这个初始查询不需要学生的解答，所以它可以省略。这提供了额外的优势，那就是模型的解答没有机会受到学生试图解答的影响。

![6](/assets/images/best-practices-3/6.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-query-sequence-2) 里是试一下

接下来，我们可以让模型使用所有可用的信息来评估学生解答的正确性。

![7](/assets/images/best-practices-3/7.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-query-sequence-2) 里是试一下

最后，我们可以让模型使用自己的分析，以一个有帮助的辅导员的角色构建回复。

![8](/assets/images/best-practices-3/8.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-query-sequence-3) 里是试一下

## 策略：询问模型是否在前几轮中遗漏了什么

​	假设我们正在使用一个模型列出一个来源中与特定问题相关的摘录。在列出每个摘录后，模型需要确定是否应该开始编写另一个或是否应该停止。如果源文件很大，模型通常会过早地停止并且无法列出所有相关的摘录。在这种情况下，通过用跟进查询提示模型找出它在前几轮中遗漏的任何摘录，通常可以获得更好的性能。

![9](/assets/images/best-practices-3/9.png)

在官网 [Playground](https://platform.openai.com/playground/p/default-2nd-pass) 里是试一下
