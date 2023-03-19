# 5 个常见的 SQL 问题让你崩溃

> 原文：<https://towardsdatascience.com/5-common-sql-problems-for-you-to-crush-10a796258643?source=collection_archive---------27----------------------->

## 详细了解这些问题，并了解解决这些问题的方法

![](img/cd76656e599ab23b5179906bfdd84864.png)

Diana Polekhina 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

对于任何数据科学家来说，拥有出色的 SQL 技能都是一项资产。学习新概念和彻底修改它们都可以通过挑选和解决该领域中的一些常见问题来完成。

在这篇文章中，我将转述 5 个这样的问题，它们是我从 LeetCode 中精选出来的，我将描述他们想要什么，并列出解决它们的方法。带着这些问题，我还希望涵盖一些你应该知道(并修改)的基本概念，以及一些高级的查询方法，这些方法在准备面试时有助于记忆，或者只是增加你的 SQL 知识。

我们开始吧👇

# 1.合并两张桌子

**问:**您得到了两个表——`Person`和`Address`，并被要求编写 SQL 查询来报告`Person`表中每个人的名字、姓氏、城市和州。

(如果`personId`的地址不在`Address`表中，则报告`null`。)

[链接到问题。](https://leetcode.com/problems/combine-two-tables/)

```
**Your Output:** 
+-----------+----------+---------------+----------+
| firstName | lastName | city          | state    |
+-----------+----------+---------------+----------+
| Allen     | Wang     | Null          | Null     |
| Bob       | Alice    | New York City | New York |
+-----------+----------+---------------+----------+
```

## **方法:**

最简单的解决方法是使用连接的概念。但是，您可能需要决定在这里应该使用哪个连接。

请记住——由于我们需要将城市和州列的值报告为 null，而对于没有该值的行，我们应该直觉地认为这里需要的是左连接。

因此，执行一个简单的左连接，我们可以在 MySQL 中编写以下查询:

```
select a.firstName, a.lastName, b.city, b.state 
from Person a
left join Address b 
on a.personId = b.personId;
```

请注意，我们需要将 Address 表中的外键与 Person 表进行匹配，以便获得每个人所在的城市和州列。

# 2.从不点餐的顾客

**问:**问题中有两个表——`Customers`和`Orders`。您的任务是编写一个 SQL 查询来报告所有从不订购任何东西的客户。

[链接到问题。](https://leetcode.com/problems/customers-who-never-order/)

```
**Your output:** +-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

## 方法:

**1 号**:使用`WHERE NOT EXISTS`条款:

首先，作为一个子查询，我们希望从 Orders 表中选择 Customers 表中的客户 id 与 Orders 表中的客户 id 相匹配的所有行。这意味着—我们将选择至少下过一次订单的所有客户。

现在，在主查询中，我们可以简单地**不选择**那些客户(行)来产生输出。这是借助于 NOT EXISTS 子句完成的。

```
select c.name as 'Customers' 
from Customers c
where not exists
(
    select null 
    from Orders o
    where c.id = o.customerId
);
```

**No 2:** 使用`NOT IN` 关键字:

有一种更慢的方法可以做到这一点，我们可以轻松地从 Order 表中选择所有客户 id，并从 Customers 表中选择子查询结果中没有的所有客户。

```
select c.name as 'Customers'
from Customers c
where c.id not in
(
    select customerId from Orders
);
```

# 3.第二高的薪水

**问:**我们的任务是找出给定的`Employee`表中第二高的薪水。如果没有第二高的薪水，查询应该输出`null`。

[链接到问题。](https://leetcode.com/problems/second-highest-salary/)

```
**Your output:**+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

## 方法:

**No 1:** 解决这个问题最简单的方法就是借助`**limit**`和`**offset**`关键词。

我们将从表中选择所有不同的薪金，按降序对它们进行排序，并将输出限制为 1，因此我们只获得 1 份薪金，最后，我们将降序偏移 1，这意味着我们选择的不是最高的薪金，而是第二高的薪金。大概是这样的:

```
select distinct e.salary as SecondHighestSalary 
from Employee e
order by e.salary desc
limit 1 offset 1
```

**No 2:** 另一种解决方法是使用 SQL 中的`**Max()**` 函数。

在子查询中，我们从雇员表中选择最高工资。最后，我们从这个子查询的输出中选择最大的薪水，并将其作为最终的输出——现在是第二高的薪水。

```
select max(e.salary) as SecondHighestSalary
from Employee e
where e.salary < (select max(salary) from Employee );
```

# 4.重复电子邮件

**问:**我们被要求根据`Person`表找出所有重复的电子邮件。

L [墨迹到问题。](https://leetcode.com/problems/duplicate-emails/)

```
**Your output:**+---------+
| Email   |
+---------+
| a@b.com |
+---------+
```

## 方法:

我们可以通过确保我们理解的第一件事来解决这个问题，即对于选择副本，我们对任何一种电子邮件的计数都应该大于 1。话虽如此，我们只想将电子邮件作为输出，最好的方法是使用“`**Group by**`”和“`**Having**`”关键字。

```
select p.email
from Person p
group by email
having count(p.email)  > 1;
```

# 5.温度上升

**问:**给定`Weather`表，我们想要编写一个 SQL 查询来查找与之前的日期相比温度更高的所有日期。

[链接到问题](https://leetcode.com/problems/rising-temperature/)。

```
**Your output:**+----+
| id |
+----+
| 2  |
| 4  |
+----+
```

## 方法:

问题的核心在于确保相邻的日期不是温度升高的日期。所以，为了解决这个问题，我们可以使用`**Datediff(date1, date2)**`函数。

请注意，我们只需要在输出表中显示较高的 dateId。因此，在我们的查询中，我们将确保只选择温度较高的日期，例如 w2，如下所示:

```
select w2.id 
from Weather w1, Weather w2
where w2.temperature > w1.temperature
and datediff(w2.recordDate, w1.recordDate) = 1;
```

接下来，我们希望将 w2 温度与之前的实例进行比较，因此我们采用另一个变量 w1。然后，我们应用两个`Where`条件:首先，我们确保 w2 的温度高于 w1 的温度。其次，我们确保日期差异只有 1 天。因此，我们已经成功地构建了我们的查询！

# 几句临别赠言…

这就是 5 个非常值得了解的问题的列表，我希望它有助于您理解或修改编写更好的查询来解决问题的一些核心概念。

将来，我会回来撰写更多基于 SQL 的文章。所以[跟着我](https://ipom.medium.com/)留在圈子里！

## [我还建议成为一名中等会员，不要错过我每周发表的任何一篇数据科学文章。](https://ipom.medium.com/membership/)在此加入👇

[](https://ipom.medium.com/membership/) [## 通过我的推荐链接加入 Medium

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

ipom.medium.com](https://ipom.medium.com/membership/) 

# 接通电话！

> *关注我* [*推特*](https://twitter.com/csandyash) *。* [*查看我所有数据科学帖子的完整代码库！*](https://github.com/yashprakash13/data-another-day)

我的另外几篇文章你可能会感兴趣:

[](/the-nice-way-to-deploy-an-ml-model-using-docker-91995f072fe8) [## 使用 Docker 部署 ML 模型的好方法

### 使用 FastAPI 部署 ML 模型并在 VSCode 中轻松封装它的快速指南。

towardsdatascience.com](/the-nice-way-to-deploy-an-ml-model-using-docker-91995f072fe8) [](https://medium.datadriveninvestor.com/5-interesting-data-science-projects-to-level-up-your-portfolio-f4d88c181061) [## 5 个有趣的数据科学项目来提升您的投资组合

### 执行以实际应用为重点的项目来强化你的形象

medium.datadriveninvestor.com](https://medium.datadriveninvestor.com/5-interesting-data-science-projects-to-level-up-your-portfolio-f4d88c181061)