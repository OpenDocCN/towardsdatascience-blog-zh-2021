# 开始使用 Elasticsearch 需要掌握的四件事

> 原文：<https://towardsdatascience.com/four-things-you-need-to-master-to-get-started-with-elasticsearch-c51bed6ae99d?source=collection_archive---------23----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 弹性研究概念和原则的简明概述

![](img/a3afb4e3f59ac31772637d5070aa6e12.png)

由[马修·施瓦茨](https://unsplash.com/@cadop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

elastic search(ES)是一个为可伸缩性和冗余性而设计的分布式搜索引擎。ES 近年来变得越来越受欢迎，因为它在[机器学习、存储和处理大量数据以进行分析以及许多其他应用](https://www.elastic.co/guide/en/elasticsearch/reference/current/elasticsearch-intro.html)方面具有健壮性和可扩展性。在这篇博文中，我们将讨论数据从业者在开始使用 ES 时需要掌握的四件事情:提供 ES 集群、设计索引、编写查询和优化索引。

# 1.设置 ES 集群

首先，您需要知道如何设置一个 ES 集群。

Elasticsearch 是一个[分布式搜索和分析引擎，是 Elastic Stack](https://www.elastic.co/guide/en/elasticsearch/reference/current/elasticsearch-intro.html) 的一部分。针对 es 索引运行的查询以及这些索引中的数据分布在各个节点上。当您建立一个 Elasticsearch 集群时，您添加这些[不同的节点(即服务器)](https://www.elastic.co/guide/en/elasticsearch/reference/current/scalability.html)。ES 自动处理集群中节点间的查询和数据分布。这确保了可伸缩性和高可用性。

您可以通过众多托管服务提供商之一提供 ES 集群，包括[亚马逊网络服务(AWS)亚马逊弹性搜索服务](https://aws.amazon.com/elasticsearch-service/)、 [Bonsai](https://bonsai.io/) 或[Elastic.co](https://www.elastic.co/)。你也可以在你的机器上本地运行 ES。关于如何以及在哪里提供、部署和管理集群的任何决定都取决于您正在构建哪种解决方案以及您打算将 ES 用于什么目的(下面将详细介绍)。

顺便说一下:AWS 最近(2021 年 4 月)发布了自己的“OpenSearch”项目——Kibana 的 Elasticsearch 的开源分支。这个项目包括 Opensearch(基于 Elasticsearch 7.10.2)和 OpenSearch 仪表盘(基于 Kibana 7.10.2)。AWS 计划将其提供的 Elasticsearch 和 OpenSearch 合并为一个新名称:亚马逊 OpenSearch 服务。

[*如果你想了解更多关于建立一个 ES 集群的知识，请查看我之前的博客文章，获得关于如何使用 Bonsai*](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) *提供免费 ES 集群的实践指导。*

[](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [## 使用 Python 创建和管理弹性搜索索引

### 从 CSV 文件创建 ES 索引以及使用 Python Elasticsearch 管理数据的实践指南…

towardsdatascience.com](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) 

# 2.设计指数

其次，设计 es 索引(即定义映射)是掌握 ES 的关键。这里，几个概念是关键:文档、字段、索引、映射、节点和碎片。

## 文档、字段和索引

Elasticsearch 中的数据以名为“ [**文档**](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/documents-indices.html) ”的 JSON 对象的形式存储在 [**索引**](https://www.elastic.co/blog/what-is-an-elasticsearch-index) 中。文档包含**字段**。这些字段是可以包含值(例如字符串、整数或布尔值)或嵌套结构的键值对。**索引**，反过来，表示两件事:它们是[遵循一个模式的数据的逻辑分组，但是它们也是*通过碎片*](https://www.elastic.co/guide/en/elasticsearch/reference/current/scalability.html)对数据的物理组织——把我们带到 es 中的下一个关键概念:节点和碎片。

## 节点和碎片

当我们说 ES 是为冗余而构建时，我们实际上指的是什么？具体来说，ES 框架通过节点和碎片，以及主碎片和副本来工作。每个索引由一个或多个物理碎片组成。这些物理碎片形成了一个逻辑组，其中每个碎片都是一个“独立的索引”。

存储在索引中的数据(即文档)跨**碎片**进行分区，而碎片分布在各个节点上。[每个文档由一个“主碎片”和一个“副本碎片”组成](https://www.elastic.co/guide/en/elasticsearch/reference/current/scalability.html)。这种设计意味着数据驻留在多个位置，因此可以快速查询，并且可以从多个位置(即碎片)获得。关键组件的这种重复正是 es 设计冗余的原因。

## 选择正确的碎片数量和尺寸

节点和分片的设计还有一个优点，即不同分片中的数据可以并行处理。通常，在 CPU 和内存允许的范围内，可以通过添加更多的碎片来提高搜索速度。然而，添加碎片会带来一些开销，所以仔细考虑集群中合适的碎片数量(及其大小)是很重要的。

## 定义您自己的 ES 映射

编写好的 ES 映射需要一些实践(尽管那些熟悉在其他数据库框架中设计模式的人可能会发现转换相当容易)。在这里，重要的是要学会定义自己的映射，而不是使用动态字段映射，并且要知道字段类型的选择对索引大小和查询灵活性的影响。

Elasticsearch 有一个特性叫做“[动态字段映射](https://www.elastic.co/guide/en/elasticsearch/reference/current/dynamic-field-mapping.html)”。启用后，当您将文档写入索引时，动态字段映射会自动推断文档的映射。但是，这不一定会得到您想要的结果，因为它可能不会选择最佳的字段类型。您通常希望定义自己的映射，因为字段类型的选择决定了索引的大小和查询数据的灵活性。

## 文本与关键字字段类型

例如，您可以将`text`字段类型用于字符串值。使用这种字段类型，当您的文档被索引时，字符串被分解成单独的术语，从而在查询时提供了部分匹配的灵活性。字符串值的另一个选项是`keyword`字段类型。但是，这种类型在索引时不会被分析(或者说:“标记化”)，并且会将您的查询选项限制为精确匹配。

## 我的数组字段呢？

编写映射时需要注意的另一件事是 ES [没有数组字段类型](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)。如果您的文档包含数组，您必须使用该数组中值的类型。例如，对于整数数组，正确的 ES 字段类型是`integer`(见[本页](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)对 ES 字段类型的完整概述)。

*要深入了解如何定义 es 映射和填充索引，* [*查看我之前关于用 Python 创建和管理 ES 索引的博客文章的第一部分。*](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113)

[](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [## 使用 Python 创建和管理弹性搜索索引

### 从 CSV 文件创建 ES 索引以及使用 Python Elasticsearch 管理数据的实践指南…

towardsdatascience.com](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) 

# 3.编写查询

你需要学习的第三件事是编写有效的查询。这里需要掌握几个概念:DSL、相关性分数和过滤与查询。

## 使用领域特定语言(DSL)进行搜索

Elasticsearch 完全是关于搜索的(嗯，大部分是)。对 Elasticsearch 的查询是用 [Elasticsearch 领域特定语言(DSL)](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html) 编写的。DSL 是查询的所谓[“抽象语法树(AST)”，基于 JSON。当执行一个查询时，ES 会计算一个**相关性分数**，这个分数](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)[会告诉我们文档与查询的匹配程度](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html#relevance-scores)。ES 返回的结果(即文档)按相关性分数、**、**排序[，相关性分数在结果的`_score`字段中找到。](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html#relevance-scores)

DSL [包括两类条款](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)。第一种类型是**叶查询子句**，用于搜索特定字段中的精确值(例如，当[使用](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html) `[term](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)` [查询来查找与精确值](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)对应的文档时)。第二种类型是**复合查询子句** —用于以逻辑方式组合多个查询的查询(可能是不同的类型，包括叶查询和复合查询)。复合查询子句也可以用来改变这些查询的行为。

## 查询和过滤上下文

ES 查询[可以包括**查询**和**过滤器**上下文。](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html)过滤上下文用于排除与您在语法中设置的特定条件不匹配的文档。因此，查询和过滤上下文与相关性分数也有不同的“关系”:过滤上下文不影响它，而布尔上下文*对它的值有贡献。*

决定是使用过滤器还是查询上下文是编写有效查询的重要部分。一般来说，由于[频繁使用的过滤器被自动缓存以增强性能](https://www.elastic.co/guide/en/elasticsearch/reference/7.12/query-filter-context.html)，过滤器更便宜(计算上)。然而，过滤器用于搜索明确的条件，即[有明确的“是”或“否”答案](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)。例如，如果您想要识别特定字段具有精确数值的文档，则筛选器非常有用。

在其他情况下，什么“匹配”你的搜索条件的问题就不那么明确了。对于这些“不明确”的情况，应该使用查询上下文。这种搜索情况的一个例子是，当您使用匹配查询在包含图书元数据的索引中查找部分书名时(我们假设您已经忘记了要查找的图书的确切书名)。这里，不同的文档(具有不同的标题字段值)可能比其他文档与您的搜索更相关。在这个具体的例子中，我们正在处理全文搜索，也就是说，对[搜索分析过的文本字段](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html)的查询(“匹配”查询是全文搜索的[标准查询](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html))。

*关于编写 es 查询和使用* [*Python Elasticsearch 客户端*](https://elasticsearch-py.readthedocs.io/en/6.8.2/) *，* [*查询 ES 的深入概述，请查看我之前关于使用 Python*](/getting-started-with-elasticsearch-query-dsl-c862c9d6cf7f) *创建和管理 ES 索引的博文。*

[](/getting-started-with-elasticsearch-query-dsl-c862c9d6cf7f) [## Elasticsearch 查询 DSL 入门

### 使用 Python Elasticsearch 客户端，用特定领域语言编写 Elasticsearch 查询的实践指南

towardsdatascience.com](/getting-started-with-elasticsearch-query-dsl-c862c9d6cf7f) 

# 4.优化您的指数

最后，你需要掌握优化 es 指数的策略。

不同的指数可能服务于不同的目的，因此需要特定的优化策略。例如，如果您使用 ES 来处理和存储大量数据，您可能需要优化磁盘使用。在这里，您需要学习如何有效地定义映射(例如，避免使用动态映射，并使用尽可能小的字段类型)，以及如何使用带有[索引生命周期管理(ILM)](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html) 和[自动索引翻转](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started-index-lifecycle-management.html)的热-暖-冷架构来优化成本。

对于其他应用程序，您可能需要优化*搜索速度，*在这种情况下，您可能需要研究[索引排序，获得更快的驱动器或更快的 CPU(取决于搜索的类型)，或者更有效的文档建模(例如，避免连接)](https://www.elastic.co/guide/en/elasticsearch/reference/master/tune-for-search-speed.html)。

*要深入了解优化您的 ES 磁盘使用指数，请查看我之前的博客文章* [*在 Elasticsearch*](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) *中优化磁盘使用。*

[](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) [## 优化弹性搜索中的磁盘使用

towardsdatascience.com](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) 

Elasticsearch 是一个非常通用的框架:它可以[用于存储、机器学习以及分析日志和事件](https://www.elastic.co/elasticsearch/features)等许多用途。毫无疑问，ES 是添加到您的数据科学工具包中的一个有用的框架。在这篇博文中，我们介绍了 Elasticsearch 的一些核心概念和原则，以帮助您开始在自己的应用程序中使用 es。

感谢阅读！

[](https://medium.com/@ndgoet/membership) [## 用我的推荐链接加入媒体。

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@ndgoet/membership) 

**如果你喜欢这篇文章，这里还有一些你可能喜欢的文章:**

[](/how-to-build-a-relational-database-from-csv-files-using-python-and-heroku-20ea89a55c63) [## 如何使用 Python 和 Heroku 从 CSV 文件构建关系数据库

### 通过三个简单的步骤免费构建您自己的 PostgreSQL 数据库

towardsdatascience.com](/how-to-build-a-relational-database-from-csv-files-using-python-and-heroku-20ea89a55c63) [](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [## 使用 Python 创建和管理弹性搜索索引

### 从 CSV 文件创建 ES 索引以及使用 Python Elasticsearch 管理数据的实践指南…

towardsdatascience.com](/creating-and-managing-elasticsearch-indices-with-python-f676ff1c8113) [](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) [## 优化弹性搜索中的磁盘使用

towardsdatascience.com](/optimising-disk-usage-in-elasticsearch-d7b4238808f7) [](/getting-started-with-elasticsearch-query-dsl-c862c9d6cf7f) [## Elasticsearch 查询 DSL 入门

### 使用 Python Elasticsearch 客户端，用特定领域语言编写 Elasticsearch 查询的实践指南

towardsdatascience.com](/getting-started-with-elasticsearch-query-dsl-c862c9d6cf7f) 

***免责声明****:“elastic search”和“Kibana”是 Elasticsearch BV 在美国和其他国家注册的商标。本文中对任何第三方服务和/或商标的描述和/或使用不应被视为对其各自权利持有人的认可。*

*在依赖* [*中的任何内容之前，请仔细阅读*](/@ndgoet) [*本免责声明*](https://medium.com/@ndgoet/disclaimer-5ad928afc841) *我关于 Medium.com 的文章* *。*