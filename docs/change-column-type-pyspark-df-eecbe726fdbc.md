# 如何更改 PySpark 数据框架中的列类型

> 原文：<https://towardsdatascience.com/change-column-type-pyspark-df-eecbe726fdbc?source=collection_archive---------7----------------------->

## 讨论如何转换 PySpark 数据帧中列的数据类型

![](img/5d45e4d341c1e68e9120103248848572.png)

Javier Allegue Barros 在 [Unsplash](https://unsplash.com/s/photos/change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

PySpark 中一个相当常见的操作是类型转换，当我们需要更改数据帧中特定列的数据类型时，通常需要进行类型转换。例如，这很常见(而且是一种不好的做法！)将日期时间存储为字符串，甚至将整数和双精度数存储为`StringType`。

在今天的简短指南中，我们将探讨如何在 PySpark 中更改某些 DataFrame 列的列类型。具体来说，我们将讨论如何使用

*   `cast()`功能
*   `selectExpr()`功能
*   Spark SQL

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示一些概念。

```
from pyspark.sql import SparkSession# Create an instance of spark session
spark_session = SparkSession.builder \
    .master('local[1]') \
    .appName('Example') \
    .getOrCreate()df = spark_session.createDataFrame(
    [
        (1, '10-01-2020', '1.0', '100'),
        (2, '14-02-2021', '2.0', '200'),
        (3, '15-06-2019', '3.0', '300'),
        (4, '12-12-2020', '4.0', '400'),
        (5, '01-09-2019', '5.0', '500'),
    ],
    ['colA', 'colB', 'colC', 'colD']
)

df.show()
*+----+----------+----+----+
|colA|      colB|colC|colD|
+----+----------+----+----+
|   1|10-01-2020| 1.0| 100|
|   2|14-02-2021| 2.0| 200|
|   3|15-06-2019| 3.0| 300|
|   4|12-12-2020| 4.0| 400|
|   5|01-09-2019| 5.0| 500|
+----+----------+----+----+*df.printSchema()
*root
 |-- colA: long (nullable = true)
 |-- colB: string (nullable = true)
 |-- colC: string (nullable = true)
 |-- colD: string (nullable = true)*
```

在接下来的章节中，我们将展示如何将`colB`、`colC`和`colD`列的类型分别更改为`DateType`、`DoubleType`和`IntegerType`。

## 使用 cast()函数

转换数据类型的第一个选项是将输入列转换为指定数据类型的`[pyspark.sql.Column.cast()](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.Column.cast.html)`函数。

```
from datetime import datetime
from pyspark.sql.functions import col, udf
from pyspark.sql.types import DoubleType, IntegerType, DateType # UDF to process the date column
func = udf(lambda x: datetime.strptime(x, '%d-%m-%Y'), DateType())**df = df \
    .withColumn('colB', func(col('colB'))) \
    .withColumn('colC', col('colC').cast(DoubleType())) \
    .withColumn('colD', col('colD').cast(IntegerType()))** df.show()
*+----+----------+----+----+
|colA|      colB|colC|colD|
+----+----------+----+----+
|   1|2020-01-10| 1.0| 100|
|   2|2021-02-14| 2.0| 200|
|   3|2019-06-15| 3.0| 300|
|   4|2020-12-12| 4.0| 400|
|   5|2019-09-01| 5.0| 500|
+----+----------+----+----+*df.printSchema()
*root
 |-- colA: long (nullable = true)
* ***|-- colB: date (nullable = true)
 |-- colC: double (nullable = true)
 |-- colD: integer (nullable = true)***
```

注意，为了将字符串转换成`DateType`，我们需要指定一个 UDF 来处理字符串日期的确切格式。

## 使用 selectExpr()函数

或者，您可以通过指定相应的 SQL 表达式来使用`[pyspark.sql.DataFrame.selectExpr](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.DataFrame.selectExpr.html)`函数，该表达式可以转换所需列的数据类型，如下所示。

```
**df = df.selectExpr(
    'colA',
    'to_date(colB, \'dd-MM-yyyy\') colB',
    'cast(colC as double) colC',
    'cast(colD as int) colD',
)**df.show()
*+----+----------+----+----+
|colA|      colB|colC|colD|
+----+----------+----+----+
|   1|2020-01-10| 1.0| 100|
|   2|2021-02-14| 2.0| 200|
|   3|2019-06-15| 3.0| 300|
|   4|2020-12-12| 4.0| 400|
|   5|2019-09-01| 5.0| 500|
+----+----------+----+----+*df.printSchema()
*root
 |-- colA: long (nullable = true)
* ***|-- colB: date (nullable = true)
 |-- colC: double (nullable = true)
 |-- colD: integer (nullable = true)***
```

## 使用 Spark SQL

最后，您甚至可以使用 Spark SQL，以类似于我们使用`selectExpr`函数的方式来转换所需的列。

```
# First we need to register the DF as a global temporary view
df.createGlobalTempView("df")**df = spark_session.sql(
    """
    SELECT 
        colA,
        to_date(colB, 'dd-MM-yyyy') colB,
        cast(colC as double) colC,
        cast(colD as int) colD
    FROM global_temp.df
    """
)**df.show()
*+----+----------+----+----+
|colA|      colB|colC|colD|
+----+----------+----+----+
|   1|2020-01-10| 1.0| 100|
|   2|2021-02-14| 2.0| 200|
|   3|2019-06-15| 3.0| 300|
|   4|2020-12-12| 4.0| 400|
|   5|2019-09-01| 5.0| 500|
+----+----------+----+----+*df.printSchema()
*root
 |-- colA: long (nullable = true)
* ***|-- colB: date (nullable = true)
 |-- colC: double (nullable = true)
 |-- colD: integer (nullable = true)***
```

## 最后的想法

在今天的简短指南中，我们讨论了在 PySpark 中更改 DataFrame 列类型的几种不同方法。具体来说，我们探索了如何将`withColumn()`函数与`cast()`结合使用，以及如何使用更多类似 SQL 的方法，如`selectExpr()`或 Spark SQL。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒体上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

</sparksession-vs-sparkcontext-vs-sqlcontext-vs-hivecontext-741d50c9486a>  </how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  </apache-spark-3-0-the-five-most-exciting-new-features-99c771a1f512> 