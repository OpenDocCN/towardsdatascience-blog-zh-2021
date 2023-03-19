# 在 PySpark 中选择多个列

> 原文：<https://towardsdatascience.com/selecting-multiple-columns-in-pyspark-d1aac072fcc0?source=collection_archive---------9----------------------->

## 讨论如何通过列名、索引或使用正则表达式从 PySpark 数据帧中选择多个列

![](img/6258e3a2b0690b138adaee6de2062e6e.png)

照片由 [Luana Azevedo](https://unsplash.com/@azevdoluana?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/select?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

在使用 Spark 时，我们通常需要处理大量的行和列，因此，有时我们只能处理列的一小部分。

在今天的简短指南中，我们将探索从 PySpark 数据帧中选择列的不同方法。具体来说，我们将讨论如何选择多个列

*   按列名
*   按索引
*   使用正则表达式

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示一些概念。

```
from pyspark.sql import SparkSession # Create an instance of spark session
spark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate()# Create an example DataFrame
df = spark_session.createDataFrame(
    [
       (1, 'a', True, 1.0, 5),
       (2, 'b', False, 2.0, None),
       (3, 'c', False, 3.0, 4),
       (4, 'd', True, 4.0, 3),
    ],
    ['colA', 'colB', 'colC', 'colD', 'E']
) df.show()
*+----+----+-----+----+----+
|colA|colB| colC|colD|   E|
+----+----+-----+----+----+
|   1|   a| true| 1.0|   5|
|   2|   b|false| 2.0|null|
|   3|   c|false| 3.0|   4|
|   4|   d| true| 4.0|   3|
+----+----+-----+----+----+*
```

## 按名称选择多个列

为了从一个现有的 PySpark 数据帧中选择多个列，您可以简单地指定您希望通过`[pyspark.sql.DataFrame.select](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.select.html)`方法检索的列名。举个例子，

```
**df.select('colA', 'colC').show()***+----+-----+
|colA| colC|
+----+-----+
|   1| true|
|   2|false|
|   3|false|
|   4| true|
+----+-----+*
```

或者，如果您希望检索的列存储在列表中，您可以使用以下符号:

```
col_names = ['colA', 'colC']**df.select(*col_names).show()***+----+-----+
|colA| colC|
+----+-----+
|   1| true|
|   2|false|
|   3|false|
|   4| true|
+----+-----+*
```

## 通过索引选择多个列

现在，如果您想基于它们的索引选择列，那么您可以简单地从返回列名列表的`df.columns`中截取结果。例如，为了检索前三列，下面的表达式应该可以做到:

```
**df.select(df.columns[:3]).show()***+----+----+-----+
|colA|colB| colC|
+----+----+-----+
|   1|   a| true|
|   2|   b|false|
|   3|   c|false|
|   4|   d| true|
+----+----+-----+*
```

或者，如果你只想获取第二列和第三列，那么`df.columns[1:3]`就可以了:

```
**df.select(df.columns[1:3]).show()***+----+-----+
|colB| colC|
+----+-----+
|   a| true|
|   b|false|
|   c|false|
|   d| true|
+----+-----+*
```

## 使用正则表达式选择多个列

最后，为了选择多个匹配特定正则表达式的列，您可以使用`[pyspark.sql.DataFrame.colRegex](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.colRegex.html)`方法。例如，为了获取所有以`col`开头或包含【】的列，下面的代码就可以做到:

```
**df.select(df.colRegex("`(col)+?.+`")).show()***+----+----+-----+----+
|colA|colB| colC|colD|
+----+----+-----+----+
|   1|   a| true| 1.0|
|   2|   b|false| 2.0|
|   3|   c|false| 3.0|
|   4|   d| true| 4.0|
+----+----+-----+----+*
```

类似地，我们可以使用下面的正则表达式来选择除`colA`之外的所有列:

```
**df.select(df.colRegex("`(colA)?+.+`")).show()***+----+-----+----+----+
|colB| colC|colD|   E|
+----+-----+----+----+
|   a| true| 1.0|   5|
|   b|false| 2.0|null|
|   c|false| 3.0|   4|
|   d| true| 4.0|   3|
+----+-----+----+----+*
```

## 最后的想法

在今天的简短指南中，我们讨论了如何在 PySpark 数据框架中执行列选择。我们探讨了如何通过指定列名或索引来选择多个列。此外，我们还看到了如何使用正则表达式来执行列选择。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a>  </how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3> 