# Spark 中的 sort()与 orderBy()

> 原文：<https://towardsdatascience.com/sort-vs-orderby-in-spark-8a912475390?source=collection_archive---------6----------------------->

## Apache Spark 中 sort()和 orderBy()有什么区别

![](img/65969e374626ac77cc570cdd7e4de045.png)

米凯尔·克里斯滕森在 [Unsplash](https://unsplash.com/s/photos/order?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

对 Spark 数据帧进行排序可能是最常用的操作之一。您可以使用`sort()`或`orderBy()`内置函数对至少一列的特定数据帧进行升序或降序排序。尽管这两个函数都应该对 Spark 数据帧中的数据进行排序，但它们有一个显著的区别。

在今天的文章中，我们将讨论`sort()`和`orderBy()`的区别，并了解何时使用其中一个。

首先，让我们创建一个示例数据框架，它将在整篇文章中用来演示`sort()`和`orderBy()`是如何工作的。注意，我将使用 Python API，但是我们今天讨论的内容也适用于 Scala。

```
df = spark.createDataFrame(
   [
      ('Andrew', 'Johnson', 'Engineering', 'UK', 34),
      ('Maria', 'Brown', 'Finance', 'US', 41),
      ('Michael', 'Stevenson', 'Sales', 'US', 31),
      ('Mark', 'Anderson', 'Engineering', 'Ireland', 28),
      ('Jen', 'White', 'Engineering', 'UK', 29)
  ],
  ['first_name', 'last_name', 'department', 'country', 'age']
)
df.show(truncate=False)+----------+---------+-----------+--------+---+
|first_name|last_name| department||country|age|
+----------+---------+-----------+--------+---+
|    Andrew|  Johnson|Engineering|      UK| 34|
|     Maria|    Brown|    Finance|      US| 41|
|   Michael|Stevenson|      Sales|      US| 31|
|      Mark| Anderson|Engineering| Ireland| 28|
|       Jen|    White|Engineering|      UK| 29|
+----------+---------+-----------+--------+---+
```

## 排序()

现在，我们可以使用`sort()`方法根据雇员的国家和年龄对数据帧`df`进行升序排序。

```
df.sort('country', 'age').show(truncate=False)+----------+---------+-----------+--------+---+
|first_name|last_name| department||country|age|
+----------+---------+-----------+--------+---+
|      Mark| Anderson|Engineering| Ireland| 28|
|       Jen|    White|Engineering|      UK| 29|
|    Andrew|  Johnson|Engineering|      UK| 34|
|   Michael|Stevenson|      Sales|      US| 31|
|     Maria|    Brown|    Finance|      US| 41|
+----------+---------+-----------+--------+---+
```

但是请注意，`sort()`方法将对每个分区中的记录进行排序，然后返回最终输出，这意味着**输出数据的顺序无法保证**，因为数据是在分区级别上排序的，但是您的数据帧可能有数千个分区分布在集群中。由于数据没有被收集到一个单独的执行器中，所以`sort()`方法是有效的，因此当排序对您的用例不重要时更合适。

例如，如果雇员`Jen`和`Andrew`存储在不同的分区上，那么最终的顺序可能是错误的，例如:

```
+----------+---------+-----------+--------+---+
|first_name|last_name| department||country|age|
+----------+---------+-----------+--------+---+
|      Mark| Anderson|Engineering| Ireland| 28|
|    Andrew|  Johnson|Engineering|      UK| 34|
|       Jen|    White|Engineering|      UK| 29|
|   Michael|Stevenson|      Sales|      US| 31|
|     Maria|    Brown|    Finance|      US| 41|
+----------+---------+-----------+--------+---+
```

## orderBy()

同样，我们可以使用`orderBy()`根据雇员的国家和年龄以升序排列数据框:

```
df.orderBy('country', 'age').show(truncate=False)+----------+---------+-----------+--------+---+
|first_name|last_name| department||country|age|
+----------+---------+-----------+--------+---+
|      Mark| Anderson|Engineering| Ireland| 28|
|       Jen|    White|Engineering|      UK| 29|
|    Andrew|  Johnson|Engineering|      UK| 34|
|   Michael|Stevenson|      Sales|      US| 31|
|     Maria|    Brown|    Finance|      US| 41|
+----------+---------+-----------+--------+---+
```

与`sort()`不同，`**orderBy()**` **功能保证输出中的总顺序。**发生这种情况是因为数据将被收集到单个执行器中以便进行排序。这意味着`orderBy()`比`sort()`效率更低。

## 最后一句话

`sort()`和`orderBy()`功能均可用于对至少一列的火花数据帧进行排序，排序顺序为任意顺序，即升序或降序。

`sort()`比`orderBy()`更有效，因为数据在每个分区上单独排序，这就是为什么输出数据的顺序不能保证。

另一方面，`orderBy()`将所有数据收集到一个执行器中，然后对它们进行排序。这意味着输出数据的顺序得到保证，但这可能是一个非常昂贵的操作。

因此，如果您需要对顺序不太重要的数据帧进行排序，同时您希望它尽可能快，那么`sort()`更合适。如果顺序很重要，确保使用`orderBy()`来保证输出数据根据用户参数排序。

## 你可能也喜欢

[](/distinct-vs-dropduplicates-in-spark-3e28af1f793c) [## Spark 中的 distinct()与 dropDuplicates()

### Spark 中 distinct()和 dropDuplicates()有什么区别？

towardsdatascience.com](/distinct-vs-dropduplicates-in-spark-3e28af1f793c) [](/sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a) [## spark session vs spark context vs SQLContext vs hive context

### SparkSession、SparkContext HiveContext 和 SQLContext 有什么区别？

towardsdatascience.com](/sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a) [](/how-to-efficiently-re-partition-spark-dataframes-c036e8261418) [## 如何有效地重新划分火花数据帧

### 如何增加或减少火花数据帧的数量

towardsdatascience.com](/how-to-efficiently-re-partition-spark-dataframes-c036e8261418)