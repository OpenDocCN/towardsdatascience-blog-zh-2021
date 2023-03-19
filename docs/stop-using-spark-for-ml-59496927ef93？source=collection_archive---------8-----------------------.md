# 停止用火花做 ML！

> 原文：<https://towardsdatascience.com/stop-using-spark-for-ml-59496927ef93?source=collection_archive---------8----------------------->

## 让你的机器学习管道尽可能简单的指导方针。

![](img/49df94791e6bceabb3fc268000bd9f95.png)

以赛亚·鲁斯塔德在 Unsplash[上的照片](https://unsplash.com/s/photos/stop-pretty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您有大量的数据需要处理，Spark 是很好的选择。spark 和 Pyspark(用于与 Spark 交互的 Python API)是数据工程师工具箱中的关键工具。有一个很好的理由:

> *“无论您的数据增长到多大，您仍然能够处理它。”*

是一个常见的论点。尽管现代公司使用 Spark 端到端构建“经典”数据管道来组合、清理、转换和聚合他们的数据以输出数据集*是有效的。*

# ML 的火花成本

对于构建数据管道的数据科学家和 ML 工程师来说，上述论点并不总是成立的，这些管道*输出机器学习模型*。对于机器学习管道来说，涉及到成本:

*   👭**熟悉度—** 从我的经验数据来看，科学家对(Py)Spark 的熟悉程度不如对熊猫。两者都使用数据帧的概念，但是用于操作数据帧中的数据的 API 是完全不同的。这里有一个熊猫对 Pyspark 的例子，我们创建了一个新的列:
    `df['Age_times_Fare'] = df['Age'] * df['Fare']
    df = df.withColumn("AgeTimesFare", $"Age" * $"Fare")`
*   🔧**维护—**Pandas、Scikit-learn、Pytorch、Tensorflow 等 Python 包是数据科学家和 ML 工程师的主要工具。通过引入 Spark，您在您需要维护的管道中引入了第二个额外的数据处理工具。
*   🏋🏽**训练复杂性—** Spark 在 Python 进程之外的集群上运行，这增加了测试和运行定制代码的复杂性。对于*测试*，您必须在您的系统上配置一个 Spark 环境。当使用 PySpark 时，*第三方 Python 库*需要被运送到 Spark 执行器以避免“导入错误”。
*   🔄**推理复杂度**—根据我的经验，Spark/SparkML 很少用于建模，虽然我看到 Spark 用于特征工程。使用特征工程，您最好在推理时从训练时复制相同的步骤。为此，您可以在训练时将特征工程步骤打包为模型，并在推理时使用该模型。因为 ML 模型通常基于不同的技术(Scikit-learn、PyTorch、Tensorflow 等)，所以您需要在推理时管理基于 Spark 的特征工程模型和 ML 模型之间的编排。这增加了运行模型的复杂性。

> 我不是说你应该把火花扔进垃圾桶。在选择它的时候反而显得挑剔。

# 简单性准则

在编写一行 Spark 代码之前，下面的指南提出了一些替代方法和更简单的方法。

## 1.垂直缩放🐣

尝试垂直扩展您的计算实例，以便您可以将所有数据保存在内存中。尝试使用 Pandas 处理您的数据，看看处理时间是否仍然合理。

## 2.你需要所有的数据吗？📈

您是否需要所有数据来实现业务价值？多一年数据的附加值是什么？

你的模型性能的收益是否超过涉及 Spark 的成本？在获取更多数据之前，请查看您的 ML 评估曲线。如果你已经有了好的结果，并且你没有过度拟合，也许你不需要更多的数据。

## 3.结构化查询语言📇

在编写一行(Py)Spark 代码之前，检查一下您是否无法通过使用 SQL 达到您的目标。一般来说，SQL 更容易编写和维护。如果您已经有了一个 Spark 集群，请尝试 SparkSQL。你也可以尝试通过一个无服务器的 SQL 引擎来公开你的数据，比如[谷歌大查询](https://cloud.google.com/bigquery)或者 [AWS Athena。](https://aws.amazon.com/athena/)您甚至可以通过 [dbt](https://github.com/dbt-labs/dbt) 使用 SQL 进行更高级的转换，看看吧！

## 4.云服务☁️

如果你需要建立一个 Spark 集群，首先检查一下*无服务器云服务*比如 [AWS Glue](https://aws.amazon.com/glue/) 。它允许您在几秒钟内启动多达 100 个执行器的 spark 集群，其中每个执行器有 4 个 CPU 和 16GB RAM。
如果你需要一个更具可配置性的集群，并且你已经对你正在做的事情有所了解，你可以尝试一个*托管服务*，比如 [AWS EMR](https://aws.amazon.com/emr/) 。

# 火花的替代品

一些替代方案可以帮助您处理大量数据:

## Vaex

[Vaex](https://github.com/vaexio/vaex) 是一个 Python 数据处理库，API 与 Pandas 类似。这是一个*核外*解决方案，每秒可以处理多达 10 亿行。数据集的大小限制等于硬盘的大小。⁴

## 达斯克

[Dask](https://github.com/dask/dask) 在某种意义上类似于 Spark，你可以水平扩展来处理大量数据。但这是不同的，因为 Dask 是用 Python 编写的，并且支持 Pandas 和 Scikit 这样的库——开箱即用。

如果你有任何建议，让我知道在回应！

# 文森特·克拉斯

👋如果您想了解更多关于 ML 工程和 ML 管道的信息，请关注我的 [Medium](https://medium.com/@vincentclaes_43752) 、 [Linkedin](https://www.linkedin.com/in/vincent-claes-0b346337/) 和 [Twitter](https://twitter.com/VincentClaes1) 。

**脚注**

[1]:[https://www . signify technology . com/blog/2019/04/pandas-vs-spark-how-to-handle-data frames-part-ii-by-Emma-grima ldi](https://www.signifytechnology.com/blog/2019/04/pandas-vs-spark-how-to-handle-dataframes-part-ii-by-emma-grimaldi)
【2】:在 AWS 上，您可以以大约 8.5 美元/小时的价格访问 ml . r 5.24 xlage 等具有 96 个 CPU 和 768 个 GiB 内存的实例。更多信息可在[此处](https://aws.amazon.com/sagemaker/pricing/)。
【3】:[https://towards data science . com/vaex-out-of-core-data frames-for-python-and-fast-visualization-12c 102 db 044 a](/vaex-out-of-core-dataframes-for-python-and-fast-visualization-12c102db044a)
【4】:[https://www . kdnugges . com/2021/05/vaex-pandas-1000 x-faster . html](https://www.kdnuggets.com/2021/05/vaex-pandas-1000x-faster.html)