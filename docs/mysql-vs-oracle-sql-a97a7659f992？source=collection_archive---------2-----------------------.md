# MySQL 与 Oracle SQL

> 原文：<https://towardsdatascience.com/mysql-vs-oracle-sql-a97a7659f992?source=collection_archive---------2----------------------->

## 比较两个 Oracle 拥有的关系数据库管理系统

![](img/1a433307a1df99b5a6b3dd76bf6358d2.png)

达里尔·洛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在我上一篇关于 MySQL 的文章中，我简单的提到了 MySQL 是由 Oracle 拥有的。但这让我想到，为什么不比较 MySQL 和 Oracle SQL 呢？它们都属于同一家公司，但在使用时，它们看起来非常不同。理论上，不同管理系统支持的每种 SQL 语言都应该至少有些不同。但是这两个 RDBMSs 的区别不仅仅是语法。这就是我们将在本文中探讨的内容。和其他人一样，我们将首先简要概述 Oracle SQL。只是简单描述一下它是什么，以及我们稍后需要参考的一些细节。最后，我们将比较 MySQL 和 Oracle SQL 之间的一些差异。像其他 MySQL 与…的文章一样，它不会包罗万象。但是我们至少会看到一些主要的区别。

**Oracle SQL 概述**

当提到 Oracle SQL 时，我们指的是 Oracle 数据库。它是一个使用商业许可的关系数据库管理系统(RDBMS)。Oracle SQL 是一个跨平台的管理系统，这意味着它可以在多种操作系统上运行。这个数据库管理系统是第一个为处理数据库中的记录而开发的关系系统。Oracle SQL 还具有可伸缩性、可移植性和易编程性。

Oracle SQL 旨在处理大量数据。它甚至有恢复管理工具。另一个主要目标是数据的可靠性，并确保维护数据的完整性。它通过遵循 ACID(原子性、一致性、隔离性和持久性)来保持完整性。

**开源数据库**

众所周知，MySQL 是一个开源数据库。然而，相比之下，Oracle SQL 是为商业开发的。这意味着没有许可证，您将无法使用 Oracle SQL。Oracle SQL 有一个免费的快速附加功能，但只推荐学生使用。

**数据库功能**

就可伸缩性而言，MySQL 可用于小型和大型企业。Oracle SQL 被设计成大规模的，可以支持大量数据。

MySQL 不支持数据分区，只支持静态系统。然而，Oracle SQL 支持数据分区。它还可以与静态和动态系统一起工作。然而，MySQL 支持一些 Oracle SQL 不支持的类型。例如，MySQL 支持空值。Oracle SQL 不支持空值。

MySQL 支持 SQL 语言。但是，Oracle SQL 同时支持 SQL 和 PL/SQL。

与 MySQL 相比，Oracle SQL 不支持那么多操作系统。例如，Oracle SQL 支持 Windows、Mac OS X、Linux、Unix 和 z/OS。除了 BSD、Symbian 和 AmigaOS 之外，MySQL 还支持所有这些。

直到版本 5，MySQL 都不支持存储过程。相比之下，Oracle SQL 支持嵌入在数据库中的存储过程。它们可以由事件执行或触发。

无法定制 Oracle SQL，因为它是一个封闭的源。相比之下，MySQL 是可以修改的。因为它是开源的，所以可以根据您的任何需求，针对不同的环境修改代码。

**安全和管理**

MySQL 和 Oracle SQL 都有用户名和密码等安全性。然而，两者之间有一些小的不同。例如，在 MySQL 中，需要标识一个主机。但是有了主机、用户名和密码，用户就可以访问数据库。使用 Oracle SQL，登录时需要用户名和口令，但是还需要验证概要文件。这意味着如果没有设置配置文件，用户就不能访问它。这也有助于定义用户角色。

因为 Oracle 同时拥有 Oracle SQL 和 MySQL，所以它们都有支持和文档。Oracle SQL 使用社区支持以及针对付费产品的各种支持选项。对于 MySQL，我们提供全天候的技术支持服务。主要是，那些支持工程师在寻找漏洞的修复，定期维护，并推出安全补丁。

**甲骨文的优势**

如果需要高度的可伸缩性，并且预计数据会更大，那么 Oracle SQL 将是更好的选择。如果大型数据库也需要托管，那么选择 Oracle SQL。当交易控制需要灵活性时，也应该选择它。当数据库需要独立于平台时，Oracle SQL 也是更好的选择。因为 Oracle SQL 是为更大的数据库和更高的交互而设置的，所以它比 MySQL 支持更大的并发池。尽管 Oracle SQL 是付费的，但由于它的支持和功能，它被设计成企业的最佳选择，尤其是它是为企业扩展而构建的。

**甲骨文的缺点**

Oracle SQL 的企业版是唯一的免费版本，因为它主要被许可用于商业用途。这意味着除非付费，只有少数人可以访问和学习它。因为是付费的，所以不是开源的。这意味着任何更新和修复，甚至价格，都是由 Oracle 制定的。这意味着产品的透明度降低。这也意味着更少的定制和灵活性，因为它是一个成品。与 MySQL 相比，最后一个缺点是语法。与 MySQL 相比，学习曲线有点陡峭。Oracle SQL 并不是很难学，只是没有 MySQL 那么简单。

**MySQL 的优势**

MySQL 在一些情况下是更好的选择。一个这样的例子是当数据库不需要大规模扩展时。另一种情况是网站或 web 应用程序需要只读数据库。如果不需要更高程度的复制，MySQL 将是更好的选择。因为 MySQL 确实有一个 GNU 许可下的免费开放版本，预算也是选择 MySQL 的一个很好的理由。选择 MySQL 而不是 Oracle SQL 的最后一个原因是当并发率较低，或者只需要简单的查询时。

**MySQL 的缺点**

因为 MySQL 属于 Oracle，所以与其他开源关系数据库管理系统相比，它有许多限制。尽管它对小型和大型企业都很灵活，但它不是为 Oracle SQL 这样的大规模数据而构建的。MySQL 也不支持与其他客户端应用程序的集成。它还允许使用触发器。尽管这些对数据非常有用，但它们也会给数据库服务器带来很高的负载。与 Oracle SQL 相比，MySQL 速度较慢，存储选项也较少。它也不支持大型线程池，但这是因为与 Oracle 相比，内存存储容量较小。正如我们在上一篇 MySQL 对比文章中提到的，MySQL 并不是完全开源的。相反，它在企业版中有一些专有代码。

**结论**

MySQL 和 Oracle SQL 都是 Oracle 公司的关系数据库管理系统。MySQL 主要是免费和开源的，而 Oracle 主要是商业和付费的。MySQL 也比 Oracle 更加可定制，因为 Oracle 是一个成品。两种管理系统都提供社区和技术支持。尽管这两个数据库都属于同一家公司，但是它们有很大的不同，特别是在比较一些功能时，比如并发线程或数据分区。这两个数据库都是非常好的选择。

当您决定需要哪个数据库时，您应该考虑项目的规模和预算。尽管 Oracle 确实有免费版本，但它主要是为学生设计的，不像 MySQL 那样开放给每个人使用。MySQL 确实有一个付费版本，可以帮助解决免费版本中缺少的许多功能，但是对于大多数项目来说，您只需要在家里使用免费版本。最终，两者都是数据库管理系统的好选择，但是如果你在家工作，你可能想要选择 MySQL 来保持预算友好。但是，如果您创建了自己的业务，随着业务的扩展和数据的增加，您可以考虑选择 Oracle SQL 来扩展您的业务。无论你做什么决定，你最了解你的项目。希望本文能为您提供每个数据库的更多细节，以及是什么让它们如此不同。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

[](/why-data-science-is-important-for-all-developers-cfe31aa6fb2b) [## 为什么数据科学对所有开发人员都很重要

### 是的，那意味着你也是

towardsdatascience.com](/why-data-science-is-important-for-all-developers-cfe31aa6fb2b) [](https://python.plainenglish.io/making-python-read-my-emails-9e3b3a48887c) [## 我如何让 Python 阅读我的电子邮件

### 一个有趣的尝试，让你拥有更好的习惯和更高的生产力

python .平原英语. io](https://python.plainenglish.io/making-python-read-my-emails-9e3b3a48887c) [](https://medium.com/codex/a-simpler-introduction-to-oauth-3533e53a4589) [## OAuth 的简单介绍

### 学习 OAuth 而不用打开同义词库

medium.com](https://medium.com/codex/a-simpler-introduction-to-oauth-3533e53a4589) [](/mysql-vs-mariadb-6dda1649de44) [## MySQL vs. MariaDB

### 比较两个非常相似但完全不同的 Rational 数据库管理系统

towardsdatascience.com](/mysql-vs-mariadb-6dda1649de44) [](https://miketechgame.medium.com/after-nearly-five-years-i-left-my-job-a2e76f0dc0b4) [## 将近五年后，我离职了

### 回想我在那里的时光，以及为什么我要寻找一个新的机会

miketechgame.medium.com](https://miketechgame.medium.com/after-nearly-five-years-i-left-my-job-a2e76f0dc0b4) 

参考资料:

[](https://www.geeksforgeeks.org/difference-between-oracle-and-mysql/) [## Oracle 和 MySQL 的区别

### 1.Oracle : Oracle 是一个关系数据库管理系统(RDBMS)。它是由甲骨文公司在 1980 年开发的…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/difference-between-oracle-and-mysql/)  [## MySQL 和 Oracle-Java point 的区别

### MySQL 和 Oracle 是两个著名的关系数据库，在大小公司中都有使用。虽然甲骨文…

www.javatpoint.com](https://www.javatpoint.com/mysql-vs-oracle) [](https://www.educba.com/mysql-vs-oracle/) [## MySQL 与 Oracle |最值得学习的 7 大区别

### MySQL 是一个关系数据库。它既快又容易使用。它是最受欢迎的开源数据库之一。这是…

www.educba.com](https://www.educba.com/mysql-vs-oracle/) [](https://hevodata.com/learn/mysql-vs-oracle/) [## MySQL 与 Oracle:全面分析-了解| Hevo

### 在当今世界，公司之间的竞争非常普遍，即使他们提供相似的产品…

hevodata.com](https://hevodata.com/learn/mysql-vs-oracle/) [](https://www.janbasktraining.com/blog/oracle-vs-sql-server-vs-mysql/) [## 数据库权威指南——Oracle vs . SQL Server vs . MySQL

### 关系数据库管理系统(RDBMS)自 20 世纪 90 年代发展以来，已经成为标准的数据库系统

www.janbasktraining.com](https://www.janbasktraining.com/blog/oracle-vs-sql-server-vs-mysql/)