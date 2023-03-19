# 数据科学面试中你应该知道的五大 SQL 窗口函数

> 原文：<https://towardsdatascience.com/top-five-sql-window-functions-you-should-know-for-data-science-interviews-b8b334af437?source=collection_archive---------6----------------------->

## 关注数据科学家的重要概念。

![](img/4405c40fecb022673ea89da67f95404e.png)

由 [Unsplash](https://unsplash.com/s/photos/window?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的[萨莎·弗里明德](https://unsplash.com/@sashafreemind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄的照片

SQL 是数据世界中的通用语言，也是数据专业人员需要掌握的最重要的技能。

SQL 如此重要的原因是，它是数据争论阶段所需的主要技能。大量的数据探索、数据操作、管道开发和仪表板创建都是通过 SQL 完成的。

伟大的数据科学家与优秀的数据科学家的区别在于，伟大的数据科学家可以在 SQL 功能允许的情况下尽可能多地争论数据。能够充分利用 SQL 提供的所有功能，很大一部分是因为知道如何使用窗口函数。

说了这么多，让我们开始吧！

# 1)具有超前()和滞后()的增量

LEAD()和 LAG()主要用于比较给定指标的一段时间和前一段时间。举几个例子…

*   你可以得到每年销售额和前一年销售额之间的差值
*   你可以得到每月注册/转换/网站访问量的增量
*   您可以按月比较用户流失情况

**示例:**
以下查询显示了如何查询每月的成本变化百分比

```
with monthly_costs as (
    SELECT
        date
      , monthlycosts
      , LEAD(monthlycosts) OVER (ORDER BY date) as
        previousCosts
    FROM
        costs
)SELECT
    date
  , (monthlycosts - previousCosts) / previousCosts * 100 AS
    costPercentChange
FROM monthly_costs
```

# SUM()或 COUNT()的累积和

通过一个以 SUM()或 COUNT()开始的 windows 函数可以简单地计算运行总数。当您想要显示特定指标随时间的增长时，这是一个强大的工具。更具体地说，它在以下情况下很有用:

*   获得一段时间内的总收入和总成本
*   获得每个用户在应用上花费的总时间
*   获得一段时间内的累计转化率

**示例:**
以下示例显示了如何包含每月成本的累计和列:

```
SELECT
    date
  , monthlycosts
  , SUM(monthlycosts) OVER (ORDER BY date) as cumCosts
FROM
    cost_table
```

# 3)AVG 的移动平均线()

AVG()在 windows 函数中非常强大，因为它允许你计算一段时间内的移动平均值。

移动平均线是一种简单而有效的短期预测方法。它们在平滑图形上不稳定的曲线时也非常有用。一般来说，移动平均线用于衡量事物运动的大致方向。

更具体地说…

*   它们可用于获得每周销售的总体趋势(平均值是否会随着时间的推移而上升？).这将表明公司的成长。
*   他们同样可以用来获得每周转换或网站访问的一般趋势。

**示例:**
以下查询是获取 10 天移动平均值进行转换的示例。

```
SELECT
    Date
  , dailyConversions
  , AVG(dailyConversions) OVER (ORDER BY Date ROWS 10 PRECEDING) AS
    10_dayMovingAverage
FROM
    conversions
```

# 4)行号()

当您想要获取第一条或最后一条记录时，ROW_NUMBER()特别有用。例如，如果您有一个健身房会员何时来到健身房的表，并且您想获得他们第一天来到健身房的日期，您可以按客户(姓名/id)进行分区，并按购买日期进行排序。然后，为了获得第一行，您可以简单地筛选 rowNumber 等于 1 的行。

**示例:**
这个示例展示了如何使用 ROW_NUMBER()获取每个成员(用户)第一次访问的日期。

```
with numbered_visits as (
    SELECT
        memberId
      , visitDate
      , ROW_NUMBER() OVER (PARTITION BY customerId ORDER BY
        purchaseDate) as rowNumber
    FROM
        gym_visits
)SELECT
    *
FROM
    numbered_visits
WHERE 
    rowNumber = 1
```

总的来说，如果您需要获取第一条或最后一条记录，ROW_NUMBER()是实现这一点的好方法。

# 5)用 DENSE_RANK()记录排名

DENSE_RANK()类似于 ROW_NUMBER()，只是它对相等的值返回相同的等级。在检索顶级记录时，密集排名非常有用，例如:

*   如果你想找出本周十大最受关注的网飞节目
*   如果您想根据花费的金额获得前 100 名用户
*   如果您想查看 1000 个最不活跃的用户的行为

**示例:**
如果您想按总销售额对顶级客户进行排名，DENSE_RANK()是一个合适的函数。

```
SELECT
    customerId
  , totalSales
  , DENSE_RANK() OVER (ORDER BY totalSales DESC) as rank
FROM
    customers
```

# 感谢阅读！

仅此而已！我希望这能对你的面试准备有所帮助——我确信如果你对这 5 个概念了如指掌，你会在大多数 SQL 窗口函数问题上做得很好。

一如既往，我祝你学习一切顺利！

不确定接下来要读什么？我为你挑选了另一篇文章:

[](/ten-advanced-sql-concepts-you-should-know-for-data-science-interviews-4d7015ec74b0) [## 数据科学面试中你应该知道的十个高级 SQL 概念

### 让您的 SQL 技能更上一层楼

towardsdatascience.com](/ten-advanced-sql-concepts-you-should-know-for-data-science-interviews-4d7015ec74b0) 

**又一个！**

[](/a-complete-52-week-curriculum-to-become-a-data-scientist-in-2021-2b5fc77bd160) [## 2021 年成为数据科学家的完整 52 周课程

### 连续 52 周，每周学点东西！

towardsdatascience.com](/a-complete-52-week-curriculum-to-become-a-data-scientist-in-2021-2b5fc77bd160) 

# 特伦斯·申

*   ***如果你喜欢这个，*** [***跟我上媒***](https://medium.com/@terenceshin) ***了解更多***
*   ***有兴趣合作吗？让我们连线上***[***LinkedIn***](https://www.linkedin.com/in/terenceshin/)