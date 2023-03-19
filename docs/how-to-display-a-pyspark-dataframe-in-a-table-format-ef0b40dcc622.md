# 如何以表格格式显示 PySpark 数据帧

> 原文：<https://towardsdatascience.com/how-to-display-a-pyspark-dataframe-in-a-table-format-ef0b40dcc622?source=collection_archive---------12----------------------->

## 如何打印巨大的 PySpark 数据帧

![](img/6e6ba08a4b5b6f3894a041e1f045ce78.png)

unsplash.com[上](https://unsplash.com/photos/Wpnoqo2plFA)[米卡·鲍梅斯特](https://unsplash.com/@mbaumi)的照片

在大数据时代，由数百甚至数千列组成的数据帧非常常见。在这种情况下，即使将它们打印出来有时也会很棘手，因为您需要以某种方式确保数据以清晰而有效的方式呈现。

在本文中，我将探索以表格格式显示 PySpark 数据帧的三种基本方法。对于每种情况，我还将讨论何时使用或避免它，这取决于您必须处理的数据的形式。

## 打印 PySpark 数据帧

首先，让我们以下面的最小 pyspark 数据帧为例:

```
spark_df = sqlContext.createDataFrame(
    [
        (1, "Mark", "Brown"), 
        (2, "Tom", "Anderson"), 
        (3, "Joshua", "Peterson")
    ], 
    ('id', 'firstName', 'lastName')
)
```

为了打印 PySpark 数据帧，可以使用的最明显的方法是`show()`方法:

```
>>> df.show()
+---+---------+--------+
| id|firstName|lastName|
+---+---------+--------+
|  1|     Mark|   Brown|
|  2|      Tom|Anderson|
|  3|   Joshua|Peterson|
+---+---------+--------+
```

默认情况下，只打印出前 20 行。如果您想显示更多的行，那么您可以简单地传递参数`n`，即`show(n=100)`。

## 垂直打印 PySpark 数据帧

现在让我们考虑另一个例子，其中我们的数据帧有许多列:

```
spark_df = sqlContext.createDataFrame(
    [
        (
            1, 'Mark', 'Brown', 25, 'student', 'E15', 
            'London', None, 'United Kingdom'
        ), 
        (
            2, 'Tom', 'Anderson', 30, 'Developer', 'SW1', 
            'London', None, 'United Kingdom'
        ), 
        (
            3, 'Joshua', 'Peterson', 33, 'Social Media Manager', 
            '99501', 'Juneau', 'Alaska', 'USA'
        ),
    ], 
    (
        'id', 'firstName', 'lastName', 'age', 'occupation',
        'postcode', 'city', 'state', 'country',
    )
)
```

现在，如果我们试图打印出数据帧(取决于屏幕的大小)，输出可能会非常混乱:

```
>>> spark_df.show()+---+---------+--------+---+--------------------+--------+------+------+--------------+| id|firstName|lastName|age|          occupation|postcode|  city| state|       country|+---+---------+--------+---+--------------------+--------+------+------+--------------+|  1|     Mark|   Brown| 25|             student|     E15|London|  null|United Kingdom||  2|      Tom|Anderson| 30|           Developer|     SW1|London|  null|United Kingdom||  3|   Joshua|Peterson| 33|Social Media Manager|   99501|Juneau|Alaska|           USA|+---+---------+--------+---+--------------------+--------+------+------+--------------+
```

假设在处理真实世界的数据时，由于数据帧的大小，上述行为经常发生，我们需要想出一种解决方案，以可读的方式正确显示我们的数据。此外，您应该避免对其他用户的屏幕大小做出假设(例如，当您想要在日志中包含此输出时)，因此，您需要确保结果总是一致的，并以相同的方式呈现给所有用户。

典型的解决方法是垂直打印数据帧。为此，我们需要将`vertical`参数传递给`show()`方法:

```
>>> spark_df.show(vertical=True)-RECORD 0--------------------------
id         | 1
firstName  | Mark
lastName   | Brown
age        | 25
occupation | student
postcode   | E15
city       | London
state      | null
country    | United Kingdom-RECORD 1--------------------------
id         | 2
firstName  | Tom
lastName   | Anderson
age        | 30
occupation | Developer
postcode   | SW1
city       | London
state      | null
country    | United Kingdom-RECORD 2--------------------------
id         | 3
firstName  | Joshua
lastName   | Peterson
age        | 33
occupation | Social Media Manager
postcode   | 99501
city       | Juneau
state      | Alaska
country    | USA
```

尽管上面的输出不是表格格式，但有时这是以一致和可读的方式显示数据的唯一方式。

## 将 PySpark 数据帧转换为熊猫

第三种选择是将 pyspark 数据帧转换成 pandas 数据帧，并最终打印出来:

```
>>> pandas_df = spark_df.toPandas()
>>> print(pandas_df)
   id firstName  lastName
0   1      Mark     Brown
1   2       Tom  Anderson
2   3    Joshua  Peterson
```

注意，当您必须处理相当大的数据帧时，不建议使用这种方法，因为 Pandas 需要将所有数据加载到内存中。如果是这种情况，以下配置将优化大 spark 数据帧到 pandas 数据帧的转换:

```
spark.conf.set("spark.sql.execution.arrow.pyspark.enabled", "true")
```

更多关于 PyArrow 优化的细节，当 spark 和 pandas 数据帧相互转换时，你可以参考我下面的文章

</how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  

## 结论

在本文中，我们探索了 PySpark 中一个非常基本的操作。在大多数情况下，垂直打印 PySpark 数据帧是可行的，因为对象的形状通常很大，不适合表格格式。假设大多数用户没有可能适合表格中的大数据框的宽屏也更安全。然而，在你有一些玩具数据的情况下，内置的带有默认参数的`show()`方法将会起作用。