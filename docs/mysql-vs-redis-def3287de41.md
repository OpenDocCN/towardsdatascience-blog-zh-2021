# MySQL vs Redis

> 原文：<https://towardsdatascience.com/mysql-vs-redis-def3287de41?source=collection_archive---------12----------------------->

## DBMS 与内存中数据存储的比较

![](img/800612fc4e39c6bccdea4e6179e1dbf3.png)

照片由[王思然·哈德森](https://unsplash.com/@hudsoncrafted?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

需要改变一下节奏，我决定回到 MySQL 系列。我已经将 MySQL 与许多其他流行的数据库进行了比较，但是看看前十名，我仍然没有涉及 Redis。不可否认，Redis 是另一个我以前没有听说过的资源。好吧，不完全正确。我听说过此事，但只是路过。只有名字和一些基本信息。例如，我对 Redis 唯一真正的背景知识是它是一个 NoSQL 数据库。但是我很想了解更多的细节，或者至少在 Redis 上获得更多的知识。目前，Redis 将是我们了解更多数据库。

因此，在我们典型的 MySQL vs 格式中，让我们先来看看 Redis 是什么，以及它的一个小背景。然后，我们可以比较 MySQL 和 Redis 之间的一些总体差异。

# **什么是 Redis？**

Redis 是一种数据结构存储，可以用作数据库、缓存甚至消息代理。存储结构是开源的和内存中的。它的总部也在 NoSQL。这意味着它允许像字符串、散列、列表、集合、数据集排序、位图等数据结构。它具有 NoSQL 数据库的优势和能力。Redis 也有事务、不同级别的磁盘持久性和内置复制。

Redis 还允许您运行原子操作，比如增加散列中的值，甚至推送列表中的元素。它也是根据 BSD 许可的。因为 Redis 在内存中，所以您必须考虑是否要备份或“持久化”数据。如果您选择不这样做，您仍将拥有联网的内存缓存。但是，如果您选择备份数据，您将决定是将数据集转储到磁盘，还是将命令追加到基于磁盘的日志。但是关于背景已经足够了，让我们看看 Redis 和 MySQL 之间的区别，这样我们可以更好地理解数据库。

# **数据库功能**

首先，让我们看看每一个都是用什么语言写的，作为一般背景。正如我们之前可能谈到的，MySQL 是用 C 和 C++编写的。然而，Redis 是用 ANSI 和 C 语言编写的。但是我们将讨论的最重要的区别是数据库的类型。我们知道，MySQL 是一个关系数据库。Redis 是一个 NoSQL 数据库，它是一个键值存储。在键值数据库中，数据像 JSON 对象一样存储。有一个键，这个键对应一个值。与其说是一份记录，不如说是一份文件。最大的区别在于，您不必像在关系数据库中那样关联表。相反，可以有大量灵活的文档集合或多个集合，它们不需要有任何公共链接。这意味着您不需要传递外键来使数据库工作。这也意味着 MySQL 实施参照完整性，而 Redis 没有。

在 MySQL 数据库中，它遵循一个定义好的模式。这是一个固定的模式。在 Redis 数据库中，数据模式是免费的。Redis 也不支持触发器，而 MySQL 允许触发器。MySQL 支持 XML 数据格式，而 Redis 不支持。当涉及到索引时，两者都允许它们。然而，MySQL 支持二级索引，没有任何限制，而 Redis 只支持带有 RediSearch 模块的二级索引。

# **管理**

在本节中，我们将比较如何保持数据的完整性以及如何备份数据。

对于分区，MySQL 使用 MySQL 集群或 MySQL Fabric 的水平分区或分片。在 Redis 中，只允许分片，可以是自动的基于散列的分片，也可以是手动的使用散列标签的分片。对于复制，MySQL 可以使用多源或源副本复制。Redis 还支持源副本复制，并允许链式复制。对于 Redis 中的多源复制，它仅在 Redis Enterprise Pack 中可用，这是一个付费包。

MapReduce 是一种允许 API 用户定义 map/reduce 方法的功能。它不是 MySQL 中使用的特性，但是 Redis 允许通过 RedisGears 使用它。两者都有某种类型的并发性。在 MySQL 中，是通过表锁还是行锁，这取决于使用的是哪种存储引擎。对于 Redis，并发性是由服务器序列化的数据访问定义的。

就每个数据库的持久性而言，我们正在寻找使数据持久的支持。MySQL 支持这一努力。因为 Redis 在内存中，所以它的持久性看起来有点不同。Redis 不是简单地允许数据持久化，而是允许通过操作日志甚至快照来配置提供数据持久化的机制。

关于交易后数据的完整性，我们知道 MySQL 是 ACID(原子的、一致的、隔离的、持久的)兼容的。然而，Redis 只是部分符合 ACID 标准。正如我们之前谈到的，它是耐用的。Redis 也有脚本和命令块的原子执行，但是使用乐观锁定。

# **连接**

我们将讨论的第一个连接是您可以用来连接数据库的方法。MySQL 使用 ADO.NET、JDBC 和专有的本地 API。Redis 使用 RESP，这是 Redis 序列化协议。

对于用户访问，我们知道 MySQL 有用户授权。在 Redis 中，有基于密码的身份验证、访问控制列表(ACL)、LDAP 和基于角色的访问控制(RBAC )(如果使用 Redis Enterprise)以及相互 TLS 身份验证。

# **结论**

Redis 是一种数据结构存储，可以用作 NoSQL 数据库。一个很大的优点是键值结构，它允许存储不需要在多个表或集合之间实施引用完整性的文档。但是，因为它位于内存中，所以存储空间有限，这并不适合扩展。如果你需要一个流行的 NoSQL 数据库，Redis 可能是一个很好的选择。但是，如果您需要一个模式结构的数据库来实现引用完整性，或者至少是 ACID 兼容的，MySQL 可能是您更好的选择。

现在，我希望您觉得这个比较有用。欢迎对您使用过 Redis 数据库的任何项目发表评论。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

<https://python.plainenglish.io/searching-for-text-in-those-annoying-pdfs-d95b6dc7a055>  </daas-data-as-a-service-78494933253f>  <https://miketechgame.medium.com/one-year-of-writing-on-medium-d4d73366e297>  </deep-learning-in-data-science-f34b4b124580>  <https://python.plainenglish.io/web-scraping-with-beautiful-soup-1d524e8ef32>  

参考资料:

  <https://db-engines.com/en/system/MySQL%3BRedis>  <https://redis.io/> 