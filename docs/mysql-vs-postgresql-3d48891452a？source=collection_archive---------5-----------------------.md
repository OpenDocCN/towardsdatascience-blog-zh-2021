# MySQL vs PostgreSQL

> 原文：<https://towardsdatascience.com/mysql-vs-postgresql-3d48891452a?source=collection_archive---------5----------------------->

## 关系数据库管理系统与对象关系数据库管理系统的比较

![](img/d86ff25b86e6bff269916f68b9331055.png)

妮可·梅森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

不久前，我比较了 MySQL 和 SQLite。这既是我喜欢写的东西，也是我觉得有趣的东西。我想将它发扬光大，并将其作为深入研究其他数据库管理系统的工具。我决定学习更多关于 Postgres 的知识，并认为这种比较将有助于澄清困惑的领域。此外，通过比较，除了语法之外，似乎还有更深入的方面。

让我们开始看看 Postgres 是什么，它与 MySQL 有何不同，不要再拖延了。

# **什么是 PostgreSQL？**

PostgreSQL 也称为 Postgres，是一个免费和开源的对象关系数据库管理系统。它可以在所有主要的操作系统上运行，并且是 ACID(原子性、一致性、隔离性、持久性)兼容的。Postgres 不仅具有各种各样的特性，还具有定制的灵活性。语法也更容易学习和初学者友好。

现在我们对 Postgres 有了一个简单的背景，让我们看看 MySQL 和 Postgres 的区别。

# **RDBMS vs ORDBMS**

MySQL 是关系数据库管理系统(RDBMS ),而 Postgres 是对象关系数据库管理系统(ORDBMS)。在关系数据库管理系统中，数据库是基于关系模型的。这意味着所有表与另一个表至少有一个关系，没有一个表没有关系。对象关系数据库管理系统兼有 RDBMS 和面向对象关系数据库管理系统的特点。这意味着不仅表是相关和链接的，而且还有面向对象管理系统的元素，这意味着它支持诸如对象、类和继承等特性。

# **数据类型**

除了传统的数据类型，MySQL 还支持字符串、字符、日期和时间、小数、大文本、布尔值，甚至 BLOB 类型。BLOB 是一种可以存储二进制数据的类型。另一方面，Postgres 支持上面列出的所有数据类型，甚至更多。它可以存储枚举类型、JSON 或 XML 等非结构化类型、几何类型，甚至网络类型。还支持空间数据类型。

# **数据库功能**

当考虑将用于每一个的 GUI 时，MySQL 有 MySQL Workbench。对于 Postgres，可以使用 PgAdmin。Postgres 也使用单个存储引擎，而 MySQL 有多个。MySQL 更像是一个产品，而 Postgres 更像是一个项目。

就 SQL 功能而言，存在一些差异。一旦临时表出现这种差异。虽然两者都可以用“CREATE temporary TABLE”来创建临时表，但是只有 MySQL 在 DROP 语句中有关键字 TEMPORARY。这意味着您必须更加小心您的命名约定，因为临时表可能与常规表同名，并且因为您没有在 DROP 语句中指定“temp ”,您可能会无意中丢失数据。

在删除或截断表时，也有很大的不同。对于 Postgres，删除的表支持级联选项。这意味着它也将删除依赖于该表的任何内容。MySQL 不支持 CASCADE。类似地，对于截断表，MySQL 不支持任何级联选项。使用 Postgres，截断允许级联，重新启动 IDENTITY 将 ID 放回起始值，就像以前的数据从未存在过一样，或者继续 IDENTITY，这更像 MySQL 所做的，即使数据已不存在，ID 仍保留在同一位置。对于标识，Postgres 支持标识，而在 MySQL 中，等价的标识是一个 AUTO_INCREMENT 整数。

对于存储过程，MySQL 要求该过程用 SQL 语法编写。在 Postgres 中，过程是基于函数的，函数可以用其他语言编写，如 SQL、Python、Perl、JavaScript 或其他语言。

对于索引，只有 Postgres 提供了不同的类型，比如部分索引甚至位图索引。与 MySQL 相比，Postgres 中的触发器对于更广泛的能力也更加灵活。然而，分区在 Postgres 中更受限制，只允许范围或列表。在 MySQL 中，分区有范围、列表、散列、键甚至复合分区选项。

MySQL 不区分大小写，但 Postgres 区分大小写。这意味着如果没有适当地设置，查询可能会失败。此外，MySQL 允许 IF 和 IFNULL 语句，而 Postgres 不允许。相反，应该使用 CASE 语句。

# **可扩展性**

当添加新的连接时，与 MySQL 的每个连接都是一个线程，而 Postgres 中的一个连接是一个进程。谈到并发性，Postgres 使用多版本并发控制(MVCC)。这是为了支持多个用户，减少锁定的机会。这是因为它实现了并行查询计划。

因为在 Postgres 中每个连接都有额外的进程，所以每个连接都需要少量的内存(大约 10 MB)。然而，Postgres 不限制数据库的大小，这使得它成为大型数据库管理的一个好选择。至于复杂性，Postgres 也更复杂，因为它允许函数、继承等等。MySQL 专注于速度和可靠性。

一个有趣的现象是，在其他方面，差距正在缩小。这很大程度上是由于 Postgres 和 MySQL 在最近的更新中解决了用户需求。事实证明，这些更新作为用户需求的标准更受期待。由于这些更新，数据库确实有相似之处，而不仅仅是不同之处。这使得决定使用哪一个有点困难。或者，这表明两个数据库都致力于解决长期存在的问题，满足需求，并且最终都是您的项目非常合适的选择。

# **结论**

如果您正在寻找一个简单、快速、流行、可靠且易于理解的数据库，MySQL 可能是更好的选择。但是如果您正在寻找一个更大的数据库或者更多的特性和复杂性，PostgreSQL 可能是更好的选择。也许您的决定归结为支持，在这种情况下，您需要数据库提供更好的帮助。也可能你的决定是基于更多的标准或者工作预期。无论如何，这两个数据库管理系统似乎是一个公平的选择。

对于你的项目，我已经说过一百次了，这真的取决于你。你可以选择任何你需要的技术来满足你自己的需求，我的观点可能不会像他们对我的观点一样符合你的项目的目标。无论哪种选择，你比任何人都更了解你的项目，所以试着用你对两者的了解来做决定，而不是简单地了解别人的决定。

从各方面来看，我希望这个分析对你的决策有所帮助。这两种数据库管理系统似乎都有优点和缺点，所以希望您已经学到了足够的知识来选择是使用 MySQL 还是 PostgreSQL，或者甚至被鼓励去学习更多关于这两者的知识。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

查看我最近的文章:

[](https://medium.com/codex/graphql-vs-rest-c0e14c9f9f1) [## GraphQL 与 REST

medium.com](https://medium.com/codex/graphql-vs-rest-c0e14c9f9f1) [](https://python.plainenglish.io/build-your-own-plex-client-using-python-11cf2e566262) [## 使用 Python 构建您自己的 Plex 客户端

### Plex 是一个客户端-服务器媒体播放器，可用于流式视频、播放音乐、观看直播电视、收听播客…

python .平原英语. io](https://python.plainenglish.io/build-your-own-plex-client-using-python-11cf2e566262) [](/mysql-vs-sqlite-ba40997d88c5) [## MySQL 与 SQLite

### 作为改变，我决定回到数据库。不过这一次，我想做一个小小的比较…

towardsdatascience.com](/mysql-vs-sqlite-ba40997d88c5) [](https://python.plainenglish.io/giving-django-another-shot-3e6f786b13f3) [## 再给姜戈一次机会

### 第 1 部分:是时候重赛了…

python .平原英语. io](https://python.plainenglish.io/giving-django-another-shot-3e6f786b13f3) [](https://python.plainenglish.io/renpy-a-simple-solution-to-building-visual-novel-games-32d6179a7840) [## Ren'Py:构建视觉小说游戏的简单解决方案

### 第 1 部分:入门

python .平原英语. io](https://python.plainenglish.io/renpy-a-simple-solution-to-building-visual-novel-games-32d6179a7840) 

参考资料:

[](https://www.xplenty.com/blog/postgresql-vs-mysql-which-one-is-better-for-your-use-case/) [## PostgreSQL 与 MySQL 的关键区别

### Postgres 是一个功能丰富的数据库，可以处理复杂的查询和海量数据库。MySQL 是更简单的数据库…

www.xplenty.com](https://www.xplenty.com/blog/postgresql-vs-mysql-which-one-is-better-for-your-use-case/) [](https://developer.okta.com/blog/2019/07/19/mysql-vs-postgres) [## MySQL 与 PostgreSQL——为您的项目选择合适的数据库

### 当开始一个新项目时，选择一个数据库管理系统通常是事后的想法，特别是在…

developer.okta.com](https://developer.okta.com/blog/2019/07/19/mysql-vs-postgres) [](https://www.postgresqltutorial.com/postgresql-vs-mysql/) [## PostgreSQL 与 MySQL

### 当选择开源关系数据库管理时，PostgreSQL 与 MySQL 是一个重要的决定…

www.postgresqltutorial.com](https://www.postgresqltutorial.com/postgresql-vs-mysql/) [](https://www.enterprisedb.com/blog/postgresql-vs-mysql-360-degree-comparison-syntax-performance-scalability-and-features) [## PostgreSQL 与 MySQL:360 度比较[语法、性能、可伸缩性和特性] | EDB

### 在这篇博客中，我们将从性能、语法、可伸缩性和特性方面讨论…

www.enterprisedb.com](https://www.enterprisedb.com/blog/postgresql-vs-mysql-360-degree-comparison-syntax-performance-scalability-and-features) [](https://www.postgresql.org/docs/11/datatype.html) [## 第八章。数据类型

### 为用户提供了丰富的本机数据类型。用户可以使用创建类型…向 PostgreSQL 添加新类型

www.postgresql.org](https://www.postgresql.org/docs/11/datatype.html) [](https://www.postgresql.org/about/) [## 关于

### PostgreSQL 是一个强大的、开源的对象关系数据库系统，它使用并扩展了 SQL 语言和

www.postgresql.org](https://www.postgresql.org/about/) [](https://pediaa.com/what-is-the-difference-between-rdbms-and-ordbms/#:~:text=The%20main%20difference%20between%20RDBMS,store%20and%20manage%20data%20efficiently.&text=ORDBMS%20has%20features%20of%20both%20RDBMS%20and%20OODBMS) [## RDBMS 和 ORDBMS 有什么区别-pedia。Com

### RDBMS 和 ORDBMS 的主要区别是 RDBMS 是基于关系模型的 DBMS，而 ORDBMS 是基于关系模型的 DBMS

pediaa.com](https://pediaa.com/what-is-the-difference-between-rdbms-and-ordbms/#:~:text=The%20main%20difference%20between%20RDBMS,store%20and%20manage%20data%20efficiently.&text=ORDBMS%20has%20features%20of%20both%20RDBMS%20and%20OODBMS)