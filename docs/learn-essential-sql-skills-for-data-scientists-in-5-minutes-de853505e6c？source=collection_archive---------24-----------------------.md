# 在 5 分钟内学会数据科学家必备的 SQL 技能

> 原文：<https://towardsdatascience.com/learn-essential-sql-skills-for-data-scientists-in-5-minutes-de853505e6c?source=collection_archive---------24----------------------->

## 想快速学习 SQL？

## 学习基本的 SQL 技能，让你的数据科学简历更有吸引力

![](img/28dcf96517b517dea367411cfab9d0f0.png)

约书亚·索蒂诺在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

先说一个有趣的事实。SQL 是数据分析师和数据工程师最需要的技能，也是数据科学家第三需要的技能。好吧，这个事实只是告诉你了解 SQL 有多重要。

好吧，但是为什么 SQL 是一个非常需要的技能呢？通常，数据科学家使用 Python 或 r 处理数据框。然而，当今数据科学中的海量数据无法完全加载到数据框中，甚至无法加载到. csv 文件中。对于这种情况，需要一个 SQL 数据库。SQL 非常简单易学。对于熟悉数据框架的数据科学家来说，学习 SQL 变得容易多了。在本文中，我们将学习执行普通数据科学操作的所有基本 SQL 技能。博客的内容如下:

*   创建表格
*   选择表和列，选择不同的值
*   条件选择、选择行、排序列
*   插入、更新和删除值
*   基于基本统计值的条件:最小值、最大值、计数、平均值、总和
*   连接表格

不用再等了，让我们开始学习不同的话题吧。

由[吉菲](https://giphy.com/)

# 创建表格

SQL 数据库由几个表组成。表格相当于数据帧，它有列和行。*创建表格*命令用于创建表格。表格的每一列都被分配了一个数据类型，例如 *char* 或 *varchar。Char* 是固定长度的字符串。 *Varchar* 是一个可变长度的字符串。该命令还使用了一些约束，如**主键**和**不为空**。关系表的主键唯一地标识表中的每个元组或行。它还可以防止表中出现重复值。Not null 确保这些字段不能包含空值。

```
*CREATE TABLE users
(id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(255),
user_name VARCHAR(255))*
```

上面的命令将创建一个名为“users”和[columns id，name，user_name]的表。

# 选择表和列

当我们已经有了一个数据库，那么我们需要一些查询语句来读取数据。在本节中，我们将看到如何做到这一点。 *SELECT* 命令决定您想要从给定的表中提取哪些列。来自 命令的*决定了你想要从哪个表中提取信息。要选择某些列，使用以下语法:*

```
SELECT column1, column2 FROM table_name;
```

要选择所有列，请执行以下操作:

```
SELECT * FROM table_name;
```

在表中，列可能包含重复值。如果我们只想要唯一的值，可以使用*选择不同的*命令。

```
SELECT DISTINCT column1, column2 FROM table_name;
```

# 条件行选择和列排序

在上一节中，我们已经学习了如何获取某个表的一些列。在本节中，我们将了解如何根据不同的标准选择一些行。SQL 中的 *WHERE* 命令允许有条件地选择行。语法是

```
SELECT column1, column2 FROM table_name WHERE condition;
```

举个例子，

```
SELECT column1, column2 FROM table_name where column1 = 10;
```

此查询从 table_name 中选择值 column1 和 column2，其中 column1 的值等于 10。*中的操作者所在的*命令包括:

*   `<`不到
*   `>`对于大于
*   `=`为平等
*   `<=`为小于或等于
*   `>=`为大于或等于
*   `<>` for not equal(在 SQL 的某些版本中可以写成`!=`)
*   `BETWEEN x AND y`用于`x`和`y`之间的所有值，以及`x`和`y`
*   `LIKE string`对于某种模式(例如，字符串`s%`返回以`s`开头的列的所有值)

如果希望列 1 满足一个条件，列 2 满足另一个条件，可以使用*和*。例如:

```
SELECT * FROM table_name WHERE column1 < 3 AND column2 > 8;
```

以上将返回列 1 值小于 3 且列 2 值大于 8 的行。同样，您也可以使用*或*和*而不是*命令。

现在，我们已经看到了如何根据不同的标准筛选出行。为了以特定的顺序得到行的输出，我们可以使用 *ORDER BY。*例如

```
SELECT column1, column2 FROM table_name ORDER BY column1 ASC WHERE column1 < 3;
```

以上将返回 column1 和 column2 的值，其中 column1 的值小于 3，返回的数据将根据 column1 的值按升序排列。 *ASC* 可替换为 *DESC* ，如果数值是从最大到最小，而不是从最小到最大(默认)。为了更好地理解，让我们再举一个例子。

```
SELECT * FROM table_name ORDER BY country, income;
```

以上将返回整个表，按国家的字母顺序排序，如果国家相同，则按收入排序。

# 插入、更新和删除值

使用插入命令将数据插入到表*中。在此命令的语法中，表名标识表，列名列表标识表中的每一列，值列表指定要添加到表中各列的数据值。请注意，values 子句中提供的值的数量等于列名列表中指定的列名的数量。*

```
INSERT INTO table_name (column1, column2, column3) VALUES (value1, value2, value3);
```

如果将值插入到每一列中，则不需要指定值要插入到哪一列，因为假设值是按照从左到右的列排列方式排序的。例如，如果您有一个包含 3 列的表格，您可以使用:

```
INSERT INTO table_name VALUES (value1, value2, value3);
```

更新值遵循以下语法:

```
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```

只要指定了条件，它就会更新 column1 和 column2 的值。

DELETE 的语法是

```
DELETE FROM table_name WHERE condition;
```

**注意，**如果没有给出 *WHERE* 命令，整个表格内容将被删除。

# 基于基本统计值的条件

它们采用值的集合或整个列来输出单个值。例子包括 *COUNT()、SUM()、MIN()、MAX()、AVG()、*等。，分别输出行数、值的总和、最小值、最大值和平均值。例如

```
SELECT COUNT(column1) from table_name where column1=3
```

以上将返回列 1 的值为 3 的行数。类似地

```
SELECT MAX(column1) from table_name where column2>1
```

以上将返回 column2 的值大于 1 的 column1 的最大值。

# 连接表

为了合并两个表中的数据，我们使用了 *JOIN* 命令。有多种连接表的方法。在这篇博客中，我们将只关注*内部连接。*在这种类型的连接中，两个表的结果是匹配的，它只显示与查询中指定的条件匹配的结果集。

说有两个表，*员工*和 *JOB_HISTORY* 。我们希望根据员工 ID 将员工与工作历史联系起来。

```
*select EMPLOYEES.F_NAME,EMPLOYEES.L_NAME, Job_History.START_DATE
from EMPLOYEES 
INNER JOIN JOB_HISTORY on EMPLOYEES.EMP_ID=JOB_HISTORY.EMPL_ID*
```

上面的命令将输出一个包含列名为" *EMPLOYEES "的组合表。F_NAME "，"员工。L_NAME "，" Job_History。开始日期”。*它将只输出两个表中员工 ID 匹配的值。还有其他类型的联接，如*左联接*、*右联接*和*全外联接、*，我们不会在本博客中一一介绍，因为本博客的主要目的是为您提供数据科学家所需的 SQL 基本功能。

# 结论

我希望这篇文章能够帮助您理解和学习数据科学家所需的基本 SQL 技能。现在继续把 SQL 作为一项技能添加到你的简历中。如果您喜欢其中的内容，并希望随时了解广阔的数据科学领域，请关注我们的 [medium](https://medium.com/@AnveeNaik) 。

*成为* [*介质会员*](https://medium.com/@AnveeNaik/membership) *解锁并阅读介质上的许多其他故事。*