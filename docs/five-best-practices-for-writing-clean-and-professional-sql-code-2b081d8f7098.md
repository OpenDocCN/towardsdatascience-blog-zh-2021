# 编写干净专业的 SQL 代码的五个最佳实践

> 原文：<https://towardsdatascience.com/five-best-practices-for-writing-clean-and-professional-sql-code-2b081d8f7098?source=collection_archive---------1----------------------->

## 用这五个技巧提升你的 SQL 代码！

![](img/2e914d6195dc6f6d1be34dfff391a8eb.png)

照片由[在](https://unsplash.com/@thecreative_exchange?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的创意交流

# 介绍

SQL 是数据世界中的通用语言。如果你是数据分析师、数据科学家、数据工程师、数据架构师等。，你需要写好**的** SQL 代码。

学习如何编写基本的 SQL 查询并不难，但是学习如何编写好的 SQL 代码却是另一回事。编写好的 SQL 代码并不困难，但是需要学习一些规则。

如果您精通 Python 或另一种编码语言，这些规则中的一些可能对您来说很熟悉，这是因为它们非常具有可移植性！

因此，**我将与您分享编写更干净、更好的 SQL 查询的五个技巧**。让我们深入了解一下:

# 1.程序设计式样

如果你认为你的编程风格对于 SQL 来说是微不足道的，请再想想。您编码的风格对于可解释性和最小化错误是至关重要的。

让我给你一个简单的例子——下面两个代码块中哪一个更易读？

```
SELECT name,height,weight,age,gender,CASE WHEN age<18 THEN "child" ELSE "adult" END AS childOrAdult,salary
FROM People LEFT JOIN Income USING (name)
WHERE height<=200 and age<=65
```

或者

```
SELECT name
       , height
       , weight
       , age
       , gender
       , CASE WHEN age < 18 THEN "child"
              ELSE "adult"
         END AS childOrAdult
       , salary
FROM People
LEFT JOIN Income USING (name)
WHERE height <= 200
      AND age <= 65
```

很明显，第二个更容易阅读，这完全归功于它的编程风格！编程风格有几个组成部分，但我们将重点放在缩进和空格上。

## 缩进/对齐

第二个代码块更易读的主要原因是因为它的缩进和垂直对齐。请注意 SELECT 子句中的所有列名是如何对齐的，WHERE 子句中的条件是如何对齐的，CASE 语句中的条件是如何对齐的。

你不一定要完全遵循这种方式，但是你应该尽可能地使用缩进并对齐你的语句。

## 间隔

空格是指您在代码中使用的空白。例如，代替…

```
WHERE height<=200 AND age<=65
```

考虑使用空白以使其更加易读:

```
WHERE height <= 200 AND age <= 65
```

**最重要的是，你要确保你的编程风格在你的代码中是一致的。**还有其他需要考虑的事情，比如命名约定和注释，我们将在本文后面讨论。

# 2.通过公共表表达式模块化代码

使用公共表表达式(cte)是模块化和分解代码的好方法，就像你将一篇文章分解成几个段落一样。

如果您想更深入地了解 cte，可以查看本文,但是如果您曾经想要查询一个查询，这就是 cte 发挥作用的时候——cte 本质上创建一个临时表。

考虑以下在 where 子句中带有子查询的查询。

```
SELECT name
       , salary
FROM People
WHERE name in (SELECT DISTINCT name 
                   FROM population 
                   WHERE country = "Canada"
                         AND city = "Toronto")
      AND salary >= (SELECT AVG(salary)
                     FROM salaries
                     WHERE gender = "Female")
```

这似乎不难理解，但是如果在子查询中有许多子查询呢？这就是 cte 发挥作用的地方。

```
with toronto_ppl as (
   SELECT DISTINCT name
   FROM population
   WHERE country = "Canada"
         AND city = "Toronto"
), avg_female_salary as (
   SELECT AVG(salary) as avgSalary
   FROM salaries
   WHERE gender = "Female"
)SELECT name
       , salary
FROM People
WHERE name in (SELECT DISTINCT FROM toronto_ppl)
      AND salary >= (SELECT avgSalary FROM avg_female_salary)
```

现在很明显，WHERE 子句正在过滤多伦多的名称。如果你注意到了，CTE 是有用的，因为你可以把你的代码分解成更小的块，但是它们也是有用的，因为它允许你给每个 CTE 分配一个变量名(例如 toronto_ppl 和 avg_female_salary)

*注意:编程风格也适用于 cte！*

说到命名约定，这引出了我的下一个观点:

# 3.变量命名约定

命名约定有两部分需要考虑:使用的字母大小写类型和变量的描述性。

## 字母大小写

我个人喜欢用 snake_case 命名 cte，用 camelCase 命名列名。你可以选择你喜欢的方式来设计你的变量，但是要确保你是一致的。

这里有一个例子:

```
with short_people as (
   SELECT firstName
   FROM people
   WHERE height < 165
)SELECT * FROM short_people
```

注意我是如何用 snake_case 表示 CTE (short_people ),用 camelCase 表示名字的。

## 描述性名称

理想情况下，您希望您的变量名描述它们所代表的内容。考虑我之前的例子:

```
with toronto_ppl as (
   SELECT DISTINCT name
   FROM population
   WHERE country = "Canada"
         AND city = "Toronto"
), avg_female_salary as (
   SELECT AVG(salary) as avgSalary
   FROM salaries
   WHERE gender = "Female"
)SELECT name
       , salary
FROM People
WHERE name in (SELECT DISTINCT FROM toronto_ppl)
      AND salary >= (SELECT avgSalary FROM avg_female_salary)
```

很明显，第一个 CTE 在询问来自多伦多的人，第二个 CTE 在拿女性的平均工资。这是一个糟糕的命名约定的例子:

```
with table1 as (
   SELECT DISTINCT name
   FROM population
   WHERE country = "Canada"
         AND city = "Toronto"
), table2 as (
   SELECT AVG(salary) as var1
   FROM salaries
   WHERE gender = "Female"
)SELECT name
       , salary
FROM People
WHERE name in (SELECT DISTINCT FROM table1)
      AND salary >= (SELECT var1 FROM table2)
```

如果只阅读最后一个查询，很难理解它在做什么。因此，请确保您的大小写一致，并且您的变量名是描述性的！

# 4.使用临时函数简化代码

如果你想了解更多关于临时函数的内容，[看看这个](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions)，但是临时函数也是分解代码、编写更简洁的代码以及能够重用代码的好方法。

考虑下面的例子:

```
SELECT name
       , CASE WHEN tenure < 1 THEN "analyst"
              WHEN tenure BETWEEN 1 and 3 THEN "associate"
              WHEN tenure BETWEEN 3 and 5 THEN "senior"
              WHEN tenure > 5 THEN "vp"
              ELSE "n/a"
         END AS seniority 
FROM employees 
```

相反，您可以利用一个临时函数来捕获 CASE 子句。

```
CREATE TEMPORARY FUNCTION seniority(tenure INT64) AS (
   CASE WHEN tenure < 1 THEN "analyst"
        WHEN tenure BETWEEN 1 and 3 THEN "associate"
        WHEN tenure BETWEEN 3 and 5 THEN "senior"
        WHEN tenure > 5 THEN "vp"
        ELSE "n/a"
   END
);SELECT name
       , seniority(tenure) as seniority
FROM employees
```

有了临时函数，查询本身就简单多了，可读性更好，还可以重用资历函数！

# 5.写有用的评论

重要的是只在需要的时候写评论。通过使用描述性的名称、编写模块化的代码以及拥有干净的编程风格，您应该不需要编写太多的注释。

也就是说，当代码本身不能解释你想要达到的目的时，注释是有用的。评论通常会回答你为什么做某事，而不是你在做什么。

下面是一个差评的例子:

```
# Getting names of people in Toronto, Canada
with table1 as (
   SELECT DISTINCT name
   FROM population
   WHERE country = "Canada"
         AND city = "Toronto"
)# Getting the average salary of females
, table2 as (
   SELECT AVG(salary) as var1
   FROM salaries
   WHERE gender = "Female"
)
```

这些是糟糕的注释，因为它告诉我们通过阅读代码本身我们已经知道了什么。记住，评论通常会回答你为什么做某事，而不是你在做什么。

# 感谢阅读！

如果你坚持到了最后，我希望你学到了一些东西。正如我之前所说，学习如何编写好的 SQL 代码对于各种数据职业来说都是必不可少的，所以请确保您花时间学习如何编写好的代码！

如果你喜欢这个，请一定要关注我的未来内容，一如既往，我祝你学习一切顺利！

**不确定接下来要读什么？我为你挑选了另一篇文章:**

</how-to-write-all-of-your-sql-queries-in-pandas-449dd8b2c94e>  

**又一个！**

</a-complete-52-week-curriculum-to-become-a-data-scientist-in-2021-2b5fc77bd160>  

## 特伦斯·申

*   ***如果你喜欢这个，*** [***跟我上媒***](https://medium.com/@terenceshin) ***了解更多***
*   ***对通敌感兴趣？让我们连线上***[***LinkedIn***](https://www.linkedin.com/in/terenceshin/)
*   ***报名我的邮箱列表*** [***这里***](https://forms.gle/tprRyQxDC5UjhXpN6) ***！***