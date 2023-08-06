---
layout: default
title:  "first app"
date:   2023-07-03
categories: openai quickstart	
typora-root-url: ../
parent: quickstart
---

# GO

# 构建第一个程序

​	经过前面的介绍，已经对基本概念有所了解。现在我们根据官方的例子实际运行一下。

> 1. 如果你本地没有python环境，请先[下载python](https://www.python.org/downloads/)并安装。你也可以通过[链接下载zip](https://github.com/openai/openai-quickstart-python/archive/refs/heads/master.zip)包
> 2. 如果你本地没有git环境，请先[下载git](https://git-scm.com/downloads)并安装，
> 3. 创建[Secret key](https://platform.openai.com/account/api-keys)

通过git从github上clone下来最新的代码

```shell
git clone https://github.com/openai/openai-quickstart-python.git
```

切换到项目目录openai-quickstart-python。 从环境配置文件样例复制一份环境配置文件。

```shell
cd openai-quickstart-python
cp .env.example .env  # window下是 copy .env.example .env
```

使用你喜欢的工具，编辑 .env 文件。配置OPEN_API_KEY。效果如下：

```properties
FLASK_APP=app
FLASK_ENV=development

# Once you add your API key below, make sure to not share it with anyone! The API key should remain private.
OPENAI_API_KEY=sk-5r6JsRQixRtCqZTvs6o8T3BlbkFJXXXXXxxxxxxxx
```

执行以下命令来运行你的第一个程序

```shell
python -m venv venv
. venv/bin/activate # windows 平台用 venv\Scripts\activate
pip install -r requirements.txt 
flask run
```

运行效果

```shell

(venv) C:\Users\user\vscodeProjects\openai-quickstart-python>flask run
 * Serving Flask app 'app.py' (lazy loading)
 * Environment: development
 * Debug mode: on
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 138-932-999
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)	
```

浏览器打开 http://localhost:5000 。尝试下输入和提交。

<img src="/assets/images/quick-start-10.png" style="zoom:38%;FLO" />

<center>图-10</center>

<img src="/assets/images/quick-start-11.png" alt="10" style="zoom:38%;" />

<center>图-11</center>

## 关键代码含义

​	在openai-quickstart-python目录的 `app.py` 文件的最底部，定义了一个生成提示(prompt)的方法。可以看到提示里说明了想要什么，以及给与了一些例子方便模型理解。

```python
def generate_prompt(animal):
    return """Suggest three names for an animal that is a superhero.

Animal: Cat
Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
Animal: Dog
Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
Animal: {}
Names:""".format(animal.capitalize())
```

​	在openai-quickstart-python目录的 `app.py` 文件第14行，这里实际调用 OPENAI 的 API ，返回结果放到response变量里。

```python
response = openai.Completion.create(
  model="text-davinci-003",
  prompt=generate_prompt(animal),
  temperature=0.6
)
```

​	截至到现在，你了解了GPTs，Tokens，Embeddings等概念，动手试了官网文档中的例子，自己基于github上的项目实际运行起来了第一个app，相信你对OPENAI，GTP有了更多的认识。

## 关于模型及价格

​	OPENAI 提供一系列具有不同功能和[价位](https://openai.com/pricing/)的[模型](https://platform.openai.com/docs/models)。上面的例子我们使用了text-davinci-003。我们建议在实验时使用此模型或 gpt-3.5-turbo，因为目前看这个模型是用来练手最合适不过。您就可以查看其他模型是否可以以更低的延迟和成本产生相同的结果。加入你需要迁移到更强大的模型，例如 gpt-4。

​	单个请求（提示和结果）中处理的令牌总数不能超过模型的最大上下文长度。对于大多数模型，这是 4,096 个标记或大约 3,000 个单词。根据粗略的经验，1 个标记大约相当于 4 个字符或英文文本的 0.75 个单词。

​	最关键的，每个新注册的账号在三个月内可以获得5美元的API使用额度。具体使用量，参考不同[模型](https://platform.openai.com/docs/models)以及[价格](https://openai.com/pricing/)

## 小总结

​	前面讲到的概念和[接口](https://platform.openai.com/docs/api-reference/completions)将在帮助您构建自己的应用程序方面发挥很大作用。也就是说，这个简单的例子只展示了一小部分可能性！灵活的使用，几乎可以解决任何语言处理任务，包括内容生成、摘要、语义搜索、主题标记、情感分析等等。

​	对于更高级的任务，你可能会需要比单个提示所能容纳的更多示例或上下文的能力。对于此类更高级的任务， [fine-tuning API](https://platform.openai.com/docs/guides/fine-tuning) 是一个不错的选择。微调允许您提供数百甚至数千个示例，以便为您的特定用例定制模型。
