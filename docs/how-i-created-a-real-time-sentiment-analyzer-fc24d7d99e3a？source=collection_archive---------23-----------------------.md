# 我如何创建一个实时情绪分析器

> 原文：<https://towardsdatascience.com/how-i-created-a-real-time-sentiment-analyzer-fc24d7d99e3a?source=collection_archive---------23----------------------->

## 实时预测你演讲的情绪

![](img/5de97858a63d04fb37b9a57f4279bd51.png)

图片由[穆罕默德·哈桑](https://pixabay.com/users/mohamed_hassan-5229782/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4141527)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4141527)

情感分析是一种自然语言处理技术，用于预测给定文本的情感或观点。它包括使用自然语言处理、文本分析、计算机语言学来识别或提取主观信息。情感分析广泛用于预测评论、评论、调查响应、社交媒体等的情感。

情感分析器模型可以预测给定文本是指积极的、消极的还是中性的情感。在本文中，我们将重点开发一个实时情感分析器，它可以使用 NLTK 和 TextBlob 等开源 Python 库来实时预测语音的情感。

# 想法:

在这个项目中，我们将开发一个可以听语音并将其转换为文本数据的模型，并使用 TextBlob 库实时分析文本的情感。

1.  使用语音识别器引擎识别语音并将其转换为文本格式。
2.  使用 TextBlob 实时预测文本的情感。
3.  重复步骤 1 和 2，直到对话结束。

# 语音识别器:

第一步是使用语音识别器引擎将语音转换成文本格式。我们将使用一个开源的 Python 库 SpeechRecognizer 来将演讲转换成文本。这是一个用于执行语音识别的库，支持在线和离线的多个引擎和 API。

## 安装:

```
**pip install SpeechRecognition**
```

# 使用 TextBlob 的情感分析器:

TextBlob 是一个基于 NLTK 的开源 Python 库，提供易于使用的 API，用于情感分析、翻译、词性标注、名词短语提取、分类等。它用于处理文本数据，并允许哪个算法使用其简单的 API。

对于情感分析，TextBlob 提供了两种实现算法:

*   **模式分析器:**(默认)情感分析器算法，使用与模式库相同的实现。它以命名元组的形式返回结果，格式如下:

```
**Sentiment(polarity, subjectivity, [assessments])**
```

*其中【评估】是被评估的表征及其极性和主观性得分的列表。*

*   **Naive Bayes analyzer:**Naive Bayes analyzer 是一个 NLTK 分类器，它在电影评论数据集上进行训练。以命名元组的形式返回结果，格式如下:

```
***Sentiment(classification, p_pos, p_neg)***
```

*param callable feature _ extractor:给定单词列表，返回特征字典的函数。*

# 实施:

*   导入必要的库(第 1–2 行)。
*   初始化语音识别器引擎(第 4 行)。
*   使用识别器引擎监听音频(第 10 行)，如果 2 秒内没有音频(timeout=2)，识别器将停止监听。
*   使用 TextBlob 函数`recognizer_google`(第 10 行)将音频输入转换成文本格式。
*   使用 TextBlob 情感分类器预测输入文本的情感(第 13 行)。
*   运行循环，直到你说出“退出”这个词，退出循环。

(作者代码)

# 结论:

在本文中，我们讨论了一个使用 TextBlob 的实时情感分析器模型。您还可以使用上面讨论的自定义函数实时实现该模型，该函数将听取您的讲话，并预测情绪。

在这个实验中，我们使用了来自 TextBlob 库的预先训练的情感分类器模型。还有一些其他的技术来开发一个情感分析模型，阅读下面的文章来获得这些方法实现的概述。

[](/5-ways-to-develop-a-sentiment-analyser-in-machine-learning-e8352872118) [## 在机器学习中开发情感分析器的 5 种方法

### 探索不同的方法来开发一个模型来预测给定文本的情感

towardsdatascience.com](/5-ways-to-develop-a-sentiment-analyser-in-machine-learning-e8352872118) 

> 感谢您的阅读