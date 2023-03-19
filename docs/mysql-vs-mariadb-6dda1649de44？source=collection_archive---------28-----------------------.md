# MySQL vs. MariaDB

> 原文：<https://towardsdatascience.com/mysql-vs-mariadb-6dda1649de44?source=collection_archive---------28----------------------->

## 比较两个非常相似但完全不同的 Rational 数据库管理系统

![](img/cdad0912c3f89d6204216fda8feed008.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[沙丘](https://unsplash.com/@sandcrain?utm_source=medium&utm_medium=referral)拍摄的照片

最近一直在想多了解一下 MariaDB。我对 MariaDB 没有太多的了解，但我也很久没有为我的 MySQL 系列写文章了，所以我想这将是一个学习更多知识的好机会，但也可以与类似的东西进行比较。关于 MariaDB，除了它如何寻求成为 MySQL 的可比较的替代者之外，没有什么背景知识，让我们先来看看 MariaDB 的一些基础知识，然后再进行比较。

# **什么是 MariaDB？**

MariaDB 是一个关系数据库管理系统(RDBMS ),旨在替代 MySQL。它是开源软件，在 GNU 通用公共许可证下也可以免费使用。MariaDB 的一大重点是它的全能速度。但是，并没有牺牲性能和动力。众所周知，MariaDB 还具有高度的可伸缩性。监督 MariaDB 的团队非常关注安全性，这意味着他们会尽快发布更新来修补这些漏洞。MariaDB 还努力在网络、云端甚至移动设备上快速工作。它也是耐酸的。

现在我们已经了解了一些关于 MariaDB 的背景知识，让我们来看看 MySQL 和 MariaDB 的异同。

# **性能**

当我们谈论性能时，我们将更多地关注功能而不是实际速度，因为两者非常具有可比性。MySQL 和 MariaDB 都使用分区，这尤其有助于提高可伸缩性。扩展时，性能变得更加重要。为了获得更快的性能，您还需要充分利用分配给您的空间。MySQL 和 MariaDB 都使用行压缩。

MariaDB 也使用线程池。如果您从未听说过线程池，请考虑尝试链接到数据库的连接。该连接将获取一个线程来查询数据库。使用线程池，有一系列开放的线程被池化在一起，并且没有被使用。当一个连接需要这些线程中的一个时，它可以直接获取它并使用它来查询数据库，然后在完成后再次丢弃它。这样，就不会在每次打开连接时都创建线程。现在，MySQL 确实有了线程池的能力。但是，它仅在企业级可用。这意味着免费使用的社区版本不包括线程池。

因为 MariaDB 试图建立在 MySQL 的基础上，所以许多其他特性加快了 MariaDB 的性能。其中一个功能是查询结果缓存。另一个是并行查询和读/写拆分。还有很多，但这只是少数。

# **真正的开源**

所以，MySQL 和 MariaDB 都是开源的。但是，MySQL 归甲骨文所有。这意味着，是的，在他们的社区版中有一些开源代码。但是，也有专有代码。该代码并不是对每个人都可用，因为它只是作为企业级的特性而可用。对于 MariaDB，它是真正的开源，这意味着随着他们的更新，他们甚至接受外部的贡献来建立更多的特性和功能。

# **数据库功能**

MySQL 和 MariaDB 的通用选项非常具有可比性，这也是它们如此相似的原因。然而，MariaDB 在 MySQL 的基础上增加了几项改进。例如，MariaDB 有一个内存存储引擎，可以加快缓存和索引的速度。这意味着 INSERT 语句的完成速度比 MySQL INSERT 快 24%。

如前所述，MariaDB 可以处理线程池。然而，让 MariaDB 与众不同的是，当适当扩展时，它可以处理 200，000 个或更多的开放连接。复制过程也比 MySQL 更安全、更快速。更新也可以比使用 MySQL 更新更快。

MariaDB 除了其他特性之外，还有其他的扩展和语句。比如可以处理 JSON 之类的扩展。对于语句，MariaDB 可以执行各种语句，如 WITH 或 KILL 语句。其中一些特性是 MariaDB 的新特性，因此 MySQL 没有提供。

MySQL 在企业版中提供了专有代码。在它提供的特性中，有一些是 MariaDB 没有的。为了解决缺失的功能，MariaDB 允许开源插件作为替代。

虽然 MariaDB 试图成为 MySQL 的改进版，但是有一些重要的特性是 MariaDB 所不具备的。例如，MySQL 允许动态列，而 MariaDB 不允许。

# **安全与行政**

MariaDB 和 MySQL 都实现了重要的安全特性，如加密、密码过期、角色、特权和试听。两者似乎都允许数据屏蔽。对于高级数据库和数据保护，Maria DB 还增加了查询限制和查询结果限制。

另一个重要因素是灾难恢复计划。MariaDB 和 MySQL 支持使用备份和恢复工具以及二进制日志来前滚事务的时间点恢复。然而，MariaDB 更进一步。它支持时间点回滚，使数据库管理员能够在很少甚至没有停机的情况下回滚数据库。

MariaDB 和 MySQL 都将多主集群与提供连续可用性的数据库代理或路由器相结合。为了进一步提高其高可用性，MariaDB 还增加了事务重放、会话恢复功能和连接迁移。这允许对应用程序隐藏故障。

# **结论**

当与 MySQL 比较时，MariaDB 试图成为一个可比较的资源。正因为如此，MariaDB 拥有 MySQL 拥有的许多功能。然而，它试图通过添加 MySQL 所没有的各种特性来更进一步。MariaDB 也更关注开源，他们的团队甚至在添加什么功能和如何改进方面接受意见。MariaDB 速度更快，并试图将重点放在安全性和性能上。然而，MariaDB 也努力实现高度可伸缩性。

当试图决定使用什么数据库时，您当然应该始终考虑您的项目需求。说到细节对比，MariaDB 和 MySQL 非常相似。没有太多方面是一方缺少另一方拥有的东西。当然，这条规则也有例外，但在大多数情况下，MariaDB 试图成为 MySQL 的改进版本。正因为如此，我认为应该给 MariaDB 一个机会。如果你正在寻找一些非常熟悉和成熟的东西，我建议你坚持使用 MySQL 的根。然而，如果您正在寻找一个新的、强大的、具有同样多有用特性的数据库，我建议尝试一下 MariaDB。

在了解了更多关于 MariaDB 的信息后，我更有兴趣尝试一下。也许我以后甚至会写一篇如何使用 MariaDB 的教程。但是现在，我希望这个比较是有帮助的和有趣的。下次见，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想阅读介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

[](https://miketechgame.medium.com/after-nearly-five-years-i-left-my-job-a2e76f0dc0b4) [## 将近五年后，我离职了

### 回想我在那里的时光，以及为什么我要寻找一个新的机会

miketechgame.medium.com](https://miketechgame.medium.com/after-nearly-five-years-i-left-my-job-a2e76f0dc0b4) [](/three-popular-machine-learning-methods-7cb2dcb40bd0) [## 三种流行的机器学习方法

### 了解基本的机器学习类型

towardsdatascience.com](/three-popular-machine-learning-methods-7cb2dcb40bd0) [](https://python.plainenglish.io/what-is-typer-9a9220d80306) [## Typer 简介:用于创建 CLI 的 Python 库

### 探索“CLIs 的 FastAPI”

python .平原英语. io](https://python.plainenglish.io/what-is-typer-9a9220d80306) [](/tensorflow-vs-keras-d51f2d68fdfc) [## 张量流与 Keras:比较

### 查看两个机器学习库的具体细节

towardsdatascience.com](/tensorflow-vs-keras-d51f2d68fdfc) [](https://python.plainenglish.io/sending-error-emails-and-text-messages-in-python-b8e9a48e00ae) [## 用 Python 发送错误电子邮件和文本消息

### 杜绝错误的电子邮件龙卷风

python .平原英语. io](https://python.plainenglish.io/sending-error-emails-and-text-messages-in-python-b8e9a48e00ae) 

参考资料:

 [## 关于 MariaDB 软件

### MariaDB 是一个开源、多线程、关系数据库管理系统，在 GNU 公共许可证下发布…

mariadb.com](https://mariadb.com/kb/en/about-mariadb-software/) [](https://mariadb.com/database-topics/mariadb-vs-mysql/) [## MariaDB 与 MySQL -开源关系数据库| MariaDB

### MariaDB 和 MySQL 是世界上部署最广泛的两种开源关系数据库，虽然它们…

mariadb.com](https://mariadb.com/database-topics/mariadb-vs-mysql/) [](https://www.guru99.com/mariadb-vs-mysql.html) [## Maria db vs MySQL:Maria db 和 MySQL 有什么区别

### MariaDB 是 MySQL 数据库管理系统的一个分支。RDBMS 提供数据处理能力，适用于小型…

www.guru99.com](https://www.guru99.com/mariadb-vs-mysql.html) [](https://hackr.io/blog/mariadb-vs-mysql) [## MariaDB vs MySQL: [2021]你需要知道的一切

### MySQL 是世界上使用最广泛的数据库之一。它是免费的，也是开源的。开发于…

hackr.io](https://hackr.io/blog/mariadb-vs-mysql) [](https://blog.panoply.io/a-comparative-vmariadb-vs-mysql) [## MariaDB 与 MySQL:性能、许可和支持

### 应该用 MySQL 还是 MariaDB？这可能是你现在心中的疑问。所以在这篇文章中，你会学到…

blog.panoply.io](https://blog.panoply.io/a-comparative-vmariadb-vs-mysql)