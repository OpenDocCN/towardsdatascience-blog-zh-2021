# 来自数据+人工智能峰会 NA 2021 的亮点

> 原文：<https://towardsdatascience.com/highlights-from-data-ai-summit-na-2021-f5320f5d0fd9?source=collection_archive---------36----------------------->

## 与 Apache Spark 新特性相关的数据+人工智能峰会笔记

![](img/3a99fb4143a014c16bc6a1a756b3891e.png)

照片由[大卫·特拉维斯](https://unsplash.com/@dtravisphd)在 Unsplash 上拍摄

数据领域最大的会议之一— [**Data + AI Summit 北美 2021**](https://databricks.com/dataaisummit/north-america-2021) 上周发生了，这次我没有用自己的演讲做出贡献，但作为一名听众，我越来越喜欢这些会议。

在这篇简短的报告中，我想总结一下我在 *Apache Spark 内部和最佳实践*主题中讨论的 Spark 3.1 和即将推出的 3.2 中的新特性。

# 禅宗项目

Zen 项目开始于一年前，目标是让 Spark 对 Python 用户更加友好。

## 类型提示

其中一个步骤是添加类型提示，例如允许 ide 和笔记本环境中的自动完成，这样可以使开发更加高效。对类型提示的完全支持是在最新版本 3.1.1 中添加的。

## 星火上的熊猫

Spark 3.2 的一个新特性是将考拉项目集成到 Spark 中。考拉允许使用 Pandas API，同时将 Spark 作为后端，因此，如果数据科学家使用 Pandas 进行数据操作，并由于数据量大而遇到性能问题，那么很容易切换到考拉，代码保持不变，但在幕后，执行在 Spark 中进行。现在，当考拉将被集成到 Spark 中时，情况变得更加简单，你可以从 Spark 中导入熊猫并使用熊猫 API，同时在引擎盖下有 Spark。熊猫的绘图和可视化工具现在可以在 Spark 中使用了。

本次峰会讨论了 Zen 项目:

*   [Zen 项目:让 PySpark 中的数据科学更简单](https://databricks.com/session_na21/project-zen-making-data-science-easier-in-pyspark)

# 性能改进

随着 Spark 的每一次发布，发动机的性能都在不断提高。在峰会上，我们讨论了几个与绩效相关的话题:

## 混合散列连接(SHJ)

混合散列连接是 Spark 中用于连接数据的算法之一。在实践中，它被避免使用，取而代之的是更健壮的排序合并连接(SMJ)。如果数据不对称，并且其中一个分区太大，SHJ 会导致 OOM 错误。然而，在合理的情况下，SHJ 可以比 SMJ 执行得更快，因为它避免了这种情况。(关于 Spark 使用的算法和决策逻辑的更多细节，请参见我的相关文章[关于 Spark 3.0 中的加入的文章](/about-joins-in-spark-3-0-1e0ea083ea86)

*3.1 中实现了几项增强功能，使 SHJ 变得更好、更可用:*

*   *添加代码生成(参见[吉拉](https://issues.apache.org/jira/browse/SPARK-32421))*
*   *支持完全外部连接(参见[吉拉](https://issues.apache.org/jira/browse/SPARK-32399)*
*   *保留分区(参见[吉拉](https://issues.apache.org/jira/browse/SPARK-32330))*

*在这次首脑会议期间，讨论了 SHJ 的改进和其他改进:*

*   *[深入了解 Apache Spark 3.1 的新特性](https://databricks.com/session_na21/deep-dive-into-the-new-features-of-apache-spark-3-1)*

## *…向量化…*

*矢量化是一种同时处理多行以加快处理速度的技术。在 Spark 的当前版本中，当使用矢量化读取器从 parquet 和 orc 格式读取数据时，会使用矢量化。除此之外，PySpark 中的熊猫 UDF 也支持矢量化。*

*实现矢量化的一种方法是使用现代硬件支持的 [SIMD](https://en.wikipedia.org/wiki/SIMD) 指令(单指令，多数据)。当前版本的 Spark 没有明确使用 SIMD，因为 JVM 中的 HotSpot 编译器在某些情况下会生成 SIMD 指令，但在某些情况下不会。然而，新的 Java 16 有 *VectorAPI* 可用，这个 API 可以确保 SIMD 指令被使用。因此，使用这个 *VectorAPI* ，可以在 Spark 中为生成的代码实现显式矢量化。本次峰会讨论了矢量化:*

*   *[在 Apache Spark 中启用矢量化引擎](https://databricks.com/session_na21/enabling-vectorized-engine-in-apache-spark)*

## *标准压缩编解码器*

*Apache Spark 支持各种压缩算法，例如，snappy 是将数据保存到 parquet 文件时使用的默认算法，另一方面，lz4 用于 shuffle 文件，也支持其他编解码器。ZStandard 是一种编解码器，可以实现与 gzip 相当的压缩速度和压缩比。*

*从 Spark 3.2 开始，ZStandard 将在三种情况下有用，它可以带来性能优势(节省本地磁盘空间和/或外部存储以及整体查询执行速度的提高):*

*   *事件日志压缩(*spark . event log . compression . codec = zstd*)*
*   *洗牌期间的 I/O 压缩(*spark . io . compression . codec = zstd*)*
*   *压缩 parquet/orc 文件*spark . conf . set(" spark . SQL . parquet . compression . codec "，" zstd")**

*相关的首脑会议是:*

*   *[z standard 的崛起:阿帕奇 Spark/Parquet/ORC/Avro](https://databricks.com/session_na21/the-rise-of-zstandard-apache-spark-parquet-orc-avro)*

## *随机播放增强*

*Spark 作业中的 Shuffle 通常是开销最大的过程，因为数据必须在集群的节点之间移动。shuffle 是两个阶段之间的界限，在一个阶段完成后，输出被写入文件，然后被提取到下一个阶段。文件保存在执行器的本地磁盘上，或者当使用外部混洗系统时，可以保存到外部存储系统，例如 HDFS。这些文件的数量随着任务的数量成二次方增长(更具体地说，它是上游阶段的任务数量乘以下游阶段的任务数量)，并且这些文件可能变得相当小。有一个[吉拉](https://issues.apache.org/jira/browse/SPARK-30602)正在进行中，旨在实现所谓的基于推送的洗牌，通过预合并块来优化洗牌 I/O。关于这项技术的相关文章可以在[这里](https://engineering.linkedin.com/blog/2020/introducing-magnet)找到。在这次首脑会议上讨论了这个主题本身:*

*   *[磁铁洗牌服务:LinkedIn 上基于推送的洗牌](https://databricks.com/session_na21/magnet-shuffle-service-push-based-shuffle-at-linkedin)*

## *复杂查询计划处理*

*对于某些查询，查询计划可能会变得非常复杂，其编译可能会成为真正的瓶颈。有一个[吉拉](https://issues.apache.org/jira/browse/SPARK-33152)正在进行中，它试图简化处理带有别名的列的约束的过程，测试显示有希望加速。与这一主题相关的首脑会议是:*

*   *[优化复杂计划的催化剂优化器](https://databricks.com/session_na21/optimizing-the-catalyst-optimizer-for-complex-plans)*

# *ANSI SQL 符合性*

*这是 spark 中正在进行的项目，在 3.0 版本中引入了实验性配置设置 *spark.sql.ansi.enabled* ，当设置为 *True* 时，Spark 将尝试符合 ANSI SQL 规范，这意味着如果输入无效，查询可能会在运行时失败，否则可能会返回空值。这方面的一个具体例子是无法安全转换或容易混淆的数据类型的转换。与此相关的一些新功能将在 3.2 中发布，例如， *TRY_CAST* ， *TRY_ADD* ， *TRY_DIVIDE* 如果用户希望使用 ANSI 模式，但如果输入无效而不是查询失败，则这些功能会很有用。*

*在 Spark 3.2 中，将发布两种新的间隔日期类型，年-月和日-时间，这将解决当前 *CalendarIntervalType* 存在的一些问题。正在进行的[吉拉](https://issues.apache.org/jira/browse/SPARK-27790)是相关子任务的保护伞。当前区间类型的问题是它不具有可比性，并且不支持排序，因此有两个区间我们无法比较它们并判断哪个更大。另一个问题是我们不能将区间类型保存为文件格式，比如 parquet/orc 甚至 json。另一方面，将在 3.2 中发布的两个新类型 *YearMonthIntervalType* 和 *DayTimeIntervalType* 将是可比较和可订购的，并且将符合 SQL 标准。*

*有关 ANSI SQL 合规性的更多信息和细节，请参见[文档](https://spark.apache.org/docs/latest/sql-ref-ansi-compliance.html)或查看峰会中讨论这些主题的以下会议:*

*   *[深入了解 Apache Spark 3.1 的新特性](https://databricks.com/session_na21/deep-dive-into-the-new-features-of-apache-spark-3-1)*
*   *[Apache Spark 3.2 中区间的综合视图](https://databricks.com/session_na21/comprehensive-view-on-intervals-in-apache-spark-3-2)*

# *DataSourceV2 API*

*DataSourceV2 API 在过去几年中一直在开发中，它旨在解决与 V1 API 相关的各种问题，例如，*data frame writer*的某些模式的连接器行为不一致，在写入表之前没有模式验证，依赖于其他内部 API，如 *SQLContext* ，以及新功能不容易扩展。*

*V2 API 使用目录来检查表是否存在，并使用一致的验证规则，与连接器的实现无关。有一个[吉拉](https://issues.apache.org/jira/browse/SPARK-23889)正在开发中，旨在支持在 V2 API 中更好地控制连接器的分布和订购，这应该允许更好的灵活性，并计划在 Spark 3.2 中发布。*

*本次峰会讨论了 V2 API 以及相关的配送和订购功能:*

*   *[高效数据源 V2 的数据分发和排序](https://databricks.com/session_na21/data-distribution-and-ordering-for-efficient-data-source-v2)*

# *结论*

*Data + AI 峰会 NA 2021 上有很多有趣的环节。在这篇简短的报告中，我总结了我在 *Spark 内部机制和最佳实践*专题的一些会议中的笔记。这些说明并不完整，我主要关注的是 3.1.1 中发布的新特性或者计划在 3.2 中发布的新特性。*