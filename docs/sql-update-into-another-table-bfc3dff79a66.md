# SQL —更新到另一个表中

> 原文：<https://towardsdatascience.com/sql-update-into-another-table-bfc3dff79a66?source=collection_archive---------20----------------------->

## 在一条语句中更新一个表中的记录，并将更新后的记录插入到另一个表中

![](img/b364a209888faba1959238417c3737c2.png)

我们的桌子上有些工作要做(图片由[梅布尔·安伯](https://www.pexels.com/@mabelamber)在[像素](https://www.pexels.com/photo/close-up-photography-of-red-and-white-road-signage-117602/)上拍摄)

“我为什么需要这样的查询？”将**更新为**语句是有利的，主要有两个原因:

*   语句是[原子](https://en.wikipedia.org/wiki/Atomicity_(database_systems))；要么两者都发生，要么什么都不发生，即如果更新或插入失败，它将回滚更改。
*   它更便宜:你的数据库只需要查找一次记录。或者，执行单独的插入和删除需要两次查找
*   吹牛的权利:给你的老板和同事留下深刻印象

深信不疑？“给我看一些代码！”。好的，但是首先我们必须设置一些表来演示查询。让我们编码:

# 设置:创建一些包含数据的表

想象一下，我们有一个有很多文章的新闻网站。如果用户注意到某篇文章中有拼写错误，他/她可以通过网站提交错误。这些错误存储在一个名为 SpellingErrors 的表中。我们将存储包含错误的文章的 Id，以及消息和报告者的电子邮件地址。我们存储了电子邮件地址，以便联系记者提问。

当我们的作者之一修复拼写错误，我们更新记录设置电子邮件地址为空；我们不再需要它，我们想保证记者的隐私。此外，我们想保留一些关于错误的信息，这样我们就可以可怕地惩罚犯了很多错误的作者。为此，我们将一些数据存储到一个名为 SpellingErrorsHistory 的表中。让我们首先创建表并插入一些错误。

用一些记录创建我们的表

执行这些查询。如果您从拼写错误表中选择，您将看到以下内容:

![](img/3c0de4754de5e153221c47a3f7313d88.png)

我们的拼写错误表

# 执行我们的查询

让我们开始创建查询吧！让我们来看看这个查询，然后解释它在做什么。

我们的一个作者已经修复了 Id 为 1 的错误。我们将把它存储在一个名为@targetId 的变量中。然后，我们更新 SpellingErrors 表并将 ReporterEmail 设置为 NULL，其中 Id 等于@targetId (1)。使用输出，我们可以访问已经从 SpellingsErrors 表中更新的列。我们将一些列以及一个额外的列(SolvedMessage)输出到 SpellingErrorsHistory 表中。还要注意，我们没有在这个表中保存用户的电子邮件数据。

恭喜你，你成功了！这是一个简单、安全的查询，允许您同时执行两个查询。

# 结论

使用这个查询，我们可以执行原子操作，更新一个表中的记录，同时将这些更新的记录插入到另一个表中。另请查看:

*   [删除到另一个表中](https://mikehuls.medium.com/sql-delete-into-another-table-b5b946a42299)
*   [使用交易撤销查询](https://mikehuls.medium.com/sql-rolling-back-statements-with-transactions-81937811e7a7?)
*   [在一条语句中插入、删除和更新](https://mikehuls.medium.com/sql-insert-delete-and-update-in-one-statement-sync-your-tables-with-merge-14814215d32c)
*   [更新选择一批记录](https://mikehuls.medium.com/sql-update-select-in-one-query-b067a7e60136)
*   [版本控制你的数据库](https://mikehuls.medium.com/version-control-your-database-part-1-creating-migrations-and-seeding-992d86c90170)

编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟我来](https://mikehuls.medium.com/)！