# 面向数据科学家的 SQL 面试初学者指南

> 原文：<https://towardsdatascience.com/the-beginners-guide-to-acing-sql-interviews-for-data-scientists-30317d6692ec?source=collection_archive---------11----------------------->

## [地面零点](https://pedram-ataee.medium.com/list/ground-zero-e79c47975d14)

## 通过基础知识、示例和日常工作中需要的实用技术的结合

![](img/0d78818689f0460a57f00f0ec094548a.png)

由 [NOAA](https://unsplash.com/@noaa?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在大多数数据科学面试中，你的 SQL 技能会以某种方式得到检验。你必须在你的技术面试中写一些 SQL 查询，并且在你的日常工作中写很多。不管你是在小公司还是大公司工作，SQL 编程都是你工具箱中的重要工具。您可以使用 SQL 编程来处理小型和大型数据集，尤其是 Spark SQL 等高级查询引擎。

[Spark](https://spark.apache.org/) (一个用于大规模数据处理的开源统一分析引擎)有一个名为 **Spark SQL(一个分布式 SQL 查询引擎)**的模块，可以让你高效地管理大数据中的 SQL 查询。 [Databricks](https://databricks.com/) 是一款增长最快的管理数据仓库的企业软件，它提供了一个用户友好的环境来设置 Spark SQL 引擎。所以，在你的职业生涯中，要做好学习 SQL 编程的准备。

一次数据科学面试通常有三个技术步骤: **SQL 编程**、 **ML 开发**、**一次现场技术面试**。如今，SQL 编程和 ML 开发步骤非常标准。不同公司的现场面试各不相同。在这篇文章中，我的目标是帮助你赢得 SQL 编程面试。

如果你觉得这篇文章有用，可以通过下面的链接订阅。这将有助于我为你创造高质量的内容。

<https://pedram-ataee.medium.com/membership>  

这篇文章的结构如下:

*   [SQL 编码的基础是什么？](/the-beginners-guide-to-acing-sql-interviews-for-data-scientists-30317d6692ec#9fb1)
*   一个简单的 SQL 问题的例子
*   [一个有用的技巧:“参数化查询”](/the-beginners-guide-to-acing-sql-interviews-for-data-scientists-30317d6692ec#a072)
*   [遗言](/the-beginners-guide-to-acing-sql-interviews-for-data-scientists-30317d6692ec#6cce)

我们开始吧。

# SQL 编码的基础是什么？

您应该从任何您熟悉的资源中学习 SQL 编程的基础知识。从 [w3school](https://www.w3schools.com/sql/default.asp) 网站学到了很多，强烈推荐以后去看看。

## 1.挑选

`SELECT`语句用于**从数据库**中提取数据，如下`SELECT column1_name FROM table_name`。如果您想提取数据库中的所有内容，也可以在`SELECT`后使用`*`。`SELECT DISTINCT`语句可以用来提取唯一的值。一个列通常包含许多重复的值，有时您只需要提取唯一的或不同的值。

## 2.在哪里

`WHERE`语句用于**从数据库中过滤记录**,如下所示`SELECT column1_name FROM table_name WHERE condition;`,它只返回那些满足在此之后写入的条件的记录。您可以在条件中使用各种运算符，例如:`>`、`=`、`IN`、`LIKE`或`BETWEEN`。您还可以使用`AND`、`OR`和`NOT`运算符组合多个条件，以缩小过滤器的范围。

## 3.以...排序

`ORDER BY`语句用于**将结果**按升序`ASC`或降序`DESC`排序。默认情况下，它按升序对记录进行排序。您可以尝试使用下面的代码来提取按`column1_name`和`column2_name`排序的表中的所有内容:`SELECT * FROM table_name ORDER BY column1_name ASC, columne2_name DESC`

## 4.数数

`COUNT()`函数**返回符合指定标准**的行数，如下:`SELECT COUNT(column_name) FROM table_name WHERE condition;`您可以尝试其他 SQL 函数，而不是`COUNT`，例如`AVG`或`SUM`来返回一个数值列的平均值或总和。

## 5.分组依据

`GROUP BY`语句**提取一列中具有相同值的行，并将它们分组为汇总行**，比如“查找每个学校的学生人数”。`GROUP BY`语句通常与`COUNT()`、`MAX()`、`MIN()`、`SUM()`、`AVG()`等聚合函数一起使用，对结果进行分组。你可以查看下面的代码。

```
SELECT *column_name(s)*
FROM *table_name*
WHERE *condition*
GROUP BY *column_name(s)* ORDER BY *column_name(s);*
```

**还有更多 SQL 命令**可以从 [w3school](https://www.w3schools.com/sql/default.asp) 比如`JOIN`或者`CASE`中学习。`JOIN`子句用于根据两个或多个表之间的相关列来组合它们的行。或者，`CASE`语句经过几个条件，当第一个条件满足时返回值，与其他编程语言中的`if-then-else`语句完全相似。

![](img/65ddfef58af8e9a02f662ba680301c42.png)

照片由[Olav Ahrens rtne](https://unsplash.com/@olav_ahrens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# —一个简单的 SQL 问题示例

下面的问题摘自 [Leetcode](https://leetcode.com/problemset/database/) ，这是一个很棒的网站，在这里你可以练习你的编码技能。你可以在那个网站上找到更多的问题。

**问题**:我们有一个名为`Activity`的表，包含一系列游戏玩家的 4 个字段:`palyer_id`、`device_id`、 `event_date`和 `games_played`。每一行都是一个玩家的记录，他在某一天使用相同的设备登录并玩了许多游戏(可能是 0 个游戏)。编写一个 SQL 查询，报告每个玩家的首次登录日期。

**解决方案:**如您所见，该解决方案使用了您在上面学到的命令。

```
SELECT 
    player_id
    MIN(event_date) AS first_login
FROM Activity
GROUP BY player_id
```

![](img/b29e9ea4801ca9b9af58733fcea4aee1.png)

Anton Maksimov juvnsky 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# —一种有用的技术:“参数化查询”

当你必须编写**大量类似的查询**时，你可以使用一种叫做“参数化查询”的技术。这将有助于你写出更高效的代码，也能展示你在编程方面的专长。在这里，我描述了如何使用 Python 编写参数化的 SQL 查询。

假设您想要编写一个可以用于不同字段和表的`COUNT`查询。在这种情况下，您可以将第 1 部分保存在下面的代码中，例如保存在`count.txt`下。然后，您必须使用第 2 部分解析(加载和填充)Python 中的参数化查询，代码紧随其后。

```
------------------------------------------------------
# PART 1 (SQL) - PARAMETRIZED QUERY saved as count.txt
------------------------------------------------------
SELECT {var}, count({var}) as count
FROM {table_name}
GROUP BY {var}
ORDER BY count DESC---------------------------------
# PART 2 (PYTHON) - QUERY PARSER 
---------------------------------
class Query:
    def __init__(self, table_name, var):
        self.table_name = table_name
        self.var = var
        self.parsed = None def parser(self):
        file_path_query = os.path.join(FILE_DIR, 'count.txt')
        with open(file_path_query, 'r') as file:
            template = file.read().replace('\n', ' ')
        self.parsed = template.format(table_name=self.table_name,
                                      var=self.var)
```

</how-to-write-a-great-resume-as-a-data-scientist-for-professionals-98359ab19a6e>  

# —遗言

SQL 编程面试可以现场，也可以离线。如果它是脱机的，将根据您的代码是否可执行来评估您。否则，将根据代码的总体质量和你的沟通能力对你进行评估。让我们深入了解每一个问题。

## 代码质量

面试官会评判你的代码质量(当然不是你！)基于几个角度。首先，他们想看看代码是否是没有任何语法错误的可执行代码。然后，他们检查你的代码是否**干净简洁**。第三，面试官会检查你是否考虑过边缘案例。如果你不能写干净的代码或者在时间盒中考虑边缘案例，确保知道**最佳实践**并与你的面试官分享。最后，他们会检查你的解决方案在效率方面是否**优化**。例如，使用一个连接或三个连接可以得到相同的结果。前者更优化。

## 沟通

在每一次面试中，与面试官进行有效的交流是很重要的。然而，有效的沟通意味着什么？有效沟通的第一步是在开始编码之前，清楚地与面试官分享你的思维过程。另外，你必须能够**为你的每一个选择阐述你的推理**。我再怎么强调分享思维过程和阐述推理的能力有多重要也不为过。我可以和在编码面试中代码不可执行的人一起工作，但不能和沟通失败的人一起工作。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我的* [*推特*](https://twitter.com/pedram_ataee) *！*

<https://pedram-ataee.medium.com/membership> 