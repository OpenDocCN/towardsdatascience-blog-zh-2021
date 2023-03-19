# 如何用 Python 对音频文件进行情感分析

> 原文：<https://towardsdatascience.com/sentiment-analysis-assemblyai-python-a4686967e0fc?source=collection_archive---------5----------------------->

## 探索如何使用 AssemblyAI API 提取语音中的情感

![](img/0599f78ee416999734c4e281b6443776.png)

照片由[伯爵克里斯](https://unsplash.com/@countchris?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/happy-sad?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 介绍

情感分析指的是为了检测输入文本中的情感而对文本数据执行的一系列过程——作为自然语言处理的一部分。

这种情绪通常可以分为积极、消极或中性，尽管这并不标准。相反，你可能更喜欢建立一个模型，最终输出更广泛的极性，如非常积极、积极、中性、消极和非常消极。此外，你可以试着预测一种情绪，而不是极性(快乐、悲伤、愤怒等)。).

由于机器和深度学习方法的应用，情感分析在过去几年中得到了很大的发展，这些方法可以非常准确和及时地执行这项任务。

情感分析工具的一些最常见的应用包括品牌监控和产品情感，帮助企业解释客户反馈并增强他们对客户需求的理解

在接下来的章节中，我将演示如何使用 **AssemblyAI API** 和 **Python** 对音频文件执行[情感分析。](https://docs.assemblyai.com/guides/sentiment-analysis)

## 对音频文件执行情感分析

如果你想跟随这个教程，那么你首先必须从 AssemblyAI 获得你的 [API 密匙(这是免费的！).现在您已经有了访问键，我们现在可以准备将发送到 API 端点的请求头了:](https://app.assemblyai.com/signup)

导入请求库并定义请求头—来源:[作者](https://gmyrianthous.medium.com/)

接下来，我们现在需要将我们的音频文件上传到 AssemblyAI 的托管服务，该服务将使用将在后续请求中使用的`upload_url`进行响应。

上传音频文件到 AssemblyAI 的 API 托管服务—来源:[作者](https://gmyrianthous.medium.com/)

现在，我们已经检索到了作为上一个调用的响应的一部分的上传 URL，我们现在可以开始获取音频文件的转录。**请注意，在一次呼叫中，我们还可以请求执行情感分析。**就这么简单！

使用 AssemblyAI API 执行语音到文本和情感分析—来源:[作者](https://gmyrianthous.medium.com/)

注意，对于上面的调用，我们同时执行语音到文本和情感分析！为了指示 API 也执行后者，我们需要将`sentiment_analysis`参数(设置为`True`)传递给`POST`请求。

值得一提的是，目前 AssemblyAI API 的情绪分析功能能够输出三种情绪，即`POSITIVE`、`NEGATIVE`和`NEUTRAL`。不过，请留意他们的[文档](https://docs.assemblyai.com/guides/sentiment-analysis)，因为他们在不断改进他们的 API 及其功能。

## 解读回应

上一步/调用中的调用的响应结构应该与下面给出的结构相同。语音到文本输出将在`text`键下可用，情感分析的结果也将是`sentiment_analysis_results`键下响应的一部分。

注意，对于情感分析结果中的每个片段，您还可以获得特定音频片段的时间戳(`start`和`end`)以及`sentiment`。

AssemblyAI API 端点在执行情感分析时获得的响应—来源:[作者](https://gmyrianthous.medium.com/)

## 完整代码

下面是作为本教程一部分的完整代码。总而言之，我们首先必须将我们的音频文件上传到 AssemblyAI API 的托管服务，以便检索映射到要转录的音频文件的 URL。然后，我们在对 AssemblyAI 的脚本端点的单次调用中执行语音到文本和情感分析，以便检索音频文件的文本形式以及各个片段的情感。

本教程的完整代码——来源:[作者](https://gmyrianthous.medium.com/)

## 最后的想法

在今天的文章中，我们讨论了什么是情感分析以及在什么样的环境下有用。此外，我们展示了如何利用 AssemblyAI API 对音频文件进行情感分析。

他们的 API 附带了许多我们在今天的指南中没有真正讨论过的特性。请务必查看他们在[官方文档](https://docs.assemblyai.com/overview/getting-started)中的完整列表以及他们的[产品更新](https://changelog.assemblyai.com/)。

此外，看看这篇文章后面的文章，它可以帮助你开始语音转文本和主题摘要。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒介上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](/summarize-audio-video-files-assemblyai-c9126918870c) [## 如何用 Python 汇总音视频文件

### 了解如何使用 AssemblyAI API 的自动章节功能摘要音频文件

towardsdatascience.com](/summarize-audio-video-files-assemblyai-c9126918870c) [](/real-time-speech-recognition-python-assemblyai-13d35eeed226) [## 如何用 Python 执行实时语音识别

### 使用 Python 中的 AssemblyAI API 执行实时语音转文本

towardsdatascience.com](/real-time-speech-recognition-python-assemblyai-13d35eeed226) [](/speech-recognition-python-assemblyai-bb5024d322d8) [## 如何使用 Python 执行语音识别

### 用 Python 中的 AssemblyAI API 执行语音转文本

towardsdatascience.com](/speech-recognition-python-assemblyai-bb5024d322d8)