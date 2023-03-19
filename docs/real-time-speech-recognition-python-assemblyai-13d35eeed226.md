# 如何用 Python 执行实时语音识别

> 原文：<https://towardsdatascience.com/real-time-speech-recognition-python-assemblyai-13d35eeed226?source=collection_archive---------7----------------------->

## 使用 Python 中的 AssemblyAI API 执行实时语音转文本

![](img/6da1351315777a3cf64ba7bd0bef1901.png)

[杰森·罗斯韦尔](https://unsplash.com/@jasonrosewell?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/microphone?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 介绍

在我最近的一篇文章中，我们探讨了如何用 AssemblyAI API 和 Python 执行离线语音识别。换句话说，我们将所需的音频文件上传到托管服务，然后使用 API 的转录端点来执行语音到文本的转换。

[](/speech-recognition-python-assemblyai-bb5024d322d8) [## 如何使用 Python 执行语音识别

### 用 Python 中的 AssemblyAI API 执行语音转文本

towardsdatascience.com](/speech-recognition-python-assemblyai-bb5024d322d8) 

在今天的指南中，我们将展示如何使用 AssemblyAI API 的[实时转录](https://www.assemblyai.com/features)功能来执行实时语音到文本转换，该功能让我们能够以高精度实时转录音频流。

我们开始吧！

## 安装 PyAudio 和 Websockets

为了能够建立实时语音识别，我们需要一个工具，让我们记录音频。PyAudio 是一个 Python 库，为跨平台音频 I/O 库 PortAudio 提供绑定。使用这个库，我们可以在几乎任何平台上实时播放或录制音频，包括 OSX、Linux 和 MS Windows。

首先我们需要安装`portaudio`。在 OSX，你可以通过自制软件来实现:

```
brew install portaudio
```

然后从 PyPI 安装 PyAudio:

```
pip install pyaudio
```

如果你在 Windows 上，你可以通过一个基于你的 Python 版本的 wheel 文件来安装 PyAudio，你可以在这里找到这个文件。

此外，我们需要安装 websockets

```
pip install websockets
```

如果你想跟随这个教程，你所需要的就是一个 API 密匙，如果你注册了一个 AssemblyAI 账户，你就可以得到它。一旦你这样做了，你的密钥应该在你的*账户*部分可见。此外，您需要升级您的帐户(转到“计费”进行升级)才能使用高级功能。

## 使用汇编 API 的实时语音转文本

AssemblyAI 提供了一个 [**语音转文本 API**](https://www.assemblyai.com/) ，它是使用先进的人工智能方法构建的，有助于视频和音频文件的转录。在今天的指南中，我们将使用这个 API 来实时执行**语音识别！**

现在，我们需要做的第一件事是通过指定一些参数，如每个缓冲区的帧数、采样率、格式和通道数，使用 PyAudio 打开一个流。我们的流将如下面的代码片段所示:

用 PyAudio 打开音频流—来源:[作者](https://gmyrianthous.medium.com/)

上面的代码片段将打开一个音频流，从我们的麦克风接收输入。

现在我们已经打开了流，我们需要使用 web 套接字将它实时传递给 AssemblyAI，以便执行语音识别。为此，我们需要定义一个异步函数来打开一个 websocket，这样我们就可以同时发送和接收数据。因此，我们需要定义两个内部异步函数——一个用于读入数据块，另一个用于接收数据块。

下面是通过 websocket 连接发送消息的第一种方法:

通过 websocket 连接发送数据—来源:[作者](https://gmyrianthous.medium.com/)

下面是通过 websocket 连接接收数据的第二种方法:

通过 websocket 连接接收数据—来源:[作者](https://gmyrianthous.medium.com/)

请注意，在上面的方法中，我们不处理任何特定的异常，但是您可能希望根据您的特定用例的要求适当地处理不同的异常和错误代码。有关错误条件的更多详细信息，您可以参考官方汇编文件[中定义关闭和状态代码](https://docs.assemblyai.com/overview/real-time-transcription)的相关章节。

现在，使用上述两个函数的主要异步函数定义如下。

通过 websocket 连接发送和接收数据—来源:[作者](https://gmyrianthous.medium.com/)

## 完整代码

下面的要点包含了我们将要使用的完整代码，以便使用 AssemblyAI 的 API 执行实时语音到文本转换。

使用 AssemblyAI API 执行语音到文本转换的完整代码—来源:[作者](https://gmyrianthous.medium.com/)

## 示范

现在我们有了一个完整的代码，它能够打开一个音频流，从我们的麦克风发送输入，并从 AssemblyAI API 异步接收响应。

为了运行代码，我们需要做的就是将我们的异步函数传递给`asyncio.run()`:

```
asyncio.run(speech_to_text())
```

现在，您应该能够通过麦克风说话，并转录流音频。

出于本教程的目的，我已经上传了从我的计算机上流式传输的音频，以便实时执行语音到文本转换。

使用上述音频流运行我们刚刚编写的程序后的输出如下所示:

示例输出—来源:[作者](https://gmyrianthous.medium.com/)

## 最后的想法

在今天的文章中，我们探讨了如何通过打开音频流和 websockets 来实时执行语音识别，以便与 AssemblyAI API 进行交互。注意，我们只涉及了 AssemblyAI API 提供的全部特性的一小部分。请务必点击查看他们的完整列表[。](https://www.assemblyai.com/features)

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership)