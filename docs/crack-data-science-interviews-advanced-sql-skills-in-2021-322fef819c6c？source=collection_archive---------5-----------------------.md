# 破解数据科学访谈:2021 年的高级 SQL 技能

> 原文：<https://towardsdatascience.com/crack-data-science-interviews-advanced-sql-skills-in-2021-322fef819c6c?source=collection_archive---------5----------------------->

## 破解数据科学面试

## 数据科学家和软件工程师的高级读物

![](img/1d80ed51b8663df3e7cb4b4da52c6713.png)

由[费尔·南多](https://unsplash.com/@fer_nando?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/t/wallpapers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

结构化查询语言 SQL 是检索和管理数据的首选编程语言。从关系数据库中有效地提取数据是任何数据专业人员的必备技能。在过去的几个月里，我一直与数据科学的领导者保持密切联系，经常出现的一个建议是编写更多更好的 SQL 查询。

> 为了跟踪谁是活跃用户，我们使用 SQL。
> 
> 为了计算业务指标，我们使用 SQL。
> 
> 为了执行任何与数据检索和管理相关的操作，我们使用 SQL。
> 
> …

在之前的两篇帖子中，我介绍了数据科学访谈中问到的几个基本 SQL 技能，这对初学者读者来说是一个很好的开始:

[](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [## 2021 年数据科学家必备的 SQL 技能

### 数据科学家/工程师的重要 SQL 技能

towardsdatascience.com](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77) [## 2021 年数据科学家面临的 4 个棘手的 SQL 问题

### 可能会让你犯错的简单查询

towardsdatascience.com](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77) 

在今天的帖子中，我对 FAANG 公司提出的 5 个真实的面试问题进行了现场编码和思考。这些问题都是 LeetCode 上的中级题，第一次试用解决不了的请放心。

# #问题 1:苹果、亚马逊和脸书的排名分数

*   [https://leetcode.com/problems/rank-scores/](https://leetcode.com/problems/rank-scores/)

*编写一个 SQL 查询来对分数进行排名。如果两个分数相同，两者的排名应该相同。请注意，在平局之后，下一个排名数字应该是下一个连续的整数值。换句话说，级别之间不应该有“漏洞”。*

```
+----+-------+
| Id | Score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
```

*例如，给定上面的* `*Scores*` *表，您的查询应该生成以下报告(按最高分排序):*

```
+-------+---------+
| score | Rank    |
+-------+---------+
| 4.00  | 1       |
| 4.00  | 1       |
| 3.85  | 2       |
| 3.65  | 3       |
| 3.65  | 3       |
| 3.50  | 4       |
+-------+---------+
```

***重要提示:*** *对于 MySQL 解决方案，要转义用作列名的保留字，可以在关键字前后使用撇号。例如***【等级】。**

## *穿过我的思维*

*苹果、亚马逊和脸书在他们的数据科学采访中提出了这个问题。这是一个简单的问题，要求等级之间没有“漏洞”或“跳跃”。*

*在 SQL 中，有两种处理秩的方式: *rank()* 和 *dense_rank()* 。区别在于 dense_rank()命令不会跳过等级，而 rank()命令会在出现平局时跳过等级。*

*问题语句明确地告诉我们，在平局之后，等级之间不应该有“洞”，这使得 dense_rank()成为一个明显的选择。如果另一个问题没有提供这样一个友好的暗示，记得让你的面试官澄清如何打领带。如果他们有意排除如此重要的信息，他们希望你提出问题。*

## *解决办法*

```
*# Write your MySQL query statement below
SELECT score, DENSE_RANK() OVER(ORDER BY Score DESC) AS `Rank`
FROM Scores*
```

*有两点需要注意:*

1.  ***DESC** 。如果问题要求按得分降序排列，记得用 DESC。*
2.  ***`Rank`** 。Rank 是 SQL 中的保留关键字，您应该在它的前后包含一对撇号，例如 **`Rank`** 。*

# *#问题 2:数苹果和橘子*

*   *[https://leetcode.com/problems/count-apples-and-oranges/](https://leetcode.com/problems/count-apples-and-oranges/)*

*表:`Boxes`*

```
*+--------------+------+
| Column Name  | Type |
+--------------+------+
| box_id       | int  |
| chest_id     | int  |
| apple_count  | int  |
| orange_count | int  |
+--------------+------+
box_id is the primary key for this table.
chest_id is a foreign key of the chests table.
This table contains information about the boxes and the number of oranges and apples they contain. Each box may contain a chest, which also can contain oranges and apples.*
```

*表:`Chests`*

```
*+--------------+------+
| Column Name  | Type |
+--------------+------+
| chest_id     | int  |
| apple_count  | int  |
| orange_count | int  |
+--------------+------+
chest_id is the primary key for this table.
This table contains information about the chests we have, and the corresponding number if oranges and apples they contain.*
```

*编写一个 SQL 查询来计算所有盒子中苹果和橘子的数量。如果一个盒子里有一个箱子，你还应该包括里面苹果和橘子的数量。*

**返回结果表中* ***任意顺序*** *。**

**查询结果格式如下:**

```
*Boxes table:
+--------+----------+-------------+--------------+
| box_id | chest_id | apple_count | orange_count |
+--------+----------+-------------+--------------+
| 2      | null     | 6           | 15           |
| 18     | 14       | 4           | 15           |
| 19     | 3        | 8           | 4            |
| 12     | 2        | 19          | 20           |
| 20     | 6        | 12          | 9            |
| 8      | 6        | 9           | 9            |
| 3      | 14       | 16          | 7            |
+--------+----------+-------------+--------------+

Chests table:
+----------+-------------+--------------+
| chest_id | apple_count | orange_count |
+----------+-------------+--------------+
| 6        | 5           | 6            |
| 14       | 20          | 10           |
| 2        | 8           | 8            |
| 3        | 19          | 4            |
| 16       | 19          | 19           |
+----------+-------------+--------------+

Result table:
+-------------+--------------+
| apple_count | orange_count |
+-------------+--------------+
| 151         | 123          |
+-------------+--------------+*
```

## *穿过我的思维*

*这是一个很好的 SQL 问题，因为它测试了候选人的分析技巧和分解问题的能力。让大多数候选人感到困惑的棘手部分是存在两个表，您应该将这两个表结合起来以获得有效值。*

*例如，box_id = 18 有来自箱子表的附加苹果和桔子计数，但是 box_id = 2 没有附加计数。*

> *这两种情况下如何计算计数？*

*答案是 IFNULL()命令。我们指定如果箱子表中没有值该怎么办，如果有值该怎么办。如果没有 IFNULL()语句，我们就会遇到错误。*

## *解决办法*

```
*# Write your MySQL query statement below
SELECT SUM(b.apple_count+IFNULL(c.apple_count,0)) AS apple_count, SUM(b.orange_count+IFNULL(c.orange_count,0)) AS orange_count
FROM Boxes b
LEFT JOIN Chests c
USING(chest_id)*
```

*有两点需要注意:*

1.  ***左加入**。我们应该使用左连接，而不是内连接，因为我们希望在 box 表中保留甚至没有 chest_id 的记录。*
2.  ***IFNULL(c.apple_count，0)** 。如果 apple_count 中有 Null，我们应该指定值。*

# *#问题 3:项目雇员 I，脸书*

*   *[https://leetcode.com/problems/project-employees-i/](https://leetcode.com/problems/project-employees-i/)*

*表:`Project`*

```
*+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
(project_id, employee_id) is the primary key of this table.
employee_id is a foreign key to Employee table.*
```

*表:`Employee`*

```
*+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
employee_id is the primary key of this table.*
```

**编写一个 SQL 查询，报告每个项目所有员工的平均******工作年限，四舍五入到两位数*** *。****

***查询结果格式如下:***

```
**Project table:
+-------------+-------------+
| project_id  | employee_id |
+-------------+-------------+
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |
+-------------+-------------+Employee table:
+-------------+--------+------------------+
| employee_id | name   | experience_years |
+-------------+--------+------------------+
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |
+-------------+--------+------------------+Result table:
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
| 1           | 2.00          |
| 2           | 2.50          |
+-------------+---------------+
The average experience years for the first project is (3 + 2 + 1) / 3 = 2.00 and for the second project is (3 + 2) / 2 = 2.50**
```

## **穿过我的思维**

**脸书包括这个问题。在写任何查询之前，我总是问自己:**

> **“你真的明白这个问题问的是什么吗？”**

**上面写着每个项目的平均经验年数。有两条关键信息。首先，平均值被定义为经验年数的总和除以员工人数。第二，结果应该按项目分组。**

**剩下的就不言自明了。**

## **解决办法**

```
**# Write your MySQL query statement below
SELECT project_id, ROUND(SUM(experience_years)/COUNT(employee_id),2) AS average_years
FROM Project
JOIN Employee
USING(employee_id)
GROUP BY project_id**
```

**一个警告，记住将结果四舍五入到 2 位数，如果问题没有说明什么，就要求澄清。**

# ****#问题 4:两个人之间的通话次数，亚马逊****

*   **[https://leet code . com/problems/number-of-calls-between-two-person/](https://leetcode.com/problems/number-of-calls-between-two-persons/)**

**表:`Calls`**

```
**+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| from_id     | int     |
| to_id       | int     |
| duration    | int     |
+-------------+---------+
This table does not have a primary key, it may contain duplicates.
This table contains the duration of a phone call between from_id and to_id.
from_id != to_id**
```

***编写一个 SQL 查询来报告每对不同的人* `*(person1, person2)*` *之间的通话次数和总通话时长，其中* `*person1 < person2*` *。***

***以任意顺序返回结果表。***

***查询结果格式如下:***

```
**Calls table:
+---------+-------+----------+
| from_id | to_id | duration |
+---------+-------+----------+
| 1       | 2     | 59       |
| 2       | 1     | 11       |
| 1       | 3     | 20       |
| 3       | 4     | 100      |
| 3       | 4     | 200      |
| 3       | 4     | 200      |
| 4       | 3     | 499      |
+---------+-------+----------+Result table:
+---------+---------+------------+----------------+
| person1 | person2 | call_count | total_duration |
+---------+---------+------------+----------------+
| 1       | 2       | 2          | 70             |
| 1       | 3       | 1          | 20             |
| 3       | 4       | 4          | 999            |
+---------+---------+------------+----------------+**
```

## **穿过我的思维**

**亚马逊问这个问题。分析问题后，我们发现一对不同的人可能是**人 1 →人 2 或人 2 →人 1。我们必须把它们加在一起，得到总持续时间。****

*   **我们如何做到这一点？**
*   **从 from_id 和 to_id 中选取值时的用例。**

**由于问题有一个额外条件 **person1 < person2** ，我们可以使用 CASE WHEN 语句从 from_id 和 to_id 中选择较小(较大)的值。具体来说，我们选择较小的值作为人员 1，较大的值作为人员 2。**

```
**SELECT CASE WHEN from_id< to_id THEN from_id ELSE to_id END AS person1**
```

**然后，我们按照相同的过程为 person2 选择较大的值，并计算数量/持续时间。**

**参见下面的代码。**

## **解决办法**

```
**# Write your MySQL query statement below
SELECT CASE WHEN from_id< to_id THEN from_id ELSE to_id END AS person1,
CASE WHEN from_id <to_id THEN to_id ELSE from_id END AS person2,
COUNT(*) AS call_count,SUM(duration) AS total_durationFROM Calls 
GROUP BY person1, person2**
```

# **问题 5:销售分析 II，亚马逊**

*   **[https://leetcode.com/problems/sales-analysis-ii/](https://leetcode.com/problems/sales-analysis-ii/)**

**表:`Product`**

```
**+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
| unit_price   | int     |
+--------------+---------+
product_id is the primary key of this table.**
```

**表:`Sales`**

```
**+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| seller_id   | int     |
| product_id  | int     |
| buyer_id    | int     |
| sale_date   | date    |
| quantity    | int     |
| price       | int     |
+------ ------+---------+
This table has no primary key, it can have repeated rows.
product_id is a foreign key to Product table.**
```

***编写一个 SQL 查询，报告* ***购买者*** *购买了 S8，但没有购买 iPhone 和 G4。请注意，S8、iPhone 和 G4 是出现在* `*Product*` *表中的产品。***

***查询结果格式如下:***

```
**Product table:
+------------+--------------+------------+
| product_id | product_name | unit_price |
+------------+--------------+------------+
| 1          | S8           | 1000       |
| 2          | G4           | 800        |
| 3          | iPhone       | 1400       |
+------------+--------------+------------+Sales table:
+-----------+------------+----------+------------+----------+-------
| seller_id | product_id | buyer_id | sale_date  | quantity | price 
+-----------+------------+----------+------------+----------+-------
| 1         | 1          | 1        | 2019-01-21 | 2        | 2000  |
| 1         | 2          | 2        | 2019-02-17 | 1        | 800   |
| 2         | 1          | 3        | 2019-06-02 | 1        | 800   |
| 3         | 3          | 3        | 2019-05-13 | 2        | 2800  |
+-----------+------------+----------+------------+----------+-------Result table:
+-------------+
| buyer_id    |
+-------------+
| 1           |
+-------------+
The buyer with id 1 bought an S8 but didn't buy an iPhone. The buyer with id 3 bought both.**
```

## **穿过我的思维**

**亚马逊问这个问题。要选择购买了 S8 但没有购买 iPhone (G4)的用户，我们可以使用 **product_name = 'S8'** 和 **product_name！= 'iPhone'** 如果是 Python 问题，则过滤出案例。不幸的是，它在 SQL 中变得更加棘手，我们不能使用！=符号。**

> **那么，如何才能筛选出病例呢？**
> 
> ****SUM(以防万一)化险为夷！****

**回想一下我们可以用 CASE WHEN 语句来赋值条件。此外，我们可以将它与 SUM()子句结合起来，计算每种情况的总值，将其相加，并选择满足条件的案例。**

## ****解决方案****

```
**# Write your MySQL query statement belowSELECT DISTINCT buyer_id
FROM Sales 
INNER JOIN Product 
USING(product_id)
GROUP BY buyer_id
HAVING SUM(CASE WHEN product_name = ‘S8’ THEN 1 ELSE 0 END) > 0
AND SUM(CASE WHEN product_name = ‘iPhone’ THEN 1 ELSE 0 END) = 0 
AND SUM(CASE WHEN product_name = ‘G4’ THEN 1 ELSE 0 END) = 0**
```

# **外卖食品**

*   **在提出解决方案之前，充分理解问题。**
*   **如果缺少关键信息，询问澄清问题。**
*   **在下一次数据科学面试之前，练习这些高级主题。**

***Medium 最近进化出了自己的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。***

**[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership)** 

# **我的数据科学面试序列**

**[](/online-controlled-experiment-8-common-pitfalls-and-solutions-ea4488e5a82e) [## 运行 A/B 测试的 8 个常见陷阱

### 如何不让你的在线控制实验失败

towardsdatascience.com](/online-controlled-experiment-8-common-pitfalls-and-solutions-ea4488e5a82e) [](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [## FAANG 在 2021 年提出这 5 个 Python 问题

### 数据科学家和数据工程师的必读！

towardsdatascience.com](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。**