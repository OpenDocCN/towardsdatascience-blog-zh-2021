# 你害怕吗，VADER？理解自然语言处理预处理如何影响 VADER 评分

> 原文：<https://towardsdatascience.com/are-you-scared-vader-understanding-how-nlp-pre-processing-impacts-vader-scoring-4f4edadbc91d?source=collection_archive---------20----------------------->

## 为什么常见的预处理活动实际上会损害 VADER 的功能，为什么仔细考虑 NLP 管道很重要

![](img/b10c529e3930614da32e56ee551560d8.png)

汤米·范·凯塞尔的照片🤙 on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

VADER ( [Valence Aware 字典和情感推理机](https://www.researchgate.net/publication/275828927_VADER_A_Parsimonious_Rule-based_Model_for_Sentiment_Analysis_of_Social_Media_Text))是由 CJ·休顿和 Eric Gilbert 开发的一个流行的情感分析工具，在研究[和现场应用中有许多用途。VADER 是一个基于规则的模型，该方法可以描述为构建一个“黄金标准”词汇特征列表，以及有用的“强度”度量。](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7968413/)

以产品评论、推文或描述等输入为例，VADER 提供了一个从-1 到 1 的“标准化加权综合得分”，其中-1 表示内容非常负面，1 表示内容非常正面。使用该工具，还可以获得输入的负/正/中性分数，这也可以为分析提供有用的背景。

在执行自然语言处理任务时，通常要执行一些预处理工作来清理我们的数据集。这些方法可以包括:

*   小写字符串
*   删除标点符号
*   删除停用词
*   字符串的首字母化
*   词干字符串

在本文中，我们将研究为什么这些常见的预处理活动实际上会损害 VADER 的能力，以及为什么仔细考虑您的 NLP 管道很重要，因为它可能依赖于您实现的模型的*类型*。

## 维德的情感强度试探法

休顿和吉伯特开发了五种关于情感强度的强有力的通用试探法。这些是:

1.  标点符号:这可以增加“强度的大小”，而不修改潜在的情绪。
2.  资本化:这也放大了情绪，影响了文本的潜在情绪。
3.  程度修饰语:这些可以减少/增加强度，比如说“尤达是一个非常好的老师”，而不是“尤达是一个好老师”
4.  “但是”:使用“但是”可以表示极性的转变，例如“克隆人战争很好，但是 RoTJ 更好”
5.  分析词汇特征之前的三元组:这使得 VADER 能够识别否定翻转文本极性的变化。

分析这些启发，我们可以开始看到常见的 NLP 清理任务实际上会导致 VADER 失去一些分析能力。为了把它放在上下文中，我们将看看最常见的以前确定的 NLP 活动；小写字符串，删除标点符号，删除停用词，词汇化和词干化字符串，看看这些活动如何改变 VADER 的输出。

## 数据集和方法

对于这个例子，我创造了几个简单的句子，真正展示了 VADER 的力量——回顾它们，你可以看到我们使用了大写和标点符号，并用“但是”改变了极性。

```
sentence_examples = ['I hate working at the mall!',
 'One day I thought today would be a good day, but it was NOT',
 'Spaghetti Bolognaise is my favourite food :)',
 'Yummy food tastes good but pizza tastes SO BAD!!!'
 ]
```

## 香草——无预处理

没有任何预处理，以下是 VADER 给这些例子的评分:

```
I hate working at the mall! — — — — — — — — — — — — — — — — — — — {‘neg’: 0.444, ‘neu’: 0.556, ‘pos’: 0.0, ‘compound’: -0.6114}One day I thought today would be a good day, but it was NOT — — — {‘neg’: 0.0, ‘neu’: 0.87, ‘pos’: 0.13, ‘compound’: 0.2382}Spaghetti Bolognaise is my favourite food :) — — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 0.667, ‘pos’: 0.333, ‘compound’: 0.4588}Yummy food tastes good but pizza tastes SO BAD!!! — — — — — — — — {‘neg’: 0.493, ‘neu’: 0.3, ‘pos’: 0.207, ‘compound’: -0.8661}
```

## 小写内容

在我们的第一个例子中，我们将简单地小写我们的内容，并检查 VADER 如何重新排序句子:

```
i hate working at the mall! — — — — — — — — — — — — — — — — — — — {‘neg’: 0.444, ‘neu’: 0.556, ‘pos’: 0.0, ‘compound’: -0.6114}one day i thought today would be a good day, but it was not — — — {‘neg’: 0.0, ‘neu’: 0.87, ‘pos’: 0.13, ‘compound’: 0.2382}spaghetti bolognaise is my favourite food :) — — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 0.667, ‘pos’: 0.333, ‘compound’: 0.4588}yummy food tastes good but pizza tastes so bad!!! — — — — — — — — {‘neg’: 0.412, ‘neu’: 0.348, ‘pos’: 0.24, ‘compound’: -0.7152}
```

在这种情况下，我们可以看到最后一句的否定性有所下降，这表明通过小写，我们已经失去了 pos/neg/neut 和复合得分的一些细节。

## 删除标点符号

接下来，我们去掉标点符号。

```
I hate working at the mall — — — — — — — — — — — — — — — — — — — — {‘neg’: 0.425, ‘neu’: 0.575, ‘pos’: 0.0, ‘compound’: -0.5719}One day I thought today would be a good day but it was NOT — — — — {‘neg’: 0.0, ‘neu’: 0.87, ‘pos’: 0.13, ‘compound’: 0.2382}Spaghetti Bolognaise is my favourite food — — — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 1.0, ‘pos’: 0.0, ‘compound’: 0.0}Yummy food tastes good but pizza tastes SO BAD — — — — — — — — — — {‘neg’: 0.47, ‘neu’: 0.314, ‘pos’: 0.217, ‘compound’: -0.8332}
```

在这种情况下，我们可以看到，在第一个和最后一个例子中，我们已经失去了负面得分，并且“意大利肉酱面是我最喜欢的食物”已经变成了中性。同样，由于执行这些预处理任务，我们丢失了数据中的细节。

## 删除停用词

对于这个例子，我使用了 NLTK 的英语停用词词典。

```
hate working mall ! — — — — — — — — — — — — — — — — — — — — — — — {‘neg’: 0.571, ‘neu’: 0.429, ‘pos’: 0.0, ‘compound’: -0.6114}One day thought today would good day , — — — — — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 0.707, ‘pos’: 0.293, ‘compound’: 0.4404}Spaghetti Bolognaise favourite food : ) — — — — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 1.0, ‘pos’: 0.0, ‘compound’: 0.0}Yummy food tastes good pizza tastes BAD ! ! ! — — — — — — — — — — {‘neg’: 0.23, ‘neu’: 0.38, ‘pos’: 0.39, ‘compound’: 0.4484}
```

在这里，我们开始看到一些得分的戏剧性变化。我们对披萨的最终评价从负面变成了 0.4484 的综合得分。

## 字符串的首字母化

词汇化和词干化允许我们[获得单词](https://www.datacamp.com/community/tutorials/stemming-lemmatization-python)的词根形式。与词干化相比，词汇化将单词简化为有效的词根形式。在这里，我们可以看到我们的 lemmatized 字符串如何导致不同的评分，以我们的香草的例子，其中所有的功能都被保留:

```
I hate working at the mall ! — — — — — — — — — — — — — — — — — — — {‘neg’: 0.4, ‘neu’: 0.6, ‘pos’: 0.0, ‘compound’: -0.6114}One day I thought today would be a good day , but it wa NOT — — — {‘neg’: 0.0, ‘neu’: 0.878, ‘pos’: 0.122, ‘compound’: 0.2382}Spaghetti Bolognaise is my favourite food : ) — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 1.0, ‘pos’: 0.0, ‘compound’: 0.0}Yummy food taste good but pizza taste SO BAD ! ! ! — — — — — — — — {‘neg’: 0.429, ‘neu’: 0.391, ‘pos’: 0.18, ‘compound’: -0.8661}
```

## 词干字符串

最后，这是我们对字符串进行词干处理的输出:

```
i hate work at the mall ! — — — — — — — — — — — — — — — — — — — — {‘neg’: 0.4, ‘neu’: 0.6, ‘pos’: 0.0, ‘compound’: -0.6114}one day i thought today would be a good day , but it wa not — — — {‘neg’: 0.0, ‘neu’: 0.878, ‘pos’: 0.122, ‘compound’: 0.2382}spaghetti bolognais is my favourit food : ) — — — — — — — — — — — {‘neg’: 0.0, ‘neu’: 1.0, ‘pos’: 0.0, ‘compound’: 0.0}yummi food tast good but pizza tast so bad ! ! ! — — — — — — — — — {‘neg’: 0.373, ‘neu’: 0.525, ‘pos’: 0.102, ‘compound’: -0.7999}
```

同样，与我们没有预处理文本的普通输出相比，我们可以发现显著的波动。

## 学习和建议

希望这篇文章能够说明为什么在探索 NLP 应用程序时仔细考虑您使用的算法或库是至关重要的。我们可以看到，常见的预处理任务，如小写、删除标点符号和单词规范化，实际上可以极大地改变 VADER 等模型的输出。

## 有用的链接

[http://www . iaeng . org/publication/imecs 2019/imecs 2019 _ pp12-16 . pdf](http://www.iaeng.org/publication/IMECS2019/IMECS2019_pp12-16.pdf)

【https://pypistats.org/packages/vadersentiment 

[https://github . com/cjhutto/vaderment](https://github.com/cjhutto/vaderSentiment)