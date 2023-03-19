# Spark 中的 select()与 selectExpr()

> 原文：<https://towardsdatascience.com/select-vs-selectexpr-in-spark-412a59416615?source=collection_archive---------7----------------------->

## 讨论 Spark 中 select()和 selectExpr()方法的区别

![](img/fda72189be88a48a82ec9603300bfbf7.png)

亚历山大·席默克在 [Unsplash](https://unsplash.com/s/photos/select?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

列选择无疑是 Spark 数据帧(和数据集)上最常用的操作之一。Spark 有两个内置的方法可以用来这样做，即`select()`和`selectExpr()`。

在今天的文章中，我们将讨论如何使用它们，并解释它们的主要区别。此外，我们将讨论何时应该使用其中一种。

在讨论如何使用`select()`和`selectExpr()`进行选择之前，让我们创建一个样本数据帧，作为本文的参考

```
from pyspark.sql import SparkSessionspark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate()df = spark_session.createDataFrame(
    [
       (1, 'Giorgos', 'Developer', 28),
       (2, 'Andrew', 'Architect', 31),
       (3, 'Maria', 'Manager', 31),
       (4, 'Ben', 'Developer', 29),
    ],
    ['id', 'name', 'occupation', 'age']
)df.show()+---+--------+----------+---+
| id|    name|occupation|age|
+---+--------+----------+---+
|  1| Giorgos| Developer| 28|
|  2|  Andrew| Architect| 31|
|  3|   Maria|   Manager| 31|
|  4|     Ben| Developer| 29|
+---+--------+----------+---+
```

## 选择()

`[pyspark.sql.DataFrame.select()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.select.html)`是一个转换函数，它返回一个新的数据帧，其中包含输入中指定的所需列。它接受一个参数`columns`，可以是`str`、`Column`或`list`，以防您想要选择多个列。该方法投射一组表达式，并将返回一个新的 Spark 数据帧。

以下表达式将返回一个包含原始数据帧所有列的新数据帧:

```
df.select('*').show()+---+--------+----------+---+
| id|    name|occupation|age|
+---+--------+----------+---+
|  1| Giorgos| Developer| 28|
|  2|  Andrew| Architect| 31|
|  3|   Maria|   Manager| 31|
|  4|     Ben| Developer| 29|
+---+--------+----------+---+
```

如果您只想选择列的子集，可以如下所示进行操作:

```
df.select('id', 'name').show()+---+--------+
| id|    name|
+---+--------+
|  1| Giorgos|
|  2|  Andrew|
|  3|   Maria|
|  4|     Ben|
+---+--------+
```

## selectExpr()

`[pyspark.sql.DataFrame.selectExpr()](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.DataFrame.selectExpr.html)`与`select()`相似，唯一的区别是它接受将要执行的 SQL 表达式(字符串格式)。同样，该表达式将根据提供的输入从原始数据中返回一个新的数据帧。

另外，与`select()`不同，这个方法只接受字符串。例如，为了将列`age`乘以 2 以及`id`和`name`列，您需要执行以下语句:

```
df.selectExpr('id', 'name', 'age * 2').show()+---+--------+-------+
| id|    name|age * 2|
+---+--------+-------+
|  1| Giorgos|     28|
|  2|  Andrew|     31|
|  3|   Maria|     31|
|  4|     Ben|     29|
+---+--------+-------+
```

因此，当您只需要从一个特定的 Spark 数据帧中选择一个列的子集时，`select()`方法非常有用。另一方面，当您需要选择特定的列，同时还需要对特定的列应用某种转换时，`selectExpr()`就派上了用场。

## 最后的想法

在今天的文章中，我们讨论了 Spark 中`select()`和`selectExpr()`方法的区别。我们展示了如何使用两者对 Spark 数据帧进行选择，并强调了何时使用其中一个。

你可以在下面阅读更多关于 Spark 的文章。

</distinct-vs-dropduplicates-in-spark-3e28af1f793c>  </sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a>  </how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3> 