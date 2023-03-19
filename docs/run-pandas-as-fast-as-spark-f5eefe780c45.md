# 跑熊猫像火花一样快

> 原文：<https://towardsdatascience.com/run-pandas-as-fast-as-spark-f5eefe780c45?source=collection_archive---------1----------------------->

## 为什么 Spark 上的熊猫 API 完全改变了游戏规则

![](img/f98a4256583d756dc9687b1bc4a37855.png)

克莱顿·霍尔姆斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

就是这样。它出来了。Spark 现在有一个熊猫 API。

似乎每次你想使用数据框架时，你都必须打开一个放着所有工具的杂乱抽屉，并仔细寻找合适的工具。

如果你处理结构化数据，[你需要 SQL](https://www.dataquest.io/blog/why-sql-is-the-most-important-language-to-learn/) 。熊猫也总是在那里。Spark 是大数据不可或缺的。工具箱里有满足各种需求的工具。但是你再也不需要工具箱了，因为 Spark 已经成为了终极的瑞士军刀。

这一切都始于 2019 Spark + AI 峰会。考拉，一个开源项目，使熊猫能够在 Spark 上使用，已经启动。起初，它只涵盖了熊猫功能的一小部分，但它逐渐成长。两年过去了，现在，在新的 Spark 3.2 版本中，考拉已经并入 PySpark。结果很棒。

Spark 现在集成了 Pandas API，所以你可以在 Spark 上运行 Pandas。你只需要修改一行代码:

```
import pyspark.pandas as ps
```

是不是很棒？

由于这一点，我们可以获得广泛的好处:

*   如果你用熊猫但是不熟悉 Spark，马上就可以用 Spark 工作，没有学习曲线。
*   你可以有一个单一的代码库来处理所有事情:小数据和大数据。单机和分布式机器。
*   你可以更快地运行你的熊猫代码。

这最后一点尤其值得注意。

一方面，您可以在 Pandas 中将分布式计算应用到您的代码中。但好处不止于此。多亏了 Spark 引擎，你的代码会更快[甚至在单机上](https://databricks.com/blog/2021/10/04/pandas-api-on-upcoming-apache-spark-3-2.html#attachment_165784)！

如果你想知道，是的，看起来熊猫星火也比 Dask 快。

我对这一突破感到非常兴奋，那么，我们为什么不进入正题呢？让我们用 Spark 上的熊猫 API 做一些代码吧！

# 在熊猫、熊猫火花和火花之间切换

我们需要知道的第一件事是我们到底在做什么。当与熊猫一起工作时，我们使用类`pandas.core.frame.DataFrame`。当在 Spark 中使用 pandas API 时，我们使用类`pyspark.pandas.frame.DataFrame`。两者相似，但不相同。主要区别是前者在一台机器上，而后者是分布式的。

我们可以创建一个熊猫在火花上的数据帧，并将其转换为熊猫，反之亦然:

```
# import Pandas-on-Spark
import pyspark.pandas as ps# Create a DataFrame with Pandas-on-Spark
ps_df = ps.DataFrame(range(10))# Convert a Pandas-on-Spark Dataframe into a Pandas Dataframe
pd_df = ps_df.to_pandas()# Convert a Pandas Dataframe into a Pandas-on-Spark Dataframe
ps_df = ps.from_pandas(pd_df)
```

请注意，如果您使用多台机器，当将 Pandas-on-Spark 数据帧转换为 Pandas 数据帧时，数据会从多台机器传输到一台机器，反之亦然(参见 [PySpark 指南](https://spark.apache.org/docs/latest/api/python/user_guide/pandas_on_spark/pandas_pyspark.html))。

我们还可以将 Pandas-on-Spark 数据帧转换为 Spark 数据帧，反之亦然:

```
# Create a DataFrame with Pandas-on-Spark
ps_df = ps.DataFrame(range(10))# Convert a Pandas-on-Spark Dataframe into a Spark Dataframe
spark_df = ps_df.to_spark()# Convert a Spark Dataframe into a Pandas-on-Spark Dataframe
ps_df_new = spark_df.to_pandas_on_spark()
```

## 数据类型会发生什么？

当使用 Pandas-on-Spark 和 Pandas 时，数据类型基本相同。当将 Pandas-on-Spark 数据帧转换为 Spark 数据帧时，数据类型会自动转换为适当的类型(参见 [PySpark 指南](https://spark.apache.org/docs/latest/api/python/user_guide/pandas_on_spark/types.html))

# **用熊猫火花复制火花功能**

本节的目的是提供一个备忘单，其中包含管理 Spark 中的数据帧以及 Pandas-on-Spark 中类似数据帧的最常用函数。注意，Pandas-on-Spark 和 Pandas 在语法上的唯一区别只是`import pyspark.pandas as ps`行。

您将看到，即使您不熟悉 Spark，也可以通过 Pandas API 轻松使用它。

## **导入库**

```
# For running Spark
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("Spark").getOrCreate()# For running Pandas on top of Spark
import pyspark.pandas as ps
```

## **读取数据**

让我们以老狗虹膜数据集为例。

```
# SPARK
sdf = spark.read.options(inferSchema='True',
              header='True').csv('iris.csv')# PANDAS-ON-SPARK
pdf = ps.read_csv('iris.csv')
```

## **选择**

```
# SPARK
sdf.select("sepal_length","sepal_width").show()# PANDAS-ON-SPARK
pdf[["sepal_length","sepal_width"]].head()
```

## **删除列**

```
# SPARK
sdf.drop('sepal_length').show()# PANDAS-ON-SPARK
pdf.drop('sepal_length').head()
```

## 删除重复项

```
# SPARK
sdf.dropDuplicates(["sepal_length","sepal_width"]).show()# PANDAS-ON-SPARK
pdf[["sepal_length", "sepal_width"]].drop_duplicates()
```

## **滤镜**

```
# SPARK
sdf.filter( (sdf.flower_type == "Iris-setosa") & (sdf.petal_length > 1.5) ).show()# PANDAS-ON-SPARK
pdf.loc[ (pdf.flower_type == "Iris-setosa") & (pdf.petal_length > 1.5) ].head()
```

## **计数**

```
# SPARK
sdf.filter(sdf.flower_type == "Iris-virginica").count()# PANDAS-ON-SPARK
pdf.loc[pdf.flower_type == "Iris-virginica"].count()
```

## **截然不同的**

```
# SPARK
sdf.select("flower_type").distinct().show()# PANDAS-ON-SPARK
pdf["flower_type"].unique()
```

## **排序**

```
# SPARK
sdf.sort("sepal_length", "sepal_width").show()# PANDAS-ON-SPARK
pdf.sort_values(["sepal_length", "sepal_width"]).head()
```

## **分组依据**

```
# SPARK
sdf.groupBy("flower_type").count().show()# PANDAS-ON-SPARK
pdf.groupby("flower_type").count()
```

## **更换**

```
# SPARK
sdf.replace("Iris-setosa", "setosa").show()# PANDAS-ON-SPARK
pdf.replace("Iris-setosa", "setosa").head()
```

## **加入**

```
# SPARK
sdf.union(sdf)# PANDAS-ON-SPARK
pdf.append(pdf)
```

# 结论

从现在开始，你将可以在 Spark 中使用熊猫。这导致了 Pandas 速度的提高，迁移到 Spark 时学习曲线的减少，以及单机计算和分布式计算在同一代码库中的合并。

我想留给读者几个问题:

*   你认为 Spark 会成为管理数据框架的终极瑞士军刀吗？
*   熊猫会像 Dask 或者 Vaex 一样杀光其他库吗？
*   熊猫用户会逐渐迁移到 Spark 吗？
*   我们将来会看到`import pandas as pd`吗？

# 参考

*   [Spark 用户指南:Spark 上的熊猫 API](https://spark.apache.org/docs/latest/api/python/user_guide/pandas_on_spark/index.html)
*   [即将发布的 Apache Spark 3.2 的熊猫 API](https://databricks.com/blog/2021/10/04/pandas-api-on-upcoming-apache-spark-3-2.html)