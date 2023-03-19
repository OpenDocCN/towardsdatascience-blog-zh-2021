# 编写可读和可维护的 SQL 代码的 10 个最佳实践

> 原文：<https://towardsdatascience.com/10-best-practices-to-write-readable-and-maintainable-sql-code-427f6bb98208?source=collection_archive---------0----------------------->

## 如何编写你的团队可以轻松阅读和维护的 SQL 查询？

没有适当的指导方针，很容易弄乱 SQL。由于团队中的每个人可能都有自己编写 SQL 的习惯，您很快就会得到一个没有人理解的令人困惑的代码。

您可能已经意识到遵循一系列良好实践的重要性。
愿这篇文章能给你带来你正在寻找的指导！

![](img/aca34ac61f204596d8551abf92697d10.png)

[@liane](https://unsplash.com/@liane) 在 [Unsplash](https://unsplash.com/) 上的照片

## 1.使用大写字母作为关键字

让我们从一个基本的开始:对 SQL 关键字[使用大写字母](https://www.w3schools.com/sql/sql_ref_keywords.asp)，对表和列使用小写字母。在 SQL 函数(FIRST_VALUE()、DATE_TRUNC()等)中使用大写字母也是一个很好的实践，尽管这更有争议。

> **避免**

```
select id, name from company.customers
```

> **偏好**

```
**SELECT** id, name **FROM** company.customers
```

## 2.对模式、表、列使用 Snake Case

编程语言在案例类型方面有自己的最佳实践:camelCase、PascalCase、kebab-case 和 snake_case 是最常见的。

谈到 SQL，Snake Case(有时也称为下划线 Case)是使用最广泛的约定。

> **避免**

```
**SELECT** Customers.id, 
       Customers.name, 
       COUNT(WebVisit.id) as nbVisit
**FROM** COMPANY.Customers
**JOIN** COMPANY.WebVisit **ON** Customers.id = WebVisit.customerId**WHERE** Customers.age <= 30
**GROUP BY** Customers.id, Customers.name
```

> **更喜欢**

```
**SELECT** customers.id, 
       customers.name, 
       COUNT(web_visit.id) as nb_visit
**FROM** company.customers
**JOIN** company.web_visit **ON** customers.id = web_visit.customer_id**WHERE** customers.age <= 30
**GROUP BY** customers.id, customers.name
```

尽管有些人喜欢使用变体来区分模式、表和列，但我建议坚持使用 snake case。

## 3.使用别名可以提高可读性

众所周知，别名是重命名没有意义的表或列的一种便捷方式。当表和列的名称没有意义时，不要犹豫给它们起别名，也不要犹豫给聚合起别名。

> **避免**

```
**SELECT** customers.id, 
       customers.name, 
       customers.context_col1,
       nested.f0_
**FROM** company.customers **JOIN** (
          **SELECT** customer_id,
                 MIN(date)
          **FROM** company.purchases
          **GROUP BY** customer_id
      ) **ON** customer_id = customers.id
```

> **更喜欢**

```
**SELECT** customers.id, 
       customers.name, 
       customers.context_col1 **as ip_address**,
       first_purchase.date    **as first_purchase_date**
**FROM** company.customers **JOIN** (
          **SELECT** customer_id,
                 MIN(date) **as date**
          **FROM** company.purchases
          **GROUP BY** customer_id
      ) **AS first_purchase** 
        **ON** first_purchase.customer_id = customers.id
```

我通常用小写字母`as`作为列的别名，用大写字母`AS`作为表的别名。

## 4.格式:小心使用缩进和空格

尽管这是一个基本原则，但它能让你的代码更具可读性。就像使用 python 一样，您应该标识您的 SQL 代码。

当使用子查询或派生表时，在关键字后使用 Ident。

> **避免**

```
SELECT customers.id, customers.name, customers.age, customers.gender, customers.salary, first_purchase.date
FROM company.customers
LEFT JOIN ( SELECT customer_id, MIN(date) as date FROM company.purchases GROUP BY customer_id ) AS first_purchase 
ON first_purchase.customer_id = customers.id 
WHERE customers.age<=30
```

> **更喜欢**

```
**SELECT** customers.id, 
       customers.name, 
       customers.age, 
       customers.gender, 
       customers.salary,
       first_purchase.date
**FROM** company.customers
**LEFT JOIN** (
              **SELECT** customer_id,
                     MIN(date) as date 
              **FROM** company.purchases
              **GROUP BY** customer_id
          ) **AS** first_purchase 
            **ON** first_purchase.customer_id = customers.id
**WHERE** customers.age <= 30
```

另外，请注意我们如何在 where 子句中使用空格。

> **避免**

```
SELECT id WHERE customers.age<=30
```

> **偏爱**

```
SELECT id WHERE customers.age <= 30
```

## 5.避免选择*

不值得提醒这种好的做法。你应该明确你想要选择什么，因此避免使用`Select *`。

让你的请求不清楚，因为它隐藏了询问背后的意图。另外，请记住，您的表可能会发生变化并影响`Select *`。这就是为什么我不是一个大风扇的`EXCEPT()`指令。

> **避开**

```
SELECT * EXCEPT(id) FROM company.customers
```

> **更喜欢**

```
**SELECT** name,
       age,
       salary
**FROM** company.customers
```

## 6.使用 ANSI-92 连接语法

…而不是用于连接表的 SQL WHERE 子句。
尽管您可以使用 WHERE 子句和 JOIN 子句来连接表，但是最好使用 JOIN / ANSI-92 语法。

虽然在性能方面没有区别，但是 JOIN 子句将关系逻辑与过滤器分开，提高了可读性。

> **避免**

```
**SELECT** customers.id, 
       customers.name, 
       COUNT(transactions.id) as nb_transaction
**FROM** company.customers, company.transactions
**WHERE** customers.id = transactions.customer_id
      **AND** customers.age <= 30
**GROUP BY** customers.id, customers.name
```

> **偏好**

```
**SELECT** customers.id, 
       customers.name, 
       COUNT(transactions.id) as nb_transaction
**FROM** company.customers
**JOIN** company.transactions **ON** customers.id = transactions.customer_id**WHERE** customers.age <= 30
**GROUP BY** customers.id, customers.name
```

“基于 Where 子句”的语法—也称为 ANSI-89 —比新的 ANSI-92 要老，这就是为什么它仍然非常普遍。如今，大多数开发人员和数据分析师都使用 JOIN 语法。

## 7.使用公用表表达式(CTE)

CTE 允许您定义和执行查询，其结果是临时存在的，可以在更大的查询中使用。cte 可以在大多数现代数据库中找到。

它像派生表一样工作，有两个优点:

*   使用 CTE 可以提高查询的可读性
*   CTE 被定义一次，然后可以被多次引用

你用指令**用…声明一个 CTE 为**:

```
**WITH** my_cte **AS**
(
  SELECT col1, col2 FROM table
)
SELECT * FROM my_cte
```

> **避免**

```
**SELECT** customers.id, 
       customers.name, 
       customers.age, 
       customers.gender, 
       customers.salary,
       persona_salary.avg_salary as persona_avg_salary,
       first_purchase.date
**FROM** company.customers **JOIN** (
          **SELECT** customer_id,
                 MIN(date) as date 
          **FROM** company.purchases
          **GROUP BY** customer_id
      ) **AS** first_purchase 
        **ON** first_purchase.customer_id = customers.id
**JOIN** (
          **SELECT** age,
             gender,
             AVG(salary) as avg_salary
         **FROM** company.customers
         **GROUP BY** age, gender
      ) **AS** persona_salary 
        **ON** persona_salary.age = customers.age
           **AND** persona_salary.gender = customers.gender
**WHERE** customers.age <= 30
```

> **更喜欢**

```
**WITH** first_purchase **AS**
(
   **SELECT** customer_id,
          MIN(date) as date 
   **FROM** company.purchases
   **GROUP BY** customer_id
),persona_salary **AS**
(
   **SELECT** age,
          gender,
          AVG(salary) as avg_salary
   **FROM** company.customers
   **GROUP BY** age, gender
)**SELECT** customers.id, 
       customers.name, 
       customers.age, 
       customers.gender, 
       customers.salary,
       persona_salary.avg_salary as persona_avg_salary,
       first_purchase.date
**FROM** company.customers **JOIN** first_purchase **ON** first_purchase.customer_id = customers.id
**JOIN** persona_salary **ON** persona_salary.age = customers.age
                       **AND** persona_salary.gender = customers.gender
**WHERE** customers.age <= 30
```

## 8.有时，将查询分成多个可能是值得的

小心这个。让我们给出一些背景:

我经常使用 AirFlow 在 Bigquery 上执行 SQL 查询，转换数据，并准备数据可视化。我们有一个工作流程编排器(Airflow ),它按照定义的顺序执行请求。在某些情况下，我们选择将复杂的查询分成多个较小的查询。

> **而不是**

```
CREATE TABLE customers_infos AS
SELECT customers.id,
       customers.salary,
       traffic_info.weeks_since_last_visit,
       category_info.most_visited_category_id,
       purchase_info.highest_purchase_valueFROM company.customers
LEFT JOIN ([..]) AS traffic_info
LEFT JOIN ([..]) AS category_info
LEFT JOIN ([..]) AS purchase_info
```

> 你可以用

```
**## STEP1: Create initial table** CREATE TABLE public.customers_infos AS
SELECT customers.id,
       customers.salary,
       0 as weeks_since_last_visit,
       0 as most_visited_category_id,
       0 as highest_purchase_value
FROM company.customers**## STEP2: Update traffic infos** UPDATE public.customers_infos
SET weeks_since_last_visit = DATE_DIFF(*CURRENT_DATE*,
                                       last_visit.date, WEEK)
FROM (
         SELECT customer_id, max(visit_date) as date
         FROM web.traffic_info
         GROUP BY customer_id
     ) AS last_visit
WHERE last_visit.customer_id = customers_infos.id**## STEP3: Update category infos** UPDATE public.customers_infos
SET most_visited_category_id = [...]
WHERE [...]**## STEP4: Update purchase infos** UPDATE public.customers_infos
SET highest_purchase_value = [...]
WHERE [...]
```

**警告:**尽管这种方法在简化复杂查询时非常有用，但它可能会带来可读性/性能的损失。

如果您使用 OLAP(或任何面向列的)数据库，尤其如此，该数据库针对聚合和分析查询(选择、AVG、最小、最大等)进行了优化，但在事务处理(更新)方面性能较差。

虽然在某些情况下，它也可能提高你的表现。即使是现代的面向列的数据库，太多的连接也会导致内存或性能问题。在这些情况下，拆分请求通常有助于提高性能和内存。

此外，值得一提的是，您需要某种程序或编排器来按照定义的顺序执行查询。

## 9.基于您自己的约定的有意义的名称

正确命名模式和表是很困难的。使用哪种命名约定是有争议的，但是选择一种并坚持使用它是没有争议的。你应该定义**你自己的**惯例，并让你的团队采纳。

> 计算机科学只有两个硬东西:缓存失效和事物命名。—菲尔·卡尔顿

以下是我使用的惯例示例:

## **模式**

如果您使用的分析数据库有多种用途，那么以有意义的模式组织您的表是一个很好的做法。

在我们的 Bigquery 数据库中，每个数据源都有一个模式。更重要的是，我们根据目的以不同的模式输出结果。

*   第三方工具可以访问的任何表都位于 ***公共*** 模式中。Dataviz 工具如 DataStudio 或 Tableau 从这里获取数据。
*   由于我们将机器学习与 [BQML](/super-fast-machine-learning-to-production-with-bigquery-ml-53c43b3825a3) 一起使用，我们得到了一个专用的 ***机器学习*** 模式。

## 桌子

根据惯例，表本身应该是名称。
在 Agorapulse，我们有几个用于数据可视化的仪表板，每个仪表板都有自己的用途:营销仪表板、产品仪表板、管理仪表板等等。

我们的公共模式中的每个表都以仪表板的名称为前缀。一些例子可能包括:

```
product_inbox_usage
product_addon_competitor_stats
marketing_acquisition_agencies
executive_funnel_overview
```

当与团队合作时，花时间定义你的惯例是值得的。当涉及到命名一个新表时，千万不要用一个你以后会“改变”的又快又脏的名字:你可能不会。

请随意使用这些例子来定义您的约定。

## 10.最后，写一些有用的评论…但不要太多

我同意这样的观点，一个写得很好并且正确命名的代码不需要注释。阅读您的代码的人应该在代码本身之前就理解其逻辑和意图。

尽管如此，注释在某些情况下还是有用的。但是你一定要避免过多评论的陷阱。

> **避免**

```
**WITH** fp **AS**
(
   **SELECT** c_id,               # customer id
          MIN(date) as dt     # date of first purchase
   **FROM** company.purchases
   **GROUP BY** c_id
),ps **AS**
(
   **SELECT** age,
          gender,
          AVG(salary) as avg
   **FROM** company.customers
   **GROUP BY** age, gender
)**SELECT** customers.id, 
       ct.name, 
       ct.c_age,            # customer age
       ct.gender,
       ct.salary,
       ps.avg,              # average salary of a similar persona
       fp.dt                # date of first purchase for this client
**FROM** company.customers ct# join the first purchase on client id
**JOIN** fp **ON** c_id = ct.id# match persona based on same age and genre
**JOIN** ps **ON** ps.age = c_age
           **AND** ps.gender = ct.gender
**WHERE** c_age <= 30
```

> **更喜欢**

```
**WITH** first_purchase **AS**
(
   **SELECT** customer_id,
          MIN(date) as date 
   **FROM** company.purchases
   **GROUP BY** customer_id
),persona_salary **AS**
(
   **SELECT** age,
          gender,
          AVG(salary) as avg_salary
   **FROM** company.customers
   **GROUP BY** age, gender
)**SELECT** customers.id, 
       customers.name, 
       customers.age, 
       customers.gender, 
       customers.salary,
       persona_salary.avg_salary **as** persona_avg_salary,
       first_purchase.date
**FROM** company.customers **JOIN** first_purchase **ON** first_purchase.customer_id = customers.id
**JOIN** persona_salary **ON** persona_salary.age = customers.age
                       **AND** persona_salary.gender = customers.gender
**WHERE** customers.age <= 30
```

# 结论

SQL 很棒。它是数据分析、数据科学、数据工程甚至软件开发的基础之一:它不会等待。它的灵活性是一种优势，但也可能是一个陷阱。

一开始你可能没有意识到这一点，尤其是如果你是唯一负责自己代码的人。但是在某些时候，当与团队一起工作时，或者如果有人必须继续您的工作，没有一组最佳实践的 SQL 代码将成为负担。

在本文中，我总结了编写 SQL 的最常见的最佳实践。当然，有些是有争议的或基于个人观点的:你可能想从这里获得灵感，并与你的团队定义一些不同的东西。

我希望它能帮助您将 SQL 质量提升到一个新的水平！