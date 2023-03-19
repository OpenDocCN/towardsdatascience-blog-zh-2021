# 查询 NoSQL 数据库的 8 个示例

> 原文：<https://towardsdatascience.com/8-examples-to-query-a-nosql-database-fc3dd1c9a8c?source=collection_archive---------2----------------------->

## MongoDB 实用指南

![](img/9ef12343db72ca9899d30c30571af703.png)

[粘土银行](https://unsplash.com/@claybanks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/select?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

NoSQL 指的是非 SQL 或非关系数据库设计。它提供了一种有组织的方式来存储数据，但不是以表格的形式(即标记的行和列)。

NoSQL 数据库存储数据的常用结构是键值对、宽列、图形或文档。数据科学生态系统中使用了几个 NoSQL 数据库。在本文中，我们将使用其中一个流行的 MongoDB。

MongoDB 将数据存储为文档。MongoDB 中的文档由字段-值对组成。文档被组织在一个称为集合的结构中。打个比方，文档可以看作是表中的一行，集合可以看作是整个表。如果您是 NoSQL 或 MongoDB 的新手，这里有一篇介绍性文章可以让您兴奋起来:

</introduction-to-nosql-with-mongodb-8e5a2513c9e8>  

我们将通过 10 个例子来演示如何从 MongoDB 数据库中检索数据。

我们有一个名为“客户”的集合。客户集合中的文档包含客户姓名、年龄、性别和最后一次购买的金额。

以下是客户集合中的一个文档:

```
{
 "_id" : ObjectId("600c1806289947de938c68ea"),
 "name" : "John",
 "age" : 32,
 "gender" : "male",
 "amount" : 32
}
```

文档以 JSON 格式显示。

## 示例 1

查询属于特定客户的文档。

我们使用 find 方法从 MongoDB 数据库中查询文档。如果不使用任何参数或集合，find 方法将检索所有文档。

我们希望看到文档属于客户 John，因此需要在 find 方法中指定 name 字段。

```
> db.customer.find( {name: "John"} ){ "_id" : ObjectId("600c1806289947de938c68ea"), "name" : "John", "age" : 32, "gender" : "male", "amount" : 32 }
```

我们可以附上漂亮的方法，使文件看起来更有吸引力。

```
> db.customer.find( {name: "John"} ).pretty(){
 "_id" : ObjectId("600c1806289947de938c68ea"),
 "name" : "John",
 "age" : 32,
 "gender" : "male",
 "amount" : 32
}
```

现在读起来更容易了。

## 示例 2

查询属于 40 岁以上客户的文档。

使用逻辑运算符将该条件应用于年龄字段。“$gt”代表“大于”,用法如下。

```
> db.customer.find( {age: {$gt:40}} ).pretty(){
 "_id" : ObjectId("600c19d2289947de938c68ee"),
 "name" : "Jenny",
 "age" : 42,
 "gender" : "female",
 "amount" : 36
}
```

## 示例 3

查询属于 25 岁以下女性客户的文档。

这个例子就像是前面两个例子的结合。这两个条件都必须满足，所以我们使用“与”逻辑来组合这两个条件。这可以通过用逗号分隔两个条件来完成。

```
> db.customer.find( {gender: "female", age: {$lt:25}} ).pretty(){
 "_id" : ObjectId("600c19d2289947de938c68f0"),
 "name" : "Samantha",
 "age" : 21,
 "gender" : "female",
 "amount" : 41
}{
 "_id" : ObjectId("600c19d2289947de938c68f1"),
 "name" : "Laura",
 "age" : 24,
 "gender" : "female",
 "amount" : 51
}
```

“$lt”代表“小于”。

## 实例 4

在这个例子中，我们将以不同的方式重复前面的例子。多个条件也可以用“与”逻辑组合，如下所示。

```
> db.customer.find( {$and :[ {gender: "female", age: {$lt:25}} ]} ).pretty()
```

用于组合条件的逻辑在开始处被指出。剩下的部分和前面的例子一样，但是我们需要把条件放在一个列表中([ ])。

## 实例 5

查询 25 岁以下的男性客户。

此示例需要一个带有“或”逻辑的复合查询。我们只需要把“$和”改成“$或”。

```
> db.customer.find( { $or: [ {gender: "male"}, {age: {$lt: 22}} ] }){ "_id" : ObjectId("600c1806289947de938c68ea"), "name" : "John", "age" : 32, "gender" : "male", "amount" : 32 }{ "_id" : ObjectId("600c19d2289947de938c68ed"), "name" : "Martin", "age" : 28, "gender" : "male", "amount" : 49 }{ "_id" : ObjectId("600c19d2289947de938c68ef"), "name" : "Mike", "age" : 29, "gender" : "male", "amount" : 22 }{ "_id" : ObjectId("600c19d2289947de938c68f0"), "name" : "Samantha", "age" : 21, "gender" : "female", "amount" : 41 }
```

## 实例 6

MongoDB 允许在从数据库中检索时聚合值。例如，我们可以计算男性和女性的总购买量。使用 aggregate 方法代替 find 方法。

```
> db.customer.aggregate([
... { $group: {_id: "$gender", total: {$sum: "$amount"} } }
... ]){ "_id" : "female", "total" : 198 }
{ "_id" : "male", "total" : 103 }
```

我们来详细说明一下语法。我们首先通过选择“$gender”作为 id，按照性别列对文档进行分组。下一部分指定了聚合函数(在我们的例子中是“$sum ”)和要聚合的列。

如果您熟悉 Pandas，语法与 groupby 函数非常相似。

## 例 7

让我们把前面的例子更进一步，添加一个条件。因此，我们首先选择“匹配”一个条件的文档并应用聚合。

以下查询是一个聚合管道，它首先选择 25 岁以上的客户，并计算男性和女性的平均购买量。

```
> db.customer.aggregate([
... { $match: { age: {$gt:25} } },
... { $group: { _id: "$gender", avg: {$avg: "$amount"} } }
... ]){ "_id" : "female", "avg" : 35.33 }
{ "_id" : "male", "avg" : 34.33 }
```

## 实施例 8

上例中的查询只包含两个组，因此没有必要对结果进行排序。然而，我们可能有返回几个值的查询。在这种情况下，对结果进行排序是一种很好的做法。

我们可以按照平均金额以升序对之前查询的结果进行排序。

```
> db.customer.aggregate([
... { $match: { age: {$gt:25} } },
... { $group: { _id: "$gender", avg: {$avg: "$amount"} } },
... { $sort: {avg: 1} }
... ]){ "_id" : "male", "avg" : 34.33 }
{ "_id" : "female", "avg" : 35.33 }
```

我们刚刚在聚合管道中添加了“$sort”。用于排序的字段与排序行为一起指定。1 表示升序，-1 表示降序。

## 结论

SQL 和 NoSQL 在数据科学生态系统中都至关重要。数据科学的燃料是数据，因此一切都始于正确、维护良好且易于访问的数据。SQL 和 NoSQL 都是这些过程的关键角色。

我们已经简要介绍了查询 MongoDB 数据库。当然，还有更多的内容需要讨论。我们可能会为一个典型的任务编写更高级的查询。然而，一旦您熟悉了基础知识，您就可以轻松地进入更高级的查询。

请继续关注更多关于 SQL 和 NoSQL 数据库的文章。感谢您的阅读。如果您有任何反馈，请告诉我。