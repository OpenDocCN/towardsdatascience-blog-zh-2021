# 在 SQL 中使用计数/分组依据/连接组合

> 原文：<https://towardsdatascience.com/using-the-count-group-by-join-combination-in-sql-414d81626764?source=collection_archive---------2----------------------->

## 计算 SQL 中条目的实例数

![](img/85426c0df01f33dcbea64eb71a548fc1.png)

来源:图片来自 [Pixabay](https://pixabay.com/photos/server-cloud-development-business-1235959/)

在使用 SQL 中的表时，经常会希望计算该表中实例的数量。这可以是产品类别、品牌等。

当使用一个表时，这就足够简单了。但是，当跨多个表工作时，这项任务可能会有些麻烦。

这里，我们将看一个例子，说明如何将 COUNT 和 GROUP BY 函数与一个内部连接结合起来，以计算特定零售商的不同产品类型的数量。

# 服装店示例

让我们假设一个繁忙的服装店有几个表存储在一个数据库中，该数据库包含在任何给定时间存储在其仓库中的所有商品。这些表包含商品名称、价格、品牌、产品类型等信息。业主难以估计仓库中每件产品的具体数量，即仓库中存放了多少条牛仔裤、多少件 t 恤？

考虑以下两个表:

```
>>> select * from clothes1 limit 1;item                | colour   | type 
--------------------+----------+----------
Grass Green T-Shirt | Green    | T-Shirt>>> select * from clothes2 limit 1;item               | price   | size 
-------------------+---------+--------
Sky Blue Jeans     |  79.99  | 31
```

假设以下场景。 **clothes1** 代表仓库中曾经出现过的所有衣物。 **clothes2** 代表仓库中任何一次出现的所有衣物。

假设只有 **clothes1** 表包含关于**类型**的信息，这意味着第一个表必须与第二个表连接，以便通过类型识别第一个表中出现的每个项目。

因为我们只想计算当前出现的服装项目的类型，而不是那些存储在第二个表中但没有出现的服装项目的类型，所以我们使用内部连接而不是完全连接来实现这个目的。

使用完全连接将返回 clothes1 中的所有条目，即曾经出现在仓库中的条目。因为我们只对计算当前库存感兴趣，所以我们使用了一个内部连接。

# 询问

以下是查询:

```
>>> select t1.type, count(*) from clothes1 as t1 inner join clothes2 as t2 on t1.item=t2.item group by t1.type;
```

从这个查询中，我们获得了一个类似如下的表:

```
 type    | count 
-----------+-------
 T-Shirt   |  2496
 Jeans     |  3133
 Sneakers  |  2990
 Shirts    |  3844
 Ties      |  1789
 Trousers  |  2500
(6 rows)
```

# 结论

在这个简短的教程中，您已经看到了如何在 SQL 中使用 COUNT/GROUP BY/JOIN 组合来聚合多个表中的条目。

虽然 GROUP BY 查询在只处理一个表时可以简单地实现这一点，但在处理多个表时，情况会变得更加复杂。

非常感谢阅读，任何问题或反馈都非常感谢！您还可以在这里找到原始文章[以及有用的 SQL 实践的更多示例。](https://www.michael-grogan.com/articles/count-groupby-join-sql)

*免责声明:本文是在“原样”的基础上编写的，没有担保。它旨在提供数据科学概念的概述，不应被解释为专业建议。本文中的发现和解释是作者的发现和解释，不被本文中提到的任何第三方认可或隶属于任何第三方。*