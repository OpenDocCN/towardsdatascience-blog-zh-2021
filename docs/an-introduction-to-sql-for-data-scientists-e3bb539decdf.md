# 数据科学家的 SQL 介绍

> 原文：<https://towardsdatascience.com/an-introduction-to-sql-for-data-scientists-e3bb539decdf?source=collection_archive---------12----------------------->

## UCL 数据科学学会工作坊 9:什么是 SQL，选择数据，查询数据，汇总统计，分组数据，连接数据

![](img/c8cc34d67c2438eb7a55144ee5f28018.png)

简·安东宁·科拉尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今年，作为 UCL 数据科学协会的科学负责人，该协会将在整个学年举办一系列 20 场研讨会，主题包括 Python 简介、数据科学家工具包和机器学习方法等。每一篇文章的目标都是创建一系列的小博客，这些小博客将概述要点，并为任何希望跟进的人提供完整研讨会的链接。所有这些都可以在我们的 [GitHub](https://github.com/UCL-DSS) 资源库中找到，并将在全年更新新的研讨会和挑战。

本系列的第九个研讨会是 SQL 简介，是数据科学家工具包系列的最后一个研讨会。在本次研讨会中，我们将向您介绍在 SQL 中查询数据、从数据中提取汇总统计数据、对数据集进行分组和连接的基础知识。如果你想继续写代码，你可以在我们的 GitHub [这里](https://github.com/UCL-DSS/SQL_workshop)找到完整的研讨会，其中包含了如何使用 docker 容器在你的系统上建立 MySQL 数据库，如何将数据推送到数据库以及如何与 MySQL workbench 交互的信息。

如果您错过了最近的任何研讨会，您可以在此找到它们:

[](/git-and-github-basics-for-data-scientists-b9fd96f8a02a) [## 面向数据科学家的 Git 和 GitHub 基础知识

### UCL 数据科学研讨会 8:什么是 Git，创建本地存储库，提交第一批文件，链接到远程…

towardsdatascience.com](/git-and-github-basics-for-data-scientists-b9fd96f8a02a) [](/an-introduction-to-plotting-with-matplotlib-in-python-6d983b9ba081) [## Python 中 Matplotlib 绘图简介

### UCL 数据科学学会研讨会 7:创建一个基本的图表，在同一图表上绘制不同的信息…

towardsdatascience.com](/an-introduction-to-plotting-with-matplotlib-in-python-6d983b9ba081) [](/ucl-data-science-society-pandas-8ad28c2b22e5) [## UCL 数据科学学会:熊猫

### 工作坊 6:什么是 Pandas，Pandas 系列，Pandas 数据框架，访问数据和 Pandas 操作

towardsdatascience.com](/ucl-data-science-society-pandas-8ad28c2b22e5) 

## 什么是 SQL？

SQL(结构查询语言)是在关系数据库管理系统中处理数据库时使用最广泛的编程语言之一。它用于执行各种不同的操作，如选择数据、查询数据、分组数据和提取汇总度量，如下所示。在处理数据科学项目的数据时，它是一个在工业和学术界广泛使用的工具，因此理解 SQL 是任何数据科学家工具包中的一个关键工具。为此，我们向您介绍 SQL 中的一些基本命令，以及如何使用它们来操作和提取数据。

## 选择数据

我们想做的第一件事是能够从我们的数据库中选择数据，看看它看起来像什么。我们可以通过使用`SELECT`命令来做到这一点，然后使用`FROM`命令告诉我们想要这个信息的位置。在我们的例子中，我们有两个名为`single_table`和`double_table`的表，所以首先我们可以使用命令从第一个表中提取所有信息:

```
SELECT *
FROM single_table;
```

该命令将返回来自`single_table`的所有数据，因为我们使用`*`来表示我们想要所有列。

这将根据表的大小返回大量数据，因此我们可能只想查看几列，可能只查看前 100 行。我们可以通过修改我们的 SQL 语句来做到这一点，这样我们就不会使用`*`变量来选择所有的列，而是使用实际的名称，然后设置一个`LIMIT`来只返回前 100 行。我们可以使用以下命令来实现这一点:

```
SELECT `Air Date`, Category, `Value`
FROM single_table
LIMIT 100;
```

在这里，我们选择了 Air Date、Category 和 Value 列(第一列和第三列用''括起来，因为它们包含一个空格或者是关键字),我们将一个`LIMIT`设置为 100 行。为此，在末尾使用`;`分隔语句非常重要，否则 MySQL 不会将它们识别为单独的语句。

## 搜索条件

上述语句将返回所选列中的所有数据，但是我们可以创建更高级的搜索，以便在特定条件下进行匹配。当您仅从数据集中搜索特定信息以便缩小分析范围时，通常会出现这种情况。

在我们的例子中，我们有`Category`、`Air Date`和`Value`列，因此我们可以搜索属于`HISTORY`类别的所有问题。这是通过使用`WHERE`命令来指定我们希望数据匹配的条件，如下所示:

```
SELECT `Air Date`, Category, `Value`
from single_table
WHERE Category = "HISTORY"
LIMIT 100;
```

这个`WHERE`语句允许我们添加对数据进行子集化的条件，并允许我们使用任何想要的比较运算符，例如`>`、`<`、`==`、`BETWEEN`、`LIKE`和`IN`。首先指定要应用条件的列，然后使用上述操作符设置条件本身。

我们还可以通过`WHERE`条件组合使用`AND`、`OR`和`NOT`的条件来进行更高级的搜索。例如，如果我们想将搜索范围缩小到类别为“历史”的问题，但该问题的价值也大于 200 美元，我们可以这样做:

```
SELECT `Air Date`, Category, `Value`
FROM single_table
WHERE Category = "HISTORY"
AND `Value` > 200
LIMIT 100;
```

## 汇总统计数据

除了提取信息以便稍后执行计算，SQL 还可以自己执行计算。这包括从列中提取所有不同值、计算数据集中值的数量或从数字列中提取平均值、最小值或最大值等功能。

例如，使用位于`CATEGORY`列中心的`DISTINCT`和`COUNT`命令从数据集中提取不同类别的数量。我们可以这样做:

```
SELECT COUNT(DISTINCT CATEGORY)FROM single_table;
```

这应该会返回一个单一的数字，我们有多少独特的类别。如果我们只想要一个唯一类别的列表，我们可以放下`COUNT`命令，我们将得到所有这些类别的列表。其他汇总统计包括`MIN`、`MAX`、`AVG`和`SUM`，就像我们在[熊猫数据帧](/ucl-data-science-society-pandas-8ad28c2b22e5)中预期的那样。

## 分组数据

对于数值，我们可以使用各自的命令从列中提取`MAX`、`MIN`和`AVG`值，就像我们对`COUNT`所做的那样。但是，我们可能希望从不同的数据组中获取这些值，然后提取每组的信息。在这种情况下，我们可以将汇总统计数据与`GROUP BY`功能结合使用。

在这里，我们可以看到每个类别的平均值是多少，这样我们就可以尝试选择将为我们带来最大价值的问题。我们可以这样做:对我们想要获取统计数据的列调用 summary statistic 命令，并对我们想要按组提取信息的列使用`GROUP BY`命令。这可以通过以下方式实现:

```
SELECT Category, AVG(`Value`)FROM single_tableGROUP BY CategoryLIMIT 100;
```

在这个命令中，我们对 Category 列进行了分组，并提取了每个类别的平均值。

目前，这种方法的用处有限，因为顺序与类别出现的顺序一致，而不是与值一致。因此，我们可以结合使用`GROUP BY`命令和`ORDER BY`命令。为此，我们指定要排序的列，然后指定是按降序还是升序排序。这里我们希望数据以降序排列，这样我们可以先看到最高值。这可以通过以下方式实现:

```
SELECT Category, AVG(`Value`)FROM single_tableGROUP BY CategoryORDER BY AVG(`Value`) DESCLIMIT 10;
```

请注意，在按问题的平均值排序时，我们仍然需要指定在使用`ORDER BY`命令显示我们想要在被操作的列上执行聚合时在`Value`列上执行的聚合。

## 连接数据集

我们要介绍的最后一件事是`JOIN`功能，该功能可用于根据左表或右表中的值、两个表中的数据将两个或多个表连接在一起，或者将表与自身连接在一起。一个简单的例子是使用左连接，它使用左侧表中的所有信息，只使用右侧表中的连接信息。

在我们的例子中，我们可以使用一个`INNER JOIN`,在这里我们希望根据平均值连接每个表的分组类别上的`single_table`和`double_table`,以查看哪个数据集在每个类别中具有更高的值。为此，我们需要为第二个表指定`INNER JOIN`,并使用`ON`命令指定我们希望连接的列。我们可以这样做:

```
SELECT a.Category, AVG(a.`Value`) as single_value, 
b.Category, AVG(b.`Value`) as double_valueFROM single_table aINNER JOIN double_table bon a.Category = b.CategoryGROUP BY a.Category, b.CategoryLIMIT 100;
```

为此，需要注意的是，我们使用`a.`和`b.`符号来访问每个数据集的相关列，并且在创建了`INNER JOIN`之后使用`GROUP BY`。在这样做的时候，我们还使用了`GROUP BY`功能在连接发生之前对每个表的类别列进行分组。虽然这可能不是最有效的方法，但它确实有效！

完整的研讨会笔记本，以及更多示例和挑战，可在的 [**处找到。**](https://github.com/UCL-DSS/SQL_workshop)如果您想了解我们协会的更多信息，请随时关注我们的社交网站:

https://www.facebook.com/ucldata[脸书](https://www.facebook.com/ucldata)

insta gram:[https://www.instagram.com/ucl.datasci/](https://www.instagram.com/ucl.datasci/)

领英:[https://www.linkedin.com/company/ucldata/](https://www.linkedin.com/company/ucldata/)

如果你想了解 UCL 数据科学协会和其他了不起的作者的故事，请随时使用我下面的推荐代码注册 medium。

[](https://philip-wilkinson.medium.com/membership) [## 通过我的推荐链接加入媒体-菲利普·威尔金森

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

philip-wilkinson.medium.com](https://philip-wilkinson.medium.com/membership) 

或者看看我在 Medium 上的其他文章:

[](/multi-variate-outlier-detection-in-python-e900a338da10) [## Python 中的多变量异常检测

### 能够检测数据集中异常值/异常值的六种方法

towardsdatascience.com](/multi-variate-outlier-detection-in-python-e900a338da10) [](/london-convenience-store-classification-using-k-means-clustering-70c82899c61f) [## 使用 K-均值聚类的伦敦便利店分类

### 伦敦的便利店怎么分类？

towardsdatascience.com](/london-convenience-store-classification-using-k-means-clustering-70c82899c61f) [](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d) [## scikit-learn 决策树分类器简介

towardsdatascience.com](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d)