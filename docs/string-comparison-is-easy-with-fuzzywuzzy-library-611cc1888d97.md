# 使用 FuzzyWuzzy 库比较字符串很容易

> 原文：<https://towardsdatascience.com/string-comparison-is-easy-with-fuzzywuzzy-library-611cc1888d97?source=collection_archive---------21----------------------->

## 了解用于字符串比较的 FuzzyWuzzy 库——它如何工作以及何时有用

![](img/72e6a572c3d41a426eede644dd704f71.png)

安娜斯塔西娅·罗曼诺娃在 [unsplash](http://www.unsplash.com) 上拍摄的图片

*大约一年前，我看到我的一个同事在处理一个大型数据集，旨在测量每对字符串之间的相似度。我的同事开始开发一种计算字符串之间“距离”的方法，并提出了一套规则。*

*自从我熟悉了*[*FuzzyWuzzy*](https://pypi.org/project/fuzzywuzzy/)*python 库，我知道有一种更快更有效的方法来确定一对字符串是相似还是不同。不需要繁琐的计算，不需要重新发明轮子，只需要熟悉 FuzzyWuzzy，以及我今天将与你分享的一些其他技巧。*

## 什么是字符串比较，FuzzyWuzzy 有什么帮助？

> FuzzyWuzzy 是一个 Python 库，它计算两个给定字符串的相似性得分。

相似性分数以 0(完全不相关)到 100(非常匹配)的范围给出。

毕竟，如果您需要的只是精确的字符串比较，Python 可以满足您:

```
>>> **"check out this example" == "check out this example"** True>>> **"check out this example"** == **"something completely different"** False
```

但是，如果您的字符串不一定完全相同，但您仍然需要知道它们有多相似，该怎么办呢？

```
>>> **"check out this example"** == **"check out this exampel"** False
```

这不是很有帮助。

但是看看这个:

```
>>> fuzz.ratio("**check out this example"**, "**check out this exampel"**)
95
```

好多了。

## 入门指南

如果您的虚拟环境中没有安装相关的库，并且在使用之前没有导入，您将需要运行以下程序:

在命令行中:

```
pip install pandas
pip install fuzzywuzzy 
***# or — preferably***pip install fuzzywuzzy[speedup] 
***# [speedup] installs python-Levenshtein library 
  for better performance***
```

在您的 ide 中:

```
>>> import pandas as pd
>>> from fuzzywuzzy import fuzz
```

## 《距离》——模糊的幕后

不同的 FuzzyWuzzy 函数使用 [Levenshtein 距离](https://en.wikipedia.org/wiki/Levenshtein_distance)——一种计算两个字符串之间距离的流行算法。这些字符串彼此“越远”，距离**分数**就越大(因为**反对**模糊不清的输出)。

然而，Levenshtein 距离有一个主要的缺点:
它有一个逻辑，而在现实生活中有几种不同的方法来定义字符串的相似性。
Levenshtein 距离无法用一条逻辑覆盖不同的情况。有些情况会被人类定义为相似的字符串，但被 Levenstein 遗漏了。

例如:

```
>>> Levenshtein.distance("**measuring with Levenshtein", 
                         "measuring differently"**)
11>>> Levenshtein.distance("**Levenshtein distance is here", 
                        "here is distance Levenshtein"**)
17
```

如果有人必须决定哪对字符串更相似，他们可能会选择第二个选项。但是，我们可以看到它实际上获得了更高的距离分数。

我们需要更好的东西。

# 了解这些功能

FuzzyWuzzy 有一些不同的功能。每个函数都接受一对字符串，并返回一个 0-100 的匹配分数。但是这些函数中的每一个都有稍微不同的逻辑，所以我们可以选择最合适的函数来满足我们的需要。

让我们来了解一些突出的模糊不清的函数:

*   **fuzz . ratio:**T2 最简单的函数。基于以下逻辑计算分数:
    给定两个字符串，其中 T 是两个序列中元素的总数，M 是匹配的数目，这些字符串之间的相似性为:
    *2*(M / T)*100*

```
>>> fuzz.ratio("**great"**, "**green"**)
60
```

在本例中-
T = len(" great ")+len(" green ")= 10
M = 3
，因此公式为 2*(3/10)*100

*   **fuzz.partial_ratio:**
    让我们看看下面三个例子，然后讨论它们:

```
>>> fuzz.partial_ratio("**let’s compare strings"**, "**strings"**)
100
>>> fuzz.partial_ratio("**let’s compare strings"**, "**stings"**)
83
>>> fuzz.ratio("**let’s compare strings"**, "**strings"**)
50
```

以上示例演示了以下内容:
1 .B 中的字符串完全包含在字符串 A 中，因此匹配分数为 100。
2。除了一个“错别字”，B 中的字符串几乎完全包含在字符串 A 中，因此匹配分数是 83。
3。与第一个示例相同，但是在 A 和 B 之间使用了 ratio 函数，而不是 partial_ratio。因为 ratio 函数的目的不是处理子串的情况，所以分数较低。

*   **fuzz.token_sort_ratio** 这将为字符串 A、B 检索 100 的匹配分数，其中 A 和 B 包含相同的标记，但顺序不同。
    看下面的例子，一次用 token_sort_ratio 函数，一次用 ratio 函数，看看不同的结果。

```
>>> fuzz.token_sort_ratio("**let's compare strings"**, 
                          "**strings compare let's"**)
100
>>> fuzz.ratio("**let's compare strings"**, "**strings compare let's"**)
57
```

同样的令牌但是不同的计数怎么办？

```
>>> fuzz.token_sort_ratio(**"let's compare", "let's compare compare"**)
76
```

这不是这个函数的专长。在这种情况下，我们有 token_set_ratio 来拯救我们。

*   **fuzz.token_set_ratio**

    我们可以看到下面的两个例子得到了相同的匹配分数，尽管在第二个例子中一些记号被重复了，并且它们的顺序被改变了。

```
>>> fuzz.token_set_ratio("**we compare strings together"**, 
                         "**together we talk"**)
81
>>> fuzz.token_set_ratio("**strings we we we compare together"**, 
                         "**together together talk we"**)
81
```

在**将**稍微简化之后，这个函数背后的逻辑如下:
给定字符串 A，B *(让 A = 'hi hey ho '和 B = ' yo yay ho ')*:
C = A 和 B 的交集*(' ho ')*
D = C+A 的余数*(' ho hi hey ')*
E = C+B 的余数*(' t*

完整的逻辑包含额外的规范化，比如对 A 和 B 应用 set()，对 C 应用 sort()等等。

由上述每个函数实现的代码，以及其他有用的 FuzzyWuzzy 函数，可以在[这里](https://github.com/seatgeek/fuzzywuzzy/blob/master/fuzzywuzzy/fuzz.py)找到。

## 触及本质

在本文中，我们介绍了 FuzzyWuzzy 库及其功能的基础知识。

然而，为我们的项目获得最好的结果并不是从知道哪些功能可用以及如何使用它们开始和结束的。

像专业人士那样做意味着知道如何在处理数据之前清理数据，如何不仅比较成对的字符串，还比较大的表格，使用哪个(或哪些)阈值分数等等。

为了了解所有这些，我邀请你阅读名为 [FuzzyWuzzy 的连续文章——之前和之后](https://medium.com/naomikriger/fuzzywuzzy-the-before-and-after-c3661ea62ef8)。