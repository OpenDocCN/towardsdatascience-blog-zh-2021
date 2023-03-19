# 掌握 SQL 的 30 个例子

> 原文：<https://towardsdatascience.com/30-examples-to-master-sql-c8004705479a?source=collection_archive---------6----------------------->

## 综合实践教程

![](img/e6c7026a4fbcd2f38d0d3e249f14afa5.png)

汤姆·温克尔斯在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

SQL 是一种编程语言，用于管理关系数据库中以表格形式(即表格)存储的数据。

关系数据库由多个相互关联的表组成。表之间的关系是在共享列的意义上形成的。

有许多不同的关系数据库管理系统(如 MySQL、PostgreSQL、SQL Server)。他们采用的 SQL 语法可能略有不同。然而，差别很小，所以如果你学会了如何使用一个，你可以很容易地切换到另一个。

在本文中，我们将介绍 30 个示例，涵盖了以下 SQL 操作:

*   创建数据库和表
*   将数据插入表格
*   从表中删除数据
*   更新表格
*   使用各种 select 语句查询表

在您的机器或云上使用 SQL 有许多替代方法。我目前通过终端在 linux 机器上使用 MySQL。另一个常用的替代方法是安装 MySQL Workbench。

## 示例 1

我们首先从终端连接到 MySQL 服务器并创建一个数据库。

```
~$ sudo mysql -u root
```

系统会提示我们输入密码。我们现在连接到我们机器中的 MySQL 服务器。

以下命令创建一个名为“retail”的数据库。

```
mysql> create database retail;
mysql> use retail;
```

我们不在零售数据库中，该数据库还不包含任何表。

## 示例 2

我们将首先使用 create table 命令创建一个名为“customer”的表。

```
mysql> create table customer (
    -> cust_id int primary key,
    -> age int,
    -> location varchar(20),
    -> gender varchar(20)
    -> );
```

我们在括号内定义列名和相关的数据类型。cust_id 列被指定为主键。

> 主键是唯一标识每行的列。这就像熊猫数据框的索引。

## 示例 3

我们将创建第二个名为“订单”的表。

```
mysql> create table orders (
    -> order_id int primary key,
    -> date date,
    -> amount decimal(5,2),
    -> cust_id int,
    -> foreign key (cust_id) references customer(cust_id)
    -> on delete cascade
    -> );
```

在开始时，我们提到关系表通过共享列相互关联。关联两个表的列是外键。

> 外键是将一个表与另一个表联系起来的东西。外键包含另一个表的主键。

orders 表中的 cust_id 列是一个外键，它将 orders 表与 customer 表相关联。我们在创建表时指定了这个条件。

在最后一行，我们用“on delete cascade”短语指定了另一个条件。它告诉 MySQL 当 customer 表中的一行被删除时该做什么。orders 表中的每一行都属于一个客户。customer 表中的每一行都包含一个唯一的客户 id，代表一个客户。如果 customer 表中的一行被删除，这意味着我们不再拥有该客户。因此，属于该客户的订单不再有关联的客户 id。“在删除级联时”表示没有关联客户 id 的订单也将被删除。

## 实例 4

零售数据库现在包含两个表。我们可以使用 show tables 命令查看数据库中的表。

```
mysql> show tables;+------------------+
| Tables_in_retail |
+------------------+
| customer         |
| orders           |
+------------------+
```

注意:SQL 中的命令以分号("；"结尾).

## 实例 5

desc 或 describe 命令根据列名、数据类型和一些附加信息提供了表的概述。

```
mysql> desc orders;+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| order_id | int(11)      | NO   | PRI | NULL    |       |
| date     | date         | YES  |     | NULL    |       |
| amount   | decimal(5,2) | YES  |     | NULL    |       |
| cust_id  | int(11)      | YES  | MUL | NULL    |       |
+----------+--------------+------+-----+---------+-------+
```

## 实例 6

我们可以修改现有的表格。例如，alter table 命令可用于添加新列或删除现有列。

让我们向 orders 表添加一个名为“is_sale”的列。

```
mysql> alter table orders add is_sale varchar(20);
```

我们编写列名和数据类型以及 add 关键字。

```
mysql> desc orders;+----------+--------------+------+-----+---------+-------+
| Field    | Type         | Null | Key | Default | Extra |
+----------+--------------+------+-----+---------+-------+
| order_id | int(11)      | NO   | PRI | NULL    |       |
| date     | date         | YES  |     | NULL    |       |
| amount   | decimal(5,2) | YES  |     | NULL    |       |
| cust_id  | int(11)      | YES  | MUL | NULL    |       |
| is_sale  | varchar(20)  | YES  |     | NULL    |       |
+----------+--------------+------+-----+---------+-------+
```

is_sale 列已添加到 orders 表中。

## 例 7

alter table 还可以用于删除一个列，只需对语法稍作修改。

```
mysql> alter table orders drop is_sale;
```

使用 drop 关键字代替 add。我们也不必编写数据类型来删除列。

## 实施例 8

我们有表格，但它们不包含任何数据。填充表的一种方法是 insert 语句。

```
mysql> insert into customer values (
    -> 1000, 42, 'Austin', 'female'
    -> );
```

指定的值以相同的顺序插入到列中。因此，我们需要保持顺序一致。

## 示例 9

我们可以通过分隔每一行来同时插入多行。

```
mysql> insert into customer values 
    -> (1001, 34, 'Austin', 'male'),
    -> (1002, 37, 'Houston', 'male'),
    -> (1003, 25, 'Austin', 'female'),
    -> (1004, 28, 'Houston', 'female'),
    -> (1005, 22, 'Dallas', 'male'),
    -> ;
```

我添加了更多的行，并以同样的方式填充了 orders 表。

用数据填充表还有其他方法。例如，我们可以使用 load data infile 或 load data local infile 语句加载一个 csv 文件。

## 实例 10

delete from 语句可用于删除表中的现有行。我们需要通过提供一个条件来确定要删除的行。例如，下面的语句将删除订单 id 为 17 的行。

```
mysql> delete from orders 
    -> where order_id = 17;
```

如果我们不指定条件，给定表中的所有行都将被删除。

## 实施例 11

我们还可以更新现有的行。让我们更新 orders 表中的一行。

```
+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        1 | 2020-10-01 |  24.40 |    1001 |
+----------+------------+--------+---------+
```

这是订单表中的第一行。我们想把订单金额改为 27.40。

```
mysql> update orders
    -> set amount = 27.40
    -> where order_id = 1; mysql> select * from orders limit 1;
+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        1 | 2020-10-01 |  27.40 |    1001 |
+----------+------------+--------+---------+
```

我们将更新后的值写在 set 关键字之后。通过在 where 关键字后提供条件来标识要更新的行。

## 实例 12

如果我们想通过复制现有表的结构来创建一个表，我们可以使用带有 like 关键字的 create table 语句。

```
mysql> create table orders_copy like orders;mysql> show tables;
+------------------+
| Tables_in_retail |
+------------------+
| customer         |
| orders           |
| orders_copy      |
+------------------+
```

orders_copy 表与 orders 表具有相同的结构，但不包含任何数据。

## 实施例 13

我们还可以通过同时使用 create table 和 select 语句来创建包含数据的现有表的副本。

```
mysql> create table new_orders
    -> select * from orders;
```

这看起来像是两个独立陈述的结合。第一行创建表，第二行用 orders 表中的数据填充该表。

## 实施例 14

drop table 语句可用于删除数据库中的表。

```
mysql> drop table orders_copy, new_orders;mysql> show tables;
+------------------+
| Tables_in_retail |
+------------------+
| customer         |
| orders           |
+------------------+
```

我们已经成功地删除了上一个示例中创建的表。

我们在一个数据库中有两个关系表。以下示例将演示如何使用 select 查询从这些表中检索数据。

## 实施例 15

最简单的查询是查看表中的所有列。

```
mysql> select * from orders
    -> limit 3;+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        1 | 2020-10-01 |  27.40 |    1001 |
|        2 | 2020-10-01 |  36.20 |    1000 |
|        3 | 2020-10-01 |  65.45 |    1002 |
+----------+------------+--------+---------+
```

“*”选择所有列，limit 关键字对要显示的行数进行约束。

## 实施例 16

我们可以通过写列的名称而不是“*”来只选择一些列。

```
mysql> select order_id, amount 
    -> from orders
    -> limit 3;+----------+--------+
| order_id | amount |
+----------+--------+
|        1 |  27.40 |
|        2 |  36.20 |
|        3 |  65.45 |
+----------+--------+
```

## 实例 17

我们可以使用 where 子句为要选择的行指定一个条件。以下查询将返回 2020 年 10 月 1 日的所有订单。

```
mysql> select * from orders
    -> where date = '2020-10-01';+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        1 | 2020-10-01 |  27.40 |    1001 |
|        2 | 2020-10-01 |  36.20 |    1000 |
|        3 | 2020-10-01 |  65.45 |    1002 |
+----------+------------+--------+---------+
```

## 实施例 18

where 子句接受多个条件。让我们在前一个示例中的查询上添加另一个条件。

```
mysql> select * from orders
    -> where date = '2020-10-01' and amount > 50;+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        3 | 2020-10-01 |  65.45 |    1002 |
+----------+------------+--------+---------+
```

## 实施例 19

我们可能希望对查询结果进行排序，这可以通过使用 order by 子句来完成。

以下查询将返回 2020–10–02 的订单，并根据金额对它们进行排序。

```
mysql> select * from orders
    -> where date = '2020-10-02'
    -> order by amount;+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        5 | 2020-10-02 |  18.80 |    1005 |
|        6 | 2020-10-02 |  21.15 |    1009 |
|        4 | 2020-10-02 |  34.40 |    1001 |
|        7 | 2020-10-02 |  34.40 |    1008 |
|        8 | 2020-10-02 |  41.10 |    1002 |
+----------+------------+--------+---------+
```

## 实施例 20

默认情况下，order by 子句按升序对行进行排序。我们可以用 desc 关键字把它改成降序。

```
mysql> select * from orders
    -> where date = '2020-10-02'
    -> order by amount desc;+----------+------------+--------+---------+
| order_id | date       | amount | cust_id |
+----------+------------+--------+---------+
|        8 | 2020-10-02 |  41.10 |    1002 |
|        4 | 2020-10-02 |  34.40 |    1001 |
|        7 | 2020-10-02 |  34.40 |    1008 |
|        6 | 2020-10-02 |  21.15 |    1009 |
|        5 | 2020-10-02 |  18.80 |    1005 |
+----------+------------+--------+---------+
```

## 实施例 21

SQL 是一种通用语言，也可以用作数据分析工具。它提供了许多功能，可以在从数据库查询时分析和转换数据。

例如，我们可以计算 orders 表中唯一天数。

```
mysql> select count(distinct(date)) as day_count
    -> from orders;+-----------+
| day_count |
+-----------+
|         4 |
+-----------+
```

orders 表包含 4 个不同日期的订单。“as”关键字用于重命名查询结果中的列。否则，列的名称将是“count(distinct(date))。

## 实施例 22

订单表中有 4 个不同的日期。我们还可以找出每天有多少订单。group by 子句将帮助我们完成这项任务。

```
mysql> select date, count(order_id) as order_count
    -> from orders
    -> group by date;+------------+-------------+
| date       | order_count |
+------------+-------------+
| 2020-10-01 |           3 |
| 2020-10-02 |           5 |
| 2020-10-03 |           6 |
| 2020-10-04 |           2 |
+------------+-------------+
```

我们对订单进行计数，并按日期列进行分组。

## 实施例 23

我们将计算每天的平均订单金额，并根据平均金额按降序排列结果。

```
mysql> select date, avg(amount)
    -> from orders
    -> group by date
    -> order by avg(amount) desc;+------------+-------------+
| date       | avg(amount) |
+------------+-------------+
| 2020-10-01 |   43.016667 |
| 2020-10-04 |   42.150000 |
| 2020-10-03 |   37.025000 |
| 2020-10-02 |   29.970000 |
+------------+-------------+
```

## 实施例 24

我们希望修改上一个示例中的查询，只包含平均数量高于 30 的天数。

```
mysql> select date, avg(amount)
    -> from orders
    -> group by date
    -> having avg(amount) > 30
    -> order by avg(amount) desc;+------------+-------------+
| date       | avg(amount) |
+------------+-------------+
| 2020-10-01 |   43.016667 |
| 2020-10-04 |   42.150000 |
| 2020-10-03 |   37.025000 |
+------------+-------------+
```

需要注意的是，查询中语句的顺序很重要。例如，如果我们将 order by 子句放在 having 子句之前，就会出现错误。

## 实施例 25

我们想知道每天的最大订购量。

```
mysql> select date, max(amount)
    -> from orders
    -> group by date;+------------+-------------+
| date       | max(amount) |
+------------+-------------+
| 2020-10-01 |       65.45 |
| 2020-10-02 |       41.10 |
| 2020-10-03 |       80.20 |
| 2020-10-04 |       50.10 |
+------------+-------------+
```

## 实施例 26

我们希望在一个 select 语句中组合多个聚合函数。为了证明这一点，让我们详细说明前面的例子。我们希望看到每个客户的最大订单和最小订单之间的差异。我们还希望按照升序对结果进行排序，并显示前三个结果。

```
mysql> select cust_id, max(amount) - min(amount) as dif
    -> from orders
    -> group by cust_id
    -> order by dif desc
    -> limit 3;+---------+-------+
| cust_id | dif   |
+---------+-------+
|    1007 | 46.00 |
|    1009 | 28.95 |
|    1002 | 24.35 |
+---------+-------+
```

dif 栏是从最大金额中减去最小金额得到的。

## 实施例 27

我们现在切换到客户表。让我们找出我们在每个城市有多少女性和男性顾客。我们需要在 group by 子句中写入位置和性别列。

```
mysql> select location, gender, count(cust_id)
    -> from customer
    -> group by location, gender;+----------+--------+----------------+
| location | gender | count(cust_id) |
+----------+--------+----------------+
| Austin   | female |              2 |
| Austin   | male   |              1 |
| Dallas   | female |              2 |
| Dallas   | male   |              2 |
| Houston  | female |              2 |
| Houston  | male   |              1 |
+----------+--------+----------------+
```

## 实施例 28

customer 和 orders 表基于 cust_id 列相互关联。我们可以使用 SQL 连接查询两个表中的数据。

我们希望在客户表中看到每个城市的平均订单量。

```
mysql> select customer.location, avg(orders.amount) as avg
    -> from customer
    -> join orders
    -> on customer.cust_id = orders.cust_id
    -> group by customer.location;+----------+-----------+
| location | avg       |
+----------+-----------+
| Austin   | 33.333333 |
| Dallas   | 34.591667 |
| Houston  | 44.450000 |
+----------+-----------+
```

因为我们从两个不同的表中选择列，所以列名由关联的表名指定。上述查询的第二、第三和第四行根据每个表中的 cust_id 列连接 customer 和 orders 表。

请注意，列名不必相同。无论我们用“on”关键字提供什么列名，比较或匹配都是基于这些列进行的。

## 实施例 29

我们希望看到 2020 年 10 月 3 日有订单的客户的平均年龄。

```
mysql> select avg(c.age) as avg_age
    -> from customer c
    -> join orders o
    -> on c.cust_id = o.cust_id
    -> where o.date = '2020-10-03';+---------+
| avg_age |
+---------+
| 30.0000 |
+---------+
```

我们也可以为表名使用别名。当我们需要多次输入表名时，这很方便。

## 示例 30

我们希望看到订单量最高的客户的位置。

```
mysql> select c.location, o.amount
    -> from customer c
    -> join orders o
    -> on c.cust_id = o.cust_id
    -> where o.amount = (select max(amount) from orders)
    -> ;+----------+--------+
| location | amount |
+----------+--------+
| Dallas   |  80.20 |
+----------+--------+
```

我们在这个查询中有一个嵌套的 select 语句。使用 orders 表中单独的 select 语句计算金额条件。

这项任务可以用不同的方式来完成。我选择了这种方法来引入嵌套查询的概念。

## 结论

我相信本文中的 30 个例子全面介绍了 SQL。我们讨论了以下主题:

*   用关系表创建数据库
*   修改表格
*   将数据插入表格
*   从表中删除数据
*   编写查询以从表中检索数据

当然，使用 SQL 可以完成更高级的查询和操作。一旦你熟练掌握了基础知识，最好是继续学习更高级的操作。

感谢您的阅读。如果您有任何反馈，请告诉我。