# 如何:市场营销中的数据分析

> 原文：<https://towardsdatascience.com/data-analysis-in-marketing-2334f29ee685?source=collection_archive---------29----------------------->

## 营销和销售领域的分析入门示例

![](img/3e02a170ed7e931f20badbf87aca8080.png)

在 [Unsplash](https://unsplash.com/s/photos/marketing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [Merakist](https://unsplash.com/@merakist?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

无论您是数据科学家、分析师还是业务分析师，了解各个部门的实际工作都很有用。他们的目标是什么，他们是如何直接或间接挣到钱的，如何衡量成功？它帮助我了解了各个部门的关键数字。这使我能够以更有针对性的方式帮助该部门，最重要的是，除了这些标准 KPI 之外，我还可以获得进一步的分析。我想解释一下这些 KPI 中的一些，以及如何获得它们的数据。

## 市场分析

为了给公司创造销售机会和做出销售决策，以下关键数据非常重要。最重要的关键数字之一是市场份额。计算方法如下:

> 用公司自己的市场份额除以总市场份额

虽然这是最重要的关键数字之一，但当然很难计算。在这里，你可能不得不求助于像 statista [1]或类似的资源。但是，如果您已经找到了整个市场的销售额，例如，您可以借助内部数据(如 ERP 系统)来近似计算您自己的市场份额。

![](img/d1212aa1365670b6be079484b920e970.png)

照片由[艾萨克·史密斯](https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/sales?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

当然，这同样适用于销售增长。这里，路径是相同的，但是是在一段时间内测量的。

## 销售关键数字

销售当然是一个非常重要的工具。毕竟，这是产生营业额的地方。还有一些常用的 KPI 来衡量销售的成功与否。比如优惠的成功与否。这是通过以下方式计算的:

> 报价成功=收到的订单数/提交的报价数

另一个很好的指标是取消率的计算:

> 取消率=取消订单/订单总值

要衡量营销活动的成功，您可以计算广告的成功度:

> 销售增长/广告成本

当然，这只是你可以应用在销售数据上的几种方法，但却是最有趣的一种。要深入探究，你可以从这里的[开始。](https://www.lucidchart.com/blog/sales-kpis)从哪里可以得到数据？如果你的公司使用 CRM 系统，这是一个不错的选择，但 ERP 或订单管理系统也可能是你正在寻找的资源。

## 在线营销 KPI

随着营销变得越来越数字化，从系统中提取关键数据成为可能。以下是一些例子:

*   每个销售线索的成本
*   订单价值—购物车的平均价值是多少？
*   点击率—您的内容是否得到了您想要的关注？
*   转化时间:访客转化速度有多快？
*   网站的平均访问时间等。

虽然大多数数字通常已经由较新的系统(如在线商店软件)生成，或者可以从数据库中提取，但 KPI(如每线索成本)将有点难以计算，因为您还需要所用资源的成本，在这里，ERP 系统和财务模块可能是一个可行的起点。你可以从[这篇文章](/sap-data-analytics-in-the-google-cloud-8f1a8a662355)中得到如何提取和分析财务数据的启示。

![](img/2eebd5b0d23593c988c4fc08cde7b9ce.png)

照片由[米利安·杰西耶](https://unsplash.com/@mjessier?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/online-marketing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

## 结论

当然，这些只是少数可能的分析。作为一名数据科学家或分析师，为了理解营销领域，尤其是为了开发可能的新方法，记住它们当然是有帮助的。

## 资料来源和进一步阅读

[1] statista，[搜索市场份额统计数据](https://de.statista.com/statistik/suche/?Suche=&q=marktanteil&qKat=search) (2021)

[2] Lucidchart，
你的计划是什么？[销售成功的 13 个最重要的 KPI](https://www.lucidchart.com/blog/sales-kpis)