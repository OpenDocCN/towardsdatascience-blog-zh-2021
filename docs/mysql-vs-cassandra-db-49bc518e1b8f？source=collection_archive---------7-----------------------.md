# MySQL vs Cassandra DB

> 原文：<https://towardsdatascience.com/mysql-vs-cassandra-db-49bc518e1b8f?source=collection_archive---------7----------------------->

## 另一个 RDBMS 和 NoSQL 摊牌…

![](img/660566609166320589490e73e5c6af18.png)

丹尼尔·库切列夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在我上一篇 MySQL vs 的文章中，我谈到了 Redis，这是一个我以前没有听说过的数据库。这一次，我想谈谈一个我听说过但没有时间试用的数据库。Cassandra DB 是另一个 NoSQL 数据库，我没有机会尝试，但经常在黑客马拉松这样的活动中听说它。除了是一个 NoSQL 数据库，Cassandra 也是开源的，它遵循我通常选择写的文章。但除此之外，我没有其他关于它的背景知识。

因此，对于另一篇 MySQL vs 文章，让我们更深入地了解一下 Cassandra DB 是什么。就像我们在以前的文章中所做的那样，它主要是管理性的，不包括语法。但是我们也要看看卡桑德拉的一些利弊。我们也可以比较一下 MySQL 的优缺点，但它们将与我们对 Cassandra 的了解直接相关，因为我们已经在本系列中讨论了很多关于 MySQL 的内容。所以，不再耽搁，让我们来学习一下什么是 Cassandra DB。

# **什么是卡珊德拉**

Cassandra 是 Apache 的一个开源数据库。这是 NoSQL，所以也很轻。Cassandra 也是一个分布式数据库。分布式数据库运行在多台机器上，但是对于用户来说，它看起来就像只有一台机器，因为他们是一个统一的整体。这通过多个节点发生，每个节点代表 Cassandra 的一个实例。从那里，节点相互通信以分配工作负载，从而改进功能。如果这个节点逻辑听起来很熟悉，那是因为 Cassandra 被设计成易于组织成一个集群。在那里，如果您愿意，您可以拥有多个数据中心。

Cassandra 在可伸缩性方面也很灵活。因为 Cassandra 是如此动态，您可以根据需要增加或缩小数据库。但这不像 MySQL，MySQL 有很长的停机时间，最终会再次达到上限。相反，Cassandra 允许更多的动态扩展，这意味着您需要做的只是添加更多的节点来增加大小、容量，甚至 CPU 能力或相关的 RAM。这意味着需要很少甚至没有停机时间，如果您过度，您可以轻松地缩减。

# **开源数据库**

正如我们过去讨论过的，MySQL 和 Cassandra 都是开源的。关于 MySQL，前几篇文章我们讨论了 MySQL 提供的专有软件。这当然是一种付费服务，具有额外的支持和功能。对于 Cassandra，我在他们的开源文档中找到了信息，但我找不到任何关于它有付费功能或专有代码的信息。如果这是不正确的，请让我在评论中知道，但从我所看到的，卡珊德拉是真正的开源。

# **数据库功能**

所以，首先，让我们谈谈数据库的结构。如你所知，MySQL 是一个 RDBMS(关系数据库管理系统)。然而，卡珊德拉是一个 NoSQL 数据库。这意味着 MySQL 将更多地遵循主/工人架构，而 Cassandra 则遵循对等架构。

我们已经知道 MySQL 支持 ACID(原子性、一致性、可靠性和持久性)事务。然而，Cassandra 不会自动跟踪 ACID 交易。这并不意味着这是不可能的。虽然最初没有提供，但是您可以调整 Cassandra 的特性来支持 ACID 属性。例如，调整 Cassandra 的复制和容错功能可以确保可靠性。另一个例子是调整一致性。Cassandra 是一个 AP (Available Partition-tolerant)数据库，但是您可以将一致性配置为基于每个查询。

当我们考虑可伸缩性时，MySQL 更普遍地支持垂直伸缩。通过复制或分片，水平扩展也是可能的。另一方面，Cassandra 支持水平和垂直可伸缩性。虽然这个更具体一点，但是我们也来看看 Read 事务的性能。但是首先，我们需要查看连接来理解逻辑。如您所知，MySQL 或任何 RDBMS 都支持查询中多个表之间的连接。另一方面，卡桑德拉不鼓励加入。相反，它更喜欢每次查询只从一个表中选择。因此，因为在一次 MySQL 读取中可以连接多个表，所以性能为 O(log(n))。一次只读取一个表，Cassandra 的性能是 O(1)。当查看 Write 语句时，MySQL 的性能可能会降低，因为在写入之前执行了搜索。Cassandra 不使用搜索，而是使用 append 模型，这在编写时提供了更高的性能。

# **行政**

可能是给定的，但是 MySQL，因为它是 RDBMS，支持参照完整性，并且有外键。因为 Cassandra 是一个 NoSQL 数据库，它不实施参照完整性，因此没有外键。

为了确保分布式系统中的一致性，MySQL 提供了即时一致性方法，但这是唯一提供的类型。Cassandra 允许即时一致性方法和最终一致性方法。

就操作系统而言，MySQL 用于 FreeBSD、Linux、OS X、Solaris 和 Windows。然而，Cassandra 只在 BSD、Linux、OS X 和 Windows 上受支持。正如我们所知，MySQL 也是用 C 和 C++语言编写的。另一方面，Cassandra 只使用 Java 编写。MySQL 也是 Oracle 开发的，其中 Cassandra 是 Apache 软件开发的。

# **卡珊德拉的优势**

正如我们在描述 Cassandra 时谈到的，它的可伸缩性是一个很大的优势。这是因为它可以在不停机的情况下快速完成，因为您不必关闭数据库来进行扩展。水平和垂直可伸缩性都是一个选项，因为 Cassandra 使用线性模型来获得更快的响应。

除了可伸缩性，数据存储也很灵活。因为它是一个 NoSQL 数据库，所以可以处理结构化、非结构化或半结构化数据。同样的，数据分布也很灵活。可以使用几个不同的数据中心，这使得分发数据更加容易。

性能是我们讨论的另一个因素。我们将在这里讨论的好处是它如何处理同时读写语句。即使是多个写请求也能得到快速处理，不会影响读请求。

使用 Cassandra 的另一个好处是简单的语言，CQL (Cassandra 查询语言)，它是作为一个替代 SQL 提供的。卡珊德拉也有权力下放的好处。这意味着，由于节点的结构，不会有单点故障。如果一个节点发生故障，另一个节点可以检索相同的数据，因此数据仍然可用。

# **卡珊德拉的缺点**

Cassandra 的一个缺点是，因为它是 NoSQL，没有结构化的 SQL 语法，所以会有一系列 Cassandra 没有的功能。例如，没有实施参照完整性、子查询(GROUP BY、ORDER BY 等。)，甚至是加入。Cassandra 的查询能力有限，也不支持聚合。此外，读取请求可能运行缓慢。我们提到写请求可以运行得很快，但是多个读请求会延迟结果并且运行得更慢。

使用预测查询在 Cassandra 中对数据建模。这意味着存在重复数据的可能性。尤其是卡桑德拉是 NoSQL，你可能必须处理重复的数据，因为它不会像 MySQL 或其他 SQL 语言那样被自动拒绝。

# **与 Cassandra 相比，MySQL 的优势**

与 Cassandra 相比，MySQL 的最大优势在于它是一个 RDBMS。首先，我们讨论的是连接、聚合和其他功能，比如实施参照完整性。还有更灵活的查询，您可以创建任何组合来产生不同的结果。然而，MySQL 比其他一些 SQL 系统更灵活，因此对 SQL 标准的遵从是有限的。

MySQL 也试图防止重复记录，而 Cassandra 没有。不仅如此，MySQL 还兼容 ACID，这可能是您数据库需要的额外结构。

# **MySQL 的缺点**

与 Cassandra 相比，第一个主要缺点是伸缩时的灵活性。虽然 MySQL 可以扩展，但是会有停机时间，即使这样，您仍然会遇到瓶颈。但是，由于查询中的所有潜在组合，查询速度也会变慢。例如，如果您正在连接多个表，无论是读请求还是写请求，都可能会降低结果的速度。

MySQL 也只是部分开源。使用付费版本时，涉及到专有代码和额外的支持。

# **结论**

在今天的 MySQL 对比系列中，我们看了 NoSQL 数据库 Cassandra。Cassandra 是另一个轻量级、开源、高度可伸缩的数据库，越来越受欢迎。它是一个 Apache 软件，设计为在一系列节点之间作为分布式数据库运行。这意味着可以有多个数据中心，如果您想要扩展，只需添加或删除节点。尽管 Cassandra 灵活而有用，但它也不遵循标准的 SQL 实践，例如强制引用完整性，并且它鼓励用户编写单独的查询，而不是不支持的连接。

如果你对卡珊德拉有更多的了解，或者想分享你的经历，请随时发表评论。我希望这些信息对你有用，或者至少是一篇有趣的文章。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

[](https://medium.com/codex/libreoffice-on-docker-1a64245468c) [## 码头上的图书馆

### 开源的 Office365

medium.com](https://medium.com/codex/libreoffice-on-docker-1a64245468c) [](/sql-relationships-with-sqlalchemy-ee619e6f2e8f) [## SQL 与 SQLAlchemy 的关系

### 手动配置如何简化查询

towardsdatascience.com](/sql-relationships-with-sqlalchemy-ee619e6f2e8f) [](/mysql-vs-redis-def3287de41) [## MySQL vs Redis

### DBMS 与内存中数据存储的比较

towardsdatascience.com](/mysql-vs-redis-def3287de41) [](https://python.plainenglish.io/searching-for-text-in-those-annoying-pdfs-d95b6dc7a055) [## 使用 Python 搜索 PDF 中的文本

### 没有人喜欢使用 pdf，但是我们必须…

python .平原英语. io](https://python.plainenglish.io/searching-for-text-in-those-annoying-pdfs-d95b6dc7a055) [](/daas-data-as-a-service-78494933253f) [## DaaS —数据即服务

### 它是什么，为什么它是下一件大事？

towardsdatascience.com](/daas-data-as-a-service-78494933253f) 

参考资料:

[](https://cassandra.apache.org/_/cassandra-basics.html) [## Apache Cassandra | Apache Cassandra 文档

### Cassandra 是一个 NoSQL 分布式数据库。按照设计，NoSQL 数据库是轻量级的、开放源码的、非关系型的，并且…

cassandra.apache.org](https://cassandra.apache.org/_/cassandra-basics.html) [](https://www.educba.com/cassandra-vs-mysql/) [## Cassandra 与 MySQL |大有价值的详细对比

### 在这种数据中，Cassandra vs MySQL 就是一切，随着今天数据量的激增，选择正确的数据库…

www.educba.com](https://www.educba.com/cassandra-vs-mysql/) [](https://www.geeksforgeeks.org/difference-between-cassandra-and-mysql/) [## Cassandra 和 MySQL 的区别

### 1.Cassandra : Cassandra 是一个免费的开源、分布式、宽列存储、NoSQL 数据库管理系统…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/difference-between-cassandra-and-mysql/) [](https://cloudinfrastructureservices.co.uk/cassandra-vs-mongodb-vs-redis-vs-mysql-vs-postgresql-pros-and-cons/) [## Cassandra vs . MongoDB vs . Redis vs . MySQL vs . PostgreSQL

### 无论何时你在开发一个软件应用程序，保存你的数据是你面临的第一个挑战…

cloudinfrastructureservices.co.uk](https://cloudinfrastructureservices.co.uk/cassandra-vs-mongodb-vs-redis-vs-mysql-vs-postgresql-pros-and-cons/)