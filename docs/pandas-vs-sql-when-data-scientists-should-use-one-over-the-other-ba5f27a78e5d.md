# 熊猫 vs SQL。当数据科学家应该使用一个而不是另一个的时候。

> 原文：<https://towardsdatascience.com/pandas-vs-sql-when-data-scientists-should-use-one-over-the-other-ba5f27a78e5d?source=collection_archive---------3----------------------->

## 意见

## 深入探究每种工具的优势

![](img/fc8ad5f5b37fbdb24e4bfc03c42987c9.png)

照片由[参宿七](https://unsplash.com/@rigels?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】拍摄。

# 目录

1.  介绍
2.  熊猫
3.  结构化查询语言
4.  摘要
5.  参考

# 介绍

这两种工具不仅对数据科学家很重要，对数据分析和商业智能等类似职位的人也很重要。也就是说，数据科学家什么时候应该专门使用 pandas 而不是 SQL，反之亦然？在某些情况下，您可以只使用 SQL，而在其他一些时候，pandas 更容易使用，特别是对于那些专注于 Jupyter 笔记本设置中的研究的数据科学家。下面，我将讨论你什么时候应该使用 SQL，什么时候应该使用 pandas。请记住，这两种工具都有特定的用例，但是它们的功能有很多重叠的地方，这也是我下面要比较的地方。

# 熊猫

![](img/f6b8dbd5d7fccc3eb4e09c697b812c82.png)

照片由[卡伦·坎普](https://unsplash.com/@kalenkempx?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

[Pandas](https://pandas.pydata.org/docs/index.html)【3】是 Python 编程语言中的一个开源数据分析工具。当您已经有了主数据集(通常来自 SQL 查询)时，pandas 的好处就开始了。这一主要区别意味着这两个工具是独立的，但是，您也可以在各自的工具中执行几个相同的功能，例如，您可以在 pandas 中从现有的列创建新的要素，这可能比在 SQL 中更容易和更快。

需要注意的是，我并不是在比较 Pandas 能做 SQL 不能做的事情，反之亦然。我将根据个人经验，选择能更有效或更适合数据科学工作的工具。

> 以下是使用 pandas 比 SQL 更有利的时候——同时还具有与 SQL 相同的功能:

*   **根据现有特征创建计算字段**

当合并一个更复杂的 SQL 查询时，通常还会合并子查询，以便划分不同列的值。在熊猫身上，你可以简单地划分特征，就像下面这样:

```
df["new_column"] = df["first_column"]/df["second_column"]
```

上面的代码显示了如何划分两个单独的列，并将这些值分配给一个新列-在这种情况下，您将对整个数据集或数据框执行要素创建。在数据科学的过程中，您可以在特征探索和特征工程中使用该功能。

*   **分组依据**

还可以参考子查询，SQL 中的 group by 可能会变得非常复杂，并且需要一行又一行的代码，这在视觉上可能会让人不知所措。在熊猫中，你可以简单地通过一行代码进行分组。*我不是在一个简单的 select from table 查询的末尾引用 group by，而是在一个包含多个子查询的查询中引用。*

```
df.groupby(by="first_column").mean()
```

这个结果将返回数据帧中每一列的 first_column 的平均值。使用这个分组功能还有很多其他的方法，下面链接的 pandas 文档中很好地概述了这些方法。

*   **检查数据类型**

在 SQL 中，您经常需要转换类型，但是有时可以更清楚地看到 pandas 以垂直格式排列数据类型的方式，而不是在 SQL 中滚动水平输出。您可以预期*返回的数据类型的一些*示例是 int64、float64、datetime64[ns]和 object。

```
df.dtypes
```

虽然这些都是 pandas 和 SQL 的相当简单的函数，但在 SQL 中，它们特别复杂，有时在 pandas 数据框架中更容易实现。现在，让我们看看 SQL 更擅长执行什么。

# 结构化查询语言

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

卡斯帕·卡米尔·鲁宾在[Unsplash](https://unsplash.com/s/photos/query?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上的照片。

SQL 可能是被最多不同职位使用最多的语言。例如，数据工程师可以使用 SQL、Tableau 开发人员或产品经理。也就是说，数据科学家倾向于频繁使用 SQL。值得注意的是，SQL 有几种不同的版本，通常都有相似的功能，只是格式略有不同。

> 以下是使用 SQL 比 pandas 更有利的时候——同时还具有与 pandas 相同的功能:

*   **WHERE 子句**

SQL 中的这个子句使用频率很高，在熊猫中也可以执行。然而，对于熊猫来说，这要稍微困难一些，或者说不那么直观。例如，您必须写出冗余的代码，而在 SQL 中，您只需要`WHERE`。

```
SELECT IDFROM TABLEWHERE ID > 100
```

对熊猫来说，可能是这样的:

```
df[df["ID"] > 100]["ID"]
```

是的，两个都很简单，一个只是更直观一点。

*   **加入**

Pandas 有几种连接方式，这可能有点让人不知所措，而在 SQL 中，您可以执行简单的连接，如下所示:`INNER, LEFT, RIGHT`

```
SELECTone.column_A,two.column_BFROM FIRST_TABLE oneINNER JOIN SECOND_TABLE two on two.ID = one.ID
```

在这段代码中，join 比 pandas 更容易阅读，在 pandas 中，您必须合并数据帧，尤其是当您合并两个以上的数据帧时，在 pandas 中会非常复杂。SQL 可以执行多重连接，无论是内部连接还是其他连接。，都在同一个查询中。

所有这些例子，无论是 SQL 还是 pandas，至少可以用于数据科学过程的探索性数据分析部分，也可以用于特征工程，以及在将模型结果存储到数据库中后对其进行查询。

# 摘要

pandas 与 SQL 的这种比较更多的是个人偏好。话虽如此，你可能会觉得我的意见相反。然而，我希望它仍然揭示了 pandas 和 SQL 之间的区别，以及您可以在这两个工具中执行相同的操作，使用稍微不同的编码技术和完全不同的语言。

> 总的来说，我们比较了使用 pandas 和 SQL 的好处，反之亦然，它们有一些共享的功能:

```
* creating calculated fields from existing features* grouping by* checking data types* WHERE clause* JOINS
```

我希望你觉得我的文章既有趣又有用。如果你同意这些比较，请在下面随意评论——为什么或为什么不同意？你认为一种工具比另一种更好吗？你能想到哪些其他数据科学工具可以进行类似的比较？pandas 和 SQL 还有哪些功能可以比较？

*请随时查看我的个人资料和其他文章，也可以通过 LinkedIn 联系我。*

# 参考

[1]照片由[参宿七](https://unsplash.com/@rigels?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)拍摄

[2]照片由[卡伦·肯普](https://unsplash.com/@kalenkempx?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄

[3]熊猫开发小组，[熊猫文献](https://pandas.pydata.org/docs/index.html)，(2008-2021)

[4]卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com/s/photos/query?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017 年)