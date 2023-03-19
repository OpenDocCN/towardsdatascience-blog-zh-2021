# SQL 中的 UNION:一个必须知道的子句

> 原文：<https://towardsdatascience.com/union-in-sql-a-must-know-clause-1e5a4d7f7bde?source=collection_archive---------19----------------------->

## 分析多个表的结果

![](img/c4eb9022be2cc5a35343b4f17dec154c.png)

来源:图片来自 [Pixabay](https://pixabay.com/photos/hands-teamwork-team-spirit-cheer-up-1939895/)

我们在 SQL 中学习的许多常见查询(如 GROUP BY)通常用于孤立地分析一个表。在将两个表连接在一起并将它们视为一个表时，也经常使用 JOIN 子句。

但是，经常会有需要跨多个表分析结果的情况。使用 UNION 子句可以做到这一点，本文将举例说明该子句如何工作。

# UNION vs UNION ALL vs INTERSECT

考虑以下两个表:

```
houses=# select * from onebedroom;
   city   | price 
----------+-------
 Paris    |  1000
 Munich   |   850
 Rome     |   930
 Helsinki |  1200
(4 rows)houses=# select * from onebedroom_v2;
   city    | price 
-----------+-------
 Helsinki  |  1200
 Berlin    |   750
 Prague    |   500
 Amsterdam |  1400
(4 rows)
```

这两个表格显示了欧洲主要城市一居室公寓的价格(数值是假设的，由作者虚构)。

UNION、UNION ALL 和 INTERSECT 如何组合这两个表的结果？

在这两个表中，我们可以看到城市赫尔辛基有一个重复的值(价格为€每月 1200)。让我们看看这个值在三个子句中是如何处理的。

## 联盟

当用 UNION 组合这两个表时，我们可以看到只保留了 Helsinki 条目的一个实例——删除了重复的条目。

```
houses=# select * from onebedroom union select * from onebedroom_v2;
   city    | price 
-----------+-------
 Paris     |  1000
 Prague    |   500
 Helsinki  |  1200
 Berlin    |   750
 Amsterdam |  1400
 Rome      |   930
 Munich    |   850
(7 rows)
```

UNION ALL 怎么样？

## 联合所有

当使用 UNION ALL 子句时，我们可以看到赫尔辛基的两个条目保留在表中，没有删除重复的条目。

```
houses=# select * from onebedroom union all select * from onebedroom_v2;
   city    | price 
-----------+-------
 Paris     |  1000
 Munich    |   850
 Rome      |   930
 Helsinki  |  1200
 Helsinki  |  1200
 Berlin    |   750
 Prague    |   500
 Amsterdam |  1400
(8 rows)
```

## 横断

INTERSECT 子句与 UNION 子句的工作方式不同，因为该子句只标识两个表共有的条目，并维护该条目—所有其他条目都被删除。

在这个例子中，我们可以看到赫尔辛基条目被返回，因为这个条目对两个表都是公共的——所有其他条目都不存在。

```
houses=# select * from onebedroom intersect select * from onebedroom_v2;
   city   | price 
----------+-------
 Helsinki |  1200
(1 row)
```

# 对表格中的值求和并求平均值

现在，让我们考虑下面的表(同样，值是假设的，由作者编造)。

```
houses=# select * from onebedroom;
  city  | price 
--------+-------
 Paris  |  1000
 Munich |   850
 Rome   |   930
(3 rows)houses=# select * from twobedroom;
  city  | price 
--------+-------
 Paris  |  1400
 Munich |  1300
 Rome   |  1500
(3 rows)houses=# select * from threebedroom;
  city  | price 
--------+-------
 Paris  |  2800
 Munich |  2200
 Rome   |  2000
(3 rows)
```

在巴黎、慕尼黑和罗马，我们希望:

1.  对每个城市的值求和
2.  平均每个城市的值

让我们看看如何在第一个实例中对值求和。我们将从前两个表开始。

```
houses=# select city,sum(price) total
houses-# from
houses-# (
houses(#     select city,price
houses(#     from onebedroom
houses(#     union all
houses(#     select city,price
houses(#     from twobedroom
houses(# ) t
houses-# group by city;
  city  | total 
--------+-------
 Rome   |  2430
 Paris  |  2400
 Munich |  2150
(3 rows)
```

在上面的脚本中，我们可以看到 UNION ALL 子句被用来组合两个表的结果，并且价格的总和( *sum(price)* )被定义为一个总值。

要对三个表中的值求和，只需使用两个 UNION ALL 子句:

```
houses=# select city,sum(price) total
houses-# from
houses-# (
houses(#     select city,price
houses(#     from onebedroom
houses(#     union all
houses(#     select city,price
houses(#     from twobedroom
houses(#     union all
houses(#     select city,price
houses(#     from threebedroom
houses(# ) t
houses-# group by city;
  city  | total 
--------+-------
 Rome   |  4430
 Paris  |  5200
 Munich |  4350
(3 rows)
```

对于平均值，该条款实际上保持不变。这一次，我们只是要求价格的平均值，而不是总和。

```
houses=# select city,avg(price) total
houses-# from
houses-# (
houses(#     select city,price
houses(#     from onebedroom
houses(#     union all
houses(#     select city,price
houses(#     from twobedroom
houses(#     union all
houses(#     select city,price
houses(#     from threebedroom
houses(# ) t
houses-# group by city;
   city   |         total         
----------+-----------------------
 Rome     | 1476.6666666666666667
 Helsinki | 1200.0000000000000000
 Paris    | 1733.3333333333333333
 Munich   | 1450.0000000000000000
(4 rows)
```

在上述子句中还可以使用其他聚合函数，如 MIN、MAX 等。

# 结论

在本文中，您已经看到:

*   工会条款如何运作
*   UNION、UNION ALL 和 INTERSECT 之间的区别
*   如何计算多个表格中数值的总和及平均值

非常感谢您的宝贵时间，非常感谢您的任何问题或反馈。

# 参考

*   [堆栈溢出- SQL:如何对不同表中的两个值求和](https://stackoverflow.com/questions/19436895/sql-how-to-to-sum-two-values-from-different-tables)
*   斯蒂芬斯、琼斯、普勒(2016)。24 小时内 SamsTeachYourself SQL

*免责声明:本文是在“原样”的基础上编写的，没有担保。它旨在提供数据科学概念的概述，不应被解释为专业建议。本文中的发现和解释是作者的发现和解释，不被本文中提到的任何第三方认可或隶属于任何第三方。作者与本文提及的任何第三方无任何关系。*