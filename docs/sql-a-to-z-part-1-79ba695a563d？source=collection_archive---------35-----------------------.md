# 从 A 到 Z 的 SQL:第 1 部分

> 原文：<https://towardsdatascience.com/sql-a-to-z-part-1-79ba695a563d?source=collection_archive---------35----------------------->

## 学习最重要和最常用的 SQL 语句

# **为什么要学习 SQL？**

作为数据科学家，我们利用数据提供见解和建议，为产品战略、增长和营销、运营以及许多其他业务领域提供信息。在许多情况下，数据以关系数据库格式存储，使用允许访问彼此相关的数据点的结构(来源: [AWS](https://aws.amazon.com/relational-database/) )。SQL(结构化查询语言)是一种用于访问关系数据库的编程语言。作为数据科学家，我们使用 SQL 对数据库执行查询，以检索数据、插入、更新和删除数据库中的记录，并创建新的数据库和表。语法可能会因关系数据库管理系统的不同而略有不同(例如，在查询末尾使用分号)，但一般逻辑应该适用。

数据作为称为表的对象存储在数据库中，表是列和行形式的数据条目的集合。列(也称为字段)包含列名及其属性。行也称为记录，包含每列的数据。

![](img/e27f1adcc1d8f9f14e810403df4b5c01.png)

照片由[奥斯丁·迪斯特尔](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 例子

让我们用一个简单的例子来完成最常用的 SQL 查询。假设有一家公司通过类似于脸书、Reddit、Pinterest 等的应用程序向用户提供内容。

当用户登录该应用程序时，该公司会向用户提供内容。这记录在*内容插入*表中。该表存储了向谁提供了什么内容以及时间戳。

我们还有一个名为 *user_content_activity* 的表，当用户对内容进行操作时，它会存储所有数据。它包括用户 id、内容 id、操作类型和用户执行操作的时间戳。

最后，我们有两个表给出了用户和内容的属性，分别命名为*用户*和*内容*。 *users* 表存储用户居住的国家和用户加入(或注册)应用程序的日期。*内容*表存储内容类型。

表名:*用户 _ 内容 _ 活动*

```
+--------+-----------+--------+---------------------+
| userid | contentid | action |      timestamp      |
+--------+-----------+--------+---------------------+
|1       | 5         | view   | 2021-01-05 10:30:20 |
|1       | 5         | click  | 2021-01-05 10:30:55 |
|2       | 21        | view   | 2021-01-06 03:12:25 |
|3       | 100       | view   | 2021–02–04 06:25:12 |
+--------+-----------+--------+---------------------+
```

表名:*用户*

```
+--------+---------------+------------+
| userid |    country    |  join_date |
+--------+---------------+------------+
|1       | United States | 2018-05-06 |
|2       | Italy         | 2019-12-31 |
|3       | Japan         | 2020-03-05 |
|4       | United States | 2021–01-26 |
+--------+---------------+------------+
```

表格名称:*内容*

```
+-----------+-----------+
| contentid |   type    |
+-----------+-----------+
|1          | video     |
|2          | photo     |
|3          | text      |
|4          | photo     |
+-----------+-----------+
```

## **基本形式**

```
***SELECT {column1, column2, …} 
FROM {tablename}***
```

这是从数据库中检索信息的最基本形式。编写查询时，要问的第一个问题是“哪个(哪些)表有我需要的信息？”然后，“哪一列有适当的信息？”举个例子，

*   *问:给我用户来自的国家列表* >从用户中选择国家
*   *问:给我所有的用户、内容和用户采取的行动*
    >从用户内容活动中选择用户标识、内容标识、行动
*   *问:给我 content_insertion 表中的所有数据*
    >从 content_insertion 中选择 userid、contentid、时间戳
    >从 content_insertion 中选择*

符号 ***** 是从表中检索所有列而不是列出所有列的简单方法。当您想要查看表中的一些数据时，这特别有用。为此，我们建议使用***SELECT * FROM content _ insertion LIMIT 100***，这将只给出表格的前 100 行。“LIMIT”总是出现在查询的最后。****

## ******添加条件******

```
****SELECT {column1, column2, …} 
FROM {tablename} 
WHERE {conditional statements}****
```

****当我们想要给我们正在检索的数据添加一些条件时，我们添加“WHERE”子句。在许多情况下，我们将需要使用众所周知的数学符号=，！=，>， =，<= or logics such as AND, OR, NOT, as well as many others. We’ll go through the most common ones in the following examples.****

*   *****问:给我美国的用户列表*
    >从国家=‘美国’的用户中选择 userid****

****请注意，我们在问题中使用了“美国”而不是“我们”。这是因为当我们查看该表时，US 被存储为 United States。一个很好的澄清问题应该是“美国是存储为‘US’，‘美国’，还是两者都是？”****

****我们使用符号“=”来表示与右边的符号完全匹配的国家。如果数据存储为“美国”,则不会检索到该行，因为字符串不完全匹配。如果我们想同时检索两者，那么使用子句*就可以解决问题(阅读更多关于[通配符](https://www.w3schools.com/sql/sql_wildcards.asp))。*****

*   *****问:给我 2018 年加入的用户的唯一国家列表*
    >从 join_date 在‘2018–01–01’和‘2018–12–31’之间的用户中选择不同的 userid*或*
    >从 join _ date>= ' 2018–01–01 '和 join _ date<= ' 2018–12–31 '的用户中选择不同的 userid****

******DISTINCT** 是检索列的唯一值的有用语法。****

****[日期函数](https://prestodb.io/docs/current/functions/datetime.html)有很多，但要知道最重要的是 CURRENT_DATE、DATE_ADD、DATE_SUB、DATE_DIFF、DATE_FORMAT。例如，
> SELECT userid FROM users，其中 join _ DATE BETWEEN CURRENT_DATE AND DATE _ SUB(' day '，-6，CURRENT _ DATE)给出一周前到今天之间加入的用户。****

*   *****问:给我照片和视频形式的内容列表*
    >从输入的内容中选择 contentid 照片’，‘视频’)****

****当我们希望用逻辑语句“或”匹配多个字符串时，使用中的**或**中的【非 。以上查询同
>从内容中选择内容 id WHERE(type = ' photo '或 type = 'video)
>从内容中选择内容 WHERE type！= 'text'
>从输入不在(' text ')的内容中选择 contentid********

## ****聚合函数****

**通常我们希望找到一个变量的集合，比如 sum、average、counts、min、max。使用它的一般语法是**

```
**SELECT column1, SUM(column2), COUNT(column3) 
FROM {tablename} WHERE {conditional statements} 
GROUP BY column1**
```

**请注意，我们可以有一个或多个聚合函数，如果我们有任何条件语句，它们总是在 FROM 之后和 GROUP BY 之前。**

*   ***问:每个国家有多少用户？*
    >选择国家，COUNT(userid)从用户组按国家**

**重命名聚合结果通常是一个好习惯。别名用于为表或表中的列提供临时名称。例如，使用*COUNT(userid)****作为*** *number_users***

**如果我们认为有重复的行，我们可以通过
*选择国家，将(不同的用户标识)作为国家用户组中的 number_users 进行计数***

*   ***问:2020–01–03*
    >点击次数最多的 contentid 是什么？SELECT contentid，COUNT(*)AS view FROM user _ content _ activity WHERE action = ' click ' AND CAST(timestamp AS DATE)= ' 2020–01–03 ' GROUP BY contentid
    ORDER BY COUNT(*)desc 限制 1**

****ORDER BY** 用于按升序(默认)或降序(通过指定 DESC)对结果集进行排序**

*   ***问:超过 100 个视图的内容有哪些*
    >选择 contentid，COUNT(*)作为来自 user_content_activity 的视图，其中 action =‘view’GROUP BY contentid HAVING COUNT(*)>100**

****当您根据聚合结果提取信息时，使用**是一个很好的方法。它总是在 GROUP BY 语句之后。**

**到目前为止，我们已经学习了使用 SQL 检索数据的基本格式。在第 2 部分中，我们将讨论更多利用关系数据库的有趣内容。**