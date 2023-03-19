# 使用时间系列数据库的案例

> 原文：<https://towardsdatascience.com/the-case-for-using-timeseries-databases-c060a8afe727?source=collection_archive---------17----------------------->

![](img/400bf6882d5a6146059be8375a05d185.png)

卢克·切瑟在 [Unsplash](https://unsplash.com/s/photos/chart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 数据工程

## 时间序列数据库简介— InfluxDB、TimescaleDB 和 QuestDB

*本文最后更新于 2021 年 9 月 1 日。*

基于特定的业务需求和用例，大量的新数据库从关系数据库发展而来。从内存中的键值存储到图形数据库，从地理空间数据库到时间序列数据库。所有这些不同类型的数据库都有特定的用途，在这种情况下，使用关系数据库的一般解决方案不是很有效。

尽管有许多不同类型的数据库，但这里我们将着眼于时间序列数据库——处理时间序列数据所需的数据库。

> 由某一时间间隔内对某事物的连续测量组成的数据是时间序列数据。

随着金融交易的现代化和物联网的出现，对时间序列数据库的需求显而易见。股票和加密货币的价格每秒都在变化。为了测量这些不断变化的数据并对其进行分析，我们需要一种高效的方法来存储和检索数据。

随着物联网设备在我们生活中的空前渗透，物联网设备产生的数据每天都在增加。无论是您汽车的诊断，还是您房子的温度读数，或者是您走失的狗的 GPS 定位，物联网设备无处不在。

物联网设备只能做一件事。通过设备上的传感器捕捉信息，并将其发送到服务器进行存储。由于现有的通信协议对于这种轻量级、高频数据、流数据来说过于复杂，因此 [MQTT](https://mqtt.org) 被开发来解决物联网的消息传递。

> 时间序列数据有两种类型—常规(通常基于测量)和非常规(通常基于事件)。

但时序数据并不局限于物联网；它也渗透了整个互联网。捕捉搜索引擎查询、标签、社交媒体帖子的病毒式传播等趋势也会生成时间序列数据。这还没有结束。在一个由软件驱动的世界中，安全性和合规性的日志记录和审计是必不可少的。所有这些数据也可以归类为时间序列数据。

时间序列数据库是专门为处理从一个或多个上述来源捕获、存储和分析时间序列数据而设计的。因此，为了简单起见，让我们将时间序列数据定义为

*   **难以置信的高容量**(来自测量的连续数据)
*   一个**自然时间顺序**(时间是一个基本维度)
*   **作为整个数据集**(比单个记录)更有价值

给定这些信息，时序数据库应该能够存储大量数据，并具有大规模记录扫描、数据分析和数据生命周期管理的能力。如前所述，传统的事务数据库虽然可以用来存储、检索和处理时间序列数据，但不能充分利用现有资源。

> 具体问题需要具体解决。

现在，随着公司意识到这一事实，他们已经开始使用专门的数据库来解决特定的问题。这又回到了我在这篇文章开始时谈论的话题。在所有其他数据库中，时间序列数据库在过去两年中的采用率较高(截至 2020 年 12 月的[数据](https://db-engines.com/en/ranking_categories))。

时间序列数据库使用量增长约 2.5 倍的主要原因可以归结为云技术和数据技术的融合，以及从以前不常见的地方捕获数据的能力，例如汽车发动机、冰箱、数十亿设备的位置数据等。除了新的数据源，公司还意识到一些旧的数据源根本不适合事务数据库。所有这些都有助于更广泛地采用时间序列数据库。

# 时间序列数据库

时间序列数据库的存在是合理的，让我们看看如果你想尝试时间序列数据库，你可以有哪些不同的选择。更完整的时间序列数据库列表可以在 [DB-engines 网站](https://db-engines.com/en/ranking/time+series+dbms)上找到。我就说其中的三个。

## [时间刻度 B](https://www.timescale.com/)

作为时间序列的 PostgreSQL，它能很快吸引你的注意力。默认情况下，PostgreSQL 对任何事情都是一种恭维。借助 hypertables 和 chunks 等新的架构构造，TimescaleDB 在插入方面实现了超过 15 倍的改进，在查询性能方面也有了实质性的提高。点击阅读更多关于[的内容。](https://blog.timescale.com/blog/time-series-data-why-and-how-to-use-a-relational-database-instead-of-nosql-d0cd6975e87c/#2362)

虽然主要的云提供商没有完全集成的云中 TimescaleDB 解决方案，但就像大多数其他时间序列数据库一样，TimescaleDB 可以在所有这些数据库上无缝运行。例如，如果您的基础设施在 AWS 中，并且您不想在时间刻度云中运行您的 TimescaleDB 实例，您可以使用 EC2 实例来安装官方的 TimescaleDB AMI，或者您可以使用 AWS Elastic Kubernetes 服务，使用[官方掌舵图](https://github.com/timescale/timescaledb-kubernetes)。

在下面的视频中，Mike Freedman 谈到了对时序数据库的需求，以及他们如何围绕 PostgreSQL 构建 TimescaleDB。

**时间尺度 b。这篇演讲激发了我写这篇文章的灵感。**

## [InfluxDB](https://www.influxdata.com/)

与受 PostgreSQL(一种关系数据库)启发的 TimescaleDB 不同，这是一个从零开始编写的 NoSQL 时间序列数据库。虽然 TimescaleDB 的优势是站在被广泛接受和推崇的关系数据库的肩膀上，但 InfluxDB 走了一条不同的道路。InfluxDB 是顶级的时间序列数据库之一，但是根据 TimescaleDB 的研究，它在许多领域都没有打败 TimescaleDB。

如果你想进行一次有趣的阅读，并想在你的系统上安装这两个数据库来自己找出答案，那么就去看看 Oleksander Bausk 今天在他的博客上发表的有趣的比较吧。

<https://bausk.dev/a-practical-comparison-of-timescaledb-and-influxdb/>  

话虽如此，InfluxDB 有一套很棒的特性。除了查询语言 InfluxQL 和 Flux，InfluxDB 还开发了一种干净、轻量级、基于文本的协议，用于向数据库写入点。值得称赞的是，这已经被其他时间序列数据库如 QuestDB 所采用。

与 TimescaleDB 一样，InfluxDB 也提供了现成的云解决方案，但您仍然可以决定在其中一个云平台上运行 InfluxDB。例如，如果您在 AWS 上运行它，您将获得对 CloudWatch metrics、Grafana、RDS、Kinesis 等的原生支持。总而言之，一个非常好的数据库。由于它相当新，很难说它能与更基于关系数据库的时间序列数据库竞争得多好。

## [QuestDB](https://questdb.io/)

QuestDB 是时间序列数据库列表中的一个新成员，来自最新一批 YCombinator。QuestDB 的一些主要优势是列存储、低内存占用、时序关系模型的使用以及可伸缩的无模式接收。与大多数时间序列数据库类似，QuestDB 还使用官方 AMIs 和 Kubernetes Helm 图表在 AWS 上提供[云部署选项。](https://questdb.io/docs/operations/deployment/)

**QuestDB，**的首席技术官 Vlad Ilyushchenko 在 GitLab 畅谈 QuestDB 的架构。

QuestDB 还采用了 InfluxDB Line 协议进行接收，而不用担心随着数据结构的变化而改变模式。作为一个列数据库，QuestDB 无缝地处理新列的创建，因此支持无模式接收。我最近在另一篇文章中写了这一点。

</schemaless-ingestion-in-questdb-using-influxdb-line-protocol-18850e69b453>  

虽然在早期，QuestDB 几乎完全支持 ANSI SQL，并对 SQL 方言进行了一些补充，但它创造了一系列完全独特的功能，使其成为一种可行的替代方案，可能比市场上其他一些主要数据库更好。

# 结论

虽然还有其他几个数据库，但我现在只谈了这三个。从公开数据来看，时序数据向时序数据库的转移是显而易见的。越来越多的公司将开始使用时间序列数据库作为其数据库堆栈的一部分，不一定取代关系数据库，但增加了其数据能力。这就是为什么今年不仅对于时间序列数据库，而且对于市场上出现的所有其他专门的数据库都特别令人兴奋的原因。