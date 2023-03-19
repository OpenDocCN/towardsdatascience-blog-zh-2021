# 使用 PostgreSQL 的 SQL 视图

> 原文：<https://towardsdatascience.com/sql-views-with-postgresql-493ba7aa0934?source=collection_archive---------14----------------------->

## 简化任务的聪明方法

![](img/033fd36aa447f6b5950ff174c44dfad0.png)

在 [Unsplash](https://unsplash.com/s/photos/save?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Arvind meena mina](https://unsplash.com/@perfectshot4u?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

SQL 是数据科学家的必备技能。由于许多公司将数据存储在关系数据库中，我们需要使用 SQL 来访问、查询和分析这些数据。

SQL 能够执行比基本查询更高级的任务。它提供了几个使 SQL 成为高效数据分析工具的函数。

下面可能是最简单的 SQL 查询。它选择表格中的所有行和列。

```
SELECT * FROM table_name;
```

随着任务复杂性的增加，查询的语法也变得更加复杂。此外，您可能需要查询多个表来获得所需的数据。因此，您的查询可能有几行代码。

您肯定不希望一遍又一遍地输入如此复杂的查询。一种选择是保存它并从那里复制。然而，SQL 提供了一种更好的方法，即 SQL 视图。

视图是数据库中存储的查询。我们可以像查询表一样查询视图。但是，除了 PostgreSQL 中的物化视图之外，视图不存储数据。

类比将帮助我们更好地理解视图的概念。假设您编写了一个 Python 脚本，该脚本读取一个 csv 文件，计算一些统计数据，然后返回结果。你需要不时地执行这项任务。如果您将脚本保存在您的工作环境中，您可以在需要执行任务时执行它。您不必每次都编写相同的脚本。

让我们做一些例子来演示如何使用视图。我用模拟数据创建了两个名为“订单”和“产品”的表。

```
select * from orders; order_id | product_id | order_qty |    date
----------+------------+-----------+------------
      101 |       1001 |         2 | 2021-01-04
      102 |       1423 |         1 | 2021-01-04
      103 |       1260 |         5 | 2021-01-13
      104 |       1590 |         5 | 2021-05-13
      105 |       1002 |         3 | 2021-01-13
      106 |       1600 |         2 | 2021-05-13 select * from product; product_id | price | description
------------+-------+-------------
       1001 |  2.50 | bread
       1002 |  1.90 | water
       1423 | 10.90 | icecream
       1260 |  5.90 | tomato
       1590 |  4.90 | egg
       1600 |  2.90 | milk
```

订单表包含产品 id、订单数量和日期信息。产品表包含产品的价格和描述。

以下查询计算每份订单的总金额。

```
SELECT 
   O.order_id, 
   (O.order_qty * P.price) AS order_amount                         FROM orders O 
LEFT JOIN product P on O.product_id = P.product_id 
ORDER BY order_amount desc; order_id | order_amount
----------+--------------
      103 |        29.50
      104 |        24.50
      102 |        10.90
      106 |         5.80
      105 |         5.70
      101 |         5.00
```

由于产品价格和订单数量在不同的表中，我们需要在这个查询中使用一个连接。

假设这是一个“高度复杂”的查询，我们不想每次都键入它。我们的解决方案是将其存储在视图中。

```
CREATE VIEW order_amounts AS
SELECT 
   O.order_id, 
   (O.order_qty * P.price) AS order_amount                         FROM orders O 
LEFT JOIN product P on O.product_id = P.product_id 
ORDER BY order_amount desc;
```

使用 create view 语句和查询创建视图。我们还需要为视图指定一个名称。

创建视图不会返回任何内容。我们可以像查询表一样查询这个视图。

```
SELECT * FROM order_amounts; order_id | order_amount
----------+--------------
      103 |        29.50
      104 |        24.50
      102 |        10.90
      106 |         5.80
      105 |         5.70
      101 |         5.00
```

我们接受金额高于 20 英镑的订单吧。

```
SELECT * FROM order_amounts
WHERE order_amount > 20; order_id | order_amount
----------+--------------
      103 |        29.50
      104 |        24.50
```

视图对于简化任务非常有用。我们用作示例的查询并不复杂。当我们执行涉及多重过滤、聚合和嵌套 select 语句的高度复杂的任务时，这些视图就派上了用场。

使用视图的另一个优点是可以更灵活地控制数据库访问。例如，表中的某些列可能包含只有特定员工才能访问的敏感信息。

当我们查询视图时，我们只能检索视图中存在的列，而不是视图中使用的表中的每一列。因此，视图允许我们授予对表的部分访问权。

我们可以使用 alter view 语句来更改视图的各种辅助属性。例如，列的名称可以更改如下:

```
ALTER VIEW order_amounts RENAME order_amount TO total_amount;SELECT * FROM order_amounts; order_id | total_amount
----------+--------------
      103 |        29.50
      104 |        24.50
      102 |        10.90
      106 |         5.80
      105 |         5.70
      101 |         5.00
```

为了删除视图，使用了 drop view 语句。它类似于用于删除表的 drop table 语句。

```
DROP VIEW order_amounts;SELECT * FROM order_amounts;
ERROR:  relation "order_amounts" does not exist
```

## 结论

SQL 视图是存储查询。它们允许我们通过保存复杂的查询以备后用来简化某些任务。这些查询可以看作是一个表，所以我们可以像查询表一样查询它们。

感谢您的阅读。如果您有任何反馈，请告诉我。