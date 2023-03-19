# 基于颗粒比较 PySpark 数据帧

> 原文：<https://towardsdatascience.com/compare-pyspark-dataframes-based-on-grain-e963e0a8aacf?source=collection_archive---------6----------------------->

## 基于粒度比较 Pyspark 数据帧并使用数据样本生成报告的简单方法

![](img/dd817d1a8c6ea8f33cdb4261bae18195.png)

米利安·耶西耶在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

比较两个数据集并生成准确而有意义的见解是大数据世界中一项常见而重要的任务。通过在 Pyspark 中运行并行作业，我们可以基于 [**粒度**](http://www.datamartist.com/dimensional-tables-and-fact-tables#:~:text=The%20GRAIN%20or%20GRANULARITY%20of,one%20line%20for%20some%20orders).) 高效地比较庞大的数据集，并生成高效的报告来查明每一列级别的差异。

**要求:**

我们从 Salesforce 获取了大量数据集。该数据集有 600 多列和大约 2000 万行。必须对开发和验证数据集进行比较，以生成一个报告，该报告可以基于 [**粒度**](http://www.datamartist.com/dimensional-tables-and-fact-tables#:~:text=The%20GRAIN%20or%20GRANULARITY%20of,one%20line%20for%20some%20orders).) 查明列级别的差异。

下面是与包含粒度列的数据样本一起生成的洞察，可用于进一步查询和分析。

*   重复记录。
*   缺失记录。
*   捕获开发和验证数据集中的列值之间的差异。

**方法:**

我们可以使用数据框的 subtract 方法减去两个数据框，这将生成不匹配的记录，但是很难确定 600 列中的哪一列实际上发生了变化以及实际值有所不同。下面是解决这个问题的一个简单方法。

从这里开始，我将验证数据集称为源数据框架(src_df)，将开发数据集称为目标数据框架(tar_df)。由于相同的两个数据帧被一次又一次地用于不同的分析查询，因此我们可以将这两个数据帧保存在内存中，以便进行更快的分析。

在下面的方法中，假设两个数据框具有相同的模式，即两个数据框具有相同的列数、相同的列名以及所有列的相同数据类型。

**识别重复和缺失的谷物记录:**

为了识别重复记录，我们可以使用 Pyspark 函数通过查询编写一个小组。grainDuplicateCheck()函数也是如此。

为了识别丢失的颗粒，我们可以从两个数据帧中选择颗粒列并直接减去它们。

需要注意的是，我们需要执行 src_df - tar_df 来获取有效的缺失颗粒，还需要执行 tar_df - src_df 来获取无效的额外记录颗粒。

**比较列值:**

现在，我们必须比较数据帧之间的列值，并逐列生成报告，同时找到特定粒度的预期值和无效值。

由于我们已经确定了丢失的记录，现在我们将连接粒度列上的两个数据框，并比较在两个数据框中具有匹配粒度的所有记录的列值。

值得注意的是

*   颗粒列可以有空值，我们必须执行**空安全连接**，否则，那些记录不会被连接，记录将被跳过。为此，我使用了 [**eqNullSafe()**](https://spark.apache.org/docs/3.1.1/api/python/reference/api/pyspark.sql.Column.eqNullSafe.html) 内置的 Pyspark 函数。
*   包含双精度值(如 amount)的列不会完全匹配，因为不同的计算可能会生成小数精度略有差异的值。因此，对于这些类型的列，我们必须减去这些值，并标记那些差异≥阈值的记录。

在下面的 compareDf()函数中，使用连接的数据框，我们选择要验证的粒度和特定列，并对它们进行比较。

**通过运行并行作业比较列值:**

上述函数的局限性在于，我们是按顺序逐列比较两个数据帧，遍历数据帧的每一列。

现在让我们来看一些统计数据。有 600 多列和大约 2000 万行，比较每列需要 15 秒。对于所有 600 列，大约需要 150 分钟(约 2.5 小时)。

列比较任务彼此独立，因此我们可以并行化它们。如果我们平行比较 8 根柱子，那么所用的时间大约是 19 分钟。这是一个巨大的差异，因为它快了 8 倍。

在下面的函数 compareDfParallel()中，我使用多处理内置 python 包创建了一个指定大小的线程池。使用*parallel _ thread _ count =*8，将并行比较 8 列。compareDFColumn()是将由每个线程为特定列执行的实际比较函数。

必须根据集群中节点的容量来选择并行线程数。我们可以使用 spark-submit 中指定的驱动核心数作为并行线程数。

假设 pyspark_utils.py 文件中存在上述所有函数，下面是示例数据的用法示例。

compareDfParallel()的输出将具有前缀为 *_src* 的列，用于引用源数据帧的列值，以及前缀为 *_tar* 的列，用于引用目标数据帧的列值。

在上面的示例中， *billing_amount_src* 是 1100，这是源数据帧中 billing_amount 的值，而 *billing_amount_tar* 是 1000，这是目标数据帧中 billing_amount 的值。

脚本的输出可以通过管道传输到一个文件，以获得一个合并的报告。可以修改该函数来生成 HTML 报告，而不是文本文件报告。

这是基于粒度有效比较 Pyspark 数据帧并使用数据样本生成报告的简单方法之一。

数据仓库快乐！