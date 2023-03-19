# 从 Heroku Postgres 迁移到亚马逊 RDS

> 原文：<https://towardsdatascience.com/migrating-from-heroku-postgres-to-amazon-rds-2e738e7730e5?source=collection_archive---------12----------------------->

## 完成如何将大型 PostgresSQL 数据库从 Heroku 迁移到 RDS 的分步指南

![](img/4e8cb6bb7ede17a0cad39706f5ebb637.png)

首先是背景， [Heroku Postgres 数据库](https://elements.heroku.com/addons/heroku-postgresql)和 [Amazon RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/) 有什么区别，为什么还要费事迁移？

每个用过 Heroku 的人都知道它的[插件](https://elements.heroku.com/addons)简单易用。它们让您在几分钟内就可以开始使用数据库、日志监控或十几个其他有用的服务。这是一个完美的组合，可以让事情顺利进行，或者只是消除管理外部服务的开销。问题是什么时候使用 Heroku 附加组件是不合适的，什么时候应该转而使用你能更好控制的服务？

在高级别上 [Heroku Postgres 数据库](https://elements.heroku.com/addons/heroku-postgresql)和 [Amazon RDS for PostgreSQL](https://aws.amazon.com/rds/postgresql/) 都是可伸缩的按需 PostgreSQL 数据库。
当我们关注控制、可扩展性和生态系统方面的细节时，差异是显而易见的。 [Heroku Postgres 附加软件](https://elements.heroku.com/addons/heroku-postgresql)提供通用的“等级”,您可以根据自己的需要选择等级。借助 RDS，您可以更精细地控制实例类型(内存、CPU、容量)、参数配置、冗余级别和成本。

**这里只是一些例子:**

*   **成本节约:**如果您认为您将使用同一类型的数据库至少一年，RDS 可以选择购买专用实例。这将立即削减你一半的成本。
*   **配置灵活性:**借助 RDS，您可以控制配置和数据库管理，并获得相同(如果不是更好)的正常运行时间、成本和监控保证。
*   还可以选择不同类型的复制实例类型；其中 Heroku 需要与主数据库的类型相同。

![](img/3cbe028d919d6916fdedab60f0111a85.png)

从 Heroku PostgresSQL 迁移到 Amazon RDS PostgresSQL

好的。您已经在 Heroku Postgres 工作了一段时间，现在决定迁移到 RDS。

当您处理一个非常大的数据库(几十到几百 GB)时，迁移可能是一项非常具有挑战性的任务。您在想，我如何才能以最短的停机时间执行迁移，并保持现有的安全性。为此，我将迁移计划分为 3 个部分:**安全性考虑、迁移优化&速度和迁移步骤**。

## 1.安全考虑

在 AWS 生态系统中，您通过 VPN 和 VPN 安全组来控制访问。
将 Heroku 应用程序连接到 RDS 实例时，您需要使用公共 URL，可能还需要 IP 限制。通常，如果您在 AWS 中工作，您希望避免使用公共 URL 进行数据库连接，但是在这种情况下，这可能是唯一的选择。那么，我们如何提高安全性:

*   **将 Postgresql 连接限制到 Heroku 的私有 IP** 只有当你有 Heroku 私有空间或者你购买了一个专用的 IP 插件(那些通常是按请求/带宽付费的)时，这个选项才**可行**。这两种选择都可能变得非常昂贵，而且会抵消迁移到 RDS 所节省的成本。
*   **使用安全组**
    限制 RDS 入站流量无论您是否拥有私有 IP，都应该将 RDS 实例的入站连接限制为 PostgreSQL TCP 流量。
    一般使用`0.0.0.0/0`或您的私有 IP(如果适用)

![](img/ec69959db354a54a2d9672178b8864b9.png)

在 RDS 安全组中限制入站流量

*   **在 AWS 中:RDS 实例上的强制实例 SSL 连接-**
    a)更改用于强制 SSL 的 RDS 实例参数组的默认配置。你将需要改变参数`rds.force_ssl = 1`
    我的建议是复制默认的参数组，并使用一个新的修改过的参数组。
    b)下载您的 RDS 实例所在地区的 SSL 证书。在写的时候，你可以从这里的下载

![](img/bddb086c8241015ff8995cc4a9118e81.png)

rds 参数组中的 rds.enforce_ssl 参数

*   **在 Heroku 上:使用 SSL 证书连接到新的 RDS 实例-** a) 在你的 app 中保存 RDS SSL 证书并推送到 Heroku
    b)在`DATABASE_URL`中引用证书。假设您在项目中的`config/amazon-rds-ca-cert.pem`保存了证书，那么将 URL 查询参数`sslca`作为`DATABASE_URL=postgres://username:password@hoistname/dbname?**sslca=config/amazon-rds-ca-cert.pem**`添加到连接 URL 就很简单了

## **2。迁移优化&速度**

使用 [EC2](https://aws.amazon.com/pm/ec2/?trk=ps_a134p000004f2ZWAAY&trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_US&sc_publisher=Google&sc_category=Cloud%20Computing&sc_country=US&sc_geo=NAMER&sc_outcome=acq&sc_detail=aws%20ec2%20instance&sc_content=EC2%20Instance_e&sc_matchtype=e&sc_segment=536392622533&sc_medium=ACQ-P|PS-GO|Brand|Desktop|SU|Cloud%20Computing|EC2|US|EN|Text&s_kwcid=AL!4422!3!536392622533!e!!g!!aws%20ec2%20instance&ef_id=Cj0KCQjw6s2IBhCnARIsAP8RfAimhJ-kSYX2etFtadS8Eo84pceL5L4HtPK89t5IvFd6qp08IUkg2KwaAgYbEALw_wcB:G:s&s_kwcid=AL!4422!3!536392622533!e!!g!!aws%20ec2%20instance) 实例而不是本地机器/本地机器来执行整个迁移。在大多数情况下，您只需要一个非常低层的实例，其中只有连接的存储可能需要更大，具体取决于您的备份大小。您将在那里执行以下步骤:下载 Heroku Postgres 备份，并使用并行作业恢复备份(在迁移过程&计划中概述)。如果你正在处理一个非常大的数据库备份，这会给你成倍增加备份和`pg_restore`命令的下载速度。此外，您将在 AWS VPC 内工作，这将使事情更加安全。您将避免本地机器很可能出现的超时问题。

在 EC2 实例中，您可能必须安装`postgres`，它也会安装`pg_restore`。这通常是通过`amazon-linux-extras`命令完成的。更多信息[点击此处](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-install-extras-library-software/)。

> 给 Mac 用户的 EC2 提示:在连接到 EC2 实例之前，将行`ServerAliveInterval 20`添加到您的`~/.ssh/config`文件中，以保持您的终端会话活动。
> 不要使用浏览器控制台会话来连接和执行步骤。它会超时。

## 3.迁移流程和计划:

***第一阶段:***

*   将 Heroku 应用程序置于维护模式
    `heroku maintanance:on -a {app-name}`
*   缩小/关闭所有后台作业动态
*   监控 Heroku 应用程序的任何活动和你可能错过的任何事情。
*   监控你的应用程序的流量

***二期:***

1.  启动 Heroku 数据库备份
    `heroku pg:backups:capture -a {app-name}`
2.  当最新的备份准备好了，你需要下载它。它将有一个类似`b03`的名字

*   列出备份的公共 URL
    `heroku pg:backups public-url {backup-name} --app {app-name}`
*   开始下载备份到一个文件`my-backup.dump`。 **理想情况下在 EC2 实例上**。
    `curl -o my-backup.dump “{public-url}”`
    **不要忘记在* `*public-url*`两边加上双引号】

3.将数据库转储导入新的 RDS 数据库实例。 **理想情况下使用 EC2 实例** `pg_restore --verbose --clean --no-acl --no-owner -h {rds-instance-url} -U {db-user} -d {db-name} --jobs={number-of-jobs} {my-backup.dump}`

> **这里的 PRO 提示是使用选项** `**--jobs**` **来表示工作数量。这将使用多个线程并行运行恢复，使恢复过程成倍加快。更多信息请点击:**[https://www.postgresql.org/docs/9.2/app-pgrestore.html](https://www.postgresql.org/docs/9.2/app-pgrestore.html)

4.步骤 5(可选):用 GUI 或命令行连接到数据库，并进行健全性检查。比较一些表的大小以及是否创建了索引。(我最喜欢的 PSQL 免费 GUI 是 [Beekeeper Studio](https://www.beekeeperstudio.io/) )

5.**最后一步/不可逆** —更改 Heroku 应用配置中的`DAATABASE_URL`。这一步是不可逆的，因为您必须删除 Heroku Postgresql 加载项，这会终止当前数据库。这是一个不幸的过程。参见我在“最后的思考”部分的观点。
将**数据库 URL** 更改为您的 **AWS RDS URL** 。
`DATABASE_URL=postgres://username:password@hostname/dbname?sslca=config/amazon-rds-ca-cert.pem`

![](img/373e2c81654c52890ab5f9c9bc47207c.png)

删除 Heroku Postgres 附加组件

6.启用所有后台作业动态

7.将应用从维护模式中移除。
`heroku maintanance:off -a {app-name}`

# 最后的想法

1.  Heroku Postgres 附加软件依赖于一个 Heroku 应用程序。
    不能单独使用。这是一个问题，因为当 HEROKU Postgres 附加组件处于活动状态时，无法更改`DATABASE_URL`。为了更改`DATABASE_URL`，需要删除附加组件。将数据库复制到另一个应用程序并不是一个正确的选择，并且/或者表现得像一个全新的`pg_restore`。如果在不移除 Postgres 插件的情况下更改`DATABASE_URL`是可能的，那么当您需要返回到之前的 Heroku 数据库时，这将是回溯情形的理想选择。
2.  执行几次试运行:
    在不改变实际`**DATABASE_URL**`的情况下执行整个过程**(跳过迁移阶段 II，步骤 5)** 。通过这样做，您可以在迁移笔记中概述迁移。这也将帮助你确定几件事:

*   完成迁移所需的总时间—因此，您可以计划一个预定的维护窗口。
*   在这个过程中你错过了什么吗？
*   监视 EC2 实例度量和 RDS 度量。
    在大多数情况下，CPU 可用内存受到的影响最大。您在`pg_restore`中使用的选项`--jobs`中的作业越多，受影响的 CPU 就越多。尽管有一点，更多的作业不会更快地完成迁移。最有可能的情况是，一些作业(在较大的表上工作)将占用与其他所有作业一样多的时间，并且整个迁移将依赖于它们来完成。

3. [AWS 服务器迁移服务](https://aws.amazon.com/server-migration-service/)
我做 Heroku 到 RDS 数据库迁移的第一直觉是，让我们使用 [AWS 服务器迁移服务](https://aws.amazon.com/server-migration-service/)。在迁移过程中，这个工具会给我 99.9%的正常运行时间，直到`DATABASE_URL`发生变化。，我艰难地发现(在我设置好一切之后),服务迁移服务依赖于逻辑复制，而逻辑复制不能使用 Heroku Postgres 数据库作为源进行配置。有关 AWS 迁移服务和 Postgresql 迁移的更多信息—[https://AWS . Amazon . com/blogs/database/migrating-Postgresql-from-on-premises-or-Amazon-ec2-to-Amazon-rds-using-logical-replication/](https://aws.amazon.com/blogs/database/migrating-postgresql-from-on-premises-or-amazon-ec2-to-amazon-rds-using-logical-replication/)

4.将数据加载到 PostgreSQL 数据库实例的有用 AWS 提示[这里是](https://docs.amazonaws.cn/en_us/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html#CHAP_BestPractices.PostgreSQL)。其中一些很有意义，但对于我的迁移来说，它们并没有太大的区别，而且这会增加整个迁移过程，因为您必须将这些更改应用到 RDS 实例，然后在完成迁移后恢复它们。在你的演习中尝试一些是理想的。

> 最后也是非常重要的 PRO 提示是在与 Heroku 实例相同的****区域创建 RDS 实例。这将大大减少与数据库通信的延迟。在为 heroku US 主持写作的时候，那是** `**us-east-1**` **地区。您可以在此** **处查看** [**设置的准确区域。**](https://devcenter.heroku.com/articles/regions)**

**希望这篇文章能给你下一次 Heroku 到 RDS Postgres 迁移带来一些好的想法。我知道有许多移动部分，但是如果您一步一步来，并执行一个模拟场景，真正的迁移将会顺利进行。如果您有任何改进流程的建议，请告诉我。**

# **联系人**

**我是一名软件、数据和机器学习顾问，通过了 [AWS 机器学习](https://www.credly.com/badges/58e38a3f-af0c-441c-be90-6ed99fc4a788/public_url) & [AWS 解决方案架构师认证——专业](https://www.credly.com/badges/0db46f4c-3f60-413d-aa31-8d5e56982d2d/public_url)。如果您的下一个机器学习项目需要帮助，请联系我们。
要了解我最新的文章和项目[，请在 Medium 上关注我](https://vel-lesikov.medium.com/subscribe)。**

**其他联系人:**

*   **[领英](https://www.linkedin.com/in/velimir-lesikov/)**
*   **[中等轮廓](https://vel-lesikov.medium.com/)**
*   **[GitHub](https://github.com/velles)**