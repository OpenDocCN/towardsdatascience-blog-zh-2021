# 两个情感分析库及其性能

> 原文：<https://towardsdatascience.com/two-sentiment-analysis-libraries-and-how-they-perform-3de4a06342ec?source=collection_archive---------31----------------------->

## 使用文本块和 VADER 库进行情感分析很容易，但是它们有多准确呢

![](img/1468f15e48e75c852e73f5e856e71966.png)

[腾雅特](https://unsplash.com/@tengyart?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

情感分析不是一门精确的科学。自然语言中充满了奇怪的表达方式，意思不止一个的单词，习语，讽刺以及任何使意思提取变得困难的事物。看看这两句话。

“我知道我为什么一直犯同样的错误——我不认为！”

“情绪分析在所有情况下都是准确的——我不这么认为！”

在第一句中，短语“我不认为”解释了句子的第一部分，但在第二句中，它是讽刺性的，否定了句子前一部分的意思。于是，我们有了两个相同的短语，根据它们的使用方式，它们有着完全不同的目的。

因此，期望情绪分析器在所有情况下都准确是不合理的，因为句子的意思可能是模糊的。

但是它们有多准确呢？显然，这取决于用来执行分析的技术，也取决于上下文。为了找到答案，我们将使用两个易于使用的库做一个简单的实验，看看我们是否能找到我们期望的精度。

您可以决定构建自己的分析器，这样做的话，您可能会学到更多关于情感分析和文本分析的知识。如果你想做这样的事情，我强烈推荐你阅读由[康纳·奥沙利文](https://medium.com/u/4ae48256fb37?source=post_page-----3de4a06342ec--------------------------------)，[撰写的文章《情感分析简介](https://conorosullyds.medium.com/)，他不仅解释了情感分析的目的，还演示了如何使用单词袋方法和称为支持向量机(SVN)的机器学习技术在 Python 中构建分析器。

另一方面，你可能更喜欢导入一个库，比如 TextBlob 或者 VADER 来完成这项工作。

在下面的节目中，我们将分析来自[sentitment 140](http://help.sentiment140.com)项目的一组推文。他们已经收集了 160 万条推文，这些推文被贴上了积极、消极或中性情绪的标签。

推文真多。

但是我们将使用这些推文中的一个子集——只有 500 条。你可以在这里下载数据[sensition 140——给学者](http://help.sentiment140.com/for-students)。在这个网站上，你会发现一个 zip 文件，其中包含两个 CSV 文件，一个有 160 万条推文，另一个我们将使用 500 条推文—*testdata . manual . 2009 . 06 . 14 . CSV*。

我们将对该文件运行 TextBlob 和 VADER 情感分析器，并查看结果与数据集中之前分配的标签相比如何。(有关分析器的更多信息，请参见我的文章[推文的情绪分析](/sentiment-analysis-of-tweets-167d040f0583)。)

我将一次检查几行代码，但在本文的最后会包括整个代码的要点。

首先，我们需要导入库。

```
from textblob import TextBlob
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
import pandas as pdanalyzer = SentimentIntensityAnalyzer()
```

那是文本博客，VADER 和熊猫——不，是惊喜，在那里——然后我们创建了一个 VADER 分析器，我们以后会用到。

接下来，我们读取数据并做一些整理。

```
completeDF = pd.read_csv("testdata.manual.2009.06.14.csv", names=["polarity", "id","date","query","user","text"])df = completeDF.drop(columns=['date','query','user'])
df.polarity = df.polarity.replace({0: -1, 2: 0, 4: 1})
```

当读取 csv 文件时，我们给每一列一个名称。然而，我们并不真的需要所有这些列，所以我们立即创建一个新的数据帧，删除不需要的列。

原始数据将情绪分为“积极”、“中性”和“消极”，并分别用数字 4、2 和 0 表示这些情绪。这些值在“极性”栏中。我们将要使用的分析器使用相同的分类，但是使用 1 到-1 之间的实数来表示情绪的极性。所以-1 是很负，+1 是很正，0 是中性。0.5 可能有些积极。

因此，我们将“极性”列的值更改为 1 表示正极，0 表示中性，1 表示负极。(这给我们带来了一个比较一系列数字的小问题，这些数字是由分析器生成的，与数据中记录的三个类别相比较——但是我们很快就会处理这个问题。)

接下来要做的是遍历数据中的推文，并通过两个分析器运行它们。我们将在 list 中记录结果，因此先定义两个空的。

真正的工作是由

```
TextBlob(text).sentiment.polarity)
```

获取 TextBlob 分析和

```
analyzer.polarity_scores(text)['compound'])
```

从 VADER 那里得到分析结果。

函数`rounder`将完成从实数到类别的转换——稍后会详细介绍。

```
TBpol = []
Vpol = []for text in df['text']:
    TBpol.append(rounder(TextBlob(text).sentiment.polarity))
    Vpol.append(rounder(analyzer.polarity_scores(text)['compound']))
```

现在，我们将这两个列表作为新列添加到我们的数据框架中。

```
df['TBPolarity'] = TBpol 
df['VPolarity'] = Vpol
```

但是我们如何将一个 0.5 的极性转换成一个类别呢？应该是正面的还是中性的。你可以采取一种简单的方法，简单地对数字进行四舍五入，这样 0.5 以上的数字就变成 1，-0.5 以下的就变成-1，其他的都是 0。但这确实有效，因为这意味着中性范围是其他两个范围的两倍。

更合理的方法是向上舍入到 1.3 以上，从-1.3 向下舍入。这意味着正值的范围。阴性和中性是一样的。但我并不是出于非常实际的目的才这么做的——效果并不太好。

让我解释一下。

如果在转换成类别时，我使范围相等，那么我得到的结果是:

```
Overall length 498 
VADER agreements/disagreements 302/196
Accuracy: 60.6425702811245% 
TextBlob agreements/disagreements 217/281
Accuracy: 43.57429718875502%
```

第一行是推文的数量，接下来的几行记录了分析师对预先分配的类别的认同程度，然后是准确率。VADER 的准确率约为 61%，TextBlob 的准确率约为 44%。不聪明。

所以，我摆弄了一下舍入函数，看看能否提高精度。把 0 以上的都当成正的，0 以下的都当成负的，我得到了以下结果。

```
Overall length 498 
VADER agreements/disagreements 360/138
Accuracy: 72.28915662650603%
TextBlob agreements/disagreements 324/174
Accuracy: 65.06024096385542%
```

这是相当好的。所以`rounder`函数是这样的:

```
def rounder(num):
    if num > 0: return 1
    if num < 0: return -1
    return 0
```

给出了 72%和 65%的准确率。

这些数字合理吗？这么简单的测试很难说。也许一个更大的数据集，或者不同类型的数据(即不是推文)会给出不同的结果。值得注意的是，上面提到的由@conorosullyds 编写的分析器只给出了比 VADER 略好的结果(73%)，公平地说，VADER 特别针对 tweets，所以应该比更一般的 TextBlob 表现得更好。

就这些，我希望这是有趣的，一如既往地感谢阅读。我会像往常一样邀请你订阅不定期的[时事通讯](https://technofile.substack.com/)，获取我发表的文章的新闻以及我的代码的完整列表。