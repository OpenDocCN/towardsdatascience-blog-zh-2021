# 用 Python 构建你的语言过滤器

> 原文：<https://towardsdatascience.com/build-your-language-filter-with-python-d6502f9c224b?source=collection_archive---------14----------------------->

## 这是一个关于如何用 python 构建语言过滤器的指南，用于审查污言秽语和其他不合适的内容

![](img/5cb79541fdd8cf38fa7fd9896dd9e5df.png)

玛利亚·克里萨诺娃在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

语言过滤器是一个你可以去除不必要的脏话的过滤器。某种过滤对于维护网站或所提供服务的真实性是必要的。

这种必要的审查对于各种自然语言处理项目也是有帮助的。为了儿童使用的安全，你可以使用语言过滤，在聊天机器人中使用来阻止亵渎的词语，以及审查被认为对工作不安全的污言秽语。

> **注意:**在这篇文章中，我将删除任何被认为冒犯公众的词语。但是，当您在您的设备上尝试这种方法时，建议使用完整的单词来测试它们，并检查它是否如您所愿完美地工作。

在这篇文章中，我们将讨论两个这样的简单方法，我们可以利用有效的语言过滤。一种方法是使用预构建的 python 库模块，它可以有效地执行这项任务，另一种方法是构建自己的定制语言过滤。

有关如何使用深度学习从头开始构建聊天机器人的更多信息，请查看下面的文章。我们今天看到的脏话过滤器对这类应用也很有用，你可以选择过滤掉可能被你的聊天机器人认为是不适当的回复的文本。

[](/innovative-chatbot-using-1-dimensional-convolutional-layers-2cab4090b0fc) [## 使用一维卷积层的创新聊天机器人

### 从头开始使用深度学习和 Conv-1D 层构建聊天机器人

towardsdatascience.com](/innovative-chatbot-using-1-dimensional-convolutional-layers-2cab4090b0fc) ![](img/fe69460f3f2df5562fbdb172273ba3b5.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément H](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

在本文的这一部分中，我们将探索几种方法来实现过滤掉特定语言中任何不合适的俚语的过程。我们将讨论完成以下任务的两个过程。在第一种方法中，我们将利用亵渎过滤器库来完成项目。在第二种方法中，我们将构建自己的自定义代码来执行这样的操作。

# 1.脏话过滤方法:

其中一种方法是使用 python 中的脏话过滤器库模块。这是一个通用的检测和过滤脏话的库。包括对英语和俄语的支持。它有助于检测两种语言范围内的脏话，并可以相应地审查它们。

该模块的安装过程非常简单，可以使用以下 pip 命令成功完成:

```
pip install profanity-filter
```

但是，在执行您安装的以下库模块时，您可能会遇到一些小问题。在运行本节后面提供的代码时，您可能会遇到的错误之一如下。

```
**ProfanityFilterError**: Couldn't load Spacy model for any of languages: 'en'
```

以下错误表明您的 Spacy 模型中没有可用的英语。Spacy 是处理自然语言处理相关数据的最佳工具之一。它建立在高端研究的基础上，旨在支持复杂的任务。如果您还没有安装这个库，我强烈建议您用下面的命令安装它。

```
pip install spacy
```

现在你已经安装了 spacy 库，你可以同时在这个模块中安装英语，这样脏话过滤库就可以利用它来过滤不合适的内容。下面提供的以下命令应该可以成功地在您的设备上为 Spacy 型号安装英语。

```
python -m spacy download en
```

让我们看一些关于如何有效利用这个库的代码。一旦我们浏览了代码，我们将理解这个库的一些特性，然后在本文的下一节中继续开发我们自己的过滤系统。

## 代码:

使用脏话过滤器库的代码块非常简单。我们可以导入下面的库和空间模块。我们可以访问需要执行审查操作的特定语言，并将返回值存储在所需的变量中。这个变量现在可以用来有效地过滤掉任何不必要的俚语。下面的代码片段及其输出显示了一个完美的用例示例。

```
from profanity_filter import ProfanityFilter
import spacyspacy.load('en')pf = ProfanityFilter(languages = ['en'])
pf.censor("That's bullshit!")
```

## 输出:

```
"That's ********!"
```

亵渎过滤器支持该库中包含的多种语言。目前，两个最突出的语言支持是英语和俄语。本节的特点来自官方来源之后的[。让我们看看脏话过滤模块能够实现的一些惊人的功能。](https://pypi.org/project/profanity-filter/)

# 特点:

1.  全文或单个单词审查。
2.  多语言支持，包括在混合语言编写的文本亵渎过滤。
3.  深度分析。该库不仅使用 Levenshtein 自动机检测精确的亵渎单词匹配，还检测衍生和扭曲的亵渎单词，忽略字典单词，包含亵渎单词作为一部分。
4.  将库用作管道一部分的空间组件。
5.  决策说明(属性`original_profane_word`)。
6.  部分文字审查。
7.  扩展性支持。可以通过提供词典来增加新的语言。
8.  RESTful web 服务。

该模块的唯一限制是它是上下文无关的。该图书馆无法检测使用由体面的话组成的亵渎性短语。反之亦然，图书馆不能检测一个亵渎的词的适当用法。这意味着过滤器不能理解一个单词的真实语义。因此，有可能在数据库没有意识到的讽刺语句或俚语中出现失误。

# 2.如何在不使用该库的情况下构建自己的过滤器系统:

在本节中，我们将了解如何在这个自定义代码块中为特定语言(如英语)构建我们自己的自定义语言过滤器。这种执行的代码很简单，我们可以构建自己的函数来执行这种任务。

第一步是为你的特定项目列出你希望保留审查的所有单词。下面是一个例子代码块的一些话，我们可能希望被审查。用户也可以随意添加和探索任何其他类型的单词。

```
Banned_List = ["idiot", "stupid", "donkey"]
```

下一步，我们将定义一个包含三个禁用单词中的两个的随机句子。下面是我们将在变量中声明的句子。

```
sentence = "You are not only stupid , but also an idiot ."
```

最后，我们将定义我们的审查自定义函数，该函数将执行审查特定句子中不适当语言的操作。在这个函数中，我们将把想要的句子作为一个参数，并相应地计算下面的内容。我们将用空格将句子分开，这样我们就有了需要的单词。然后，我们将继续检查以下单词是否是禁用列表的一部分。如果是，那么我们将用星号代替这个词。如果没有，我们将继续按照最初的解释打印句子。下面是解释这个函数的代码片段。

```
def censor(sentence = ""):
    new_sentence = ""

    for word in sentence.split():
#         print(word)
        if word in Banned_List:
            new_sentence += '* '
        else:
            new_sentence += word + ' '

    return new_sentence
```

最后，让我们调用函数并查看下面句子的相应输出。

```
censor(sentence)
```

## 输出:

```
'You are not only * , but also an * . '
```

还有一些额外的改进，观众可以选择进行，例如为每个单词加入正确数量的星号空格，并确保阅读单词时标点符号不会干扰。这两项任务都很容易完成，我建议有兴趣的人试一试！

# 结论:

![](img/1e6e55feb44b968169f7b4d9f1c27bc5.png)

照片由[乔·凯恩](https://unsplash.com/@joeyc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在本文中，我们讨论了两种简单的方法，通过这两种方法，我们可以针对任何特定的任务有效地使用语言过滤方法。然而，这些过滤系统的一个主要问题是缺乏对所用语言的语义理解。因此，它无法找出什么合适的短语或特定的巧妙方法来绕过它的方法。

尽管如此，他们在从网站设计到使用深度学习方法创建的聊天机器人的广泛应用中，出色地完成了与大多数常见做法的对抗。您可以找到更好的方法来使用这些函数产生更有效的结果。

如果你想在我的文章发表后第一时间得到通知，请点击下面的[链接](https://bharath-k1297.medium.com/membership)订阅邮件推荐。如果你希望支持其他作者和我，请订阅下面的链接。

[](https://bharath-k1297.medium.com/membership) [## 通过我的推荐链接加入 Medium-bharat K

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

bharath-k1297.medium.com](https://bharath-k1297.medium.com/membership) 

看看我的其他一些文章，你可能会喜欢读！

[](/generating-qr-codes-with-python-in-less-than-10-lines-f6e398df6c8b) [## 用 Python 生成二维码，不到 10 行

### 构建一个简单的 Python 项目，用 QR 码加密您的信息

towardsdatascience.com](/generating-qr-codes-with-python-in-less-than-10-lines-f6e398df6c8b) [](/6-reasons-why-your-ai-and-data-science-projects-fail-2a1ecb77743b) [## 你的人工智能和数据科学项目失败的 6 个原因

### 分析导致你的人工智能和数据科学项目失败的 6 个主要原因

towardsdatascience.com](/6-reasons-why-your-ai-and-data-science-projects-fail-2a1ecb77743b) [](/how-to-write-code-effectively-in-python-105dc5f2d293) [## 如何有效地用 Python 写代码

### 分析使用 Python 编写代码时应该遵循的最佳实践

towardsdatascience.com](/how-to-write-code-effectively-in-python-105dc5f2d293) 

谢谢你们坚持到最后。我希望你们喜欢阅读这篇文章。我希望你们都有美好的一天！