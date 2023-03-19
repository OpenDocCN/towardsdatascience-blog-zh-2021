# 如何从 PySpark 数据帧中删除列

> 原文：<https://towardsdatascience.com/delete-columns-pyspark-df-ba9272db1bb4?source=collection_archive---------10----------------------->

## 讨论在 PySpark 中从数据帧中删除列的不同方法

![](img/fe5cdb4591014ca1b8418533d0b9df8b.png)

由[萨姆·帕克](https://unsplash.com/@melocokr?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/delete?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 介绍

从数据帧中删除列是 PySpark 中最常执行的任务之一。在今天的简短指南中，我们将探索从 PySpark 数据帧中删除列的几种不同方法。具体来说，我们将讨论如何

*   删除单个列
*   删除多列
*   反向操作，在更方便的情况下选择所需的列。

首先，让我们创建一个示例数据框架，我们将在本指南中引用它来演示一些概念。

```
from pyspark.sql import SparkSession# Create an instance of spark session
spark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate()# Create an example DataFrame
df = spark_session.createDataFrame(
    [
        (1, True, 'a', 1.0),
        (2, True, 'b', 2.0),
        (3, False, 'c', 3.0),
        (4, False, 'd', 4.0),
    ],
    ['colA', 'colB', 'colC', 'colD']
)df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   1| true|   a| 1.0|
|   2| true|   b| 2.0|
|   3|false|   c| 3.0|
|   4|false|   d| 4.0|
+----+-----+----+----+*
```

## 删除单个列

删除列最优雅的方式是使用`[pyspark.sql.DataFrame.drop](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.drop.html)`函数，该函数返回一个新的数据帧，其中包含要删除的指定列:

```
**df  = df.drop('colC')**df.show()
*+----+-----+----+
|colA| colB|colD|
+----+-----+----+
|   1| true| 1.0|
|   2| true| 2.0|
|   3|false| 3.0|
|   4|false| 4.0|
+----+-----+----+*
```

请注意，如果指定的列在该列中不存在，这将是一个空操作，意味着操作不会失败，也不会有任何影响。

## 删除多列

通常，您可能需要一次删除多个列。如果是这种情况，那么您可以将希望删除的列指定为一个列表，然后使用星号对它们进行解包，如下所示。

```
**cols_to_drop = ['colB', 'colC']
df = df.drop(*cols_to_drop)**df.show()
*+----+----+
|colA|colD|
+----+----+
|   1| 1.0|
|   2| 2.0|
|   3| 3.0|
|   4| 4.0|
+----+----+*
```

## 颠倒逻辑

在某些情况下，更方便的做法是反转拖放操作，实际上只选择想要保留的列的子集。例如，如果要删除的列数大于要在结果数据帧中保留的列数，则执行选择是有意义的。

例如，假设我们只想保留上面数据帧中的一列。在这种情况下，选择该列比删除其他 3 列更有意义:

```
**df = df.select('colA')**df.show()
*+----+
|colA|
+----+
|   1|
|   2|
|   3|
|   4|
+----+*
```

## 最后的想法

在今天的简短指南中，我们讨论了从 PySpark 数据帧中删除列的几种不同方法。除了直接删除列之外，我们还看到，在某些情况下，反向操作可能更方便，实际上只选择您希望保留在结果数据帧中的所需列。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

</how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  </sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a>  </apache-spark-3-0-the-five-most-exciting-new-features-99c771a1f512> 