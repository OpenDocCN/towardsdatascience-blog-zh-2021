# 破解数据科学访谈:数据科学家的五项 SQL 技能

> 原文：<https://towardsdatascience.com/crack-data-science-interviews-five-sql-skills-for-data-scientists-cc6b32df1987?source=collection_archive---------3----------------------->

## 破解数据科学面试

## Leetcode 助你获得高薪数据职位

![](img/0078a21998802dd78b2f3fd9beb45c81.png)

[泽通李](https://unsplash.com/@zetong?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/big-data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

结构化查询语言 SQL 是数据从业者用来检索存储在关系数据库中的数据的首选编程语言。对于数据科学家来说，编写有效的查询请求不再被认为是一项不错的技能，而是一项基本技能。这种趋势可以通过在 DS 招聘信息和面试环节中具体包含 SQL 经验得到支持。

除了编程( [Python](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0?sk=1e974890a4089d3697b2f3b00967bbd4) )、[机器学习](/classifying-rare-events-using-five-machine-learning-techniques-fab464573233?sk=f015318a6eb37b81a33b5dc004ee4be3)、 [A/B 测试](/how-user-interference-may-mess-up-your-a-b-tests-f29abfcfccf8?sk=9ff3bbae01ff9951e6eca33e1c75cff4)和[统计](/crack-data-science-interviews-essential-statistics-concepts-d4491d85219e?sk=af5291865f239ad505e8a8a1a80cceb8)，数据科学家经常被要求定义和从多个来源提取数据，以构建感兴趣的指标。不幸的是，SQL 仍然是一项不受重视的技能，没有足够的数据科学家充分认识到它的多功能性和重要性。

在过去的几个月里，我一直与主要技术公司的高级数据科学家和招聘经理保持密切联系。我收到的评价最高的建议和推荐之一是掌握 SQL 技能，并知道如何有效地提取数据。

为此，我创建了一个三部曲帖子，专为初级程序员量身定制。它从简单到中等难度的问题逐步进行。如果你最近开始了你的 SQL 之旅，我建议在阅读这篇文章之前先看看这三篇文章:1。[容易](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f?sk=152837507c40a42f5132c7a649222b0a)，2。[容易/中等](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77?sk=fcd25ff25193a68415e17fac109ee92a)和 3。[中等](/crack-data-science-interviews-advanced-sql-skills-in-2021-322fef819c6c?sk=cd796b2b53f0b9106e59967d3e8fb8f3)。

# 问题 1:不同性别的累计总数

<https://leetcode.com/problems/running-total-for-different-genders/>  

*编写一个 SQL 查询来查找每种性别每天的总分。*

*按性别和日期排序结果表*

*查询结果格式如下:*

```
Scores table:
+-------------+--------+------------+--------------+
| player_name | gender | day        | score_points |
+-------------+--------+------------+--------------+
| Aron        | F      | 2020-01-01 | 17           |
| Alice       | F      | 2020-01-07 | 23           |
| Bajrang     | M      | 2020-01-07 | 7            |
| Khali       | M      | 2019-12-25 | 11           |
| Slaman      | M      | 2019-12-30 | 13           |
| Joe         | M      | 2019-12-31 | 3            |
| Jose        | M      | 2019-12-18 | 2            |
| Priya       | F      | 2019-12-31 | 23           |
| Priyanka    | F      | 2019-12-30 | 17           |
+-------------+--------+------------+--------------+
Result table:
+--------+------------+-------+
| gender | day        | total |
+--------+------------+-------+
| F      | 2019-12-30 | 17    |
| F      | 2019-12-31 | 40    |
| F      | 2020-01-01 | 57    |
| F      | 2020-01-07 | 80    |
| M      | 2019-12-18 | 2     |
| M      | 2019-12-25 | 13    |
| M      | 2019-12-30 | 26    |
| M      | 2019-12-31 | 29    |
| M      | 2020-01-07 | 36    |
+--------+------------+-------+
```

## 穿过我的思维

这是那种你第一眼就能解决或者感觉完全迷失的问题。它很容易把你引入歧途。

原因如下。

问题要求“*每个性别每天的总分，”*我的第一反应是计算累计总和，然后按性别和天分组。所以，我错误地试图在 SQL 中找到一个不存在的计算累积和的语法。

经过一番努力之后，我意识到我们可以应用 *SUM() OVER( PARTITION BY…* )语句来计算按性别分组的累计总和。然后，按天排序结果。

## 解决办法

```
# Write your MySQL query statement below
SELECT gender, day, SUM(score_points) OVER(PARTITION BY gender ORDER BY day) AS total
FROM Scores
```

## 经验法则:

*   许多 SQL 命令遵循相同的语法，如 SUM() OVER()、ROW_NUMBER() OVER()、LEAD() OVER 等。理解了其中一个命令的用法，您就为其他命令做好了准备。
*   你一定要把你的测试结果分组吗？如果是这样，请使用 PARTITION BY。
*   成绩排名怎么样？如果是这样，请使用 ORDER BY。

# 问题 2:找出连续范围的起始数和结束数

<https://leetcode.com/problems/find-the-start-and-end-number-of-continuous-ranges/>  

*由于* `*Logs*` *中删除了部分 id。编写一个 SQL 查询来查找表* `*Logs*` *中连续范围的开始和结束编号。*

*按* `*start_id*` *顺序排列结果表。*

*查询结果格式如下:*

```
Logs table:
+------------+
| log_id     |
+------------+
| 1          |
| 2          |
| 3          |
| 7          |
| 8          |
| 10         |
+------------+Result table:
+------------+--------------+
| start_id   | end_id       |
+------------+--------------+
| 1          | 3            |
| 7          | 8            |
| 10         | 10           |
+------------+--------------+
```

## 穿过我的思维

亚马逊在面试过程中包括了这个问题。

此问题询问缺少数字的连续范围的开始和结束。识别连续范围的起点和终点相对容易，可以分别使用 *MIN(log_id)* 和 *MAX(log_id)* 找到。棘手的部分是识别两个范围之间的不连续性。例如，我们如何辨别这两个连续范围之间的不连续性:1–3 和 7–8？在这里，我们遗漏了 5、6 和 7。

解决方案在于我们如何对结果进行分组。**如果存在不连续，我们将观察到 log_id 和每个观察的行排名之间的差距。**对于连续范围，观测值应具有相同的范围；如果有任何差距，那么我们观察到组差异的跳跃。

```
Logs table:
+------------+
| log_id     |
+------------+
| 1          |
| 2          |
| 3          |
| 7          |
| 8          |
| 10         |
+------------+
```

在 Logs 表中，数字 1、2、3 构成了一个连续的范围，log_id 与行排名之差在同一范围内是一致的，为 0(即 log_id —行排名，1–1 = 2–2 = 3–3 = 0)。同样的规则也适用于 7 和 8，两者相差 3(即 log _ id-row ranking = 7-4 = 8–5 = 3)。正如所看到的，当我们从第一范围移动到第二范围时，我们观察到差异的跳跃(不连续)(从 0 到 3)。

## 解决办法

```
# Write your MySQL query statement below
SELECT MIN(log_id) AS start_id, MAX(log_id) AS end_id 
FROM (
 SELECT log_id, ROW_NUMBER() OVER(ORDER BY log_id) AS row_id
 FROM Logs
) sub
GROUP BY log_id — row_id
ORDER BY start_id
```

## 经验法则:

*   分析手头的问题，把它分解成不同的部分。如果需要，使用子查询来完成次要任务，并移至外部查询。
*   ROW_NUMBER() OVER(ORDER BY …):获取每个单元的行号。
*   GROUP BY:log _ id 和行号之间的差值，用于标识不连续性。

![](img/2b3d248eb2875c397b29e83c77b61a0e.png)

照片由[法比安·金特罗](https://unsplash.com/@onefabian?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/nature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

# 问题 3:部门最高工资

<https://leetcode.com/problems/department-highest-salary/>  

*`*Employee*`*表包含所有雇员。每个雇员都有一个 Id，一份薪水，还有一个部门 Id 列。**

```
*+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+*
```

**`*Department*`*表包含公司的所有部门。***

```
**+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+**
```

***编写一个 SQL 查询来查找每个部门中工资最高的雇员。对于上面的表，您的 SQL 查询应该返回下面的行(行的顺序无关紧要)。***

```
**+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+**
```

## **穿过我的思维**

**亚马逊在面试过程中包括了这个问题。**

**在给出具体细节之前，让我浏览一下总体分析。问题问的是各部门工资最高的员工。因此，自然的问题是可能有多个工资最高的雇员，我们需要返回匹配的雇员。**

**下面是具体的细分。我将使用一个子查询来获取按部门 Id 分组的最高(最大)薪金，然后在匹配部门 ID 和薪金之前连接雇员和部门表。**

## **解决办法**

```
**# Write your MySQL query statement below
SELECT d.Name AS Department, e.Name AS Employee, Salary
FROM Employee e
JOIN Department d
ON d.Id = e.DepartmentIdWHERE (DepartmentId,Salary) IN (
 SELECT DepartmentId, MAX(Salary)
 FROM Employee
 GROUP BY DepartmentId
)**
```

## **经验法则:**

*   **`*Department*` 表只包含两列，其唯一的功能是返回部门名称为部门 id。这应该会变成看问题提示时的第二本能。**
*   **如果问题是返回每个组(如 department)的最大/最小值，典型的方法是应用子查询为每个组创建一对最大/最小值，然后将该对与外部查询进行匹配(如 WHERE 子句中所示)。**

# **问题 4:第二高的薪水**

**<https://leetcode.com/problems/second-highest-salary/submissions/>  

*编写一个 SQL 查询，从* `*Employee*` *表中获取第二高的薪水。*

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

*例如，给定上面的雇员表，查询应该返回* `*200*` *作为第二高的薪水。如果没有第二高的薪水，那么查询应该返回* `*null*` *。*

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

## 穿过我的思维

亚马逊和苹果在他们的采访循环中包含了这个问题。

我在另一篇博文中介绍了如何解决这个问题:

</4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77>  

这是一个简单的问题，可以通过以下步骤解决:

```
1\. Rank Salary in a descending order2\. LIMIT result by 1 and OFFSET 1, which returns the second highest salary3\. Use DISTINCT to deal with duplicates 4\. Include the IFNULL statement in the outer query 
```

[查看其他帖子了解详细解释](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77?sk=fcd25ff25193a68415e17fac109ee92a)。包含它的唯一原因是为问题 5 提供基线分析，并允许我们比较和对比解决方案。

## 解决办法

```
SELECT IFNULL(
 (SELECT DISTINCT Salary
  FROM Employee
  ORDER BY Salary DESC
  LIMIT 1 
  OFFSET 1),
NULL) AS SecondHighestSalary
```

## 经验法则:

*   理解 OFFSET 和 IFNULL 是如何工作的。

# 问题 5:第 n 高工资

<https://leetcode.com/problems/nth-highest-salary/>  

*编写一个 SQL 查询，从* `*Employee*` *表中获取第 n 份最高工资。*

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

*例如，给定上面的员工表，其中 n = 2 的第 n 个最高工资是* `*200*` *。如果没有第 n 个最高工资，那么查询应该返回* `*null*` *。*

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```

## 穿过我的思维

这是一个普遍的问题，几乎所有的 FAANG 公司都希望求职者表现出色。

它要求第 n 个最高工资，如果没有这样的值，则返回`*null*`。例如，如果我们输入 n=2，则返回第二高的值；n=3，第三高。

第一步，我们需要根据薪水值(降序)在子查询中创建一个排名索引。在采访中，我们需要澄清我们应该如何处理平局:我们应该跳过下一个指数还是返回一个连续的数字？

对于我们的例子，我们选择 DENSE_RANK 来确保连续的排名；说出排名*排名 _ 薪资*。

在外部查询中，我们选择 DISTINCT Salary 并在 WHERE 子句中设置 *n = rankings_salary* ，这将返回排名第 n(最高)的薪金。

## 解决办法

```
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
 RETURN (
 # Write your MySQL query statement below.
 SELECT IFNULL((SELECT DISTINCT Salary
 FROM (
 SELECT Salary, DENSE_RANK() OVER(ORDER BY Salary DESC) AS rankings_salary
 FROM Employee
 ) AS aaa
 WHERE n = rankings_salary), NULL) AS getNthHighestSalary)

 ;
END
```

## **经验法则:**

*   如果您必须编写嵌套查询请求，请不要惊慌。推荐的方法是首先编写子查询中最里面的部分，然后一层一层地移动到外部查询。
*   一个常见的错误是:忘记命名子查询。
*   不要忘记使用 DISTINCT 和 IFNULL。

# 外卖食品

熟能生巧。

当我开始用 SQL 编码时，我能执行的唯一查询是对表中的唯一值进行计数。快进到今天，我已经习惯了需要子查询、窗口函数和其他高级应用程序的更高级的编码任务。

我想与我的数据同事们分享的最大收获是从小处着手，从错误中学习，并相信增量学习的价值。做那种成长，变得比昨天好 1%的人。你最终会得到回报的。

感谢你的读者，祝一切顺利。

*Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

<https://leihua-ye.medium.com/membership>  

# 我的数据科学面试序列

</how-user-interference-may-mess-up-your-a-b-tests-f29abfcfccf8>  </online-controlled-experiment-8-common-pitfalls-and-solutions-ea4488e5a82e>  

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。**