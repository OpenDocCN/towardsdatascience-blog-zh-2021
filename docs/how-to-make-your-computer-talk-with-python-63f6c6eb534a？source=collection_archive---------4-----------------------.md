# 如何让你的电脑与 Python 对话

> 原文：<https://towardsdatascience.com/how-to-make-your-computer-talk-with-python-63f6c6eb534a?source=collection_archive---------4----------------------->

## 你成为亿万富翁花花公子的第一步

![](img/8227d89592769eba5d3653bf1f7142d3.png)

你的机器人管家。图片由[史蒂芬·米勒](https://www.flickr.com/photos/aloha75/10046436574)

如果你是《钢铁侠》等电影的粉丝，你可能会幻想拥有自己的贾维斯。在这篇文章中，我将向你展示如何开始制作你自己的电脑助手。我们将通过一点编程和一些智能 python 包在引擎盖下做数据科学来做到这一点。

现在，制作像超级智能人工智能这样复杂的东西是很难的，对我来说太难了，以至于我无法在一篇帖子中完成，甚至很可能是在一般情况下。然而，我们能做的是把这个问题分解成更小的部分，使问题看起来更简单。这是你应该在你的每一个项目中做的事情，这样才不会一下子陷入所有复杂的事情中。

从我对这个主题的简短思考中，我相信我们可以将一个超级智能的人工智能助手分成四个主要部分:

1.  文本到语音转换(获得响应)
2.  语音转文字(要东西)
3.  一些计算来理解我们的问题并做出回应
4.  将反应转化为现实世界中的行动

在今天的帖子中，我将重点介绍我们的计算机助手的文本到语音方面，它允许我们的计算机与我们交谈。如果一切顺利，我将在未来继续撰写这篇文章，让我们的助手变得更加复杂和有用。

# 查找文本到语音库

现在，用 python 做这样的事情的一个巨大的好处是，我们有大量的库可以用来快速完成工作。毕竟，如果我们从头开始开发每一点，我们会在这里呆很长时间，以至于我们永远也做不了任何事情。让我们站在巨人的肩膀上，使用 python 包。

对于文本到语音转换，有几个 python 包脱颖而出:

*   谷歌文本到语音(gTTs)，以及
*   pyttsx3(我不知道那到底代表什么)。

Google Text To Speech 是一个 Python 库，用于与 Google Translate 的 text to speech API 接口。它具有谷歌自己的文本到语音应用程序的性能，但需要使用互联网连接。

[pyttsx3](https://github.com/nateshmbhat/pyttsx3) ，另一方面，是一个文本到语音转换库，它寻找预装在你的平台上的文本到语音引擎并使用它们。因此，它离线工作。

以下是它在主要操作系统上使用的文本到语音转换引擎:

1.  Windows 上的 [SAPI5](https://en.wikipedia.org/wiki/Microsoft_Speech_API)

2.MacOSX 上的 NSSpeechSynthesizer

3.在每隔一个平台上讲话

看着这两者，我不希望我的助手依赖于谷歌或在线连接来工作。我更愿意使用 pyttsx3，让所有东西都在我自己的机器上运行。

# 设置项目

现在，在我们开始运行一切之前，让我们设置我们的项目。

我们将通过文本编辑器和终端做所有的事情。如果你不知道这是什么意思，那么我向入门者推荐的文本编辑器是 [vscode](https://code.visualstudio.com/) ，终端通常内置在你的文本编辑器中(就像在 vscode 中一样),或者是你计算机上的一个叫做“终端”或“cmd”的程序。

现在，我希望您打开终端，将目录更改为保存项目的位置，例如

```
cd ~/projects
```

接下来，我们需要创建一个目录来存储我们的项目。这完全由你决定，但我希望我的助手叫罗伯特。因此，我正在创建一个名为“robert”的新目录，然后用

```
mkdir robertcd robert
```

你可以把名字改成你喜欢的任何名字，比如 Brandy 或者 Kumar 什么的。

接下来，我们需要让 python 开始运行。为此，我们需要安装 python 3。如果您没有安装，请参见[https://www.python.org/](https://www.python.org/)获取安装说明。我们还需要创建一个 python 虚拟环境。如果你想了解更多，请看这里的[我最近的一篇文章](/getting-started-with-python-virtual-environments-252a6bd2240)。

假设您安装了 python，您可以在终端中用

```
python3 --version
```

现在，您应该能够使用以下命令在 robert 目录中创建 python 虚拟环境:

```
python3 -m venv venv
```

*注意，如果你安装的 python 版本叫* `python` *、* `python3.7` *或者* `python3.9` *或者别的什么，那就用那个*

然后，您应该能够通过以下方式激活您的虚拟环境:

(在 MacOS 和 Linux 上)

```
source venv/bin/activate
```

或(Windows)

```
venv\Scripts\activate
```

我们现在需要安装我们需要的软件包。为此，我们将创建一个 requirements.txt 文件。打开你最喜欢的文本编辑器，例如 vscode，或者，如果你喜欢冒险，打开你的“robert”文件夹，现在就创建这个文件。

对于我们的项目，到目前为止，我们只需要 pyttsx3。简单。现在让我们将它添加到 requirements.txt 文件中，如下所示

接下来，让我们使用 pip 安装我们的需求

```
pip install -r requirements.txt
```

# 使用 pyttsx3

现在一切都安装好了，让我们开始使用 pyttsx3。为了了解该怎么做，我查阅了这里的文件。

然后，您可以通过创建一个名为`speech.py`的文件并添加以下代码来制作一个很好的示例:

我们首先导入 pyttsx3 来加载它的所有类和变量。然后我们初始化语音引擎，设置我们想要的声音，然后是我们想要说的文字。我们终于用`engine.runAndWait()`说话了。

然后，我们可以在终端中使用以下命令运行该文件:

```
python speech.py
```

摆弄这个，改变`text_to_say`变量。你应该想说什么就说什么。

# 很酷的调整

现在我们有了一些工作，让我们给我们的助手一些调整。Pyttsx3 可以让我们调整声音和速度。

在上面的例子中，您可以将 voice_num 更改为一个不同的数字来获得一个新的语音。根据我的测试，这似乎是平台相关的(可能取决于你的平台是否有 [SAPI5](https://en.wikipedia.org/wiki/Microsoft_Speech_API) 、 [NSSpeechSynthesizer](https://developer.apple.com/documentation/appkit/nsspeechsynthesizer) 或 [espeak](http://espeak.sourceforge.net/) )。

我创建了这个巨大的文件(当很多机器人用奇怪的口音跟你说话时，你会明白为什么)来帮助你决定什么声音最适合你。一旦你找到你喜欢的声音号码，就用在`voice_num`变量中找到的号码替换掉。

# 后续步骤

祝贺你到达终点。如果你有任何问题或者只是想打个招呼，请在下面发表。

如果你想进一步阅读，并对即将发布的帖子有所了解，我建议你查看下面的链接。

*   [https://www . geeks forgeeks . org/python-text-to-speech-by-using-pyttsx 3/](https://www.geeksforgeeks.org/python-text-to-speech-by-using-pyttsx3/)
*   [https://realpython.com/python-speech-recognition/](https://realpython.com/python-speech-recognition/)

在我的下一篇文章中，我将关注语音转文本，这样我们的助手就可以响应我们的命令了🤖。给我一个关注，以确保你不会错过它。