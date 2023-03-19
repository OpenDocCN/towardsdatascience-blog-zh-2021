# 用 Amazon Lambda 和 API Gateway 构建一个无服务器 API

> 原文：<https://towardsdatascience.com/build-a-serverless-api-with-amazon-lambda-and-api-gateway-dfd688510436?source=collection_archive---------8----------------------->

## 如何以最简单的方式托管可伸缩的无服务器 API

![](img/a0a529157343192be23fb14006ec3672.png)

照片由 Unsplash 的 Saish Menon 拍摄。

# 目录

1.  介绍
2.  使用 GingerIt 创建一个拼写和语法纠正器
3.  将 Python 服务部署到 Lambda
4.  将 API 网关与 Lambda 相关联
5.  使用 API 密钥保护和抑制 API
6.  结论
7.  参考

# 介绍

在我以前的文章“[使用亚马逊 S3 和 Route 53](/build-a-serverless-website-using-amazon-s3-and-route-53-c741fae6ef8d) 构建一个无服务器网站”中，我演示了如何构建一个由静态(客户端)内容组成的无服务器网站。这种方法允许轻松地将网站从 10 个扩展到 1000 个，同时保持低成本。

为了将无服务器扩展到动态(服务器端)内容，Amazon Web Services (AWS)提供了更多的服务，如 AWS Lambda。与 API Gateway 一起，我们可以创建 RESTful APIs，实现实时双向通信应用程序。

在这篇文章中，我们将介绍如何使用 AWS Lambda 创建和部署拼写和语法纠正应用程序，并通过 API Gateway 响应标准 HTTP 请求。

# 使用 GingerIt 创建一个拼写和语法纠正器

GingerIt 是一个人工智能驱动的开源 Python 包，它可以根据完整句子的上下文来帮助纠正文本中的拼写和语法错误。

要使用它，让我们首先创建一个虚拟环境，并在其中安装 GingerIt。

为了测试它，在终端中运行下面的代码片段。

这将显示原始文本，然后是由 GingerIt 修改的文本。

```
 Original text: 'This is a corrects snetence'
Corrected text: 'This is a correct sentence'
```

# 将 python 服务部署到 Lambda

*“AWS Lambda 是一种无服务器计算服务，让您无需配置或管理服务器就能运行代码(…)。Lambda 自动精确地分配计算执行能力，并基于传入的请求或事件运行您的代码(…)。你可以用自己喜欢的语言(Node.js、Python、Go、Java 等)编写 Lambda 函数，同时使用无服务器和容器工具，比如 AWS SAM 或 Docker CLI。”*

Amazon 的上述声明总结了 Lambda 的目的:提供一个简单的服务，使用任何主要语言的定制代码对请求做出反应，并且不需要管理运行时。为了进一步促进 API 的开发和部署，Lambda 还支持 Docker。

为了部署 GingerIt 拼写和语法纠正服务，我们将利用 Lambda 的 Docker 支持。为此，我们首先需要将 Python 服务打包到 Docker 容器中。

让我们从创建 lambda 处理程序开始，每次执行 Lambda 函数时都会调用这个处理程序。这个处理程序将接收一个 json，其中包含我们想要修改的文本，用 GingerIt 处理它，并返回修改后的文本。将下面的代码片段另存为 **app.py** 。

现在我们只需将 app.py 文件包装在一个 **Dockerfile** 中，如下所示。请注意，您应该使用 AWS 支持的图像。这将确保正确设置执行 Lambda 所需的所有包。

要测试 lambda 函数，首先要构建并运行 docker 容器。

然后在终端或笔记本中运行下面的代码片段。

如果所有步骤都成功，您应该会看到以下输出。

```
{‘body’: ‘{“corrected_text”: “This is the correct sentence”}’, ‘headers’: {‘Content-Type’: ‘application/json’}, ‘isBase64Encoded’: False, ‘statusCode’: 200}
```

此时，我们知道本地 Lambda 工作正常。下一步是远程运行它(无服务器)。为此，我们需要:

1.  将 docker 映像推送到 Amazon 弹性容器注册中心(ECR)
2.  创建一个新的 Lambda 函数，并将其与 ECR 图像相关联
3.  为 Lambda 函数设置 IAM 访问权限

幸运的是，通过使用*无服务器框架，整个过程(以及后面介绍的额外步骤)可以被简化。*要安装它，只需按照[官方无服务器网站](https://www.serverless.com/framework/docs/getting-started) ⁴.

安装完 serverless 后，创建一个名为 **serverless.yml** 的新文件，并在其中添加以下内容。

这个文件定义了 Lambda 中的关键参数。

```
**service**: Name of the service we are creating. This name will be user across all required resources.**provider:
  name**: Name of cloud provider to use (AWS)
  **stage**: Stage to use (dev, prod)
  **region**: Region to where to deploy the lambda function
  **runtime**: Runtime that will be used to run the docker container. the python version should match the docker one.
  **ecr**:
    **imager**:
      **gingerit**: Target name for the docker image
        **path**: Defines where the docker file is**functions: 
  gingerit**: Defines the parameters for the lambda function named ‘gingerit’
    **timeout:**If request takes longer than the defined value, return time out
    **memory_size**: Total docker RAM in Megabytes
    **image:
      name**: Name of the docker image to use. Must match provider.ecr.image
```

要部署我们的服务，我们现在只需在终端中调用**无服务器部署**。应该会出现以下输出(将映像部署到 ECR 和 Lambda 函数可能需要几分钟时间)。

```
Serverless: Stack update finished...
Service Information
service: language-corrector
stage: dev
region: eu-west-1
stack: language-corrector-dev
resources: 6
api keys:
  None
endpoints:
  None
functions:
  gingerit: language-corrector-dev-gingerit
```

这个输出显示了我们的 lambda 函数的全名**(**language-corrector-dev-gingerit**)**以及我们定义的一些变量。要测试该函数，请在终端或笔记本单元格中运行以下代码片段。****

****应该可以看到以下输出:****

```
**{'body': '{"corrected_text": "This is the correct sentence"}',
 'headers': {'Content-Type': 'application/json'},
 'isBase64Encoded': False,
 'statusCode': 200}**
```

****万岁！您刚刚创建了一个拼写和语法纠正工具，并将其部署为无服务器。****

# ****将 API 网关与 Lambda 相关联****

*****“API Gateway 处理与接受和处理多达数十万个并发 API 调用相关的所有任务，包括流量管理、CORS 支持、授权和访问控制、节流、监控和 API 版本管理。(…) API Gateway 支持容器化和无服务器工作负载，以及 web 应用程序。”⁵*****

****总之，API Gateway 充当了客户端浏览器和 Lambda 函数之间的中间人，接受并处理 HTTP 请求和响应。此外，它还可以用来设置每秒允许的调用次数的限制，或者给定函数或 api 键的每月上限。****

****要使用它，我们只需在无服务器的函数中添加一个 **events** 对象。我们新的无服务器看起来像这样:****

****事件对象定义了设置 API 网关所需的字段。****

```
****http:** Defines the event as an HTTP method
  **path:** Endpoint path name
  **method:** HTTP method used to access this endpoint
  **cors:** If true allows Cross-Origin Resource Sharing
  **async:** Endpoint can be accessed async
  **private:** Endpoint requires an API key to be accessed**
```

****如果我们现在重新运行**无服务器部署**，则会出现一个新字段(**端点**)。这个字段指示 API 网关的地址，我们需要它通过 HTTP 请求调用 Lambda 函数。****

```
**endpoints:
  POST - [https://olx5ac31og.execute-api.eu-west-1.amazonaws.com/dev/language_corrector](https://olx5ac31og.execute-api.eu-west-1.amazonaws.com/dev/language_corrector)**
```

****这表示我们需要通过标准 HTTP 请求调用 Lambda 函数的 API 网关地址。要测试新端点，请运行下面的代码片段。****

****如果成功实现了 API 网关，应该会出现正确的文本:****

```
**{'corrected_text': 'This is the correct sentence'}**
```

# ****使用 API 密钥保护和抑制 API****

****到目前为止，我们有一个部署在 Lambda 中的工作 API，它可以通过标准的 HTTP 方法接受请求。****

****然而，任何知道 URL 的人都可以访问这个函数，并且对每秒的请求数没有限制。这是很危险的，因为如果有太多的请求，它会很快增加你每月的花费。****

****为了减轻这种情况，我们可以在 API Gateway 中设置 API 密钥。要使用它，我们只需在无服务器中向我们的提供者添加一个 **apiGateway** 对象，并将我们的 **events** 对象中的私有标志设置为 true。我们新的无服务器看起来像这样:****

****apiGateway 对象为我们的 API 定义了使用计划。在这个例子中，我们设置了两个 API 键。一个有一些限制的免费 API 密匙，和一个允许每秒更多请求和更高上限的付费 API 密匙。****

```
**apiGateway:
    apiKeys:
      - free: name of the usage plan
          - name of the api key
    usagePlan:
      - free:
          quota:
            limit: Maximum number of requests in a time period
            offset: The day that a time period starts
            period: The time period in which the limit applies
          throttle:
            burstLimit: Rate limit over a short amount of time
            rateLimit: API request steady-state rate limit**
```

****如果我们现在重新运行**无服务器部署**，则会出现一个新字段( **api 密钥**)。该字段指示我们的新 API 密钥的名称和值。****

```
**api keys:
  medium-language-corrector-free: FREE_KEY
  medium-language-corrector-paid: PAID_KEY**
```

****要测试新端点，请运行下面的代码片段。****

****如果成功实现了 API 网关，应该会出现正确的文本:****

```
**{'corrected_text': 'This is the correct sentence'}**
```

## ****完成脚本****

****如需完整的脚本，请点击以下链接进入我的 GitHub 页面:****

****[](https://github.com/andreRibeiro1989/medium/tree/main/spell_grammar_correction) [## medium/main and Ribeiro 1989/medium 拼写语法纠错

### 这是我的中型文章 X 的辅助代码，解释了如何用 Amazon Lambda 和

github.com](https://github.com/andreRibeiro1989/medium/tree/main/spell_grammar_correction) 

# 结论

使用无服务器计算允许您在不管理 web 服务器的情况下扩展应用程序，同时保持低成本。

虽然对于静态内容，最简单和最实惠的选择是亚马逊简单存储服务(S3)，但这种方法不允许我们使用动态服务器端代码。

为了将无服务器扩展到更复杂的应用程序，在本教程中，我们探索了如何使用 Amazon Lambda 和 API Gateway 来部署无服务器拼写和语法纠正工具。

这个简单的例子将为您提供构建更复杂的应用程序所需的所有工具，比如无服务器聊天机器人或图像处理服务。**** 

****[**加入我的邮件列表，我一发布就有新内容！**](https://andrefsr.medium.com/subscribe)****

****如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。每月 5 美元，让你可以无限制地访问 Python、机器学习和数据科学文章。如果你使用[我的链接](https://andrefsr.medium.com/membership)注册，我会赚一小笔佣金，不需要你额外付费。****

****[](https://andrefsr.medium.com/membership) [## 通过我的推荐链接加入 Medium-andréRibeiro

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

andrefsr.medium.com](https://andrefsr.medium.com/membership) 

# 参考

[1] A .里贝罗。"*用亚马逊 S3 和 Route 53* 搭建一个无服务器网站"
https://towardsdatascience . com/Build-a-server less-website-using-Amazon-S3-and-Route-53-c 741 FAE 6 ef 8d

[2] T .克莱因施密特。"*欢迎使用 Gingerit 的文档！*(2015)
[https://gingerit.readthedocs.io/en/latest/](https://gingerit.readthedocs.io/en/latest/)

[3]亚马逊团队。*AWS Lambda*
[https://aws.amazon.com/lambda/](https://aws.amazon.com/lambda/)

[4]无服务器团队。*无服务器框架—文档*
[https://www.serverless.com/framework/docs/getting-started](https://www.serverless.com/framework/docs/getting-started)

[5]亚马逊团队。*亚马逊 API 网关*
[https://aws.amazon.com/api-gateway/](https://aws.amazon.com/api-gateway/)****