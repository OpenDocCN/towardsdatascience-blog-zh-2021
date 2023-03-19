# 4 种数据分析师面试问题类型

> 原文：<https://towardsdatascience.com/4-data-analyst-interview-question-types-a372c73ea18b?source=collection_archive---------17----------------------->

## 意见

## …以及如何回答这些问题

![](img/6d919cc598ebdb8f7d1c6beb0224cd03.png)

克里斯蒂娜@ wocintechchat.com 在[Unsplash](https://unsplash.com/s/photos/interview?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片[。](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 目录

1.  介绍
2.  结构化查询语言
3.  业务指标
4.  形象化
5.  预测
6.  摘要
7.  参考

# 介绍

面试可能彼此大相径庭，所以很难预测他们会有什么样的表现。然而，根据我的经验，他们似乎总是遵循某种趋势，这就是他们问的问题类型。虽然所有数据分析师(*和数据科学*)的面试可能不尽相同，都有相同的问题，但有些人可以预期两次面试之间的类型相似。下面，我将讨论我在面试中遇到的四种主要的数据分析师问题，以及我从其他人那里听到的问题。

# 结构化查询语言

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

卡斯帕·卡米尔·鲁宾在[Unsplash](https://unsplash.com/s/photos/sql?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上的照片。

此问题类型是数据分析师面试中更具技术性的方面。这个问题或这些问题的目的是看您是否对如何用通用语言查询数据库有一个总体的了解，并对业务有所了解。面试官还想知道你是如何解决和思考问题的，以及当你不知道答案时会发生什么。例如，最有可能的是，如果你不确定，只是说得很少，你将不会得到这份工作，但如果你是透明的，回答问题的基础，并向面试官提问，他们会看到你解决问题的兴趣和动机，即使你并不马上知道。

> SQL 问题示例:

*   2020 年 8 月至 2021 年 1 月期间有多少用户登录？
*   哪个月的用户最多？

虽然这个例子可能看起来非常简单，但有一些故意模糊的措辞，以便面试官希望你向他们提问，作为一种描述真实生活的方式，在这种生活中，你必须与产品经理或利益相关者交谈以获得澄清。大多数人可以查询简单的表格，但是，面试官真正想知道的是，你是否可以合作定义问题，以便双方对结果的预期保持一致。

> SQL 示例答案(伪代码—不是真正的 SQL 代码):

```
SELECT month,count(distinct(users)) as unique_user_loginsFROM date_tableWHERE login_date >= “2020–08–01”AND login_date < “2021–02-01”GROUP BY month, countORDER BY count DESC;
```

正如我所说的，面试官的意图是让问题变得模糊，而不是欺骗你，所以要重现真实的职业世界是什么样子的，在这个世界里，问你这个问题的人可能不是技术人员或不了解数据，所以你必须一起努力让问题 100%澄清。

*   哪个月的用户最多，他们想让你反过来问:" ***用户，比如唯一用户，或者总登录数？***

很有可能，你会遇到一个比这个更难的 SQL 问题，但是，我要强调的是，当你回答一个问题时，面试官想要一个对话——他们想要一个容易相处的人，他们可以找到解决方案。

# 业务指标

![](img/d36718475ada61f9270add78c1c68d56.png)

照片由[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄。

下一类数据分析师面试问题旨在确保您能够理解公司的常见 KPI(*关键绩效指标*)。是的，有些公司可能有不同的 KPI，但了解主要的 KPI 很重要，如`clicks per user`、`login time`、`costs per user`等。如果你真的了解了你申请的公司，你会看起来更好，这样你就可以知道常见的问题和跟踪这些问题的指标，并在面试中展示出来。例如，如果你正在申请优步，你想调出像`drive_time_per_driver`、`trips_per_driver`、`trips_per_user`、`cancellations_per_user`等业务指标。

> 以下是一个业务指标类型问题的示例:

*   你开始了一个实验，看看哪些用户应该首先得到提升，你的目标是谁，你用什么指标来跟踪成功？

> 以下是业务指标类型问题答案示例:

*   “你们现在有什么促销活动，你们的客户是什么样的，哪些地区会受到影响？我会瞄准每月乘坐次数最少的人，因为这些人最有机会增长，因此，我会使用指标`rides_per_month`来找到目标人群，然后跟踪相同的指标，看看一旦他们看到来自一组测试数据/人群的促销活动，该指标是否会随着时间的推移而发生变化。”

这种类型的问题可能没有确切的答案，但目标是了解情况的变量或特征，以及如何跟踪成功，以及在实施变更后要跟踪什么指标。

# 形象化

![](img/d33e38adb0c35e47a464c8b4d16acb33.png)

[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍照。

这种题型比较独特，所以你可能没有前两种题型看得多，但是，作为一个数据分析师，学习和了解还是很重要的。面试官可能会展示一些图表，你必须指出趋势中的差异，这是一种人工异常检测。他们可能会给出一个排序不正确的图表，如果你注意到这个图表是按某个数字排序的，而不是按日期排序的，他们会看出来。

> 建议:

*   就像您对 SQL 和业务指标问题类型所做的那样，询问探查性问题，如 x 轴和 y 轴代表什么，当前空间是什么，目标空间是什么，此图表的目标是什么，此图表影响什么类型的系统或流程，以及您采取了什么措施来解释缺失数据或错误数据等？

这种类型的面试问题更重要的是要知道，并确保你很好地理解与图表相关的常见问题，以及你应该从图表中确定的指标和结果的类型。

# 预测

![](img/6cf40e1bf7659afc21fa6401d4559a54.png)

照片由[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】拍摄。

作为数据分析师，您经常会遇到数据科学家的交叉工作。它不一定是机器学习算法，但很可能是某种回归分析或 ARIMA 模型(*自回归综合移动平均*)，专注于使用过去的历史和数据来预测未来的数据。有些工作可以在 Microsoft Excel、Tableau 或 Python 中完成( *R 和*)。如果你认为预测是工作描述中的一项必备技能，那么你可以期待一些面试来测试你常用的预测技巧。

> 预测问题示例:

*   使用表中的数据，帮助我们预测下个月服装产品的销售额

这个问题的答案将是回顾这个问题，更好地定义它，询问可用的数据，询问当前的空间-是否已经有一些您可以调整或建立的预测？什么类型的特征在预测销售额时很重要— `time/date`、`age`、`location`等。如果你有这些工具中的任何一个，你当然可以在几秒钟内做出预测，在 Tableau 中，这就像将字段拖放到预先建立的预测图中一样简单，但是正如我之前所说的，这些问题的目标不是获得 99%的解决方案准确性，而是通过你会做什么，会出现什么问题，以及如果有更多的时间你会做什么。

例如，“如果我有更多的时间，我会更多地调查丢失的数据，了解它是否真的是异常，或者是否是预期的，并了解为什么会有负面数据，等等。”。

# 摘要

我们已经讨论了四种常见的数据分析面试问题。我亲身经历过这些问题，你很可能也会看到这些类型的问题(*不是每个人*)。虽然我所说的可能不会全部发生，但了解围绕 SQL、业务指标、可视化和预测的示例仍然很重要，因为作为数据分析师，这些是需要了解的重要概念，即使在访谈中没有讨论它们。其他一些工具包括但不限于:Python、R、Tableau、Microsoft Excel 和 Google Slides 或 Microsft PowerPoint 的使用。

> 同样需要注意的是，所有这些问题也可能是数据科学家面试的一部分。

> 总而言之，在面试数据分析师职位时，会遇到以下几类问题:

```
* SQL* Business Metrics* Visualization* Forecasting
```

我希望你觉得我的文章既有趣又有用。如果您在与数据分析师或数据科学家的访谈中遇到过这些问题，请随时在下面发表评论。你同意还是不同意，为什么？

请随时查看我的个人资料和其他文章，也可以通过 LinkedIn 联系我。我不隶属于上述任何公司。

# 参考

[1]照片由[克里斯蒂娜@ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/interview?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)上拍摄

[2]卡斯帕·卡米尔·鲁宾在 [Unsplash](https://unsplash.com/s/photos/sql?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)

[3]照片由[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2016)上拍摄

[4]图为[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2021)

[5]照片由[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2018)上拍摄