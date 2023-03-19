# 构建用户事件数据管道

> 原文：<https://towardsdatascience.com/building-a-user-event-data-pipeline-f57ca088e766?source=collection_archive---------22----------------------->

## 使用定制数据管道控制您的数据从未如此简单

![](img/6e65fd5645a2a4f6656323a2b5a8b3f8.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[科学高清](https://unsplash.com/@scienceinhd?utm_source=medium&utm_medium=referral)拍摄的照片

如果您只能为您的企业存储一种类型的分析数据，那么应该是用户事件——他们查看了什么页面，他们点击了什么按钮，他们提交了什么表单，他们购买了什么，等等。这个基本的事件流是大多数数据分析的基础。

公司通常从像谷歌分析这样的一体化平台开始，但最终需要更加定制和灵活的东西。过去，构建定制数据管道需要大量的数据和工程资源，但最近托管服务和开源工具的爆炸式增长大大降低了障碍。现在，大多数公司都可以实现定制数据管道。

用户事件数据管道由几个部分组成。有数据仓库、事件跟踪系统、数据建模层和报告/分析工具。下面的部分将详细介绍每一个选项，并链接到一些流行的商业和开源选项。

# **数据仓库**

典型的应用程序数据库(MySQL、Postgres、MSSQL)针对读写混合查询进行了优化，这些查询一次插入/选择少量的行，可以相当好地处理高达 1TB 的数据。像 MongoDB 这样的非关系数据库更具可伸缩性，但是缺乏 SQL 支持对于大多数分析用例来说是一个障碍。另一方面，数据仓库为您提供了巨大的可伸缩性和完整的 SQL 支持。它们针对读取量大的工作负载进行了优化，这些工作负载会在大量的行中扫描少量的列，并且可以轻松扩展到数 Pb 的数据。

这个空间的主要玩家有 [**亚马逊红移**](https://aws.amazon.com/redshift/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)[**Google biqquery**](https://cloud.google.com/bigquery)[**雪花**](https://www.snowflake.com/) 。当您的数据工程师希望控制基础设施成本和调优时，Redshift 是最佳选择。当您的工作负载非常大时，BigQuery 是最好的。当你有一个更连续的使用模式时，雪花是最好的。Elvin Li 提供了一个很棒的[深度对比](https://medium.com/2359media/redshift-vs-bigquery-vs-snowflake-a-comparison-of-the-most-popular-data-warehouse-for-data-driven-cb1c10ac8555)如果你想了解更多细节，你应该去看看。

如果你更喜欢开源解决方案，还有 [**ClickHouse**](https://clickhouse.tech/) 。它是由俄罗斯搜索引擎 Yandex 创建的，并迅速受到欢迎。

# **事件跟踪系统**

理论上，当用户在你的应用程序或网站上做事情时，你可以实时地直接插入到你的数据仓库中，但这种方法不能很好地扩展，也不能容错。相反，您确实需要将事件排队并定期批量插入它们。自己做起来可能相当复杂，但幸运的是有许多托管服务。 [**段**](https://segment.com/) 是最受欢迎的选项，但也有一些缺点。它非常昂贵，容易受到广告拦截器的影响，一两个小时才同步一次数据，并且在它生成的模式中缺少一些关键字段(特别是会话和页面 id)。Freshpaint 是一种较新的商业替代品，旨在解决其中一些问题。

除了 Segment 和 Freshpaint 之外，还有许多开源选项(如果您不想自己托管，每个选项都有一个托管服务)。 [**扫雪机**](https://snowplowanalytics.com/) 是最老最受欢迎的，但是需要一段时间进行设置和配置。 [**舵栈**](https://rudderstack.com/) 是一款功能齐全的分段替代。Jitsu 是一个精简的事件跟踪库，专注于让事件尽可能快地进入你的仓库。

# **数据建模层**

原始事件流数据有时很难处理。数据建模就是将原始数据转换成更加用户友好的东西。当你第一次开始时，这一层并不是必需的，但是你肯定会想要添加，所以很高兴知道它的存在。这个领域实际上只有两个主要参与者，都是提供托管服务的开源工具。

[**Airflow**](https://airflow.apache.org/) 类似于 Unix cron 实用程序——你用 Python 编写脚本，调度它们每 X 分钟运行一次。Airflow 可用于任何类型的调度任务，但通常用于数据建模。天文学家提供有管理的气流服务。

[**dbt**](https://www.getdbt.com/) 则是专门为数据建模而建的。您用 SQL 而不是 Python 来编写转换，并且有对 CI/CD 管道的内置支持来测试您的模型，并在提交到产品之前对它们进行准备。

# **报告/分析工具**

这就是从一体化平台中脱离出来的巨大回报所在。一旦数据进入仓库，有数百种令人惊叹的工具可以与数据进行交互，您可以使用任意多或任意少的工具，而不用担心被供应商锁定。

有许多 SQL 报告生成器，您可以在其中编写 SQL 并创建可以共享的数据图表和图形。 [**Looker**](https://looker.com/) ， [**Mode**](https://mode.com/) ， [**PopSQL**](https://popsql.com/) ， [**GoodData**](https://www.gooddata.com/) 是流行的商业选项， [**Redash**](https://redash.io/) 是一个很好的开源替代方案。

还有一些不需要 SQL 知识的自助式拖放工具。有商业选项[**【Tableau】**](https://www.tableau.com/)[**power bi**](https://powerbi.microsoft.com/)[**指示性**](https://www.indicative.com/) 以及开源 [**Apache 超集**](https://superset.apache.org/) 和 [**元数据库**](https://www.metabase.com/) 。

如果你有一个数据科学团队，他们可能会使用 [**Jupyter 笔记本**](https://jupyter.org/) 直接与仓库交互。在需要时深入原始数据的灵活性对于在您的公司建立强大的数据文化是绝对重要的。

此外，还有一大堆专门的工具。 [**DataFold**](https://www.datafold.com/) 监控您的仓库，并在出现任何异常时向您发出警报(例如，如果在部署后结账转换率突然下降)。 [**Hightouch**](https://www.hightouch.io/) 可让您将数据从仓库同步到营销和销售平台。 [**Whale**](https://github.com/dataframehq/whale) 是一个开源工具，可以记录和编目你的数据。 [**重组**](https://retool.com/) 允许您将仓库数据集成到您的内部管理工具中。

最后，是时候来个无耻的插播了。使用用户事件数据的最令人兴奋的方式之一是用于 A/B 测试。我一直在开发一个名为 [**Growth Book**](https://www.growthbook.io/) 的开源平台，它可以插入你的数据仓库，处理健壮的 A/B 测试分析所需的所有复杂的查询和统计。如果你认为这是你可能会想用的东西，那就去看看，并在 GitHub 上给它一颗星。

# 结论

现在是构建用户事件数据管道的最佳时机。近年来，托管服务和开源工具的数量呈爆炸式增长，这使得任何公司都可以轻松地在一天内完成一些工作。不再有任何借口不控制您的数据并使用它来推动业务决策。