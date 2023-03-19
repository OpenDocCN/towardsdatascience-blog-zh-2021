# 如何有效地重新划分火花数据帧

> 原文：<https://towardsdatascience.com/how-to-efficiently-re-partition-spark-dataframes-c036e8261418?source=collection_archive---------11----------------------->

## 如何增加或减少火花数据帧的数量

![](img/68a17288aaa460b510c1aacc3f5e6844.png)

照片由[梅姆](https://unsplash.com/@picoftasty?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/chunks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Apache Spark 是一个能够在合理的时间内处理大量数据的框架。这个统一引擎的效率在很大程度上取决于它对数据集合执行的工作进行分配和并行处理的能力。

在本文中，我们将介绍 Spark 中的分区，并解释如何对数据帧进行重新分区。此外，我们还将讨论何时值得增加或减少 Spark 数据帧的分区数量，以便尽可能优化执行时间。

## 简单地说，火花分割

为了实现高并行性，Spark 将数据分割成更小的块，称为分区，分布在 Spark 集群的不同节点上。每个节点可以有多个执行器，每个执行器可以执行一个任务。

将工作分配给多个执行器需要将数据划分并分布在执行器之间，这样工作可以并行完成，以便优化特定作业的数据处理。

## 如何获得当前的分区数量

在开始重新分区之前，有必要描述一下获取 Spark 数据帧当前分区数量的方法。例如，假设我们有以下最小火花数据帧

为了获得上述数据帧的分区数量，我们只需运行以下命令

请注意，输出取决于您当前的设置和配置，因此您可能会看到不同的输出。

## 如何增加分区数量

如果想增加数据帧的分区，只需运行`[repartition()](https://spark.apache.org/docs/2.4.0/api/python/pyspark.sql.html#pyspark.sql.DataFrame.repartition)`函数。

> 返回由给定分区表达式分区的新的`[**DataFrame**](https://spark.apache.org/docs/2.4.0/api/python/pyspark.sql.html#pyspark.sql.DataFrame)`。产生的数据帧是散列分区的。

下面的代码会将分区数量增加到 1000:

## 如何减少分区的数量

现在，如果您想对 Spark 数据帧进行重新分区，使其具有更少的分区，您仍然可以使用`repartition()`然而，**有一种更有效的方法可以做到这一点。**

`[coalesce()](https://spark.apache.org/docs/2.4.0/api/python/pyspark.sql.html#pyspark.sql.DataFrame.coalesce)`导致一个狭窄的依赖关系，这意味着当用于减少分区数量时，将会有 **no shuffle，**这可能是 Spark 中代价最大的操作之一。

> 返回一个正好有 N 个分区的新的`[**DataFrame**](https://spark.apache.org/docs/2.4.0/api/python/pyspark.sql.html#pyspark.sql.DataFrame)`。

在下面的例子中，我们将分区限制为 100 个。最初有 1000 个分区的 Spark 数据帧将被重新分区为 100 个分区，而不进行洗牌。我们所说的不洗牌是指 100 个新分区中的每一个都将被分配给 10 个现有分区。因此，当想要减少 Spark 数据帧的分区数量时，调用`[coalesce()](https://spark.apache.org/docs/2.4.0/api/python/pyspark.sql.html#pyspark.sql.DataFrame.coalesce)`会更有效。

## 结论

在本文中，我们讨论了如何通过分区优化数据处理，分区允许工作分布在 Spark 集群的执行器上。此外，我们还探讨了增加或减少数据帧中分区数量的两种可能方法。

`repartition()`可用于增加或减少火花数据帧的分区数量。然而，`repartition()`涉及洗牌，这是一个昂贵的操作。

另一方面，当我们想要减少分区数量时，可以使用`coalesce()`,因为这种方法不会触发 Spark 集群节点之间的数据洗牌，因此效率更高。