# MySQL 与 Access

> 原文：<https://towardsdatascience.com/mysql-vs-access-5db036bcdf4d?source=collection_archive---------10----------------------->

## 开源和专有的冲突

![](img/b4216eed7bb5cb30885c861ee2bd950e.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) 拍照

正如你可能从标题中猜到的，我们正在做 MySQL 对比系列的另一篇文章。数据库再次引起了我的兴趣，还有一个数据库我们还没有谈到。Microsoft Access 数据库。我们甚至不必开始深入研究，你可能已经知道访问将会有多么不同。但这正是我认为挖掘更多信息会很有趣的地方。有那么多明显的不同，但是当我们去挖掘的时候，又有多少相似之处呢？也有一些明显的相似之处，但是那么又有多少不同呢？

正如你所猜测的，我刚刚发现这是一个有趣的话题。根据我在几堂课上的经验，我们没有把 Access 作为一个完整的数据库来使用。但是在我女朋友的一堂课上，她做到了。那么，让我们深入了解 MySQL 和 Microsoft Access 之间的差异。在这一点上，我们将更多地关注两者之间的差异，而不是积极/消极的方面。在我看来，你已经看到 MySQL 是我最喜欢在家里使用的开源数据库之一，而我从来没有真正提到过 Access。所以，今天我们只看 MySQL 和 Access 的区别。

# **什么是微软 Access**

首先要注意的是，Access 不仅仅是一个数据库管理系统(DBMS)。Access 合并了 Jet 数据库引擎和图形用户界面(GUI)。它还增加了软件开发工具。传统上，Access 使用关系数据库管理系统(RDBMS)。但是，Access 也可以简单地作为 DBMS 运行。

虽然 Jet 数据库引擎是默认的，但是您也可以选择 Access 支持的其他常用数据库，如 SQL Server、Oracle、DB2，或者查找对开放式数据库连接(ODBC)标准的支持。Access 还允许您从电子表格、文字处理文件或数据库文件中导出或导入数据。您还可以导入甚至直接链接到存储在其他数据库或应用程序中的数据。Access 还可以理解各种各样的数据格式。

当然，访问点是能够访问各种资源。这可能意味着它访问的文件，或者它可以读取的不同数据格式。但它也适用于访问数据的方式。例如，Access 可以处理来自其他来源、其他 SQL 数据库、流行的 PC 数据库程序、服务器、小型机、大型机的数据，甚至可以处理 Internet 或 intranet web 服务器上的数据。关于访问已经说得够多了，让我们开始比较一些不同之处。

# **开源与不开源**

正如我们之前多次讨论过的，MySQL 是一个开源的 RDBMS，大部分可以免费使用。MySQL 只涉及一些专有代码。当然，这是你不能只看到的代码，也是你要付费的领域。对于 MySQL，你有更多的争议。所以，Access 是微软 Office 自带的应用程序之一。但是可以肯定的是，这些并不是完全免费的。你必须有办公室才能免费进入那里。你可以获得微软 365 应用的 30 天免费试用，但也有免费的 Access 运行时版本可供你使用。现在，应该有免费使用 Access 的方法，但当然，这并不等同于 MySQL 是开源的，因为 Access 使用的是商业许可证。

# **管理**

说到分区，我们已经讨论了 MySQL 如何使用水平分区，或者使用 MySQL Cluster 或 MySQL Fabric 进行分片。另一方面，Access 不支持分区。复制也是如此。MySQL 支持多源或源副本复制，而 Access 不支持复制。

对于用户概念，MySQL 使用了细粒度的用户授权概念。然而，Access 并不包含用户概念，尽管在 Access 2003 之前有简单的用户级内置安全性。

MySQL 和 Access 都是为了经久耐用而构建的。它们都支持数据的持久性。但是，Access 不包括用于事务日志记录的文件。MySQL 和 Access 也是 ACID 兼容的，但是 Access 没有用于事务日志的文件。

我们还应该提到操作系统。MySQL 可以在 Linux、Windows、Solaris、OS X 和 FreeBSD 上运行。但是，Access 只能在 Windows 上运行。这是因为它不是真正的数据库服务器。它只是通过使用 dll(动态链接库)来发挥作用。也许没那么重要，MySQL 是用 C++和 C 写的，而 Access 只用 C++写的。

服务器端脚本也可能是一个考虑因素，这取决于您使用的版本。例如，MySQL 只有专有版本的服务器端脚本。对于 Access，仅当您将 Access 2010 或更高版本与 ACE 引擎一起使用时。Access 中的触发器也是如此。它们仅在带有 ACE 引擎的 Access 2010 或更高版本中可用，而触发器在 MySQL 中始终可用。

就安全性而言，访问权限要有限得多。您可能已经听说过，MySQL 有不同类型的安全性，并且可以配置 SSL 支持。为了安全起见，Access 仅支持用户名/密码。

既然我们已经了解了这些差异，那么让我们快速了解一下 Access 的优点和缺点。

# **访问的好处**

Access 易于导航，因为它使用经典的 Microsoft UI(用户界面)。它也是微软办公套件的一部分。

访问不仅仅是针对表。如果你需要你的图表都绘制出来，没有必要在纸上做。Access 使得为表或模拟布局创建实体关系图变得很容易。

Access 还支持标准的 SQL 语法/脚本，因此您不必花时间学习一门新语言来使用它。

# **访问的缺点**

虽然有对访问的支持，但它并不总是有用的。就教程而言，它们有时会受限于所涵盖的材料。这使得“帮助信息”并不总是有用的。

虽然有更新，但没有太多大的变化。这可能是好事，但也可能是坏事。没有太多的变化也意味着他们会落后。

很难展示未使用的表格、报告、表格、文档等等。虽然开始时这可能不是很重要，但由于内存有限，您需要找到哪些对象可以删除，而不会损害任何底层查询或报告。

另一个挑剔的问题，但是对话框并不总是可调整大小的，所以长名字可能会被删掉。这不会是太大的问题，除非您有类似的命名约定。在这种情况下很难分辨出哪个是哪个。

# **结论**

在本文中，我们研究了 MySQL 和 Microsoft Access 之间的区别。总而言之，这两者之间有很多相似之处，但都具有标准的 SQL 特性。至于更技术性的方法，主要是底层结构，有更多的差异。在我看来，这是对数据库的一次有趣的探索。我认为有比我认为的更多的相似之处，但也许差异是重要的决定因素，比如付费或开源。最后，我希望您发现这是对 MySQL 和 Access 的一次有趣的探索。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

<https://python.plainenglish.io/how-fast-are-sqlalchemy-relationships-5b6787dc9276>  </mysql-vs-cassandra-db-49bc518e1b8f>  <https://medium.com/codex/libreoffice-on-docker-1a64245468c>  </sql-relationships-with-sqlalchemy-ee619e6f2e8f>  </mysql-vs-redis-def3287de41>  

参考资料:

<https://dba.stackexchange.com/questions/62851/mysql-vs-microsoft-access>  <https://db-engines.com/en/system/Microsoft+Access%3BMySQL>  <https://www.toolbox.com/tech/devops/question/difference-between-microsoft-access-and-mysql-061011/>  <https://www.tutorialspoint.com/ms_access/ms_access_overview.htm> 