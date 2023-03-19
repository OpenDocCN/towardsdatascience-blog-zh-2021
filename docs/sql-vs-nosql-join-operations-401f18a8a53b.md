# SQL 与 NoSQL —连接操作

> 原文：<https://towardsdatascience.com/sql-vs-nosql-join-operations-401f18a8a53b?source=collection_archive---------17----------------------->

## 比较两者中连接操作的实用指南

![](img/9c8b2bca8e3c32a3b3c43410c7f0bf41.png)

在 [Unsplash](https://unsplash.com/s/photos/share?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的 [Mineragua 苏打水](https://unsplash.com/@mineragua?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

SQL(结构化查询语言)用于管理以表格形式存储数据的数据库，这些表格具有带标签的行和列。NoSQL 指的是非 SQL 或非关系数据库设计。它仍然提供了一种有组织的方式来存储数据，但不是以表格的形式。

NoSQL 数据库存储数据的常用结构是键值对、宽列、图形或文档。一个流行的 NoSQL 数据库是 MongoDB，它将数据存储为文档。

MongoDB 中的文档由字段-值对组成。文档被组织在一个称为集合的结构中。打个比方，文档可以看作是表中的一行，集合可以看作是整个表。

在本文中，我们将从连接操作的角度比较 SQL 数据库(MySQL)和 NoSQL 数据库(MongoDB)。我还写了一篇[文章](/sql-vs-nosql-in-8-examples-25aebcf49922)，演示如何执行查询 SQL 和 NoSQL 数据库的基本操作。

当我们从关系数据库中检索数据时，所需的数据通常分布在多个表中。在这种情况下，我们使用 SQL 连接来处理包括从两个或更多相关表中选择行的任务。

在 NoSQL 的情况下，一个项目(或数据点)的数据主要存储在一个集合中。然而，在某些情况下，我们可能需要跨越多个集合来获取我们需要的所有数据。

因此，连接查询对于这两种类型数据库都至关重要。

我准备了两个具有相同虚构数据的表和集合。第一个包含零售企业的客户信息。第二个包含关于这些客户所下订单的信息。

下面是客户和订单表中的一个条目(即行):

```
+---------+------+----------+--------+
| cust_id | age  | location | gender |
+---------+------+----------+--------+
|    1000 |   42 | Austin   | female |
+---------+------+----------+--------++----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        1 | 2020-10-01 |  27.40 |    1001 |
+----------+------------+--------+---------+
```

下面是客户和订单集合中的一个文档:

```
{
 "_id" : ObjectId("600e120b44284c416405dd7e"),
 "cust_id" : "1000",
 "age" : 42,
 "location" : "Austin",
 "gender" : "Female"
}{
 "_id" : ObjectId("600e141d44e046eb7c92c4fe"),
 "order_id" : "1",
 "date" : "2020-10-01",
 "amount" : 27.4,
 "cust_id" : "1001"
}
```

这两个表通过 cust_id 列相互关联。以下示例包括需要使用联接操作的查询。我将在两个数据库中完成相同的任务，以便我们可以看到不同之处和相似之处。

## 示例 1

我们希望看到 40 岁以上的客户所下的订单。

NoSQL(蒙古数据库):

我们可以在聚合管道中使用“$lookup”关键字执行连接操作。聚合管道在 MongoDB 中非常有用，因为它们允许在一个管道中执行许多不同种类的操作，比如过滤、排序、分组、应用数据聚合等等。

在本例中，我们首先使用“$match”关键字根据客户年龄过滤文档，然后从 orders 表中选择符合过滤条件的文档。

```
> db.customer.aggregate([
... { $match: { age: {$gt:40} }},
... { $lookup: { from: "orders",
...              localField: "cust_id",
...              foreignField: "cust_id",
...              as: "orders_docs" }}
... ]).pretty(){
 "_id" : ObjectId("600e120b44284c416405dd7e"),
 "cust_id" : "1000",
 "age" : 42,
 "location" : "Austin",
 "gender" : "Female",
 "orders_docs" : [
     {
        "_id" : ObjectId("600e141d44e046eb7c92c4ff"),
        "order_id" : "2",
        "date" : "2020-10-01",
        "amount" : 36.2,
        "cust_id" : "1000"
     },
     {
        "_id" : ObjectId("600e157c44e046eb7c92c50a"),
        "order_id" : "13",
        "date" : "2020-10-03",
        "amount" : 46.1,
        "cust_id" : "1000"
     }
 ]
}
```

本地和外部字段指示用于连接值的字段名称。输出包含客户集合中符合指定条件的文档以及这些客户的订单。碰巧只有一位顾客年龄超过 40 岁，她有两份订单。

SQL (MySQL)

我们可以在一个选择查询中连接两个表，如下所示。

```
mysql> select orders.* 
    -> from customer
    -> join orders
    -> on customer.cust_id = orders.cust_id
    -> where customer.age > 40;+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        2 | 2020-10-01 |  36.20 |    1000 |
|       13 | 2020-10-03 |  46.10 |    1000 |
+----------+------------+--------+---------+
```

在普通的 select 语句中，我们只写入要选择的列的名称。当我们连接表时，用表的名称指定列，以便 SQL 知道列来自哪里。

然后我们写下带有连接关键字的表名(例如，客户连接订单)。“on”关键字用于指示这些表是如何相关的。where 语句根据给定的条件筛选行。

## 示例 2

我们希望看到每个位置的客户的平均订单量。

NoSQL(蒙古数据库):

此任务要求联接两个集合，然后应用数据聚合。这两者都可以在聚合管道中使用“$lookup”和“$group”阶段来实现。

```
> db.customer.aggregate([
... { $lookup: { from: "orders",
...              localField: "cust_id",
...              foreignField: "cust_id",
...              as: "orders_docs" }},
... { $group: { _id: "$location", 
...             avg_amount: { $avg: "$amount" }}}
... ]){ "_id" : "Houston", "avg_amount" : 44.450000 }
{ "_id" : "Dallas", "avg_amount" : 34.591667 }
{ "_id" : "Austin", "avg_amount" : 33.333333 }
```

在“$lookup”阶段的连接操作之后，我们通过选择“$location”作为 id，基于位置对文档进行分组。下一部分指定了聚合函数(在我们的例子中是“$avg ”)和要聚合的字段。

SQL (MySQL)

我们在选择列时应用聚合函数。使用 group by 子句根据位置对结果进行分组。

```
mysql> select customer.location, avg(orders.amount) as avg_amount
    -> from customer
    -> join orders
    -> on customer.cust_id = orders.cust_id
    -> group by customer.location;+----------+------------+
| location | avg_amount |
+----------+------------+
| Austin   |  33.333333 |
| Dallas   |  34.591667 |
| Houston  |  44.450000 |
+----------+------------+
```

## 示例 3

在本例中，我们将在前面的示例中添加一个过滤标准。对于每个地点，我们来计算 30 岁以下客户的平均订单额。

NoSQL(蒙古数据库):

我们只需要在管道的开头添加一个“$match”阶段来应用过滤标准。

```
> db.customer.aggregate([
... { $match: { age: {$lt: 30} }},
... { $lookup: { from: "orders",
...              localField: "cust_id",
...              foreignField: "cust_id",
...              as: "orders_docs" }},
... { $group: { _id: "$location",
...             avg_amount: { $avg: "$amount" }}}
... ]){ "_id" : "Houston", "avg_amount" : 35.625000 }
{ "_id" : "Dallas", "avg_amount" : 34.591667 }
{ "_id" : "Austin", "avg_amount" : 36.000000 }
```

在“$match”阶段，我们指定条件以及要过滤的字段名。

SQL (MySQL)

筛选条件是通过在 select 语句中使用 where 子句添加的。

```
mysql> select customer.location, avg(orders.amount) as avg_amount
    -> from customer
    -> join orders
    -> on customer.cust_id = orders.cust_id
    -> where customer.age < 30
    -> group by customer.location;+----------+------------+
| location | avg_amount |
+----------+------------+
| Austin   |  36.000000 |
| Dallas   |  34.591667 |
| Houston  |  35.625000 |
+----------+------------+
```

由于行是在 group by 子句之前过滤的，所以我们可以使用 where 子句。如果我们需要应用基于聚合值的过滤器(例如 avg_amount > 35)，应该使用 having 子句。

## 结论

我们需要从关系(SQL)或非关系(NoSQL)数据库中检索的数据可能分散到多个表或集合中。因此，全面理解连接操作非常重要。

我们已经做了三个基本的例子来演示 SQL 和 NoSQL 数据库中连接操作的思想和实现。

感谢您的阅读。如果您有任何反馈，请告诉我。