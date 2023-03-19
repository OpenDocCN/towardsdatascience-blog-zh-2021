# 熊猫靠火花奔跑！

> 原文：<https://towardsdatascience.com/pandas-on-spark-current-issues-and-workarounds-dc9ed30840ce?source=collection_archive---------11----------------------->

## 在那里，我参加了一个常规的熊猫项目，并尝试在 Spark pandas 下运行它

![](img/9a6775e2fef21d2e0785e7da5a02cd1a.png)

[Zan](https://unsplash.com/@zanilic?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 安达斯和火花

Pandas 是数据分析和数据科学的关键工具，已经存在了十多年。它是稳定的，经过验证的。但是 pandas 有一个重大的限制，每个数据工程师都会在某个时候碰到——它只能在一台计算机上运行。pandas 的数据大小限制约为 100M 行或 100GB，只有在功能强大、价格昂贵的计算机上才能达到这些限制。

Apache Spark 最近发布了这个问题的解决方案，在 [Spark 3.2](https://spark.apache.org) 中包含了 pyspark.pandas 库。由于 Spark 运行在几乎无限的计算机集群上，它可以处理的数据集大小实际上没有限制。熊猫程序员可以移动他们的代码来激发和消除以前的数据约束。

# 测试

当然，像这样的 API 仿真工作在第一个版本中是不完美的，也不会覆盖所有的目标特性。因此，我在 Spark 中测试了 pandas 的功能，将一些中等复杂的 pandas 代码从 Python 3.8.8 和 pandas 1.2.4 迁移到 Spark 3.2。我在 [Databricks 10.0](https://databricks.com) 中这样做，因为该平台使得创建和管理 Spark 集群变得容易。

我的目标是在 Spark 下运行代码，尽可能少做改动。我移植的 pandas 代码是一对程序——一个是[导入/清理/规范化/连接](https://github.com/ChuckConnell/articles/blob/master/MakeVaccineMortalityFile.py)三个数据集，一个是[分析](https://github.com/ChuckConnell/articles/blob/master/AnalyzeCountyVaccineMortality.py)组合数据。由此产生的 pyspark.pandas 代码是一个 Databricks 笔记本，这里称为 [DBC](https://github.com/ChuckConnell/articles/blob/master/VaxMortality_pyspark_pandas.dbc) 和 [Python](https://github.com/ChuckConnell/articles/blob/master/VaxMortality_pyspark_pandas.py) 。

# 结果

总的来说，pyspark.pandas 工作得很好，产生了正确的结果，无论是在数字上还是在各种图形的视觉上。需要对代码进行一些修改，但没有一处是令人失望的，也没有一处会导致错误或不完整的答案。

本文列出了我发现的妨碍我的代码不加修改地运行的问题，以及每个问题的解决方法。我已经向 Apache-Spark 吉拉报告了这些问题，并附上了这些链接。它们是:

*   版本检查
*   读取输入文件
*   保存结果
*   输入的拉丁 1 编码
*   to_numeric(错误=)
*   。地图()。菲尔娜
*   从字符串中删除后缀
*   调整日期时间值
*   数据帧中一列的直方图
*   直方图标题
*   直方图范围

我使用的计算环境是免费的 [Databricks 社区版](https://community.cloud.databricks.com/) 10.0，其中包括 Spark 3.2。为了清楚起见，我用普通的别名“pd”来称呼普通的熊猫，用“pspd”来称呼 PySpark 熊猫。

## 检查熊猫版本

鉴于 pyspark.pandas 不是独立的软件，除了 pyspark 之外不进行更新，请将第一行中出现的任何内容更改为第二行。

```
#print(pd.__version__)print (sc.version)  # sc = Spark context
```

要跟踪此问题，请参见[https://issues.apache.org/jira/browse/SPARK-37180](https://issues.apache.org/jira/browse/SPARK-37180)。

## 读取输入文件

对于从普通熊猫转向 Spark-cluster 熊猫的程序员来说，这个问题可能会令人困惑。对于普通的熊猫来说，程序、数据和用户都在同一台电脑上。有了 PySpark pandas，你的电脑只是操作 Spark 集群的前端，而 Spark 集群位于云中的某个地方。因此，`pspd.read_csv()`看不到电脑本地磁盘上的文件。

你如何给你的程序输入信息？有两种方法都很有效。

*   使用 Databricks GUI 将输入文件从您的计算机复制到 Databricks 文件系统(DBFS)。详细说明在我的[上一篇](https://medium.com/@chuck.connell.3/pandas-on-databricks-via-koalas-a-review-9876b0a92541)里。一旦到了 DBFS，文件就在 Spark 的本地，并且`pspd.read_csv()`可以正常打开它们。
*   将文件放入云存储中，如[亚马逊 S3](https://docs.databricks.com/data/data-sources/aws/amazon-s3.html) 或 [Azure blob](https://docs.databricks.com/data/data-sources/azure/azure-storage.html) ，将该存储挂载到 DBFS，然后在`pspd.read_csv().`中使用该路径

要跟踪此问题，请参见[https://issues.apache.org/jira/browse/SPARK-37198](https://issues.apache.org/jira/browse/SPARK-37198)。

## 保存结果

用`pspd.to_csv()`写输出文件会导致两个问题。Spark 写 DBFS 而不是你的本地机器。由于 Spark 运行在多个计算节点上，它将输出写成一组名称丑陋、不可预测的文件*集合*。你可以找到这些文件，并把它们合并成一个普通的 CSV 文件，但这很麻烦。

要解决这些问题:

*   对于可视化结果，如直方图或散点图，按下图中的相机图标以在本地机器上获得 PNG 图像。
*   对于多达 10，000 行的数据集，使用`display(pspdDataFrame).`初始输出显示 1000 行，您可以选择获得 10，000 行。有一个按钮可以将 CSV 文件下载到您的本地机器上。
*   对于稍后要处理的较大数据集，将数据帧保存为 Databricks 表，该表在计算会话之间作为单个实体保存在 DBFS 中。当您以后想要读取该数据时，只需在 Databricks 中打开该表。

```
# DataFrame to table
pspdDF.to_spark().write.mode(“OVERWRITE”).saveAsTable(“my_table”)

# Table to DataFrame
pspdDF = sqlContext.table(“my_table”).to_pandas_on_spark()
```

要跟踪此问题，请参见[https://issues.apache.org/jira/browse/SPARK-37198](https://issues.apache.org/jira/browse/SPARK-37198)。

## 拉丁 1 编码

在撰写本文时，`pspd.read_csv()`还不支持 latin-1 编码。相反，使用`pspd.read_csv(encoding='Windows-1252')`这两种编码并不相同，但它们很接近。

要跟踪此问题，请参见[https://issues.apache.org/jira/browse/SPARK-37181](https://issues.apache.org/jira/browse/SPARK-37181)。

## to_numeric(错误=)

pandas 代码的一个常见行如下所示，它将文本数字转换成适当的数字数据类型。

```
VaxDF["CompleteCount"] = \
    pd.to_numeric(VaxDF["CompleteCount"], errors='coerce')\
    .fillna(0)\
    .astype(int)
```

`errors='coerce'`参数导致任何错误的值被设置为 NaN。但是 pspd.to_numeric()不支持 Spark 3.2 中的 errors 参数。幸运的是，pspd 的默认行为是模仿上面的，所以下面的代码就像包含了`errors='coerce'.`一样工作

```
VaxDF["CompleteCount"] = \
    pspd.to_numeric(VaxDF["CompleteCount"])\
    .fillna(0)\
    .astype(int)
```

要跟踪此问题，请参见[https://issues.apache.org/jira/browse/SPARK-36609](https://issues.apache.org/jira/browse/SPARK-36609)。请注意，此修复将提交给 Spark 代码库，并将出现在 Spark 3.3 中。

## DF[列]。地图()。菲尔娜

习语。地图()。fillna()可用于控制在字典不包含某个键的情况下设置什么值。酪之后不支持 fillna()。PySpark Pandas 3.2 中的 map()，所以改一行这样的:

```
DF["state_abbr"] = \ 
    DF['state'].map(us_state_to_abbrev).fillna(DF["state"])
```

对此:

```
DF["state_abbr"] = DF['state'].map(us_state_to_abbrev)
```

确保字典包含所有需要的键，或者为缺少的键提供有意义的值。

追踪本期:[https://issues.apache.org/jira/browse/SPARK-37183](https://issues.apache.org/jira/browse/SPARK-37183)

## . str.split 删除即可

在普通的熊猫中，你可以像这样去掉一个后缀:

`DF["column"] = DF["column"].str.split("suffix").str[0]`

在 pyspark.pandas 3.2 中，这种说法不起作用。相反，请这样做:

`DF["column"] = DF["column"].str.replace("suffix", '', 1)`

如果后缀只在末尾出现一次，两者都将正常工作。但是，请注意，如果后缀不止一次出现或者出现在整个字符串的中间，这两行代码的行为会有所不同。

追踪本期:[https://issues.apache.org/jira/browse/SPARK-37184](https://issues.apache.org/jira/browse/SPARK-37184)

## offsets()来调整日期时间

在常规的 pandas 中，您可以使用 pandas.offsets 来创建一个时间增量，允许这样的行:

`this_period_start = OVERALL_START_DATE + pd.offsets.Day(NN)`

这在 pyspark.pandas 3.2 中不起作用。改为写:

```
import datetimethis_period_start = OVERALL_START_DATE + datetime.timedelta(days=NN)
```

追踪本期:[https://issues.apache.org/jira/browse/SPARK-37186](https://issues.apache.org/jira/browse/SPARK-37186)

## 大型数据框架中一列的直方图

当试图从一个较大的数据帧中创建一个列的直方图时，我的代码总是崩溃。下面一行产生了如下所示的错误:

```
DF.plot.hist(column=”FullVaxPer100", bins=20) # there are many other columnscannot resolve ‘least(min(EndDate), min(EndDeaths), min(`STATE-COUNTY`), min(StartDate), min(StartDeaths), min(POPESTIMATE2020), min(ST_ABBR), min(VaxStartDate), min(Series_Complete_Yes_Start), min(Administered_Dose1_Recip_Start), ...’ due to data type mismatch: The expressions should all have the same type, got LEAST(timestamp, bigint, string, timestamp, bigint, bigint, string, timestamp, bigint, bigint, timestamp, bigint, bigint,...).;
```

奇怪的是，pyspark.pandas 似乎在所有的列上操作，而只需要一个列。

作为一种解决方法，首先提取列，然后绘制直方图:

```
OneColumnDF = FullDF["FullVaxPer100"]OneColumnDF.plot.hist(bins=20, title="US Counties - FullVaxPer100")
```

追踪本期:[https://issues.apache.org/jira/browse/SPARK-37187](https://issues.apache.org/jira/browse/SPARK-37187)。

## 直方图标题不起作用

在 pyspark.pandas 3.2 中，直方图的 title 参数没有任何作用。所以这个命令:

```
DF.plot.hist(bins=20, title="US Counties – FullVaxPer100")
```

运行时没有错误，但标题不与直方图一起显示。

作为一种变通方法，使用普通的 print()语句在绘图之前或之后输出标题。

追踪本期:[https://issues.apache.org/jira/browse/SPARK-37188](https://issues.apache.org/jira/browse/SPARK-37188)

## 直方图范围不起作用

在 pyspark.pandas 3.2 中，直方图的范围参数不起任何作用。所以这个命令:

```
DF.plot.hist(bins=20, range=[0, 50], title="US Counties")
```

运行时没有错误，但显示的值不限于 0 到 50 之间。

一种解决方法是，在运行直方图之前选择范围。

```
OneRangeDF = (DF[DF.DeathsPer100k <= 50])["DeathsPer100k"]
print ("US Counties -- DeathsPer100k (<=50)")
OneRangeDF.plot.hist(bins=20)
```

追踪本期:[https://issues.apache.org/jira/browse/SPARK-37189](https://issues.apache.org/jira/browse/SPARK-37189)。

就像我上面说的，星火上的熊猫效果不错。上面链接的 pandas 代码在 Spark 上运行并产生了正确的结果，给出了这里概述的小的代码更改。

欢迎对本文进行评论和更正。