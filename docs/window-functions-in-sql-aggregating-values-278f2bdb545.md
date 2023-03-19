# SQL 中的窗口函数:聚集值

> 原文：<https://towardsdatascience.com/window-functions-in-sql-aggregating-values-278f2bdb545?source=collection_archive---------31----------------------->

## 用窗口函数计算累计

![](img/57e7bdf25b5572ba5c21f6e0f82fa980.png)

来源:照片由 [ChristopherPluta](https://pixabay.com/users/christopherpluta-108394/) 从 [Pixabay](https://pixabay.com/photos/surface-rain-drops-raindrops-455120/) 拍摄

在 SQL 中使用表时，通常会希望聚合值，或者计算表中值的累计。

在本文中，我们将研究如何使用所谓的**窗口函数**来实现这一点。

此外，我们还将看到如何将 **CASE** 语句嵌套在窗口函数中，以进一步定制基于特定条件对数组执行的计算。

# 使用窗口函数求和平均

考虑以下假设的某一天商店中列出的服装项目表(由作者虚构的值)。

```
date            | item               | price   | size 
----------------+--------------------+---------+--------
2021-01-01      | Sky Blue Jeans     |  79.99  | 31
2021-01-02      | Fire Red Jeans     |  89.99  | 36
2021-01-03      | Sky Blue Shirt     |  59.99  | 38
2021-01-04      | Grass Green Shirt  |  69.99  | 34
2021-01-05      | Peach Purple Hat   |  79.99  | 40
2021-01-06      | Sun Yellow Jeans   |  109.99 | 42
2021-01-07      | Olive Green Hat    |  89.99  | 37
```

现在，假设所有者希望在累积的基础上对每个值求和并求平均值，即创建一个新数组，显示前两个值的总和，然后是前三个值，依此类推。这同样适用于计算平均值。

可以使用窗口函数对这些值求和，如下所示(显示了前五行):

```
>>> SELECT date,price,
>>>   SUM(price) OVER (ORDER BY date)
>>>   AS total_price
>>> FROM table;date                 | price   | total_price 
---------------------+---------+------------
 2021-01-01          |  79.99  |      79.99
 2021-01-02          |  89.99  |     169.98
 2021-01-03          |  59.99  |     229.97
 2021-01-04          |  69.99  |     299.96
 2020-01-05          |  79.99  |     379.95
(5 rows)
```

同理，也可以计算出平均累计价格。

```
>>> SELECT date,price,
>>>   AVG(price) OVER (ORDER BY date)
>>>   AS mean_price
>>> FROM table;date                 | price   | mean_price 
---------------------+---------+------------
 2021-01-01          |  79.99  |      79.99
 2021-01-02          |  89.99  |      84.99
 2021-01-03          |  59.99  |      76.66
 2021-01-04          |  69.99  |      74.99
 2020-01-05          |  79.99  |      75.99
(5 rows)
```

# 将 CASE 语句与窗口函数相结合

CASE 语句的功能类似于 if-then 语句。如果满足条件，则返回一个特定值，否则，如果不满足条件，则返回另一个值。

让我们考虑这个例子。假设对于这个特定的服装店，商家必须为某些商品提供退款。这如何反映在累计总数中？

考虑这个扩展的表。

```
date            | item               | price   | size   | refund
----------------+--------------------+---------+--------+---------
2021-01-01      | Sky Blue Jeans     |  79.99  | 31     | no
2021-01-02      | Fire Red Jeans     |  89.99  | 36     | no    
2021-01-03      | Sky Blue Shirt     |  59.99  | 38     | no
2021-01-04      | Grass Green Shirt  |  69.99  | 34     | yes
2021-01-05      | Peach Purple Hat   |  79.99  | 40     | yes
2021-01-06      | Sun Yellow Jeans   |  109.99 | 42     | no
2021-01-07      | Olive Green Hat    |  89.99  | 37     | no
```

从上面我们可以看到，在这种情况下，商家在 1 月 4 日和 5 日提供退款。为了计算新的累积和，这些值需要从**中减去**——而不是加到总数中。

在这方面，CASE 语句嵌套在 window 函数中——如果**退款**变量包含*是*值，则使用一条指令使价格值为负。

```
>>> SELECT date,price,refund,SUM(CASE WHEN refund = 'yes' THEN -1*price ELSE price END) OVER (ORDER BY date) AS total_price FROM table;date                 | price   | total_price  | refund
---------------------+---------+--------------+------------
 2021-01-01          |  79.99  |      79.99   | no
 2021-01-02          |  89.99  |     169.98   | no
 2021-01-03          |  59.99  |     229.97   | no
 2021-01-04          |  69.99  |     159.98   | yes
 2020-01-05          |  79.99  |     79.99    | yes
 2020-01-06          |  79.99  |     189.98   | no
 2020-01-07          |  79.99  |     279.97   | no(5 rows)
```

正如我们所看到的，一旦在 1 月 4 日和 5 日减去 69.99 和 79.99 的价格，就会计算出 279.97 的总价格，这是由 CASE 语句执行的，因为退款变量的 *yes* 条目会导致 CASE 语句指定一个负价格。

# 结论

在本文中，您已经看到:

*   使用窗口功能的目的
*   如何使用窗口函数获得累积值
*   将窗口函数与 CASE 语句结合起来，使窗口函数更加灵活

非常感谢阅读，任何问题或反馈都非常感谢！您还可以在这里找到原始文章[以及有用的 SQL 实践的更多示例。](https://www.michael-grogan.com/articles/window-functions-postgresql)

# 参考

*   [learnsql.com:什么是 sql 运行总数，如何计算？](https://learnsql.com/blog/what-is-a-running-total-and-how-to-compute-it-in-sql/)
*   [堆栈溢出:如何在 SQL 中添加包含另一列中某些值的累积的列？](https://stackoverflow.com/questions/69452575/how-to-add-column-with-accumulations-of-certain-values-from-another-column-in-sq)
*   [w3schools.com: SQL Case 语句](https://www.w3schools.com/sql/sql_case.asp)

*免责声明:本文是在“原样”的基础上编写的，没有任何担保。它旨在提供数据科学概念的概述，不应被解释为专业建议。本文中的发现和解释是作者的发现和解释，不被本文中提到的任何第三方认可或隶属于任何第三方。作者与本文提及的任何第三方无任何关系。*