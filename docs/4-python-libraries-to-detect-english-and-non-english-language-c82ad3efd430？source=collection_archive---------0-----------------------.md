# 4 个 Python 库来检测英语和非英语语言

> 原文：<https://towardsdatascience.com/4-python-libraries-to-detect-english-and-non-english-language-c82ad3efd430?source=collection_archive---------0----------------------->

## 我们将讨论用于语言检测的 spacy-langdetect、Pycld2、TextBlob 和 Googletrans

![](img/48f4d9a0c5d0f55fb449d5b29a2fde9d.png)

照片由 [e](https://unsplash.com/@eyf?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/japanese-text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

您确定您的模型的输入文本数据是英文的吗？嗯，没有人能确定这一点，因为没有人会阅读大约 20k 条文本数据记录。

那么，**非英语文本会如何影响你的英语文本训练模型呢？**

挑选任何非英语文本，并将其作为输入传递给英语文本训练分类模型。你会发现这个类别是由模型分配给非英语文本的。

如果您的模型依赖于一种语言，那么文本数据中的其他语言应该被视为干扰。

但是为什么呢？

文本分类模型的工作是分类。而且，不管输入文本是不是英文，它都会完成它的工作。

我们能做些什么来避免这种情况？

你的模型不会停止对非英语文本的分类。因此，您必须检测非英语文本，并将其从训练数据和预测数据中移除。

这个过程属于数据清理部分。数据中的不一致将导致模型准确性的降低。有时，文本数据中存在的多种语言可能是您的模型行为异常的原因之一。

因此，在本文中，我们将讨论检测文本数据语言的不同 python 库。

让我们从空间库开始。

# 1.空间

您需要安装 spacy-langdetect 和 spacy python 库，下面的代码才能运行。

#1.下载最匹配的默认模型并创建快捷链接。

#2.将 LanguageDetector()函数和模型添加到 NLP 管道。

#3.将文本数据传递到管道中进行语言检测。

#4 将检测到的语言和精度存储在 detect_language 变量中。

**我们仅使用单一语言文本数据测试了上述库。当文本有多个语言句子时会发生什么？**

让我们找出答案。

预测的语言是 en，也就是英语。预测准确率也低。但是，这篇文章也有一个德语句子。这个图书馆没有预测到这一点。

原因？

该库返回检测到的较长句子的语言。

spaCy 是一个 python 库，用于不同的自然语言处理(NLP)任务。它正在 spaCy NLP 管道中部署 spacy_langdetect 库模型。

在这里了解的[空间。在这里了解](https://spacy.io/)[spacy _ lang detect](https://pypi.org/project/spacy-langdetect/)。

# 2.Pycld2

如果我们想知道所有句子的检测语言呢？

在这种情况下，使用下面的代码。您需要安装 pycld2 python 库才能运行下面的代码。

Pycld2 python 库是用于压缩语言 Detect 2 (CLD2)的 python 绑定。您可以探索 Pycld2 的不同功能。在这里了解一下[pyc LD 2](https://pypi.org/project/pycld2/)。

# 3.文本 Blob

假设您正在使用 TextBlob 执行 NLP 任务。那么你会使用 spaCy 还是 TextBlob 进行语言检测呢？我会使用 TextBlob，你也应该。

我们也喜欢选择。

您需要安装 textblob python 库才能运行以下代码。

TextBlob 还返回检测到的较长句子的语言。对多语言句子使用 pycld2 库。

***你知道“汝”代表哪种语言吗？***

如果你没有，那就访问这个 [*维基百科链接*](https://meta.wikimedia.org/wiki/Template:List_of_language_names_ordered_by_code) *。或者你可以查看下面的列表。它是数据科学中使用的语言的标准缩写形式(ISO 639-1 代码)。*

TextBlob 提供了一个 API 来执行不同的 NLP 任务。TextBlob 的一些应用是文本处理、情感分析、分类、拼写校正、关键词提取、词性标注等。在这里了解[text blob](https://textblob.readthedocs.io/en/dev/)。

# 4.Googletrans

Googletrans python 库使用 google translate API 检测文本数据的语言。但是这个库不靠谱。所以，在使用这个库之前要小心。您可以将这个库视为语言检测的另一个选择。

您需要安装 googletrans python 库才能运行以下代码。

Googletrans 也有语言翻译的功能。在这里了解一下[Google trans](https://pypi.org/project/googletrans/)。

# 语言检测的应用

1.  根据语言找出文本数据中的偏差。
2.  你可以根据不同的语言对文章进行分类。
3.  语言通常与地区相关联。这种方法可以帮助你根据语言对文章进行分类。
4.  您可以在语言翻译模型中使用这种方法。
5.  您可以在数据清理和数据操作过程中使用它。

# 结论

对于文本数据，我们应该将语言检测视为数据清理过程之一。互联网文本数据并不总是以英语呈现。

在这篇文章中，我解释了不同的 python 库来检测语言。

这些库将帮助您消除数据中的噪声。最好知道你的数据是无噪声的。如果您的模型在新数据上表现不佳，您就消除了一个原因。

希望这篇文章能帮助你减少数据中的噪音。