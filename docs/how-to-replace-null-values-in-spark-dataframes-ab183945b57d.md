# 如何替换 Spark 数据帧中的空值

> 原文：<https://towardsdatascience.com/how-to-replace-null-values-in-spark-dataframes-ab183945b57d?source=collection_archive---------6----------------------->

## 讨论如何使用 fillna()和 fill()替换 PySpark 中的空值

![](img/08d2e2b85283f24b3c267abab1cca42b.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/empty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

替换 PySpark 数据帧中的空值是最常见的操作之一。这可以通过使用`DataFrame.fillna()`或`DataFrameNaFunctions.fill()`方法来实现。在今天的文章中，我们将讨论这两个函数之间的主要区别。

## 为什么我们需要替换空值

在处理 Spark 数据帧时，我们通常对它们执行的许多操作可能会在某些记录中返回空值。从那时起，如果观察到 null/空值，一些其他操作可能会导致错误，因此我们必须以某种方式替换这些值，以便继续处理数据帧。

此外，当报告表格时(例如，当将它们输出到`csv`文件中时)，避免包含空值是很常见的。例如，如果为创建计数而执行的操作返回空值，那么用`0`替换这些值会更好。

在开始讨论如何在 PySpark 中替换空值以及探索`fill()`和`fillNa()`之间的区别之前，让我们创建一个示例数据帧，作为整篇文章的参考。

```
from pyspark.sql import SparkSession spark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate() df = spark_session.createDataFrame(
    [
       (1, 'UK', 'London', None),
       (2, 'US', 'New York', 8000000),
       (3, 'US', 'Washington DC', None),
       (4, 'UK', 'Manchester', 550000),
    ],
    ['id', 'country', 'city', 'population']
)df.show()+---+---------+--------------+-----------+
| id|  country|         city | population|
+---+---------+--------------+-----------+
|  1|       UK|        London|       null|
|  2|       US|      New York|    8000000|
|  3|       US| Washington DC|       null|
|  4|       UK|    Manchester|     550000|
+---+---------+--------------+-----------+
```

## 菲尔娜

`[pyspark.sql.DataFrame.fillna()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.fillna.html#pyspark.sql.DataFrame.fillna)`函数在 Spark 版本`1.3.1`中引入，用于将空值替换为另一个指定值。它接受两个参数，即`value`和`subset`。

*   `value`对应于您想要替换空值的所需值。如果`value`是一个`dict`对象，那么它应该是一个映射，其中键对应于列名，值对应于替换值，替换值必须是`int`、`float`、`bool`或`str`。
*   `subset`对应于替换空值时要考虑的列名列表。如果`value`参数是一个`dict`，那么该参数将被忽略。

现在，如果我们想要替换一个数据帧中的所有空值，我们可以简单地只提供`value`参数: `df.na.fill(value=0).show()

#Replace Replace 0 for null on only population column
df.na.fill(value=0,subset=["population"]).show()`

```
df.fillna(value=0).show()+---+---------+--------------+-----------+
| id|  country|         city | population|
+---+---------+--------------+-----------+
|  1|       UK|        London|          0|
|  2|       US|      New York|    8000000|
|  3|       US| Washington DC|          0|
|  4|       UK|    Manchester|     550000|
+---+---------+--------------+-----------+
```

上述操作将用`0`的值替换整数列中的所有空值。

我们甚至可以使用`subset`参数显式指定列名:

```
df.fillna(value=0, subset=['population']).show()+---+---------+--------------+-----------+
| id|  country|         city | population|
+---+---------+--------------+-----------+
|  1|       UK|        London|          0|
|  2|       US|      New York|    8000000|
|  3|       US| Washington DC|          0|
|  4|       UK|    Manchester|     550000|
+---+---------+--------------+-----------+
```

## 填充()

现在`[pyspark.sql.DataFrameNaFunctions.fill()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrameNaFunctions.fill.html#pyspark.sql.DataFrameNaFunctions.fill)`(在版本`1.3.1`中再次引入)是`[pyspark.sql.DataFrame.fillna()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.fillna.html#pyspark.sql.DataFrame.fillna)`的别名，两种方法将导致完全相同的结果。

正如我们在下面看到的，`na.fill()`的结果与`[pyspark.sql.DataFrame.fillna()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.fillna.html#pyspark.sql.DataFrame.fillna)`应用于数据帧时观察到的结果相同。

```
df.na.fill(value=0).show()+---+---------+--------------+-----------+
| id|  country|         city | population|
+---+---------+--------------+-----------+
|  1|       UK|        London|          0|
|  2|       US|      New York|    8000000|
|  3|       US| Washington DC|          0|
|  4|       UK|    Manchester|     550000|
+---+---------+--------------+-----------+
```

上述操作将用`0`的值替换整数列中的所有空值。类似地，我们可以使用`subset`参数显式指定列名:

```
df.na.fill(value=0, subset=['population']).show()+---+---------+--------------+-----------+
| id|  country|         city | population|
+---+---------+--------------+-----------+
|  1|       UK|        London|          0|
|  2|       US|      New York|    8000000|
|  3|       US| Washington DC|          0|
|  4|       UK|    Manchester|     550000|
+---+---------+--------------+-----------+
```

## 最后的想法

在今天的文章中，我们讨论了为什么在 Spark 数据帧中替换空值有时很重要。此外，我们还讨论了如何使用`fillna()`和`fill()`来实现这一点，它们实际上是彼此的别名。

你可以在下面找到更多 Spark 相关的文章。

</distinct-vs-dropduplicates-in-spark-3e28af1f793c>  </sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a>  </sort-vs-orderby-in-spark-8a912475390> [## Spark 中的 sort()与 orderBy()

towardsdatascience.com](/sort-vs-orderby-in-spark-8a912475390)