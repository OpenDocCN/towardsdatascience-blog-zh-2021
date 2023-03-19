# Python 文本清理指南

> 原文：<https://towardsdatascience.com/a-guide-to-cleaning-text-in-python-943356ac86ca?source=collection_archive---------11----------------------->

## [自然语言处理笔记](https://towardsdatascience.com/tagged/nlpnotes)

## 为机器阅读准备自然语言

![](img/d5e220d443657e229e89bc04fd59cb65.png)

[摄](https://unsplash.com/@thecreative_exchange?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的创意交流

文本是非结构化数据的一种形式。根据维基百科，非结构化数据被描述为“没有预定义的数据模型或者没有以预定义的方式组织的信息。”【**来源** : [维基百科](https://en.wikipedia.org/wiki/Unstructured_data)。

不幸的是，计算机不像人类；机器不能像我们人类那样阅读原始文本。当我们处理文本数据时，我们不能从原始文本直接进入我们的机器学习模型。相反，我们必须遵循这样一个过程:首先清理文本，然后将其编码成机器可读的格式。

让我们来介绍一些清理文本的方法——在另一篇文章中，我将介绍一些对文本进行编码的方法。

## 案例规范化

当我们写作时，出于不同的原因，我们在句子/段落中使用不同的大写单词。例如，我们用大写字母开始一个新句子，或者如果某物是一个名词，我们将大写第一个字母来表示我们正在谈论一个地方/人，等等。

对于人类来说，我们可以阅读文本，并直观地判断出句子开头使用的“the”与后面句子中间出现的“the”是同一个单词，但是，计算机不能——机器会将“the”和“The”视为两个不同的单词。

因此，规范单词的大小写很重要，这样每个单词都是相同的大小写，计算机就不会把同一个单词当作两个不同的符号来处理。

```
*# Python Example*
text = "The UK lockdown restrictions will be dropped in the summer so we can go partying again!" *# lowercasing the text*
text = text.lower()
**print**(text)**>>>> the uk lockdown restrictions will be dropped in the summer so we can go partying again!** 
```

## 删除停用词

在大多数自然语言任务中，我们希望我们的机器学习模型能够识别文档中为文档提供价值的单词。例如，在情感分析任务中，我们想要找到使文本情感向一个方向或另一个方向倾斜的单词。

在英语中(我相信大多数语言也是如此，但不要引用我的话)，有些词比其他词使用得更频繁，但它们不一定会给句子增加更多价值，因此可以肯定地说，我们可以通过从文本中删除来忽略它们。

> **注意**:删除停用词并不总是最好的主意！

```
*# Importing the libraries* 
import nltk
from nltk.corpus import stopwords
nltk.download("stopwords")stop_words = set(stopwords.words("english"))
**print**(stop_words)**>>>> {'over', 'is', 'than', 'can', 'these', "isn't", 'so', 'my', 'each', 'an', 'between', 'through', 'up', 'where', 'hadn', 'very', "you'll", 'while', "weren't", 'too', 'doesn', 'only', 'needn', 'has', 'just', 'd', 'some', 'into', 've', 'didn', 'further', 'why', 'mightn', 'and', 'haven', 'own', "mightn't", 'during', 'both', 'me', 'shan', "doesn't", 'theirs', 'herself', 'the', 'few', 'our', 'its', 'yourself', 'under', 'at', "you've", 're', 'themselves', 'y', 'ma', 'because', 'him', 'above', 'such', 'we', "wouldn't", 'of', 'from', 'hers', 'nor', "shouldn't", 'a', 'hasn', 'them', 'myself', 'this', 'being', 'your', 'those', 'i', 'if', 'couldn', 'not', 'will', 'it', 'm', 'to', 'isn', 'aren', 'when', 'o', 'about', 'their', 'more', 'been', "needn't", 'had', 'll', 'most', 'against', 'once', 'how', "didn't", "shan't", 'there', 'all', "should've", 'he', "don't", 'she', 'which', 'below', 'on', 'no', 'yourselves', "wasn't", 'shouldn', 'by', 'be', 'have', 'does', "aren't", 'itself', 'same', 'should', 'in', 'before', 'am', "won't", 'having', "you'd", 'mustn', 'for', "that'll", 'that', "couldn't", 'wasn', 'won', "hasn't", 'as', 'until', 'wouldn', "mustn't", 'his', 'ain', "you're", 'out', "she's", 'other', 'are', 't', 'you', 'off', 'yours', 'ourselves', 'himself', 'down', "haven't", 'ours', 'now', "hadn't", 'do', 's', 'her', 'with', "it's", 'then', 'weren', 'any', 'after', 'whom', 'what', 'who', 'but', 'again', 'here', 'did', 'doing', 'were', 'they', 'was', 'or', 'don'}***# example text*
text = "The UK lockdown restrictions will be dropped in the summer so we can go partying again!"*# removing stopwords*
text = " ".join([word **for** word **in** text.split() **if** word **not in** stop_words])**print**(text)**>>>> uk lockdown restrictions dropped summer go partying again!**
```

## **删除 Unicode**

ASCII 将表情符号和其他非 ASCII 字符格式化为 Unicode。从本质上讲，Unicode 是一种通用的字符编码标准，所有语言中的每个字符和符号都被分配了一个代码。Unicode 是必需的，因为它是唯一允许我们使用各种不同语言检索或连接数据的编码标准，但问题是…它在 [ASCII](https://en.wikipedia.org/wiki/ASCII) 格式中不可读。

> **注**:来自 [Python 指南](https://pythonguides.com/remove-unicode-characters-in-python/#:~:text=In%20python%2C%20to%20remove%20Unicode,Unicode%20characters%20from%20the%20string.)的示例代码

```
*# creating a unicode string*
text_unicode = "Python is easy \u200c to learn" *# encoding the text to ASCII format*
text_encode = text_unicode.encode(**encoding**="ascii", **errors**="ignore")# decoding the text
text_decode = text_encode.decode()# cleaning the text to remove extra whitespace 
clean_text = " ".join([word **for** word **in** text_decode.split()])
**print**(clean_text)**>>>> Python is easy to learn.**
```

## **删除网址、标签、标点、提及等。**

根据我们正在处理的数据类型，我们可能会面临各种增加噪声的挑战。例如，如果我们正在处理来自 Twitter 的数据，找到各种标签和提及并不罕见——这是指包含 Twitter 行话中另一个用户用户名的推文。

如果这些特征对我们试图解决的问题没有价值，那么我们最好将它们从数据中删除。然而，由于在许多情况下我们不能依赖于一个定义的字符，我们可以利用名为 Regex 的模式匹配工具来帮助我们。

```
import re *# removing mentions* 
text = "You should get @BlockFiZac from @BlockFi to talk about bitcoin lending, stablecoins, institution adoption, and the future of crypto"text = re.sub("@\S+", "", text)
**print**(text)**>>>> You should get  from  to talk about bitcoin lending, stablecoins, institution adoption, and the future of crypto
------------------------------------------------------------------**
*# remove market tickers*
text = """#BITCOIN LOVES MARCH 13th A year ago the price of Bitcoin collapsed to $3,800 one of the lowest levels in the last 4 years. Today, exactly one year later it reaches the new all-time high of $60,000 Thank you Bitcoin for always making my birthday exciting"""text = re.sub("\$", "", text)
**print**(text)**>>>> #BITCOIN LOVES MARCH 13th A year ago the price of Bitcoin collapsed to  3,800 one of the lowest levels in the last 4 years. Today, exactly one year  later it reaches the new all-time high of 60,000 Thank you Bitcoin for  always making my birthday exciting**
**------------------------------------------------------------------**
*# remove urls*
text = "Did someone just say “Feature Engineering”? https://buff.ly/3rRzL0s"text = re.sub("https?:\/\/.*[\r\n]*", "", text)
**print**(text)**>>>> Did someone just say “Feature Engineering”?
------------------------------------------------------------------**
*# removing hashtags* 
text = """.#FreedomofExpression which includes #FreedomToProtest should be the cornerstone of any democracy. I’m looking forward to speaking in the 2 day debate on the #PoliceCrackdownBill & explaining why I will be voting against it."""text = re.sub("#", "", text)
**print**(text)**>>>> .FreedomofExpression which includes FreedomToProtest should be the  cornerstone of any democracy. I’m looking forward to speaking in the 2 day  debate on the PoliceCrackdownBill & explaining why I will be voting against it.
------------------------------------------------------------------**
*# remove punctuation* import stringtext = "Thank you! Not making sense. Just adding, lots of random punctuation."punct = set(string.punctuation) 
text = "".join([ch for ch in tweet if ch not in punct])**print**(text)**>>>> Thank you Not making sense Just adding lots of random punctuation**
```

## 词干化和词汇化

当我们在做 NLP 任务时，我们可能希望计算机能够理解“walked”、“walk”和“walking”只是同一个单词的不同时态，否则，它们会被不同地对待。词干化和词汇化都是用于在 NLP 中规范化文本的技术——为了进一步简化这个定义，我们简单地将一个单词简化为它的核心词根。

根据维基百科的定义:

*   **词干化(** [**链接**](https://en.wikipedia.org/wiki/Stemming) **) —** 在语言形态学和信息检索中，词干化是将屈折词还原为其词干、词基或词根形式——一般为书面词形的过程。
*   **词条释义化(** [**链接**](https://en.wikipedia.org/wiki/Lemmatisation)**)——**语言学中的词条释义化是将一个词的屈折形式组合在一起的过程，这样它们就可以作为一个单项来分析，通过该词的词条或词典形式来识别。

尽管定义非常相似，但是每种技术缩减单词的方式却非常不同(我可能会在另一篇文章中探讨)，这意味着这两种技术的结果并不一致。

```
import nltk
from nltk.stem.porter import PorterStemmer
from nltk.stem import WordNetLemmatizerwords = ["walk", "walking", "walked", "walks", "ran", "run", "running", "runs"]
**-----------------------------------------------------------------**
*# example of stemming*
stemmer = PorterStemmer()for word in words: 
    **print**(word + " ---> " + stemmer.stem(word))**>>>> walk ---> walk
     walking ---> walk
     walked ---> walk
     walks ---> walk
     ran ---> ran
     run ---> run
     running ---> run
     runs ---> run
------------------------------------------------------------------** *# example of lemmatization* lemmatizer = WordNetLemmatizer()for word in words:
    **print**(word + " ---> " + lemmatizer.lemmatize(word))**>>>> walk ---> walk 
     walking ---> walking 
     walked ---> walked 
     walks ---> walk 
     ran ---> ran 
     run ---> run 
     running ---> running 
     runs ---> run**
```

## 包裹

在本教程中，我们介绍了如何在 Python 中清理文本。

具体来说，我们涵盖了:

*   我们为什么清理文本
*   清理文本的不同方法

感谢您的阅读！在 LinkedIn 和 T2 Twitter 上与我保持联系，了解我关于数据科学、人工智能和自由职业的最新消息。