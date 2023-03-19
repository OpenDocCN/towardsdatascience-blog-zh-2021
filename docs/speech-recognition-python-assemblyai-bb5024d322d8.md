# 如何使用 Python 执行语音识别

> 原文：<https://towardsdatascience.com/speech-recognition-python-assemblyai-bb5024d322d8?source=collection_archive---------7----------------------->

## 用 Python 中的 AssemblyAI API 执行语音转文本

![](img/3a0758dac967f16f834b7f1a5222412e.png)

马特·博茨福德在 [Unsplash](https://unsplash.com/s/photos/speech?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 什么是语音识别

语音识别，也被称为**自动语音识别**或**语音到文本，**是位于中的的一个领域，是*计算机科学*和*计算语言学*的交叉，开发某些技术使计算机系统能够处理人类语音并将其转换为文本格式。换句话说，语音识别方法和工具用于**将语音从口头格式翻译成文本**。

在语音识别设置中使用的最佳执行算法利用了来自人工智能和机器学习领域的技术和概念。这些算法中的大多数都随着时间的推移而改进，因为它们能够通过交互来增强它们的能力和性能。

语音识别的应用可以在许多行业中找到，这些行业使用这种技术来帮助用户、消费者和企业提高效率。苹果 Siri、亚马逊 Alexa、T21 和谷歌助手等虚拟代理利用语音识别技术，通过语音命令访问某些功能。语音识别的其他应用包括汽车中的**声控导航系统**，医疗保健中的**文档口述**，甚至安全设置中的**语音认证**。

由于对语音识别技术的需求不断增加，该领域已经取得了巨大的发展，这使得开发人员和组织可以很容易地将它们分别集成到他们的代码库和产品中。在接下来的章节中，我们将探索如何用 **Python** 和 **AssemblyAI API** 、**只用几行代码**就能让**执行语音识别**。

## 使用 AssemblyAI API 进行语音到文本转换

[AssemblyAI](https://www.assemblyai.com/) 提供强大的**语音转文本 API** ，由先进的 AI 提供动力，使用户能够准确转录音频和视频文件。在今天的指南中，我们将使用这个 API 来对 mp3 音频文件执行语音识别。

如果你想跟随这个教程，你所需要的就是一个 API 密匙，如果你注册了一个免费的 AssemblyAI 账户，你就可以得到它。一旦您这样做了，您的密钥应该会出现在您的*帐户*部分。

出于本教程的目的，我准备了一个简短的音频文件，您可以在下面找到。请随意创建您自己的或使用我已经为您创建的。

测试我们将在示例语音到文本教程中使用的音频文件——来源:[作者](https://gmyrianthous.medium.com/)

现在，我们将使用`requests`库来调用 AssemblyAI API，它需要您获得的 API 键以及下面定义的头。

导入请求库并定义请求头—来源:[作者](https://gmyrianthous.medium.com/)

下一步，是读入文件并上传到 AssemblyAI 主机服务上，以获得一个链接，然后我们将使用它来转录实际的音频。

上传音频文件到 AssemblyAI 主机服务—来源:[作者](https://gmyrianthous.medium.com/)

现在，我们已经成功地将音频文件上传到 AssemblyAI 的托管服务，我们可以继续将上一步中授予的上传 url 发送到 AssemblyAI 的转录端点。来自端点的示例响应显示在要点末尾的注释中。

向抄本端点发送请求—来源:[作者](https://gmyrianthous.medium.com/)

最后，我们可以通过提供从上一步的转录端点的响应中收到的转录 ID 来访问转录结果。请注意，**我们将不得不重复** `**GET**` **请求**，直到响应中的状态为`completed`或`error`，以防音频文件处理失败。

在成功完成时接收到的来自抄本端点的示例响应作为评论被共享在下面的要点的末尾。

发送重复的 GET 请求，直到失败或成功完成—来源:[作者](https://gmyrianthous.medium.com/)

最后，假设文件处理成功结束，我们可以将最终响应写入文本文件。

将输出脚本写入文件—来源:[作者](https://gmyrianthous.medium.com/)

对于我们的示例音频文件，我们从 AssemblyAI 语音到文本 API 获得的输出将被写入输出文本文件

> 你知道，电视上的恶魔就是这样。让人们暴露自己在电视上被拒绝或被恐惧因素羞辱。

这是相当准确的！

## 完整代码

我们使用 AssemblyAI 语音转文本 API 的完整代码可以在下面的 GitHub Gist 中找到。概括地说，代码将从您的本地系统上传一个音频文件到 AssemblyAI 主机服务，然后将它提交给执行语音到文本任务的抄本服务。最后，我们处理输出响应，并将其存储到本地文件系统的一个文本文件中。

使用 AssemblyAI API 实现语音转文本的完整代码来源:[作者](https://gmyrianthous.medium.com/)

请注意，在本教程中，我已经上传了一个本地文件到 AssemblyAI 托管服务，但你甚至可以从任何云服务(如 AWS)提交音频 URL。更多细节和例子，可以参考 AssemblyAI 官方文档中的[这一节。](https://docs.assemblyai.com/overview/getting-started)

## 最后的想法

语音识别是一个快速发展的领域，它极大地受益于机器学习、人工智能和自然语言处理的巨大进步。由于对语音到文本应用程序的需求不断增长，出现了大量能够快速访问这些技术的工具。

在今天的文章中，我们探索了如何用 Python 快速执行语音识别，只需几行代码，使用 AssemblyAI，这是一个强大的 API，被全球成千上万的组织使用。该 API 提供了一系列我们在本文中没有涉及的特性，但是您可以在这里探索。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership> 