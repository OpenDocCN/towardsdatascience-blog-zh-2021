# 如何根据列值从 PySpark 数据帧中选择行

> 原文：<https://towardsdatascience.com/select-rows-pyspark-df-based-on-column-values-3146afe4dee3?source=collection_archive---------4----------------------->

## 探索如何根据 PySpark 数据帧中的特定条件选择一系列行

![](img/503badd72d217fb25bf3f06af72e4786.png)

Anthony Yin 在 [Unsplash](https://unsplash.com/collections/317099/unsplash-editorial?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

过滤数据帧行是 PySpark 中最常执行的操作之一。在今天的简短指南中，我们将讨论如何以几种不同的方式根据特定条件选择一系列行。

具体来说，我们将探索如何使用

*   `filter()`功能
*   `where()`功能
*   Spark SQL

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示几个概念。

```
from pyspark.sql import SparkSession# Create an instance of spark session
spark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate()df = spark_session.createDataFrame(
    [
        (1, True, 1.0, 100),
        (2, False, 2.0, 200),
        (3, False, 3.0, 300),
        (4, True, 4.0, 400),
        (5, True, 5.0, 500),
    ],
    ['colA', 'colB', 'colC', 'colD']
)

df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   1| true| 1.0| 100|
|   2|false| 2.0| 200|
|   3|false| 3.0| 300|
|   4| true| 4.0| 400|
|   5| true| 5.0| 500|
+----+-----+----+----+*
```

## 使用 filter()函数选择行

过滤数据帧行的第一个选项是基于指定条件执行过滤的`[pyspark.sql.DataFrame.filter()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.filter.html)`函数。

例如，假设我们只想保留那些在`colC`中的值大于或等于`3.0`的行。下面的表达式可以解决这个问题:

```
**df = df.filter(df.colC >= 3.0)**df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   3|false| 3.0| 300|
|   4| true| 4.0| 400|
|   5| true| 5.0| 500|
+----+-----+----+----+*
```

您甚至可以指定`Column`函数，比如`pyspark.sql.Column.between`，以便只保留指定的下限和上限之间的行，如下所示。

```
**df = df.filter(df.colD.between(200, 400))**df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   2|false| 2.0| 200|
|   3|false| 3.0| 300|
|   4| true| 4.0| 400|
+----+-----+----+----+*
```

## 使用 where()函数选择行

`[pyspark.sql.DataFrame.where()](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.DataFrame.where.html)`是我们在上一节中讨论的`filter()`的别名。它可以以同样的方式使用，以便根据提供的条件过滤数据帧的行。

```
**df = df.where(~df.colB)**df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   2|false| 2.0| 200|
|   3|false| 3.0| 300|
+----+-----+----+----+*
```

## 使用 Spark SQL 选择行

或者，您甚至可以使用 Spark SQL 来使用 SQL 表达式查询数据帧。举个例子，

```
# Create a view for the dataframe
df.createOrReplaceTempView("df_view")

**df = spark_session.sql(
    """
    SELECT * 
    FROM df_view
    WHERE colC >= 2.0
    """
)**

df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   2|false| 2.0| 200|
|   3|false| 3.0| 300|
|   4| true| 4.0| 400|
|   5| true| 5.0| 500|
+----+-----+----+----+*
```

## 最后的想法

在今天的简短指南中，我们讨论了如何根据特定条件从 PySpark 数据帧中执行行选择。具体来说，我们展示了如何使用`filter()`和`where()`方法以及 Spark SQL 来实现这一点。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

</how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  </sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a>  </mastering-indexing-and-slicing-in-python-443e23457125> 