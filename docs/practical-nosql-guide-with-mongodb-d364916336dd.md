# MongoDB 实用 NoSQL 指南

> 原文：<https://towardsdatascience.com/practical-nosql-guide-with-mongodb-d364916336dd?source=collection_archive---------39----------------------->

## 通过练习提高你的技能

![](img/62f112c21140fd0a03f0f9b1cca8a1a7.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com/s/photos/practice?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

NoSQL 指的是非 SQL 或非关系数据库设计。它提供了一种有组织的方式来存储数据，但不是像 SQL 那样以表格的形式。NoSQL 数据库存储数据的常用结构是键值对、宽列、图形或文档。

数据科学生态系统中有几个 NoSQL 数据库。其中一个流行的是 MongoDB，它将数据存储为文档。

MongoDB 中的文档由字段-值对组成。文档被组织在一个称为“集合”的结构中。打个比方，我们可以把文档想象成表格中的行，把集合想象成表格。

在本文中，我们将通过几个例子来练习在 MongoDB 中查询数据库。如果你想了解更多关于 NoSQL 以及如何在你的电脑上设置 MongoDB 的信息，这里有一个介绍性的[指南](/introduction-to-nosql-with-mongodb-8e5a2513c9e8)供你参考。

这些示例将查询一个名为“marketing”的集合，该集合存储了关于零售企业营销活动的数据[。a 该集合中的文档包括以下信息。](https://www.kaggle.com/yoghurtpatil/direct-marketing)

```
{
 "_id" : ObjectId("6014dc988c628fa57a508088"),
 "Age" : "Middle",
 "Gender" : "Male",
 "OwnHome" : "Rent",
 "Married" : "Single",
 "Location" : "Close",
 "Salary" : 63600,
 "Children" : 0,
 "History" : "High",
 "Catalogs" : 6,
 "AmountSpent" : 1318
}
```

## 示例 1

默认情况下，find 方法检索所有文档。我们可以使用 limit 方法只显示一些文档。例如，我们可以显示集合中的第一个文档，如下所示:

```
> db.marketing.find().limit(1).pretty(){
 "_id" : ObjectId("6014dc988c628fa57a508088"),
 "Age" : "Middle",
 "Gender" : "Male",
 "OwnHome" : "Rent",
 "Married" : "Single",
 "Location" : "Close",
 "Salary" : 63600,
 "Children" : 0,
 "History" : "High",
 "Catalogs" : 6,
 "AmountSpent" : 1318
}
```

数据库指的是当前数据库。我们需要在点号后指定集合名称。漂亮的方法是以更结构化的方式显示文档。如果没有 pretty 方法，输出如下所示:

```
{ "_id" : ObjectId("6014dc988c628fa57a508088"), "Age" : "Middle", "Gender" : "Male", "OwnHome" : "Rent", "Married" : "Single", "Location" : "Close", "Salary" : 63600, "Children" : 0, "History" : "High", "Catalogs" : 6, "AmountSpent" : 1318 }
```

## 示例 2

find 方法允许一些基本的过滤。我们可以为 find 方法中的字段指定所需的值，如下所示:

```
> db.marketing.find( {"Children": 1} ).limit(1).pretty(){
 "_id" : ObjectId("6014dc988c628fa57a50808a"),
 "Age" : "Middle",
 "Gender" : "Male",
 "OwnHome" : "Own",
 "Married" : "Married",
 "Location" : "Close",
 "Salary" : 85600,
 "Children" : 1,
 "History" : "High",
 "Catalogs" : 18,
 "AmountSpent" : 2436
}
```

但是，这不是过滤的最佳方式。MongoDB 的聚合管道提供了一种更有效的方式来过滤、转换和聚合数据，我们将在下面的例子中看到这一点。

## 示例 3

我们想了解收到 10 份以上目录的顾客的平均消费金额。聚合管道可用于完成此任务，如下所示。

```
> db.marketing.aggregate([
... { $match : { Catalogs : {$gt : 10} } },
... { $group : { _id : null, avgSpent : {$avg : "$AmountSpent"} } }
... ]){ "_id" : null, "avgSpent" : 1418.9411764705883 }
```

管道的第一步是匹配阶段，它根据给定的条件过滤文档。“$gt”表达式代表“大于”。分组阶段根据给定的字段和聚合函数执行聚合。

## 实例 4

在前面的例子中，我们发现收到 10 个以上目录的客户的平均消费金额。我们再进一步，分别求出不同年龄段的相同值。

```
> db.marketing.aggregate([
... { $match : { Catalogs : {$gt : 10} } },
... { $group : { _id : "$Age", avgSpent : {$avg : "$AmountSpent"} } }
... ]){ "_id" : "Middle", "avgSpent" : 1678.3965087281795 }
{ "_id" : "Old", "avgSpent" : 1666.9056603773586 }
{ "_id" : "Young", "avgSpent" : 655.813829787234 }
```

唯一的变化是“_id”值。它指定要用作分组字段的字段。

## 实例 5

可以在多个字段上执行多个聚合。例如，我们可以计算至少有一个孩子的客户的平均工资和总支出。

```
> db.marketing.aggregate([
... { $match : { Children : {$gt : 0} } },
... { $group: { 
...             _id : null,
...             "avgSalary" : {$avg: "$Salary"},
...             "totalSpent" : {$sum: "$AmountSpent"}
...           } 
... }
... ]){ "_id" : null, "avgSalary" : 57140.89219330855, "totalSpent" : 566902 }
```

每个聚合都作为组阶段中的一个单独条目写入。

## 实例 6

MongoDB 的聚合管道有一个对查询结果进行排序的阶段。当结果由几个条目组成时，这很有用。此外，一组经过排序的数据提供了一个更加结构化的概览。

以下查询返回消费超过 1000 美元的客户的平均工资。结果根据孩子的数量分组，并按平均工资降序排序。

```
> db.marketing.aggregate([
... { $match: { AmountSpent : {$gt : 1000}}},
... { $group: { _id : "$Children", avgSalary : {$avg : "$Salary"}}},
... { $sort: { avgSalary : -1 }}
... ]){ "_id" : 3, "avgSalary" : 84279.48717948717 }
{ "_id" : 2, "avgSalary" : 83111.11111111111 }
{ "_id" : 1, "avgSalary" : 78855.97014925373 }
{ "_id" : 0, "avgSalary" : 71900.76045627377 }
```

在排序阶段，“-1”表示降序，“1”表示升序。

## 例 7

在本例中，我们将看到聚合管道的两个新阶段。

项目阶段允许选择要显示的字段。由于典型的文档可能有许多字段，因此显示所有字段可能不是最佳选择。

在项目阶段，我们通过值 1 选择要显示的字段。结果仅显示指定的字段。请务必注意，在所有情况下，默认情况下都会显示 id 字段。我们需要将其显式设置为 0，以便从结果中排除 id 字段。

聚合管道中的限制阶段限制了要显示的文档数量。它与 SQL 中的 limit 关键字相同。

下面的查询根据花费的金额过滤文档，然后根据薪水进行排序。它只显示前 5 个单据的薪资和支出金额字段。

```
> db.marketing.aggregate([
... { $match : { AmountSpent : {$gt : 1500} } },
... { $sort : { Salary : -1 } },
... { $project : { _id : 0, Salary : 1, AmountSpent : 1 } },
... { $limit : 5 }
... ]){ "Salary" : 168800, "AmountSpent" : 1512 }
{ "Salary" : 140000, "AmountSpent" : 4894 }
{ "Salary" : 135700, "AmountSpent" : 2746 }
{ "Salary" : 134500, "AmountSpent" : 4558 }
{ "Salary" : 131500, "AmountSpent" : 2840 }
```

## 结论

我们已经介绍了几个演示如何查询 MongoDB 数据库的例子。聚合管道允许创建简单和高度复杂的查询。

我们已经看到了聚合管道中的 5 个基本阶段，即匹配、分组、排序、项目和限制。还有更多的阶段使查询操作更加通用和强大。

与任何其他主题一样，要熟练使用 MongoDB 编写查询需要大量的练习。

感谢您的阅读。如果您有任何反馈，请告诉我。