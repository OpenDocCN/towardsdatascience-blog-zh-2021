# 使用 MongoDB 介绍 NoSQL

> 原文：<https://towardsdatascience.com/introduction-to-nosql-with-mongodb-8e5a2513c9e8?source=collection_archive---------24----------------------->

## 实用入门指南

![](img/c9d5cf284f088c10eb81fab5fe3760af.png)

照片由[金莎·艾利斯](https://unsplash.com/@kymellis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/view?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

关系数据库以带有标签的行和列的表格形式存储数据。尽管关系数据库通常为存储数据提供了一个不错的解决方案，但在某些情况下，速度和可伸缩性可能是一个问题。

大多数关系数据库管理系统使用 SQL(结构化查询语言)来管理以表格形式存储数据的数据库。NoSQL 指的是非 SQL 或非关系数据库设计。它仍然提供了一种有组织的方式来存储数据，但不是以表格的形式。

NoSQL 数据库存储数据的常用结构是键值对、宽列、图形或文档。数据科学生态系统中使用了几个 NoSQL 数据库。在本文中，我们将使用其中一个流行的 MongoDB。

MongoDB 将数据存储为文档。MongoDB 中的文档由字段-值对组成。例如，下面的内容可以是 MongoDB 中的一个文档。

```
{
 "name": "John",
 "age": 26,
 "gender": "Male",
}
```

文档被组织在一个称为“集合”的结构中。打个比方，我们可以把文档想象成表格中的行，把集合想象成表格。

文档采用 JSON (JavaScript 对象表示法)格式。JSON 是一种常用的格式，但是它有一些缺点。

因为 JSON 是基于文本的格式，所以很难解析。就内存效率而言，这也不是一个非常理想的选择。JSON 还限制了支持的数据类型的数量。

为了克服这些挑战，MongoDB 引入了一种叫做 BSON (Binary JSON)的新格式。可以认为是 JSON 的二进制表示。与 JSON 相比，它非常灵活和快速。BSON 的另一个优点是它比 JSON 消耗更少的内存。

由于 BSON 是二进制编码，它不是人类可读的，这可能是一个问题。然而，MongoDB 通过允许用户以 JSON 格式导出 BSON 文件解决了这个问题。

我们可以以 BSON 格式存储数据，并以 JSON 格式查看。任何 JSON 格式的文件都可以作为 BSON 存储在 MongoDB 中。

简单介绍之后，我们来做一些练习。您可以在 Linux、macOS 或 Windows 上安装 MongoDB 社区版。如何安装在 MongoDB [文档](https://docs.mongodb.com/manual/administration/install-community/)中解释得很清楚。

我已经在我的 Linux 机器上安装了它。我们使用以下命令从终端启动 MongoDB。

```
$ sudo systemctl start mongod
```

然后，我们只需在终端中键入 mongo 即可开始使用它。

```
$ mongo
```

以下命令将打印出服务器中的 MongoDB 数据库。

```
> show dbsadmin   0.000GB
config  0.000GB
local   0.000GB
test    0.000GB
```

我们可以用 use 命令选择一个数据库。

```
> use test
switched to db test
```

我们之前提到过，MongoDB 中的文档被组织在一个称为集合的结构中。为了查看数据库中的集合，我们使用 show collections 命令。

```
> show collections
inventory
```

测试数据库中当前有一个集合。我们想创建一个名为“客户”的新集合。这相当简单。我们只需创建一个新文档并插入到集合中。如果数据库中不存在指定的集合，MongoDB 会自动创建它。

```
> db.customers.insertOne(
... {name: "John",
...  age: 26,
...  gender: "Male",
... }
... ){
 "acknowledged" : true,
 "insertedId" : ObjectId("60098638be451fc77a72a108")
}
```

正如我们在输出中看到的，文档成功地插入到集合中。让我们再次运行“显示集合”命令。

```
> show collections
customers
inventory
```

我们现在在测试数据库中有两个集合。我们可以使用 find 命令查询集合中的文档。

```
> db.customers.find(){ "_id" : ObjectId("60098638be451fc77a72a108"), "name" : "John", "age" : 26, "gender" : "Male" }
```

将显示字段-值对以及对象 id。因为 customers 集合中只有一个文档，所以我们没有在 find 函数中指定任何条件。

查找功能允许指定查询数据库的条件。从这个意义上说，它类似于 SQL 中的 select 语句。

## 结论

我们已经用 MongoDB 简单介绍了 NoSQL。当然，在 NoSQL 和 MongoDB 中还有更多的内容要介绍。

SQL 和 NoSQL 在数据科学生态系统中都至关重要。数据科学的燃料是数据，因此一切都始于正确、维护良好且易于访问的数据。SQL 和 NoSQL 都是这些过程的关键角色。

我将会写更多关于 SQL 和 NoSQL 数据库的文章。敬请期待！

感谢您的阅读。如果您有任何反馈，请告诉我。