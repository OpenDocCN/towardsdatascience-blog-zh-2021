# 2021 年数据+人工智能峰会亮点💥

> 原文：<https://towardsdatascience.com/highlights-from-data-ai-summit-2021-3abfd9aaccaa?source=collection_archive---------38----------------------->

![](img/009142a33fc478a976d4351d225895a5.png)

崇高的紫色夜空。【数码影像】[https://unsplash.com/](https://unsplash.com/@strong18philip)[@文森特](http://twitter.com/vincentiu)

又来了，我最喜欢的免费数据会议之一！这个名字已经被重新命名为更好。最初是“火花峰会”，后来是“火花+AI 峰会”，现在是“数据+AI”峰会。Databricks 现在提供几个产品，它不再仅仅被认为是“火花公司”。我下面谈论的很多内容与我去年的重点[在这里](https://engineering.klarna.com/highlights-from-spark-ai-summit-2020-for-data-engineers-359211b1eec2)的趋势相同。但是 Databricks 今年发布了很多有趣的产品！

# 增量共享

这个新的 Databricks 产品被定义为业界第一个安全数据共享的开放协议。

随着公司转向多云以及云数据仓库的兴起，在数据可用性方面存在巨大挑战。数据工程师花费大量时间移动/复制数据，以便在不同的地方以经济高效和安全的方式访问和查询数据。

Delta sharing 旨在通过存储“once”并在任何地方读取它来解决这一问题。它使用中间件(增量共享服务器)在阅读器和数据提供者之间进行交流。

在我看来，理论上的 Delta 共享是自 Delta 格式以来最大的产品发布。然而，有一些问题值得一提:

*   尽管 Databricks 声称查询是优化的并且便宜，但我认为我们需要考虑出口/入口云的成本。如果数据接收者正在做一个没有优化的难看的查询，会发生什么？
*   只要全球采用，任何开放标准听起来都有希望。尽管如此，德尔塔显然比他们的其他酸性格式兄弟(如冰山和胡迪)更有吸引力，所以它绝对是最好的赌注。

# Delta 活动表

另一个 Delta Databricks 产品！您可以从一个增量表中将它视为一个超级强大的“视图”,您可以使用纯 SQL 或 python 进行处理。您可以创建整个数据流，并基于单个增量活动表创建多个表。delta live 引擎足够智能，可以进行缓存和检查点操作，只处理需要的内容。

这非常有趣，因为在数据湖架构(原始/青铜/白银)中，不是将数据的多个副本视为经典的数据管道，而是有一个真实的来源。这使得清晰的谱系成为可能，因此也是转换的良好文档。

此外，因为数据质量很热门(见下文)，Databricks 添加了他们自己的数据质量工具，带有声明性的质量期望。

# Unity 目录

Databricks 正在推出自己的数据目录。数据目录是数据行业的另一个趋势。主要开源项目的开发(如[阿蒙森](https://www.amundsen.io/)、[数据中心](https://datahubproject.io/)等)。)去年一直保持这个速度。与此同时，其他大型云提供商，如谷歌，也发布了他们自己的[数据目录](https://cloud.google.com/data-catalog)。最重要的是，随着公司越来越多地转向云计算战略，数据发现和治理成为一个更大的问题。

Databricks 用自己的目录解决的一个有趣的问题是，他们希望通过更高的 API 来简化数据访问管理。这是其他解决方案没有真正关注的，这是一个主要的优势，因为管理低级别的访问(例如基于文件的权限，如 s3 或 GCS)可能非常棘手。细粒度的权限是困难的，并且数据的布局并不真正灵活，因为它常常被像 Hive/Glue 这样的 metastore 所限制。

# 更多 python

数据档案(数据科学家、数据工程师、数据分析师、ML 工程师等)之间的最大共同点。)可能是 SQL，第二个可能是 python。根据 Databricks 的数据，如今大多数 spark API 调用都是通过 Python (45%)和 SQL (43%)完成的。

很明显，Databricks 希望缩小“笔记本电脑数据科学”和分布式计算之间的差距。降低准入门槛将使更多的用户从事人工智能，并为数据 SAAS 公司带来更多资金😉由于 python 被广泛采用，并且对初学者友好，所以投资 python 是有意义的。

大多数改进都被称为“禅计划”，其中包括:

*   pyspark 日志的可读性
*   类型提示改进
*   更智能的自动完成提及

## 熊猫天生就有火花

如果你不熟悉[考拉](https://github.com/databricks/koalas)项目，它是 Apache Spark 之上的熊猫 API。考拉项目将通过 Spark 合并。只要有 spark 数据框，就有 pandas 数据框，无需进行显式转换。

当然，对于小数据用例，Spark 在独立的节点集群上仍然是一个开销，但是它可以在不改变代码库的情况下进行扩展，这非常方便。

# 构建低代码工具以使数据工程/数据科学民主化

令人难以置信的是，今年有这么多关于 ETL 管道和数据质量框架的讨论。这又是对我去年谈到的趋势的双重押注。许多公司希望降低数据工程的准入门槛，其动机是

*   通过 ETL 降低增加可重用性的复杂性
*   元数据和配置驱动的 ETL。配置可以作为数据流的文档
*   使 SQL 开发人员更容易编写生产就绪的管道，并扩大贡献者的范围。

# 结论

很高兴看到 Databrick 的产品目录不断增加。感觉他们更倾向于整合战略，而不是试图成为下一个你可以运行一切的平台(即使这也是他们正在销售的)。

但是，围绕 delta 的主要产品也依赖于供应商的采用，所以让我们看看数据社区采用它的速度有多快！

*资源:*

[数据+AI 主题演讲，*https://www.youtube.com/playlist?list = PLTPXxbhUt-ywquxdhuxgu 8 ehj 3 bjbhfe*](https://www.youtube.com/playlist?list=PLTPXxbhUt-YWquxdhuhXGU8ehj3bjbHFE)

# 迈赫迪·瓦扎又名迈赫迪欧·🧢

感谢阅读！🤗 🙌如果你喜欢这个，**跟随我上**🎥 [**Youtube**](https://www.youtube.com/channel/UCiZxJB0xWfPBE2omVZeWPpQ) ，✍️ [**中型**](https://medium.com/@mehdio) ，或者🔗 [**LinkedIn**](https://linkedin.com/in/mehd-io/) 了解更多数据/代码内容！

**支持我写作** ✍️通过加入媒介通过这个[**链接**](https://mehdio.medium.com/membership)