# 你绝对应该知道的 7 个 SQL 功能

> 原文：<https://towardsdatascience.com/7-sql-functionalities-that-you-should-definitely-know-2b3fd9174608?source=collection_archive---------16----------------------->

## [入门](https://towardsdatascience.com/tagged/getting-started)

## 更好地使用 SQL 将会节省您的时间并减少您的挫败感

![](img/5b8a9c7c64728156c6e13adc16b996c0.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

SQL 是许多数据科学家的一项重要技能。SQL(结构化查询语言)是一种非常灵活的语言，读起来很像普通英语。它允许轻松访问数据库中最复杂的表结构。毕竟，如果不能访问数据，数据有什么用？市场上的许多工作都需要 SQL 知识，所以至少学习一些基础知识绝对是一个明智的想法。作为一个曾经管理过不使用 SQL 的数据库的人，我可以自信地说，如果你有一份使用 SQL 的工作，你一定会很感激。

在本文中，我想与您分享 SQL 的一些功能，以帮助您更好地进行搜索，从而成为查询高手。

# 1.格条款

`CASE`子句允许您在查询中创建条件情况，并将它们作为列返回。

```
SELECT employee_name, salary,
CASE
    WHEN salary >= 100000 THEN 'Senior'
    WHEN salary < 100000 AND salary >= 50000 THEN 'Midlevel'
    WHEN salary < 50000 THEN 'Entrylevel' END
FROM employees;
```

该查询将返回雇员姓名和薪金列，而`CASE`子句将是一个新列，根据薪金条件语句所满足的条件，包含高级、中级或入门级的值。注意`CASE`子句的语法。您可以使用`WHEN`和`THEN`添加任意多的条件语句，并在开始时将它们全部夹在`CASE`中，当您完成条件语句时，将它们夹在`END`中。

# 2.聚合函数

您可以在查询中执行许多非常有用的功能。它们是非常直接的函数，通常只需要意识到它们的存在。所以要知道，像`MIN()`、`MAX()`、`SUM()`、`COUNT()`和`AVG()`这样的东西会让你的查询更有效率。请注意，您总是需要将它们与 GROUP BY 子句配对。

```
SELECT region, COUNT(employee_name)
FROM employees
GROUP BY region;
```

该查询将返回地区列表以及每个地区的雇员人数。如前所述，您必须将 count 函数与一个`GROUP BY`子句配对，以告诉数据库如何对计数进行分组。

```
SELECT department, SUM(salary)
FROM employees
GROUP BY department;
```

该查询将返回部门列表，其中包含该部门所有员工的工资总额。同样，您必须使用一个`GROUP BY`子句来告诉数据库按部门对合计工资进行分组。

# 3.WHERE 和 HAVING 子句

`WHERE`和`HAVING`子句彼此相似，因为它们都是基于标准进行过滤的简单方法。需要记住的两者之间的最大区别是，`HAVING`将基于聚合数据列进行过滤，而`WHERE`不会。

```
SELECT employee_name, department
FROM employees
WHERE salary > 50000;
```

该查询将只返回符合`WHERE`语句标准的雇员的所有姓名和部门。也就是说，只有工资高于$50，000 的雇员才会被返回。

```
SELECT department, SUM(salary)
FROM employees
GROUP BY department
HAVING SUM(salary) > 1000000;
```

这个查询与上面的查询相同，但是现在返回的部门是那些工资总额大于 1，000，000 美元的部门。注意在这个查询中我们如何使用了一个`HAVING`语句，因为它是在一个聚合列上过滤的。

# 4.串联运算符

这是将列合并成一列的一个方便的小技巧。例如，假设您有一个表，其中包含两个不同列中的雇员姓名，即名字和姓氏。也许您希望创建一个查询，返回同一列中的姓名，并显示全名。串联运算符将为您完成这项工作。它被表示为两个相邻的管道，如 so `||`。

```
SELECT first_name || ' ' || last_name
FROM employees;
```

该查询将返回一列，该列的名字和姓氏用空格隔开。你可以按照你希望名字出现的方式写这一行。如果您希望姓氏出现在最前面，您可以将该行写成`last_name || ', '|| first_name`。这将首先显示姓氏列中的信息，然后是逗号和空格，最后是名字信息。

# 5.子查询

许多希望从两个表中获取信息的人通常会立即转向连接。但是子查询也是组合来自两个来源的信息的有效方法，您可能会发现在某些情况下它们更容易。子查询可以用在几乎任何子句中(包括`SELECT`、`FROM`、`WHERE`等)。).

```
SELECT *
FROM employees
WHERE department IN (SELECT department 
                     FROM departments
                     WHERE division = 'Technology');
```

这个查询乍一看可能很奇怪，但是让我们一次一部分来看。括号中的子查询返回属于技术部门的部门列表。因此，您可以将括号中的内容视为部门名称列表。主查询选择条目的所有列，这些条目的部门至少与子查询返回的列表中的一个部门名称匹配。

我发现保持子查询的新鲜感有助于我有更多的解决方案来处理棘手的情况。连接很好，但知道如何用多种方法解谜更有用。

# 6.LIKE 运算符

通常情况下，数据库中的信息必须精确搜索，并且区分大小写。也许您有一些条目的大写有所不同，或者有轻微的拼写差异。在这里,`LIKE`操作符将是你最好的朋友，因为它是你搜索时不需要精确拼写的方式。

```
SELECT department
FROM departments
WHERE department LIKE '%omputer%';
```

这将返回包含字符串“omputer”的所有部门。因此，如果您将一些部门输入为“计算机”、“计算机”或“计算机”，所有这些条目都将被返回。

# 7.美化你的结果

最后一个是一些相关技巧的集合，可以让你的结果以简洁易读的格式显示。

**注意列的排列顺序。**如果您希望某列首先出现，那么在查询中首先列出它。像`SELECT last_name, first_name, department, salary`这样的语句将按这个顺序显示列。

**利用别名重命名列。**别名在使用聚合函数时特别有用。让我们看看上面的一个查询，看看我们如何使用别名来重命名列。

```
SELECT region, COUNT(employee_name) AS number_of_employees
FROM employees
GROUP BY region;
```

计数列现在将返回标题为“number_of_employees ”,这增加了可读性。

**使用一个** `**ORDER BY**` **语句。**让我们再次使用相同的查询，看看如何使用`ORDER BY`来改进结果。

```
SELECT region, COUNT(employee_name) AS number_of_employees
FROM employees
GROUP BY region
ORDER BY number_of_employees DESC;
```

默认的排序方法是升序，所以如果你想首先显示最大的结果，你可以像例子中那样添加`DESC`。这将极大地提高查询结果的可读性，因为我们现在可以在列表的顶部看到雇员人数最多的地区。

**使用一个** `**LIMIT**` **语句。** `LIMIT`就像它听起来那样限制返回结果的数量。同样，我们可以使用相同的查询来看看这是如何工作的。

```
SELECT region, COUNT(employee_name) AS number_of_employees
FROM employees
GROUP BY region
ORDER BY number_of_employees DESC
LIMIT 5;
```

现在将只返回前 5 个结果，这将是拥有最多员工的前 5 个地区。

# 结论

有了这么多的功能，学习 SQL 会很有趣。我喜欢编写 SQL 查询，因为每个查询都像是一个谜题。我希望这篇文章能帮助您认识到更多可用的功能，并为您的下一个查询找到一些有用的东西！