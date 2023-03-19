# SQL 面试准备:下一个级别

> 原文：<https://towardsdatascience.com/sql-interview-prep-the-next-level-e329a67086d?source=collection_archive---------25----------------------->

## 仅仅知道内连接和左连接之间的区别是不够的

![](img/6370218c4e1ebe4ce1af3b43590d187c.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/video-game?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

作为数据科学和数据工程团队的成员，我也参加了很多面试。每个面试官都有自己的破局者；我的是 SQL。如果您对 SQL 有概念上的问题，那么这意味着您可能会无意中错误地提取数据。数据科学家需要正确提取数据，以便为他们的机器学习(ML)模型构建训练/测试集。数据工程师需要正确地提取数据，以便为利益相关者提供可靠的数据源。如果一个数据工程师犯了一个错误，这个错误可能会影响不止一个 ML 模型——它可能会传播到订阅该数据集的所有数据科学、分析、营销和财务团队。SQL 技能很重要。大多数初级受访者知道 SELECT *语句、GROUP BY 和所有不同类型的连接的基本原理，但是他们很难真正将数据概念化。让我们来看几个真实世界的例子，它们有望将你的 SQL 面试准备提高到一个新的水平。

# 挑战 1:占总数的百分比

假设这是你作为在线零售商 Pets R Us 的数据科学家的第一天。您有一个表 **customers** ，它跟踪所有的客户记录。每个客户记录包括一个客户 ID**cust _ ID**，以及该记录的创建日期 **cre8_dt** 。您能否编写一个 SQL 查询来计算在“2007-12-22”之前创建的客户记录的百分比？

作为一名数据科学家，我在第一天的时候会用这种类似黑客的方式来做这件事。首先，我会计算表中记录的总数:

```
-- calculate total number of records
SELECT COUNT(cust_id)
FROM customers
```

给了我们:

```
+----------------+
| COUNT(cust_id) |
+----------------+
|      1,210,000 |
+----------------+
```

然后，我会计算在“2007 年 12 月 22 日”之前创建的记录的数量:

```
-- calculate number of records created before 2007-12-22
SELECT COUNT(cust_id)
FROM customers
WHERE cre8_dt < '2007-12-22'
```

给了我们:

```
+----------------+
| COUNT(cust_id) |
+----------------+
|          3,400 |
+----------------+
```

然后我会用手机上的计算器将 3400 除以 1210000，再乘以 100 得到 0.28%。哈！明白了！

但是…不要在面试中那样做！这里有一个更简洁的方法:

```
SELECT cre8_flag,
       COUNT(*)*100.0/SUM(COUNT(*)) OVER() AS perc_of_tot
FROM (
      SELECT cust_id,
             CASE WHEN cre8_dt < '2007-12-22' THEN 'before'
                  ELSE 'after'
                  END AS cre8_flag
      FROM customers
      )
GROUP BY 1
```

给了我们:

```
+-----------+-------------+
| cre8_flag | perc_of_tot |
+-----------+-------------+
| before    |        0.28 |
| after     |       99.72 |
+-----------+-------------+
```

这里的技巧是首先使用一个子查询来创建一个标志，表示帐户是在我们的日期阈值之前还是之后创建的。然后，我们可以根据主查询中标志进行分组。当我们构建 **perc_of_tot** 列时，COUNT(*)计算每组中的帐户总数(一组用于**cre 8 _ flag**=‘之前’，另一组用于**cre 8 _ flag**=‘之后’)。因为我们没有在 OVER()之后包含 PARTITION BY，SUM(COUNT(*)对整个结果集的计数求和，在本例中，结果集由两个值组成:COUNT(*)表示“before”组，COUNT(*)表示“after”组。

# 挑战#2:获取蛋糕客户

你加入 Pets R Us 的数据科学团队才一周，你的经理希望你参与新的 Pupcake 收购活动。她要求您建立一个 ML 模型，预测从未购买过蛋糕的现有客户在未来四周内购买蛋糕的可能性。

让我们考虑一下，为了建立这个模型的训练/测试集，您需要提取哪些数据。首先，您需要汇总两个时间段的客户销售数据:(1)结果期(假设今天之前的四周)和(2)基线期(假设结果期之前的十二周)。

假设到目前为止，您已经成功提取了两个数据集。您将每个数据集写入一个表:

1.  **cust_basel** :在基线期内至少从 Pets R Us 购买了一件商品的客户列表。
2.  **cust_pupcakes_basel** :基线期内至少从 Pets R Us 购买过一个 pupcakes 的客户列表。

你真正需要的是在基线期内至少从 Pets R Us 购买了一件商品，但在此期间没有购买任何蛋糕的顾客名单。您能使用上面的两个表编写一个 SQL 查询来生成这样一个客户列表吗？

解决方案如下:

```
SELECT *
FROM cust_basel all
LEFT JOIN cust_pupcakes_basel cakes ON cakes.cust_id = all.cust_id
WHERE cakes.cust_id IS NULL
```

这是一个有用的技巧，我以前在一次面试中被要求这样做！在 Scala 和 PySpark 中，它实际上被称为左反连接。上面的左反联接将只返回左侧表( **cust_basel** )中不存在于右侧表( **cust_pupcakes_basel** )中的客户。

# 挑战#3:按客户对信用卡进行分组

让我们回到第一个例子中的 Pets R Us **customers** 表。我们已经检查了 **cust_id** 和 **cre8_dt** 列。每条记录还有一个 **full_nm** 列和一个 **email_adr** 列。假设这个表在 **cust_id** 上是唯一的(*即*每个 **cust_id** 只出现一次)。以下是表格中的五行示例:

```
+---------+------------+----------------+--------------------+
| cust_id |  cre8_dt   |    full_nm     |     email_adr      |
+---------+------------+----------------+--------------------+
|       1 | 2003-06-14 | Karen Gillan   | [kgil@mail.com](mailto:kgil@mail.com)      |
|       2 | 2020-12-19 | Dwayne Johnson | [therock@mail.com](mailto:therock@mail.com)   |
|       3 | 2007-10-28 | Kevin Hart     | [kevin@hart.com](mailto:kevin@hart.com)     |
|       4 | 2008-01-01 | Awkwafina      | [awkwa@fina.com](mailto:awkwa@fina.com)     |
|       5 | 2015-05-17 | Nick Jonas     | [jonasbro3@mail.com](mailto:jonasbro3@mail.com) |
+---------+------------+----------------+--------------------+
```

除了 customers 表，您还可以访问存储信用卡信息的 cards 表。每条记录都有一个 **card_id** (由 Pets R Us 内部生成，以确保每张卡都有一个唯一的 id)、持卡人的全名( **full_nm** )、卡的到期日期( **exp_dt** )、与卡相关联的 **cust_id** ，以及使用卡进行 Pets R Us 购买的最近日期( **last_purch_dt** )。

第一个问题(没有检查数据):你认为**卡片**表在 **cust_id** 上是唯一的吗？

回答:不能，因为每个客户可以有多张信用卡。

以下是**纸牌**牌桌的小样本:

```
+---------+------------+---------------+---------+-----------------+
| card_id |   exp_dt   | last_purch_dt | cust_id |    card_adr     |
+---------+------------+---------------+---------+-----------------+
|    1234 | 2099-08-31 | 2020-12-24    |       2 | 123 Rock St     |
|    2345 | 2019-01-31 | 2018-11-15    |       3 | 5 Main St       |
|    3456 | 2023-03-21 | 2020-07-13    |       2 | 123 Rock Street |
|    4567 | 2020-01-19 | 2019-12-31    |       2 | 21 Rock Ln      |
|    5678 | 2022-10-31 | 2020-12-05    |       4 | 345 Awka Blvd   |
+---------+------------+---------------+---------+-----------------+
```

第二个问题:您能否编写一个 SQL 查询来显示与每个 **cust_id** 相关联的活动信用卡的总数，以及该客户的全名和电子邮件地址？

以下是大多数入门级求职者的做法:

```
SELECT c.cust_id,
       c.full_nm,
       c.email_adr,
       nbr.nbr_cards
FROM customers c
INNER JOIN (
            -- nbr of cards per customer
            SELECT cust_id,
                   COUNT(card_id) AS nbr_cards
            FROM cards
            WHERE exp_dt > Current_Date
            GROUP BY 1
            ) nbr ON nbr.cust_id = c.cust_id
```

大多数人首先使用**卡片**表来计算每个 **cust_id** 的卡片数量。然后，他们将其放入一个子查询中，并将其连接回 **customers** 表，以获得 **full_nm** 和 **email_adr** 。

下面是一种更简洁的方法:

```
SELECT c.cust_id,
       c.full_nm,
       c.email_adr,
       COUNT(cards.card_id) AS nbr_cards
FROM customers c
INNER JOIN cards ON cards.cust_id = c.cust_id
WHERE card.exp_dt > Current_Date
GROUP BY 1,2,3
```

请注意，这里我们实际上不需要子查询！因为我们知道每个 **cust_id** 只有一个唯一的 **full_nm** 和 **email_adr** 与之相关联，所以我们可以通过 **cust_id** 、 **full_nm** 和 **email_adr** 对**卡片**表和组进行联接。然而，如果 **customers** 表在 **cust_id** 上不是唯一的，那么上面的查询可能会产生重复的 **cust_id**

跟进问题:如何检查**客户**表中是否有重复的**客户标识**？

回答:

```
-- check for duplicates
SELECT COUNT(cust_id),
       COUNT(DISTINCT cust_id)
FROM customers
```

这里的要点是，您可以向 GROUP BY 子句添加额外的列，即使您不一定要根据它们进行分组。在上面的例子中，从技术上讲，我们只是按照 **cust_id** 进行分组，而 **full_nm** 和 **email_adr** 列只是客户属性。但是，我们可以将其他列放入组中，只要它们在 **cust_id** 上是唯一的。我以前在一次 SQL 访谈中看到过这方面的讨论！

# 挑战#4:为每个客户选择一个地址

这里还有另一个挑战:使用**客户**和**卡**表，编写一个 SQL 查询，显示每个**客户标识**，他们的**完整客户标识**，和**卡地址标识**。

首先，在深入 SQL 之前，让我们检查一下现有的数据，并从概念上考虑一下。向上滚动到上一部分，再看一下**牌**表中的几行。看起来道恩·强森( **cust_id** = 2)至少有三张信用卡，每张信用卡都有一个稍微不同的地址。因此，如果一些客户有多张信用卡，并且每张卡理论上可以有不同的地址与之相关联(或者甚至相同的地址格式不同)，我们如何为每个客户选择一个确切的地址呢？

让我们仔细看看德韦恩的三张牌。一个已经过期，而另外两个仍然有效。活动卡具有相同的地址，但格式不同。过期的卡有完全不同的地址。看起来德韦恩最近可能搬家了，过期的卡反映了他的旧地址。

解决方案:

```
-- select one address per customer
SELECT cust_id,
       full_nm,
       card_adr
FROM (
      -- rank each customer's addresses
      SELECT c.cust_id,
             c.full_nm,
             cards.card_adr,
             row_number() OVER (PARTITION BY cust_id 
                             ORDER BY last_purch_dt DESC) AS rnk_nbr
      FROM customers c
      INNER JOIN cards ON cards.cust_id = c.cust_id
      WHERE card.exp_dt > Current_Date
      )
WHERE rnk_nbr = 1
```

我们首先需要做的是按照 **cust_id** 对 **cards** 表中的所有记录进行分区。这意味着每个客户都有自己的分区，其中只包含他们自己的信用卡。我们告诉 SQL 如何在每个分区内对卡进行排序:通过 **last_purch_dt** 。最近的 **last_purch_dt** 的卡会排在最前面。一旦我们对所有的卡进行了排名，我们就只为每个客户选择排名第一的卡。这样，我们将只剩下每个客户一张卡(和一个地址)。

还有其他方法为每位顾客选择一张卡。例如，您可以将 **exp_dt** 考虑在内，或者使用某种交易表来确定每张卡在过去一个月中的使用频率。这里的要点是要知道你应该使用某种 PARTITION BY 语句。

此外，一定不要在面试中过多考虑任务；尽量在不增加不必要的复杂性的情况下解决问题。如果面试官想跟进的话，你可以在你的解决方案上再加一层。如果一个候选人是 SQL 高手，但却把简单的事情变得不必要的复杂，这仍然会给他们留下不好的印象。

# 挑战 5:黑色星期五销售——第一部分

您在 Pets R Us 的业务利益相关者希望分析过去 10 年中每个客户的黑色星期五销售额分布情况。首先，在我们一头扎进 SQL 之前，让我们想一想为了满足这个请求我们需要什么。

我们需要一个包含所有 Pets R Us 事务的表。每笔交易都将与一个客户和一个日期相关联。首先，我们应该只过滤黑色星期五的交易。然后我们可以按客户汇总销售额。

您可能只需要从过去 10 年中查找黑色星期五的日期，并将它们硬编码到您的 SQL 查询中。然而，在大多数情况下，通常最好避免硬编码。另外，这是一次采访，那有什么意思呢？？

数据工程团队为数据科学团队提供了一个维度日期表 **dim_date** ，以帮助完成类似这样的日期筛选任务。以下是来自 **dim_date** 的一些示例行:

```
+------------+-------+--------+--------+
|  id_date   | id_yr | id_mth | id_dow |
+------------+-------+--------+--------+
| 2020-12-01 |  2020 |     12 |      2 |
| 2020-12-02 |  2020 |     12 |      3 |
| 2020-12-03 |  2020 |     12 |      4 |
| 2020-12-04 |  2020 |     12 |      5 |
| 2020-12-05 |  2020 |     12 |      6 |
| 2020-12-06 |  2020 |     12 |      7 |
| 2020-12-07 |  2020 |     12 |      1 |
| 2020-12-08 |  2020 |     12 |      2 |
| 2020-12-09 |  2020 |     12 |      3 |
+------------+-------+--------+--------+
```

问题:使用 **dim_date** ，您能编写一个 SQL 查询来生成 2010 年到 2020 年间所有黑色星期五的日期列表吗？

提示:使用感恩节发生在每年 11 月的第四个星期四的规则。

解决方案:

```
%sql
-- black friday
SELECT d.id_date AS thanksgiving,
       date_add(d.id_date, 1) AS black_friday
FROM (
      SELECT id_yr, 
             id_mth, 
             id_dow, 
             id_date, 
             row_number() OVER (PARTITION BY id_yr, 
                                             id_mth, 
                                             id_dow 
                                ORDER BY id_date) AS numocc 
      FROM dim_date
      WHERE id_yr BETWEEN 2010 AND 2020
      ) d
WHERE (d.id_mth=11 AND d.id_dow=4 AND d.numocc=4)
```

输出应该如下所示:

```
+--------------+--------------+
| thanksgiving | black_friday |
+--------------+--------------+
| 2010-11-25   | 2010-11-26   |
| 2011-11-24   | 2011-11-25   |
| 2012-11-22   | 2012-11-23   |
| 2013-11-28   | 2013-11-29   |
| 2014-11-27   | 2014-11-28   |
| 2015-11-26   | 2015-11-27   |
| 2016-11-24   | 2016-11-25   |
| 2017-11-23   | 2017-11-24   |
| 2018-11-22   | 2018-11-23   |
| 2019-11-28   | 2019-11-29   |
| 2020-11-26   | 2020-11-27   |
+--------------+--------------+
```

后续问题:如果我们改变 PARTITION BY 子句中列名的顺序，会发生什么情况？

答:这不会改变输出，因为 **dim_date** 中的相同记录会被分组在一起，不管您是先按年、先按月还是先按星期几排序。但是，如果 ORDER BY 子句中有多个列，并且更改了它们的顺序，则输出可能会发生变化。

假设我们将黑色星期五查询的输出写到一个表中: **blk_fri_dates** 。现在我们已经有了一个表，其中包含了我们想要用来过滤事务的所有日期，让我们来看看来自 **transactions** 表的几条记录，这样我们就可以对事务数据有一个大致的了解:

```
+---------+------------+---------+--------+
| tran_id |  id_date   | card_id |  amt   |
+---------+------------+---------+--------+
|    1111 | 2020-11-10 |    1234 |  50.99 |
|    1122 | 2020-11-12 |    5678 |  32.34 |
|    1133 | 2020-11-13 |    0123 |  24.78 |
|    1144 | 2020-11-15 |    1234 |  19.85 |
|    1155 | 2010-11-15 |    9012 | 112.00 |
+---------+------------+---------+--------+
```

问题:使用 **transactions** 表、 **blk_fri_dates** 表以及我们上面讨论过的任何其他 Pets R Us 表，对 2010 年到 2020 年之间每个黑色星期五每个客户的总销售额进行求和。

解决方案:

```
-- customer sales for Black Friday 2010-2020
SELECT c.cust_id,
       t.id_date,
       SUM(t.amt) AS tot_sales
FROM transactions t
INNER JOIN blk_fri_dates bf ON bf.black_friday = t.id_date
INNER JOIN cards c ON c.card_id = t.card_id 
GROUP BY 1,2
```

首先，我们从**事务**表开始。我们将**事务**表内部连接到 **blk_fri_dates** 表，因为我们只想获取发生在黑色星期五的事务。我们还必须将**交易**表内部连接到**卡**表，因为我们需要为每个**交易**查找哪个 **cust_id** 对应哪个 **card_id** 。

以下是几行示例输出:

```
+---------+------------+-----------+
| cust_id |  id_date   | tot_sales |
+---------+------------+-----------+
|       1 | 2010-11-26 |     87.54 |
|       1 | 2013-11-29 |     45.78 |
|       5 | 2015-11-27 |     32.18 |
|       5 | 2016-11-25 |     19.98 |
|       3 | 2020-11-27 |     40.35 |
+---------+------------+-----------+
```

后续问题:如果 **blk_fri_dates** 表中有重复的行，那么上面的输出会怎么样？

回答:想象一下 **blk_fri_dates** 表错误地包含了 2020 年的两行:

```
+--------------+--------------+
| thanksgiving | black_friday |
+--------------+--------------+
| 2010-11-25   | 2010-11-26   |
| 2011-11-24   | 2011-11-25   |
| 2012-11-22   | 2012-11-23   |
| 2013-11-28   | 2013-11-29   |
| 2014-11-27   | 2014-11-28   |
| 2015-11-26   | 2015-11-27   |
| 2016-11-24   | 2016-11-25   |
| 2017-11-23   | 2017-11-24   |
| 2018-11-22   | 2018-11-23   |
| 2019-11-28   | 2019-11-29   |
| 2020-11-26   | 2020-11-27   |
| 2020-11-26   | 2020-11-27   |
+--------------+--------------+
```

在这种情况下，如果我们将该表内连接到**交易**，那么从 2020 年开始的每个黑色星期五交易都将被重复计算。

后续问题:如果 **blk_fri_dates** 有重复的行，如何调整上面的 SQL 查询，以便仍然获得正确的每个客户的黑色星期五销售额？

回答:

```
-- customer sales for Black Friday 2010-2020
SELECT c.cust_id,
       t.id_date,
       SUM(t.amt) AS tot_sales
FROM transactions t
INNER JOIN (
            -- use a sub-query!
            SELECT DISTINCT black_friday
            FROM blk_fri_dates
            ) bf ON bf.black_friday = t.id_date
INNER JOIN cards c ON c.card_id = t.card_id 
GROUP BY 1,2
```

额外收获:我们可以通过在 **blk_fri_dates** 表上使用左半连接而不是内连接来进一步优化这个查询:

```
-- customer sales for Black Friday 2010-2020
SELECT c.cust_id,
       t.id_date,
       SUM(t.amt) AS tot_sales
FROM transactions t
LEFT SEMI JOIN blk_fri_dates bf ON bf.black_friday = t.id_date
INNER JOIN cards c ON c.card_id = t.card_id 
GROUP BY 1,2
```

上面的左半连接将只选择来自**事务**(左侧表格)的事务，其中在 **blk_fri_dates** (右侧表格)中的 **id_date** 列上有一个或多个匹配。左半连接的优点是，如果在 **blk_fri_dates** 中有重复的行，左半连接不会像内连接那样重复事务。

左半连接的执行速度通常比内连接快，因为它只能返回左侧表中的列。因为在这个查询中我们不需要从 **blk_fri_dates** 中选择任何列，所以左半连接是一个很好的选择。下次你需要做一些过滤的时候，考虑一个左半连接！

# 挑战#6:黑色星期五销售——第二部分

让我们重温一下上面的查询，它汇总了 2010 年到 2020 年间每个黑色星期五每个客户的总销售额。想想你的业务利益相关者最初的要求:“过去 10 年每个客户的黑色星期五销售额的分布。”上述解决方案是否完全满足此目的？

提示:如果客户在交易表中有 2010-2014 黑色星期五和 2016-2020 黑色星期五的关联交易，但没有 2015 黑色星期五的关联交易，会发生什么情况？

回答:如果一个客户(假设 **cust_id** =20)在 2015 年黑色星期五没有进行任何交易，那么该客户黑色星期五总销售额的输出行将如下所示:

```
+---------+------------+-----------+
| cust_id |  id_date   | tot_sales |
+---------+------------+-----------+
|      20 | 2010-11-26 |     87.54 |
|      20 | 2011-11-25 |     15.14 |
|      20 | 2012-11-23 |     42.71 |
|      20 | 2013-11-29 |     45.78 |
|      20 | 2014-11-28 |      8.07 |
|      20 | 2016-11-25 |     32.18 |
|      20 | 2017-11-24 |     19.98 |
|      20 | 2018-11-23 |     38.16 |
|      20 | 2019-11-29 |     19.57 |
|      20 | 2020-11-27 |     40.35 |
+---------+------------+-----------+
```

注意到 2015 年黑色星期五根本没有记录吗？我们真正想要的是这样的东西:

```
+---------+------------+-----------+
| cust_id |  id_date   | tot_sales |
+---------+------------+-----------+
|      20 | 2010-11-26 |     87.54 |
|      20 | 2011-11-25 |     15.14 |
|      20 | 2012-11-23 |     42.71 |
|      20 | 2013-11-29 |     45.78 |
|      20 | 2014-11-28 |      8.07 |
|      20 | 2015-11-27 |      0.00 |
|      20 | 2016-11-25 |     32.18 |
|      20 | 2017-11-24 |     19.98 |
|      20 | 2018-11-23 |     38.16 |
|      20 | 2019-11-29 |     19.57 |
|      20 | 2020-11-27 |     40.35 |
+---------+------------+-----------+
```

当你在面试中遇到这样的问题时，几乎可以肯定这是一个左连接问题。黑色星期五的总销售额将出现在右侧。但是左手边应该放什么呢？

在左边，我们需要顾客和黑色星期五的所有可能的组合。换句话说，我们需要编写一个 SQL 查询来生成每一个可能的 **cust_id** ， **black_friday** 对。

首先，为每个独特的客户创造产出:

```
-- every unique customer
SELECT cust_id
FROM customers
```

其次，利用 2010 年至 2020 年间每个独特的黑色星期五创造产出:

```
-- every unique Black Friday
SELECT black_friday
FROM blk_fri_dates
```

第三，用交叉连接将它们相乘:

```
-- every possible unique customer, Black Friday pair
SELECT c.cust_id,
       bf.black_friday
FROM customers c
CROSS JOIN blk_fri_dates bf
```

注意:记住交叉连接的计算开销非常大，所以要小心使用。

问题:使用我们刚刚编写的查询来生成每个可能的唯一客户，黑色星期五对，以及我们在上一节中编写的查询来合计每个客户的黑色星期五总销售额，编写一个 SQL 查询来输出每个客户每年的黑色星期五总销售额*。如果客户在任何一年的黑色星期五都没有交易，那么该年的交易应该显示为 0.00 美元。*

```
WITH bf_sales AS (
-- customer sales for Black Friday 2010-2020
SELECT c.cust_id,
       t.id_date,
       SUM(t.amt) AS tot_sales
FROM transactions t
LEFT SEMI JOIN blk_fri_dates bf ON bf.black_friday = t.id_date
INNER JOIN cards c ON c.card_id = t.card_id 
GROUP BY 1,2
)
SELECT c_bf.cust_id,
       c_bf.black_friday,
       COALESCE(bf_sales.tot_sales, 0.0) AS tot_sales
FROM (
      -- every possible customer, BF pair
      SELECT c.cust_id,
             bf.black_friday
      FROM customers
      CROSS JOIN blk_fri_dates
      ) c_bf
LEFT JOIN bf_sales ON bf_sales.cust_id = c_bf.cust_id
                   AND bf_sales.id_date = c_bf.black_friday
```

我使用了一个 WITH 子句来构建一个临时表，因为我不想让我的 SQL 查询因为太多的子查询而变得混乱。注意，我从左边的表中选择了 **cust_id** 和 **black_friday** 列。这一点很重要:如果您意外地从右边的表中选择了 **cust_id** 或 **black_friday** ，那么您可能会得到 nulls，这就否定了左连接的目的。我使用了一个 COALESCE 来用 0.0 替换左连接中的任何 nulls 值。

还要注意，我喜欢在每个子查询的顶部添加定性注释，以跟踪我的代码在做什么。我见过有人使用两层、三层或更多层的子查询，而不包含任何注释。我觉得这很难读懂。我建议加入注释，以促进协作，并使团队成员更容易审查您的代码。

做完这些工作后，让我们后退一步。实际上，在我们将一堆直方图放在一起并发送给我们的利益相关者之前，我们还需要考虑一件事。

提示:仔细查看一些客户记录:

```
+---------+------------+----------------+--------------------+
| cust_id |  cre8_dt   |    full_nm     |     email_adr      |
+---------+------------+----------------+--------------------+
|       1 | 2003-06-14 | Karen Gillan   | [kgil@mail.com](mailto:kgil@mail.com)      |
|       2 | 2020-12-19 | Dwayne Johnson | [therock@mail.com](mailto:therock@mail.com)   |
|       3 | 2007-10-28 | Kevin Hart     | [kevin@hart.com](mailto:kevin@hart.com)     |
|       4 | 2008-01-01 | Awkwafina      | [awkwa@fina.com](mailto:awkwa@fina.com)     |
|       5 | 2015-05-17 | Nick Jonas     | [jonasbro3@mail.com](mailto:jonasbro3@mail.com) |
+---------+------------+----------------+--------------------+
```

在 2010 年至 2020 年间的每个黑色星期五，只有凯伦·吉兰的宠物反斗城账户存在。道恩·强森的账户是在 2020 年黑色星期五之后才创建的。道恩·强森的黑色星期五零销售额应该被列入 2010 年的柱状图吗？对于 2015 年？2020 年？不，不，不。奥卡菲娜的黑色星期五零销售额应该被列入 2007 年的柱状图吗？不是。2008 年的？是的。

在本文中，我们不会专门讨论如何解决上述问题，而是考虑如何解决这个问题，无论是通过编程还是使用 SQL。对此有多种正确的方法！如果考虑多种方法，哪种方法的性能最好？如果你只想到一种方法，你如何优化它？在面试的最后，这可能是一个很好的定性讨论。

如您所见，提取数据会变得非常棘手。对手头的任务进行定性思考，并预测如何规避数据中的任何古怪之处或已知缺陷，这一点极其重要。

最后一条建议:

> 了解你的数据。
> 
> —我见过的所有数据工程师

如果你在面试，不要害怕问关于手头数据集的问题。这向面试官展示了你注重细节，考虑周到，在工作中不太可能出错。

# 勇往直前去面试吧！

上面的例子是实际面试问题的变体，我见过很多面试者纠结于这些问题。有时候我的团队真的很喜欢一个候选人，但是他们的 SQL 太落后了，我们担心他们会花太多时间来适应。

还有，一定要毫不留情地表现出积极的态度。这意味着，即使事情变得艰难，你认为你已经表现得不能再差了，或者你认为面试问题完全不公平，保持积极的态度！作为一名面试官，我不只是在寻找正确的答案。我在寻找一个专业、合作、坦率、友善的人。你想雇佣谁加入你的团队:布雷弗斯通还是范佩尔？？

我希望这篇文章有助于提升你的 SQL 面试准备水平。走上前，向你的下一位面试官展示你已经准备好加入他们的团队。

祝你好运！