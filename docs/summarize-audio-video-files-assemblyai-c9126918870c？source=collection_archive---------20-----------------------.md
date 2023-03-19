# 如何用 Python 汇总音视频文件

> 原文：<https://towardsdatascience.com/summarize-audio-video-files-assemblyai-c9126918870c?source=collection_archive---------20----------------------->

## 了解如何使用 AssemblyAI API 的自动章节功能摘要音频文件

![](img/1dc90fc226f272a2fe6dd5dba85e6c63.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/audio-signal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

文本摘要是指在自然语言处理(NLP)环境中应用的一组技术，能够以保留关键信息的方式缩短原始文本转录。

在需要更容易、更快地使用大量数据的情况下，文本摘要非常有用。此外，这种应用可以应用于我们需要处理音频文件的环境中。这意味着第一步将是在文本摘要之前执行语音到文本的转换，然后使用该输出作为将执行摘要任务的服务的输入。

在今天的文章中，我们将探讨如何使用直观且易于使用的 API 来总结音频和视频文件。

## 用一个简单的 API 总结音频文件

我们将使用 AssemblyAI API Auto Chapters 特性执行[音频摘要](https://docs.assemblyai.com/guides/auto-chapters)，该特性为之前使用语音到文本 API 转录的音频文件提供**随时间的摘要**。

在本教程中，作为音频文件，我们将使用拜登在 2021 年 4 月 28 日向美国国会发表的演讲。

你需要做的第一件事(尤其是如果你打算按照这个教程做的话)是[从 AssemblyAI 网站(免费)获得你的 API 密匙](https://www.assemblyai.com/)。

导入请求库并定义请求头—来源:[作者](https://gmyrianthous.medium.com/)

现在，我们需要做的第二件事是将我们的音频文件上传到 AssemblyAI 的托管服务，这反过来会给我们一个链接，我们将使用它来进行后续的请求，以便执行实际的转录和总结。

将我们的音频文件上传到 AssemblyAI 的 API 托管服务，以便取回 URL——来源:[作者](https://gmyrianthous.medium.com/)

上面的调用返回上传 url，它实际上是我们上传的音频文件的宿主。现在我们已经完成了，我们可以继续获取音频文件的转录以及由 AssemblyAI API 算法生成的摘要章节。

使用 AssemblyAI API 执行语音转文本和摘要—来源:[作者](https://gmyrianthous.medium.com/)

在上面的调用中，请注意，我们必须将`auto_chapters`设置为`True`,以便指示 API 对转录的文本执行汇总。

## 解读回应

先前 API 调用的响应如下所示:

包含转录文本以及输入音频文件的提取章节(摘要)的输出—来源:[作者](https://gmyrianthous.medium.com/)

在返回的输出中，可以在`text`键中找到全文转录，而在`chapters`键中可以找到 AssemblyAI 生成的摘要章节。对于每一个提取的章节，响应还将包括开始和结束时间戳以及`summary`，后者实质上包括总结特定时间帧的音频的几个句子以及`headline`。

## 完整代码

完整的代码，用于本教程，以上传一个音频文件到汇编 AI API，执行语音到文本和总结可以在下面找到。

我们教程的完整代码—来源:[作者](https://gmyrianthous.medium.com/)

## 最后的想法

在今天的简短指南中，我们讨论了如何使用名为**自动章节**的 AssemblyAI API 特性对音频或视频文件执行摘要。作为本教程的一部分，我们只讨论了他们的 API 所提供的一小部分特性，所以如果你想看他们提供的完整列表，请确保查看他们的官方文档。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

## 你可能也喜欢

[](/real-time-speech-recognition-python-assemblyai-13d35eeed226) [## 如何用 Python 执行实时语音识别

### 使用 Python 中的 AssemblyAI API 执行实时语音转文本

towardsdatascience.com](/real-time-speech-recognition-python-assemblyai-13d35eeed226) [](/speech-recognition-python-assemblyai-bb5024d322d8) [## 如何使用 Python 执行语音识别

### 用 Python 中的 AssemblyAI API 执行语音转文本

towardsdatascience.com](/speech-recognition-python-assemblyai-bb5024d322d8)