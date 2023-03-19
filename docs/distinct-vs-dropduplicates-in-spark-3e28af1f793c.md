# Apache Spark 中的 distinct()与 dropDuplicates()

> 原文：<https://towardsdatascience.com/distinct-vs-dropduplicates-in-spark-3e28af1f793c?source=collection_archive---------2----------------------->

## Spark 中 distinct()和 dropDuplicates()有什么区别？

![](img/5b5b466e71f3c51fb9dac6e0d9344a50.png)

由[朱莉安娜](https://unsplash.com/@julianasnaps)在[unsplash.com](https://unsplash.com/photos/8Xt1O_SalCE)拍摄的照片

Spark 数据帧 API 提供了两个函数，可以用来从给定的数据帧中删除重复数据。这些是`distinct()`和`dropDuplicates()`。尽管这两种方法做的工作差不多，但它们实际上有一个区别，这在某些用例中非常重要。

在本文中，我们将探讨这两个函数是如何工作的，以及它们的主要区别是什么。此外，我们将讨论何时使用其中一个。

请注意，我们将用来探索这些方法的示例是使用 Python API 构建的。然而，它们相当简单，因此也可以通过 Scala API 使用(尽管提供的一些链接会引用以前的 API)。

## distinct()方法

> `[**distinct()**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame.distinct)`
> 
> 返回一个新的`[**DataFrame**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame)`，其中包含此`[**DataFrame**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame)`中的不同行。

`distinct()`将返回数据帧的不同行。例如，考虑下面的数据帧

```
>>> df.show()
+---+------+---+                                                                
| id|  name|age|
+---+------+---+
|  1|Andrew| 25|
|  1|Andrew| 25|
|  1|Andrew| 26|
|  2| Maria| 30|
+---+------+---+
```

该方法不采用参数，因此在删除重复项时会考虑所有列:

```
>>> df.distinct().show()

+---+------+---+
| id|  name|age|
+---+------+---+
|  1|Andrew| 26|
|  2| Maria| 30|
|  1|Andrew| 25|
+---+------+---+
```

现在，如果您在删除重复项时只需要考虑列的子集，那么您首先必须在调用`distinct()`之前选择一个列，如下所示。

```
>>> df.select(['id', 'name']).distinct().show()
+---+------+
| id|  name|
+---+------+
|  2| Maria|
|  1|Andrew|
+---+------+
```

这意味着返回的数据帧将只包含用于消除重复的列的子集。如果是这样的话，那么大概`distinct()`就不行了。

## dropDuplicates()方法

> `[**dropDuplicates(subset=None)**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame.dropDuplicates)`
> 
> 返回一个删除了重复行的新的`[**DataFrame**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame)`，可选地只考虑某些列。
> 
> 对于静态批处理`[**DataFrame**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame)`，它只是删除重复的行。对于一个流`[**DataFrame**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame)`，它将跨触发器保存所有数据作为中间状态，以删除重复的行。您可以使用`[**withWatermark()**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame.withWatermark)`来限制重复数据的延迟时间，系统将相应地限制状态。此外，将丢弃比水位线更早的数据，以避免任何重复的可能性。
> 
> `[**drop_duplicates()**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame.drop_duplicates)`是`[**dropDuplicates()**](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=distinct#pyspark.sql.DataFrame.dropDuplicates)`的别名。

现在`dropDuplicates()`将删除在一组指定的列(如果提供的话)上检测到的重复项，但是与`distinct()`相反，它将返回原始数据帧的所有列。例如，如果您想通过考虑所有列来删除重复项，您可以运行以下命令

```
>>> df.dropDuplicates().show()+---+------+---+
| id|  name|age|
+---+------+---+
|  1|Andrew| 26|
|  2| Maria| 30|
|  1|Andrew| 25|
+---+------+---+
```

因此，如果您想删除列子集上的重复项，但同时又想保留原始结构的所有列，那么可以使用`dropDuplicates()`。

```
df.dropDuplicates(['id', 'name']).show()+---+------+---+
| id|  name|age|
+---+------+---+
|  2| Maria| 30|
|  1|Andrew| 25|
+---+------+---+
```

## 结论

在本文中，我们探索了 Spark DataFrame API 的两个有用的函数，即`distinct()`和`dropDuplicates()`方法。这两种方法都可以用来消除 Spark 数据帧中的重复行，但是，它们的不同之处在于，`distinct()`不接受任何参数，而`dropDuplicates()`在删除重复记录时可以考虑列的子集。

这意味着当一个人想通过只考虑列的子集来删除重复项，但同时又要返回原始数据帧的所有列时，`dropDuplicates()`是一个更合适的选择。