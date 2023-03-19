# MySQL 与 SQLite

> 原文：<https://towardsdatascience.com/mysql-vs-sqlite-ba40997d88c5?source=collection_archive---------7----------------------->

## 探索两个流行数据库之间的差异。

![](img/85c6803bc7b05db8993f8b98a982d596.png)

照片由[贾瓦德](https://unsplash.com/@_javardh_001?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为改变，我决定回到数据库。不过这一次，我想做一点比较，而不是另一个教程风格的帖子。回想我在大学的时候，我记得有一节课我们讨论了使用 MySQL 还是 SQLite。最后，由于可移植性的原因，我们使用了 SQLite，但这让我开始思考。对话暗示 MySQL 和 SQLite 非常相似，然而，我知道事实并非如此。那么它们有什么不同呢？它们有什么相似的地方吗？所以在这篇文章中，我们将讨论这一点。MySQL 和 SQLite 有多大区别？对于您的项目，使用哪一种更好？

# **快速浏览对比**

MySQL 和 SQLite 都是关系数据库管理系统(RDBMS)的形式。这些是基于结构化查询语言(SQL)的。此外，每一个的语法可以是相似的，并且，在我看来，一个接一个地学习并不太难。话虽如此，尽管这听起来像是非常相似的背景，但这是我们相似之处的终点。

# **一看背景细节**

首先，作为开始，我们应该看看每一个的背景代码。对于 MySQL 和 SQLite，都使用 C 作为开发语言。也就是说，只有 MySQL 也是用 C++开发的。两者都是开源的。虽然 MySQL 是由 Oracle 管理的，但是 SQLite 的代码可以在公共领域中用于个人和商业用途。

接下来，我们可以看看它是如何运行的，或者它是靠什么运行的。MySQL 使用数据库服务器在网络上运行，然后客户端可以访问它。然而，SQLite 是所谓的嵌入式数据库。这意味着该结构存储在应用程序本身上。通过存储在应用程序上，SQLite 是可移植的并且易于移动，因为所有的表信息都位于文件中。也就是说，这也意味着网络上没有其他应用程序可以访问数据库。

SQLite 是可移植的，因此它也必须是非常轻量级的。事实上，尽管取决于系统，所需的空间可以小于 600 KiB。SQLite 也不需要预先安装或设置。此外，它还是完全独立的，因此没有外部依赖性。SQLite 有时也被称为“开箱即用的”RDBMS，因为它不需要启动或停止程序之类的配置。另一方面，MySQL 确实需要安装。它也跑得更重。运行所需的空间约为 600 Mb。也就是说，MySQL 也支持复制和可伸缩性。

# 但是数据类型呢？

支持的数据类型是 MySQL 和 SQLite 的一大区别。MySQL 支持多种数据类型，包括不同的数字类型、日期和时间类型以及字符串类型。如果选择得当，这允许大多数数据安全地存储在正确的类型中。另一方面，SQLite 不支持那么多类型。事实上，SQLite 只支持 NULL、integer、real、text 和 blob。如您所见，数据可能并不总是最适合这些类型之一。这样的挫折是保持 SQLite 对初学者友好和轻量级所必需的。

# **可扩展性和其他重要特性**

正如前面已经提到的，网络中的连通性是 MySQL 和 SQLite 的一个区别。与 MySQL 不同，SQLite 是独立的，网络上的其他客户端无法访问数据库。这也扩展到多用户能力。MySQL 能够同时处理许多连接。然而，SQLite 只能处理一个连接。此外，在 MySQL 中，不同的用户可能被创建为具有一系列不同的权限，而在 SQLite 中，用户管理不是一项功能，因此不受支持。

就可伸缩性而言，有几个不同的因素需要考虑。首先，MySQL 能够处理大量数据。这可能是因为有大量不同的表，也可能是因为每个表都有许多条目。对于 SQLite，它是为运行轻量级和小型而构建的，因此存储的数据越多，效率越低，性能越差。MySQL 包含的 SQLite 没有的一个小特性是支持 XML 格式。另一个非常重要的因素是认证问题。使用 MySQL，可以创建具有权限的用户，这意味着这些用户需要经过身份验证。用户名和密码就是这样一种形式。这是为了防止外部用户能够更改或访问数据库中的任何信息。然而，在 SQLite 中，没有受支持的内置身份验证。这意味着不仅任何人都可以访问数据库，而且任何人都可以添加、更新或删除条目甚至整个表。

尽管 SQLite 可能不支持多用户可访问性，但是并发性非常有限。这意味着多个进程能够同时访问数据库，但是不支持同时进行更改。然而，MySQL 的并发性更有可能。也就是说，同时进行更改仍然会产生问题，这将使 RDBMS 中的这些问题变得相似。

安全性是 MySQL 的一个重要的特性。因为它是服务器端的，所以 MySQL 有内置的特性来防止不受欢迎的人轻易访问数据。尽管并不完美，但与 SQLite 相比，它提供了一个完全不同的世界。这是因为 SQLite 是嵌入式的，因此不具备 MySQL 作为服务器端数据库所具备的 bug 预防和其他重要的安全特性。

# **如何以及何时选择**

总的来说，MySQL 在交易比较频繁的时候使用。它通常用于 web 和桌面应用程序。如果网络功能是必须的，也应该选择 MySQL。如果需要多个用户，如果需要更强的安全性，或者如果涉及身份验证，这也是一种选择。如果需要存储大量数据，MySQL 也是合适的选择。

当数据被预定义并在移动等应用程序上使用时，SQLite 更常用。对于这样的应用，被限制在设备的文件中不是问题，并且不需要网络。也就是说，有时候可能会选择 SQLite。如果剩余少量空间，或者如果应用程序需要存储少量数据，而该应用程序只需要最低限度地访问数据库，并且不需要大量计算，那么就会出现这种情况。我知道当我在大学做小项目时，SQLite 是快速创建数据库的好方法。这些小型独立应用程序可能不需要用户，甚至不需要应用程序以外的功能来访问数据库，这就是 SQLite 可能适合您的原因。

总的来说，这实际上取决于你使用起来是否舒服，以及你认为哪个更合适。毕竟没有人会像你一样了解你的项目。

# **结论**

作为一种改变，我认为比较两种不同的 RDBMS，MySQL 和 SQLite，是一种有趣的尝试。它不仅给出了每一个的一点背景，而且更多的是对每一个是如何工作的理解。我们还研究了两者之间的一些主要差异，它们实际上并不像你可能会相信的那样相似。最后，我们简要地看了一下什么时候您可能想使用一个而不是另一个，尽管这更多地取决于用户和您的特定项目。我希望您能从中学到一些东西，并且发现这些信息是有用的。我知道我觉得很有趣。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

查看我最近的文章:

[](https://miketechgame.medium.com/giving-django-another-shot-3e6f786b13f3) [## 再给姜戈一次机会

### 第 1 部分:是时候重赛了…

miketechgame.medium.com](https://miketechgame.medium.com/giving-django-another-shot-3e6f786b13f3) [](https://python.plainenglish.io/renpy-a-simple-solution-to-building-visual-novel-games-32d6179a7840) [## Ren'Py:构建视觉小说游戏的简单解决方案

### 第 1 部分:入门

python .平原英语. io](https://python.plainenglish.io/renpy-a-simple-solution-to-building-visual-novel-games-32d6179a7840) [](/mongodb-92370a2641ad) [## MongoDB

### 从安装到实现:第 6 部分

towardsdatascience.com](/mongodb-92370a2641ad) [](https://python.plainenglish.io/organizing-my-pictures-with-mysql-and-python-ca5dee8fe02f) [## 用 MySQL 和 Python 组织我的图片

### 又快又脏，但很管用…

python .平原英语. io](https://python.plainenglish.io/organizing-my-pictures-with-mysql-and-python-ca5dee8fe02f) [](https://medium.com/codex/all-along-ive-been-using-a-sql-antipattern-50f9a6232f89) [## 一直以来，我都在使用 SQL 反模式

### 下面是如何修复它…

medium.com](https://medium.com/codex/all-along-ive-been-using-a-sql-antipattern-50f9a6232f89) 

参考资料:

[](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) [## SQLite vs MySQL vs PostgreSQL:关系数据库管理系统的比较

### 关系数据模型在数据库管理中占主导地位，它以行和列的表格来组织数据

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) [](https://www.educba.com/mysql-vs-sqlite/) [## MySQL 与 SQLite |你应该了解的 14 大区别

### MySQL 是最流行和最受欢迎的开源关系数据库管理系统之一。广泛认为…

www.educba.com](https://www.educba.com/mysql-vs-sqlite/)