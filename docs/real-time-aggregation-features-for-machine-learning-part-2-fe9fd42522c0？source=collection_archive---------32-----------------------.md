# 机器学习的实时聚合特性(第二部分)

> 原文：<https://towardsdatascience.com/real-time-aggregation-features-for-machine-learning-part-2-fe9fd42522c0?source=collection_archive---------32----------------------->

## 为什么实时 ML 特性对 ML 应用程序至关重要并且难以实现

作者:凯文·斯坦普夫(泰克顿)，迈克·伊斯特汉姆(泰克顿)，尼基尔·辛哈(Airbnb)

![](img/88a5efbb1ec7ec8a678b4398e5013306.png)

(*图片作者*)

# 介绍

[本博客系列的第 1 部分](https://stumpfkevin.medium.com/real-time-aggregation-features-for-machine-learning-part-1-ec7337c0a504)讨论了时间窗聚合对于 ML 用例的重要性。在接下来的章节中，我们将描述一种解决这些挑战的方法，这种方法已经在泰克顿大规模验证过，并且已经在 Airbnb 和优步的生产中成功使用了几年。讨论的方法解释了 Tecton 的实现完全依赖于开源技术。

# 解决方案:平铺时间窗口聚合

在较高层次上，该解决方案将一个全职窗口内的时间窗口聚合分解为 a)多个较小时间窗口的压缩分块，用于存储分块间隔内的聚合，以及 b)聚合时间窗口头部和尾部的一组预计原始事件。压缩的图块是预先计算的。在特征请求时，最终特征值是通过组合存储在图块中的聚合以及原始事件上的聚合按需计算的:

![](img/e91d4a0e6771d45254a6b5f3ea17aba9.png)

*实时服务路径*(作者的*图片)中平铺式时间窗聚合的架构*

# 功能配置

这些平铺时间窗口功能的配置需要以下详细信息:

*   从一个或几个原始事件源中选择和投射原始事件
*   这些计划事件的汇总定义

## 原始事件的选择和投影

Tecton 允许用户使用 SQL 选择和设计事件。该查询允许用户选择原始源，去除所有不必要的数据，并执行内联转换。转换示例可能如下所示:

```
select
    user_uuid,
    amount,
    timestamp
from
    Transactions
```

这个 SQL 通常可以由 Apache Spark 或 Flink 之类的流处理器本地执行。默认情况下，泰克顿在连续模式下使用火花流。

## 聚合定义

Tecton 允许用户使用简单的 DSL 定义聚合信息。按照上面的示例，用户可能希望在**金额**列上创建几个 **sum** 聚合:

```
aggregation_type: sum
aggregation_column: amount
time_windows: 2h, 24h, 48h, 72h
```

您可以浏览[我们的文档](https://docs.tecton.ai/)来查看 Tecton 中这种特性定义的真实例子。上面的定义足以编排一组摄取作业，这些作业将新定义的功能加载到在线和离线功能存储中。

# 在线商店的流媒体摄取

![](img/378dc3e558ab68052fa46bd57b6cfc67.png)

在线商店中基于 Spark 流的功能摄取(*作者图片*

Tecton 使用 Spark 结构化流将来自流数据源的最新数据写入在线要素存储。用户的 SQL 转换应用于流输入，以投射事件并去除所有不必要的数据。结果被直接写入键值存储，没有进一步的聚合。

默认情况下，我们使用 DynamoDB 作为在线商店(支持 Redis 作为替代)。

# 批量摄取到在线和离线商店

![](img/71edf07a27bf53a169ecb2cf955ab394.png)

基于 Spark Batch 的功能摄取到线下和线上商店(*图片由作者*提供)

串流摄取仅将最近的事件从串流来源向前填充到在线商店。其他一切都是离线批处理摄取作业的责任。这包括:

*   回填和正向填充线下商店
*   回填网店

批处理摄取也使用 Spark，但是是在批处理模式下，而不是在流模式下。批处理作业从批处理源读取，该批处理源是流式处理源的离线镜像(例如，配置单元或 DWH 表)，并且应该包含流式处理事件的完整历史日志。Tecton 使用 Delta Lake 作为默认的离线商店(数据仓库作为未来受支持的替代方案)。

# 将原始事件压缩到图块

到目前为止，我们只讨论了将原始事件直接写入在线或离线商店。当然，这种幼稚的实现本身可能会导致在线服务路径中不可接受的高查询延迟。为一个聚合提供服务可能需要大量数据，特别是在长聚合窗口或具有大量更新的数据源的情况下。这可能会对服务路径的延迟产生严重后果，类似于在生产中直接查询事务存储。为了缓解这个问题，你需要周期性地**压缩**原始事件到不同间隔长度的瓦片中。

结果是，在线和离线要素存储将包含两种不同类别的数据:

*   未压缩的数据:原始事件的投影
*   压缩数据，这是使用用户配置的聚合操作压缩一个**压缩间隔**的数据的结果。在实践中，典型的**压缩间隔**是 5 分钟，也是一个“长”间隔，它跨越给定聚合窗口的最长完整多日切片

未压缩的数据是作为上述批处理/流摄取部分中描述的常规流的一部分产生的。压缩数据由一个单独的定期批处理 Spark 作业生成，该作业从流源的离线镜像中读取数据:

![](img/c1c04eea1971f434b7a185ed0c11b1cd.png)

基于火花批的压实工艺(作者提供*图片)*

在离线或离线服务时，必须从存储中提取未压缩和压缩数据的混合，如下图所示:

![](img/9a16cf681006a18513595e9e043d3745.png)

利用图块和原始事件的实时服务路径(*图片作者*

从图中可以看出，压缩行是在聚合范围的中间提取的。一般来说，在聚集的头部和尾部会有时间范围，这些时间范围只是压缩行的一部分，对于这些时间范围，我们提取未压缩的行来完成聚集。这种优化减少了必须从脱机或联机存储中提取的最坏情况下的行数。

> **注**
> 
> 这个博客到目前为止已经解决了提供超新鲜特征值的问题(<1s). As a result, it’s required that you stream a projection of raw events into the store. The curious reader may have noticed that you could already write pre-computed tiles, instead of raw events, to the online and offline store. In this case, the tile length defines the ultimate freshness of the feature: The larger the interval, the lower the guaranteed feature freshness. Further, the tile length controls the write-frequency to the store: The larger the interval, the lower the write-frequency. At the limit, you can choose a tile size of “None”, which results in a write to the store for every event, resulting in maximal freshness and the highest write-load. Therefore, the minimal tile size really gives the user a knob to tune the cost/freshness-tradeoff of a given feature.

# Online Serving

During online serving, the Feature Server receives a request containing the join keys to be retrieved. From these join keys Tecton generates an online store query for all of the data for the given join keys in the time range [Now — (aggregation time window), Now). This results in a series of compacted tiles as well as raw events, which are then aggregated by Tecton’s Feature Server to produce the final feature value.

# Offline Serving

Offline Serving requests consist of a DataFrame with the following columns:

*   **连接键值**
*   一个**时间戳**。与始终从当前时间开始聚合的在线服务不同，离线服务请求能够从每行的不同时间开始聚合。这是生成训练数据集和通过时间旅行提供时间点正确性的重要功能。

离线服务流程类似于在线服务流程，只是它是作为批处理 Spark 查询来执行的。

# 优势总结

总之，所讨论的实现提供了以下好处:

*   **回填**现在解决了，因为讨论的解决方案允许您从批处理源回填和从事件流向前填充
*   **长时间运行的时间窗口**很容易支持
*   **支持超新鲜功能**,因为在请求时，您可以始终考虑到直到预测的那一刻已经流入商店的事件
*   **计算和内存-经济高效的处理:**
*   只有当新事件到来时，以及当您向存储区写入切片时，您才会向存储区写入更新
*   您只计算切片，而不是完整的时间窗。前者更便宜，需要更少的内存
*   **计算和存储高效处理多个时间窗口:**您可以将切片重新组合成不同长度的时间窗口:例如，您可以存储 1h 切片，您可以获得 2h、12h、24h、7d 等。不具体化存储中的全职窗口的聚合
*   **快速特征检索:**无论是离线还是在线，您都不需要在请求时聚集(仅仅)原始事件。您可以在预先计算好的切片上聚合，从而加快整个操作

# 深入探讨高级性能优化:锯齿窗口

Nikhil Simha (Airbnb)

如上所述，将预计算的图块与投影的原始事件相结合的概念是由 Airbnb 的 Zipline 背后的团队首创的。这种方法的下一个迭代是他们所谓的锯齿窗。

上述方法的一个含义是，在线商店必须在窗口的尾部以及整个窗口保持原始事件，以便于窗口随着时间向前滑动。随着窗口变大(180d 以上)，这成为存储可扩展性的瓶颈。

为了避免这个瓶颈，你可以滑动窗口的头部，但是跳过窗口的尾部(一个一个的平铺)。这意味着窗口大小随着时间的变化而随着跳数的变化而变化。并非所有的工作负载都可以容忍这种变化，但在 Airbnb 的经验中，机器学习应用程序可以很好地容忍这种变化——甚至可能会因为特征数据中这种噪声的正则化效应而略有受益。

具有 1 天跃点的 30 天窗口只需存储 30 个每日部分聚合(切片)和 1 天的原始数据。此前，Airbnb 存储了 30 天的原始数据，以方便窗户尾部的滑动。这将原始数据的存储需求降低了一个数量级。

因此，实际的窗口长度在 30 天到 31 天之间变化。我们称这些窗口为“锯齿窗”:一种对空间要求低、新鲜感高的[跳跃](https://docs.microsoft.com/en-us/stream-analytics-query/hopping-window-azure-stream-analytics)和[滑动](https://docs.microsoft.com/en-us/stream-analytics-query/sliding-window-azure-stream-analytics)窗口的混合体。只需允许窗口的长度按照其定义大小的一小部分变化，就有可能在不影响新鲜度的情况下同时改善存储大小和延迟。

# 结论

总之，我们已经展示了如何通过以高性能、经济高效以及在线/离线一致的方式将预计算的数据资产与按需转换相结合，来解决极其常见和具有挑战性的 ML 特征类型。好奇的读者可能已经意识到，这种预先计算与按需计算相结合的模式可以推广到时间窗口聚合之外，并扩展到完全由用户定义的转换步骤。这种一般化的方法允许您最终表达跨越离线分析世界和在线操作世界的计算 Dag:

![](img/a5603c5b8891e4d930f0f4bc479325cc.png)

跨越在线和离线环境的通用计算 DAG(*图片作者*

在未来的博客系列中，我们将讨论这个强大的 DAG 概念的好处和可能性，以及如果您想在 ML 应用程序中利用它，现在如何使用 Tecton。如果你想了解更多关于这个实现的技术细节，请随意给我们发一条松弛消息[这里](https://slack.feast.dev/)。

[1]高效的压缩实现还可以将较小粒度的瓦片压缩成较大粒度的瓦片

*关于其他技术的说明:我们上面描述的，当然不是一个完全新颖的实现。我们描述了“视图物化”的一般过程，并展示了如何使用开源流/批处理技术来实现它，这些技术可以直接插入组织的现有键值存储以及现有的离线存储(如数据湖或数据仓库)。像 timescaledb、ksqldb、materialize 或 Druid 这样的工具利用了类似的策略。根据您的现有堆栈、您引入新存储技术的意愿、您对一致的离线和在线商店的需求以及您对从批处理源高效回填要素的需求，您可能能够直接在这些工具的基础上构建您的应用程序。*

*还值得注意的是，上述解决方案解决了引言中列出的约束，这些约束在高价值的操作性 ML 应用中常见:在大量原始事件(> 1000s)上的特征聚合，这些事件需要以非常高的规模(> 1000s QPS)、低服务延迟(< < 100ms)、高新鲜度(< < 1s)、以及高特征准确性(例如，有保证的而非近似的时间窗口长度)来提供服务。如果其中一些约束不适用于您的用例，可以使用上面讨论的架构的更简单版本。让我们在 Slack 上讨论更多细节。*