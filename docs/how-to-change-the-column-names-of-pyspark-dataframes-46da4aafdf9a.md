# 如何更改 PySpark 数据帧的列名

> 原文：<https://towardsdatascience.com/how-to-change-the-column-names-of-pyspark-dataframes-46da4aafdf9a?source=collection_archive---------7----------------------->

## 讨论 PySpark 数据框架中改变列名的 5 种方法

![](img/84dc6a99f611e912c5f06b0549c7b606.png)

照片由 [Linus Nylund](https://unsplash.com/@dreamsoftheoceans?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

在今天的简短指南中，我们将讨论在 Spark 数据帧中更改列名的 4 种方法。具体来说，我们将使用以下工具来探索如何做到这一点:

*   `selectExpr()`方法
*   `withColumnRenamed()`方法
*   `toDF()`法
*   别名
*   Spark 会话和 Spark SQL

并一次重命名一列或多列。

首先，让我们创建一个示例 PySpark 数据帧，我们将在本指南中引用它来演示一些概念。

```
>>> from pyspark.sql import SparkSession# Create an instance of spark session
>>> spark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate()>>> df = spark_session.createDataFrame(
    [
       (1, 'a', True, 1.0),
       (2, 'b', False, 2.0),
       (3, 'c', False, 3.0),
       (4, 'd', True, 4.0),
    ],
    ['colA', 'colB', 'colC', 'colD']
)>>> df.show()
*+----+----+-----+----+                                                          
|colA|colB| colC|colD|
+----+----+-----+----+
|   1|   a| true| 1.0|
|   2|   b|false| 2.0|
|   3|   c|false| 3.0|
|   4|   d| true| 4.0|
+----+----+-----+----+*>>> df.printSchema()
*root
 |-- colA: long (nullable = true)
 |-- colB: string (nullable = true)
 |-- colC: boolean (nullable = true)
 |-- colD: double (nullable = true)*
```

## 使用 selectExpr()

第一个选项是`[pyspark.sql.DataFrame.selectExpr()](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.DataFrame.selectExpr.html)`方法，它是接受 SQL 表达式的`[select()](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.DataFrame.select.html#pyspark.sql.DataFrame.select)`方法的变体。

```
**>>> df =  df.selectExpr(
    'colA AS A',
    'colB AS B',
    'colC AS C',
    'colD AS D',
)**>>> df.show()
*+---+---+-----+---+
|  A|  B|    C|  D|
+---+---+-----+---+
|  1|  a| true|1.0|
|  2|  b|false|2.0|
|  3|  c|false|3.0|
|  4|  d| true|4.0|
+---+---+-----+---+*
```

但是请注意，当您需要重命名大多数列，并且还必须处理相对较少的列时，这种方法最适合。

此时，您可能还想回忆一下 Spark 中`select()`和`selectExpr()`的区别:

[](/select-vs-selectexpr-in-spark-412a59416615) [## Spark 中的 select()与 selectExpr()

### 讨论 Spark 中 select()和 selectExpr()方法的区别

towardsdatascience.com](/select-vs-selectexpr-in-spark-412a59416615) 

## 使用 withColumnRenamed()

重命名 PySpark 数据帧列的第二个选项是`[pyspark.sql.DataFrame.withColumnRenamed()](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.withColumnRenamed.html)`。此方法通过重命名现有列来返回新的 DataFrame。

```
**>>> df = df.withColumnRenamed('colA', 'A')**>>> df.show()
*+---+----+-----+----+
|  A|colB| colC|colD|
+---+----+-----+----+
|  1|   a| true| 1.0|
|  2|   b|false| 2.0|
|  3|   c|false| 3.0|
|  4|   d| true| 4.0|
+---+----+-----+----+*
```

当您需要一次重命名一列时，这种方法最合适。如果您需要一次性重命名多个列，那么本文中讨论的其他方法会更有帮助。

## 使用 toDF()方法

方法使用新的指定列名返回一个新的 DataFrame。

```
**>>> df = df.toDF('A', 'colB', 'C', 'colD')**>>> df.show()
*+---+----+-----+----+
|  A|colB|    C|colD|
+---+----+-----+----+
|  1|   a| true| 1.0|
|  2|   b|false| 2.0|
|  3|   c|false| 3.0|
|  4|   d| true| 4.0|
+---+----+-----+----+*
```

当您需要**同时重命名多个列**时，这种方法很有用。

请注意，如果您有一个包含新列名的列表，那么下面的代码也应该可以完成这个任务:

```
>>> new_col_names = ['A', 'colB', 'C', 'colD']
>>> **df = df.toDF(*new_col_names)**
```

## 使用别名

我们的另一个选择是使用`[alias](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.alias.html)`方法，返回一个带有别名集的新数据帧。

```
>>> from pyspark.sql.functions import col**>>> df = df.select(
** **col('colA').alias('A'), 
    col('colB'),
    col('colC').alias('C'),
    col('colD')** **)**>>> df.show()
*+---+----+-----+----+
|  A|colB|    C|colD|
+---+----+-----+----+
|  1|   a| true| 1.0|
|  2|   b|false| 2.0|
|  3|   c|false| 3.0|
|  4|   d| true| 4.0|
+---+----+-----+----+*
```

同样，当需要重命名多个列并且不需要处理大量列时，应该使用这种方法，否则这会变得非常冗长。

## 使用 Spark SQL

最后，您可以使用 Spark SQL 对存储为表的数据帧使用传统的 SQL 语法来重命名列。举个例子，

```
>>> df.createOrReplaceTempView('test_table')
>>> df = spark_session.sql(
    'SELECT colA AS A, colB, colC AS C, colD FROM test_table'
)>>> df.show()
*+---+----+-----+----+
|  A|colB|    C|colD|
+---+----+-----+----+
|  1|   a| true| 1.0|
|  2|   b|false| 2.0|
|  3|   c|false| 3.0|
|  4|   d| true| 4.0|
+---+----+-----+----+*
```

## 最后的想法

在今天的简短指南中，我们讨论了如何以多种不同的方式重命名 PySpark 数据帧的列。根据您是否需要重命名一个或多个列，您必须选择最适合您的特定用例的方法。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒介上的每一个故事。你的会员费直接支持我和你看的其他作家。**

## 你可能也喜欢

[](/sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a) [## spark session vs spark context vs SQLContext vs hive context

### SparkSession、SparkContext HiveContext 和 SQLContext 有什么区别？

towardsdatascience.com](/sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a) [](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [## 加快 PySpark 和 Pandas 数据帧之间的转换

### 将大火花数据帧转换为熊猫时节省时间

towardsdatascience.com](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [](/distinct-vs-dropduplicates-in-spark-3e28af1f793c) [## Spark 中的 distinct()与 dropDuplicates()

### Spark 中 distinct()和 dropDuplicates()有什么区别？

towardsdatascience.com](/distinct-vs-dropduplicates-in-spark-3e28af1f793c)