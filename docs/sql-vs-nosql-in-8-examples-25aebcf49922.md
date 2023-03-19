# SQL 与 NoSQL 在 8 个例子中的对比

> 原文：<https://towardsdatascience.com/sql-vs-nosql-in-8-examples-25aebcf49922?source=collection_archive---------3----------------------->

## 比较两者基本操作的实用指南

![](img/61364a88e800159f7f62a869bce8f986.png)

诺德伍德主题公司在 [Unsplash](https://unsplash.com/s/photos/comparison?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

关系数据库以带有标签的行和列的表格形式存储数据。尽管关系数据库通常为存储数据提供了一个不错的解决方案，但在某些情况下，速度和可伸缩性可能是一个问题。

大多数关系数据库管理系统使用 SQL(结构化查询语言)来管理以表格形式存储数据的数据库。NoSQL 指的是非 SQL 或非关系数据库设计。它仍然提供了一种有组织的方式来存储数据，但不是以表格的形式。

抛开对速度和可伸缩性的考虑，SQL 和 NoSQL 数据库都提供了通用而高效的数据查询方式。这对于数据库来说是必须的，因为可访问性也是至关重要的。

在本文中，我们将涵盖 8 个示例，演示如何查询 SQL 和 NoSQL 数据库。这些示例将涵盖如何:

*   基于条件选择数据
*   插入新项目
*   更新现有项目
*   应用聚合函数

我将在两个数据库中完成相同的任务，以便我们可以看到不同之处和相似之处。

我将使用 SQL 的 MySQL 和 NoSQL 的 MongoDB。在开始举例之前，让我们简要解释一下数据是如何存储在 SQL 和 NoSQL 中的。

SQL 以表格形式存储数据，表格中有带标签的行和列。NoSQL 数据库存储数据的常用结构是键值对、宽列、图形或文档。MongoDB 将数据存储为文档。MongoDB 中的文档由字段-值对组成。文档被组织在一个称为“集合”的结构中。打个比方，我们可以把文档想象成表格中的行，把集合想象成表格。

我在 MySQL 中创建了一个简单的表，并在 MongoDB 中创建了一个集合，其中包含一些汽车的特性及其价格。

这里有一个文件，确定了汽车系列中的一个项目:

```
{
 "_id" : ObjectId("600c626932e0e6419cee81a7"),
 "year" : "2017",
 "make" : "hyundai",
 "color" : "white",
 "km" : 22000,
 "price" : 32000
}
```

在 SQL 中，一个数据点(在我们的例子中是一辆汽车)由一行标识。

```
+------+---------+-------+-------+-------+
| year | make    | color | km    | price |
+------+---------+-------+-------+-------+
| 2017 | hyundai | white | 22000 | 32000 |
+------+---------+-------+-------+-------+
```

## 示例 1

找到福特制造的汽车。

NoSQL(蒙古数据库):

我们将条件传递给 find 函数。“db”是指当前数据库,“car”是我们正在查询的集合。

```
> db.car.find( {make: "ford"} ).limit(1).pretty(){
 "_id" : ObjectId("600c63cf32e0e6419cee81ab"),
 "year" : "2017",
 "make" : "ford",
 "color" : "black",
 "km" : 34000,
 "price" : 28000
}
```

福特生产的汽车不止一辆，但我使用 limit 函数只显示了一辆。

pretty 函数使输出更具可读性和吸引力。这是没有漂亮功能时的样子。

```
> db.car.find( {make: "ford"} ).limit(1){ "_id" : ObjectId("600c63cf32e0e6419cee81ab"), "year" : "2017", "make" : "ford", "color" : "black", "km" : 34000, "price" : 28000 }
```

SQL (MySQL):

我们选择所有列(*)，并在 where 子句中指定条件。

```
mysql> select * from car
    -> where make = "ford"
    -> limit 1;+------+------+-------+-------+-------+
| year | make | color | km    | price |
+------+------+-------+-------+-------+
| 2017 | ford | black | 34000 | 28000 |
+------+------+-------+-------+-------+
```

## 示例 2

找到 2019 年福特生产的汽车。

NoSQL(蒙古数据库):

我们可以传递由逗号分隔的多个条件，以表示条件上的“与”逻辑。

```
> db.car.find( {make: "ford", year: "2019"} ).pretty(){
 "_id" : ObjectId("600c63cf32e0e6419cee81af"),
 "year" : "2019",
 "make" : "ford",
 "color" : "white",
 "km" : 8000,
 "price" : 42000
}
```

SQL (MySQL):

它类似于前面的例子。我们可以使用 and 运算符在 where 子句中组合多个条件。

```
mysql> select * from car
    -> where make = "ford" and year = "2019";+------+------+-------+------+-------+
| year | make | color | km   | price |
+------+------+-------+------+-------+
| 2019 | ford | white | 8000 | 42000 |
+------+------+-------+------+-------+
```

## 示例 3

找出 2017 年福特或现代制造的汽车。

NoSQL(蒙古数据库):

我们首先将品牌的条件与“或”逻辑相结合，然后使用“与”逻辑与年份相结合。“$in”运算符可用于“或”逻辑。

```
> db.car.find( {make: {$in: ["ford","hyundai"] } , year: "2017"} ).pretty(){
 "_id" : ObjectId("600c626932e0e6419cee81a7"),
 "year" : "2017",
 "make" : "hyundai",
 "color" : "white",
 "km" : 22000,
 "price" : 32000
}{
 "_id" : ObjectId("600c63cf32e0e6419cee81ab"),
 "year" : "2017",
 "make" : "ford",
 "color" : "black",
 "km" : 34000,
 "price" : 28000
}
```

SQL (MySQL):

where 子句接受“in”操作符，因此我们可以指定类似于 NoSQL 的条件。

```
mysql> select * from car
    -> where make in ("ford","hyundai") and year = "2017";+------+---------+-------+-------+-------+
| year | make    | color | km    | price |
+------+---------+-------+-------+-------+
| 2017 | hyundai | white | 22000 | 32000 |
| 2017 | ford    | black | 34000 | 28000 |
+------+---------+-------+-------+-------+
```

## 实例 4

插入新项目。

NoSQL(蒙古数据库):

“insertOne”函数用于将单个文档插入到集合中。我们需要编写新文档的字段-值对。

```
> db.car.insertOne(
... {year: "2017", make: "bmw", color: "silver", 
...  km: 28000, price: 39000}
... ){
 "acknowledged" : true,
 "insertedId" : ObjectId("600c6bc79445b834692e3b91")
}
```

SQL (MySQL):

“插入”功能用于向表中添加新行。不像 NoSQL，我们不需要写列名。但是，值的顺序必须与表中列的顺序相匹配。

```
mysql> insert into car values 
    -> ("2017", "bmw", "silver", 28000, 39000);Query OK, 1 row affected (0.03 sec)
```

## 实例 5

将品牌“宝马”更新为“宝马”。

NoSQL(蒙古数据库):

使用更新功能。我们首先传递指示要更新的文档的条件，然后传递更新后的值和 set 关键字。

```
> db.car.update(
... { make: "bmw" },
... { $set: { make: "BMW" }},
... { multi: true }
... )WriteResult({ "nMatched" : 5, "nUpserted" : 0, "nModified" : 5 })
```

我们需要使用 multi 参数来更新满足给定条件的所有文档。否则，只有一个文档得到更新。

SQL (MySQL):

我们使用如下的更新语句:

```
mysql> update car
    -> set make = "BMW"
    -> where make = "bmw";Query OK, 5 rows affected (0.05 sec)
Rows matched: 5  Changed: 5  Warnings: 0
```

## 实例 6

在查询数据库时，SQL 和 NoSQL 在数据聚合方面都非常通用。例如，我们可以很容易地计算出每个品牌的平均价格。

NoSQL(蒙古数据库):

我们使用聚合函数。

```
> db.car.aggregate([
... { $group: { _id: "$make", avg_price: { $avg: "$price" }}}
... ]){ "_id" : "hyundai", "avg_price" : 36333.333333333336 }
{ "_id" : "BMW", "avg_price" : 47400 }
{ "_id" : "ford", "avg_price" : 35333.333333333336 }
```

我们首先通过选择“$make”作为 id，根据品牌对文档进行分组。下一部分指定了聚合函数(在我们的例子中是“$avg ”)和要聚合的字段。

如果您熟悉 Pandas，语法与 groupby 函数非常相似。

SQL (MySQL):

group by 子句用于根据给定列中的类别对行进行分组。选择列时应用聚合函数。

```
mysql> select make, avg(price)
    -> from car
    -> group by make;+---------+------------+
| make    | avg(price) |
+---------+------------+
| BMW     | 47400.0000 |
| ford    | 35333.3333 |
| hyundai | 36333.3333 |
+---------+------------+
```

## 实施例 8

我们可以在聚合函数中实现条件。对于每个品牌，我们来计算一下 2019 年制造的汽车的平均价格。

NoSQL(蒙古数据库):

我们只需要添加匹配关键字来指定条件。

```
> db.car.aggregate([
... { $match: { year: "2019" }},
... { $group: { _id: "$make", avg_price: { $avg: "$price" }}}
... ]){ "_id" : "BMW", "avg_price" : 53000 }
{ "_id" : "ford", "avg_price" : 42000 }
{ "_id" : "hyundai", "avg_price" : 41000 }
```

SQL (MySQL):

我们使用 where 和 group by 子句，如下所示:

```
mysql> select make, avg(price)
    -> from car
    -> where year = "2019"
    -> group by make;+---------+------------+
| make    | avg(price) |
+---------+------------+
| BMW     | 53000.0000 |
| ford    | 42000.0000 |
| hyundai | 41000.0000 |
+---------+------------+
```

## 结论

我们已经讨论了 8 个例子，展示了处理 SQL 和 NoSQL 数据库的基本操作。

它们都提供了更多的函数和方法来创建更高级的查询。因此，它们也可以用作数据分析和操作工具。

感谢您的阅读。如果您有任何反馈，请告诉我。