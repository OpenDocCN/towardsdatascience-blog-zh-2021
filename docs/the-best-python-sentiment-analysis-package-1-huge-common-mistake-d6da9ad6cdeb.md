# 最好的 Python 情绪分析包(+1 个巨大的常见错误)

> 原文：<https://towardsdatascience.com/the-best-python-sentiment-analysis-package-1-huge-common-mistake-d6da9ad6cdeb?source=collection_archive---------3----------------------->

## [提示和技巧](https://towardsdatascience.com/tagged/tips-and-tricks)

## 如何在不训练自己模型的情况下获得近乎完美的性能

![](img/3b73cbbff9db8ada1331c358c182e474.png)

[奥比·奥尼耶多尔](https://unsplash.com/@thenewmalcolm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

情感分析是最常被研究的自然语言处理任务之一，在从公司管理他们的社交媒体形象到情感股票交易到产品评论的使用案例中都可以看到。

</building-a-budget-news-based-algorithmic-trader-well-then-you-need-hard-to-find-data-f7b4d6f3bb2>  

随着它越来越受欢迎，可用的软件包也越来越多，为了选出最好的，我把三个流行的软件包放在一起:NLTK (VADER)、TextBlob 和 Flair。

## **NLTK (VADER)和 TextBlob**

这两个软件包都依赖于基于规则的情感分析器。因此，它会对某些单词(例如恐怖有负面联想)，注意否定是否存在，根据这些词返回值。这种方法工作起来很好，并且具有简单和速度极快的优点，但是也有一些缺点。

*   随着句子变得越来越长，中性词就越来越多，因此，总得分也越来越趋于中性
*   讽刺和行话经常被误解

NLTK 可以使用以下代码找到文本的整个数据帧的极性得分(从-1 到 1)和情感:

```
from nltk.sentiment import SentimentIntensityAnalyzer
import operator
sia = SentimentIntensityAnalyzer()
df["sentiment_score"] = df["reviews.text"].apply(lambda x: sia.polarity_scores(x)["compound"])
df["sentiment"] = np.select([df["sentiment_score"] < 0, df["sentiment_score"] == 0, df["sentiment_score"] > 0],
                           ['neg', 'neu', 'pos'])
```

而 Textblob 可以以更简洁的方式完成同样的工作

```
from textblob import TextBlob
df["sentiment_score"] = df["reviews.text"].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
df["sentiment"] = np.select([df["sentiment_score"] < 0, df["sentiment_score"] == 0, df["sentiment_score"] > 0],
                           ['neg', 'neu', 'pos'])
```

## **天赋**

Flair 是一个预训练的基于嵌入的模型。这意味着每个单词都在一个向量空间中表示。具有与另一个单词最相似的矢量表示的单词经常在相同的上下文中使用。因此，这使得我们能够确定任何给定向量的情感，从而确定任何给定句子的情感。如果您对更多的技术方面感兴趣，这些嵌入是基于本文的。

Flair 往往比基于规则的模型慢得多，但它的优势在于它是一个经过训练的 NLP 模型，而不是基于规则的模型，如果做得好，它会带来额外的性能。为了客观地看待慢了多少，在运行 1200 个句子时，NLTK 花了 0.78 秒，Textblob 花了令人印象深刻的 0.55 秒，而 Flair 花了 49 秒(长 50-100 倍)，这就引出了一个问题，即增加的准确性是否真正值得增加的运行时间。

一个完整的数据帧可以这样训练

```
from flair.models import TextClassifier
from flair.data import Sentencesia = TextClassifier.load('en-sentiment')def flair_prediction(x):
    sentence = Sentence(x)
    sia.predict(sentence)
    score = sentence.labels[0]
    if "POSITIVE" in str(score):
        return "pos"
    elif "NEGATIVE" in str(score):
        return "neg"
    else:
        return "neu"df["sentiment"] = df["reviews.text"].apply(flair_prediction)
```

## 对他们进行测试

为了测试这些包，我将使用由[data inity](https://datafiniti.co/)提供的来自 Kaggle 的亚马逊评论的[大型数据库。该数据集包括 34，000 条从 1 到 5 星的评论，其中大多数是 5 星评论。为了准确评估，我会从每个星级二次抽样 300。为了确定我的准确性，我将只使用 300 个 1 星评论(假设它们都应该是负面的)和 300 个 5 星评论(假设它们都应该是正面的)](https://www.kaggle.com/datafiniti/consumer-reviews-of-amazon-products/version/5)

我将使用的性能指标是准确性(所有预测中的正确预测)、精确度(有多少正面预测是正确的)、特异性(衡量有多少负面预测是正确的)和 F1 值(精确度和召回率的调和平均值)

分数如下

```
NLTK (VADER)
Accuracy: 68.69712351945854
Precision: 62.38938053097345
Specificity: 42.17687074829932
F1: 75.30040053404538 TEXTBLOB
Accuracy: 65.97582037996546
Precision: 60.29411764705882
Specificity: 33.45070422535211
F1: 74.44876783398183FLAIR
Accuracy: 96.0
Precision: 95.69536423841059
Specificity: 95.66666666666667
F1: 96.01328903654486
```

结果不言自明——基于规则的模型很难确定负面评论何时是真正负面的。VADER 正确识别了 280/300 个五星评论，但只正确识别了 126/300 个负面评论(哎哟)，导致了可怕的 42%特异性分数。我认为这是因为负面评论中大量使用了强调、行话和讽刺。

另一方面，Flair 没有落入这个陷阱，正确地识别了 289/300 的正面评论，以及 287/300 的负面评论。它在 2 星评价中也有类似的良好表现，290/300 的评价是负面的。详细结果如下！

```
NLTK (VADER)
reviews.rating  sentiment
1.0             neg          124
                neu            6
                pos          170
5.0             neg           15
                neu            3
                pos          282TEXTBLOB
reviews.rating  sentiment
1.0             neg           95
                neu           16
                pos          189
5.0             neg            8
                neu            5
                pos          287FLAIR
reviews.rating  sentiment
1.0             neg          287
                pos           13
5.0             neg           11
                pos          289
```

这些结果意味着您只需使用 Flair 的几行代码就可以获得一流的性能！

**一个常见的陷阱**

大多数情感分析模型会建议拆分评论的每个句子，因为模型是在单个句子上训练的。我发现这既不准确又计算量大，我这样做的结果如下:

```
NLTK (VADER)
Accuracy: 67.66666666666666
Precision: 61.67400881057269
Specificity: 42.0
F1: 74.27055702917772
Runtime: 0.825s (vs 0.784s)TEXTBLOB
Accuracy: 65.86620926243569
Precision: 60.475161987041034
Specificity: 36.23693379790941
F1: 73.78129117259552
Runtime: 0.958s (vs 0.552s)FLAIR
Accuracy: 91.5
Precision: 89.52380952380953
Specificity: 89.0
F1: 91.70731707317074
Runtime: 121s (vs 49s)
```

请注意所有这些值是如何变低的，即使我控制了超过 100 字的评论，这一点仍然是正确的。显然，一定要将超过 200 个单词的文本分割开来，但是对于长达 1-2 段的文本，我发现将文本分割成句子是一种误导，不值得追求。

## 结论

对于仅使用预训练模型的出色性能，Flair 提供了可能与定制模型相匹配的性能。我还了解到，所有这些模型对文本长度的鲁棒性都比它们各自的文档可能让您相信的要高。如果你想要源代码，我把它上传到这里。我从 Neptune.ai 的[这个博客中学到了很多关于如何使用这些模型的知识。我希望这对你的情绪分析之旅有所帮助，如果你喜欢你所读的，请随意](https://neptune.ai/blog/sentiment-analysis-python-textblob-vs-vader-vs-flair)[跟随我](https://jerdibattista.medium.com/)，阅读我写的更多内容。我倾向于每个月深入一个主题一次(因为这些文章通常要花我 3 天以上的时间)！

</deep-learning-on-a-budget-450-egpu-vs-google-colab-494f9a2ff0db> 