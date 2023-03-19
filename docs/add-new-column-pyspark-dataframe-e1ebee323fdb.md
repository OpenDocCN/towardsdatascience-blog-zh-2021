# 如何向 PySpark 数据框架添加新列

> 原文：<https://towardsdatascience.com/add-new-column-pyspark-dataframe-e1ebee323fdb?source=collection_archive---------10----------------------->

## 探索向现有 Spark 数据框架添加新列的多种方法

![](img/8026ff7ee60d19b843c27ecbbf534c36.png)

[Adrian Trinkaus](https://unsplash.com/@adrian_trinkaus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/column?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

向 PySpark 数据帧添加新列可能是您在日常工作中需要执行的最常见操作之一。

在今天的简短指南中，我们将讨论如何以多种不同的方式做到这一点。具体来说，我们将探索如何添加新列并填充它们

*   带文字
*   通过转换现有列
*   使用联接
*   使用函数或 UDF

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示我们感兴趣的概念

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
        (3, False, None, 3.0),
        (4, False, 'd', None),
    ],
    ['colA', 'colB', 'colC', 'colD']
)df.show()
*+----+-----+----+----+
|colA| colB|colC|colD|
+----+-----+----+----+
|   1| true|   a| 1.0|
|   2| true|   b| 2.0|
|   3|false|null| 3.0|
|   4|false|   d|null|
+----+-----+----+----+*
```

## 使用文本添加新列

假设您想要添加一个包含文字的新列，您可以利用用于创建文字列的`[pyspark.sql.functions.lit](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.functions.lit.html)`函数。

例如，下面的命令将在每行中添加一个名为`colE`的新列，其中包含`100`的值。

```
**df.withColumn('colE', lit(100))**df.show()
*+----+-----+----+----+----+
|colA| colB|colC|colD|colE|
+----+-----+----+----+----+
|   1| true|   a| 1.0| 100|
|   2| true|   b| 2.0| 100|
|   3|false|null| 3.0| 100|
|   4|false|   d|null| 100|
+----+-----+----+----+----+*
```

注意，您必须使用`lit`函数，因为`withColumn`的第二个参数必须是类型`Column`。

现在，如果您想添加一个包含更复杂数据结构(如数组)的列，可以如下所示进行操作:

```
from pyspark.sql.functions import lit, array**df = df.withColumn('colE', array(lit(100), lit(200), lit(300)))**df.show()
*+----+-----+----+----+---------------+
|colA| colB|colC|colD|           colE|
+----+-----+----+----+---------------+
|   1| true|   a| 1.0|[100, 200, 300]|
|   2| true|   b| 2.0|[100, 200, 300]|
|   3|false|null| 3.0|[100, 200, 300]|
|   4|false|   d|null|[100, 200, 300]|
+----+-----+----+----+---------------+*
```

## 通过转换现有列来添加列

如果你想基于一个现有的列创建一个新的列，那么你应该在`withColumn`方法中指定想要的操作。

例如，如果您想通过将现有列的值(比如说`colD`)乘以一个常数(比如说`2`)来创建一个新列，那么下面的方法就可以做到:

```
from pyspark.sql.functions import col**df = df.withColumn('colE', col('colD') * 2)**df.show()
*+----+-----+----+----+----+
|colA| colB|colC|colD|colE|
+----+-----+----+----+----+
|   1| true|   a| 1.0| 2.0|
|   2| true|   b| 2.0| 4.0|
|   3|false|null| 3.0| 6.0|
|   4|false|   d|null|null|
+----+-----+----+----+----+*
```

## 使用联接添加新列

或者，我们仍然可以创建一个新的数据帧，并将其连接回原来的数据帧。首先，需要创建一个新的数据帧，其中包含要添加的新列以及要在两个数据帧上连接的键

```
new_col = spark_session.createDataFrame(
    [(1, 'hello'), (2, 'hi'), (3, 'hey'), (4, 'howdy')],
    ('key', 'colE')
)new_col.show()
*+---+-----+
|key| colE|
+---+-----+
|  1|hello|
|  2|   hi|
|  3|  hey|
|  4|howdy|
+---+-----+*
```

最后进行连接:

```
from pyspark.sql.functions import col**df = df \
    .join(new_col, col('colA') == col('key'), 'leftouter') \
    .drop('key')**df.show()
*+----+-----+----+----+-----+
|colA| colB|colC|colD| colE|
+----+-----+----+----+-----+
|   1| true|   a| 1.0|hello|
|   2| true|   b| 2.0|   hi|
|   3|false|null| 3.0|  hey|
|   4|false|   d|null|howdy|
+----+-----+----+----+-----+*
```

## 使用函数或 UDF 添加列

另一种可能性是使用返回`Column`的函数，并将该函数传递给`withColumn`。例如，您可以使用内置的`[pyspark.sql.functions.rand](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.functions.rand.html)`函数创建一个包含随机数的列，如下所示:

```
from pyspark.sql.functions import rand**df = df.withColumn('colE', rand())**df.show()
*+----+-----+----+----+--------------------+
|colA| colB|colC|colD|                colE|
+----+-----+----+----+--------------------+
|   1| true|   a| 1.0|0.026110187082684866|
|   2| true|   b| 2.0|0.046264104329627576|
|   3|false|null| 3.0|  0.7892572670252188|
|   4|false|   d|null|  0.7963792998818318|
+----+-----+----+----+--------------------+*
```

## 最后的想法

在今天的简短指南中，我们讨论了如何向现有的 PySpark 数据帧中插入额外的列。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒体上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

[](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [## 加快 PySpark 和 Pandas 数据帧之间的转换

### 将大火花数据帧转换为熊猫时节省时间

towardsdatascience.com](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [](/dynamic-typing-in-python-307f7c22b24e) [## Python 中的动态类型

### 探索 Python 中对象引用的工作方式

towardsdatascience.com](/dynamic-typing-in-python-307f7c22b24e) [](/mastering-indexing-and-slicing-in-python-443e23457125) [## 掌握 Python 中的索引和切片

### 深入研究有序集合的索引和切片

towardsdatascience.com](/mastering-indexing-and-slicing-in-python-443e23457125)