# 顶级数据科学业务指标

> 原文：<https://towardsdatascience.com/top-data-science-business-metrics-c7ae905c076a?source=collection_archive---------23----------------------->

## 意见

## …以及便于利益相关方协作的用例示例

![](img/d4bdc63161d3d64994a1f80e768b058a.png)

由[Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的[路径数字](https://unsplash.com/@pathdigital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄。

# 目录

1.  介绍
2.  分时段单位范围
3.  影响的变化
4.  摘要
5.  参考

# 介绍

虽然有大量的数据科学或特定的算法指标，如梅和 RMSE，知道这些很有用，但还有其他指标对利益相关者和你的业务整体来说更有意义。在学术界，这一点通常不会教得太多，但了解、实践和运用这一点同样重要。出于这些原因，我将研究三个用例，在这些用例中，您可以从利用业务指标与利益相关者(*特别是那些不在数据科学领域的*)进行协作中受益。

# 分时段单位范围

![](img/ab57e1afde9a261b36e0dc45a5304d93.png)

照片由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

请记住，并非所有这些指标都适用于所有用例，这很重要，因此我给出了一个例子，以便您可以确定它是否适合您的情况。话虽如此，让我们首先讨论一个用例，其中您预测一个连续的目标，比如从 1 到 100 的数字。

在当前的空间中，你可以用梅或 RMSE 为例，来了解你的具体模型做得如何。然而，当与高管或其他利益相关者交谈时，您会希望从它如何影响业务的角度来解释错误。

用例的例子是预测一栋房子出售的价格。预期目标可以是 20 万到 30 万美元之间。建立要素和模型后，您将看到您的 MAE 为 10，000 美元，RMSE 为 30，000 美元。虽然这听起来很棒，并且对您的模型有用，但它可能不会帮助您的利益相关者做出进一步的业务决策。

> **您可以替代的一些业务指标如下——基本上来自分时段的美元金额范围:**

*   比实际少 10，000 美元的预测百分比→

例如:40%的预测比实际少 10，000 美元以内，另一种看待这个业务指标的方式是，当与实际相比时，我们的预测有多少是低估的

*   预测值比实际值高出 10，000 美元的百分比→

例如:20%的预测值比实际值高出 10，000 美元以内，另一种看待这个业务指标的方式是，与实际值相比，我们的预测值有多少是**高估了**

您想要使用这种业务度量方法的原因是，利益相关者和您自己能够知道您在预测的任何一侧偏离了多少。例如，MAE 的局限性在于，它在总体水平上查看低于和高于实际值的预测值(如平均值**误差*所示的*)。根据您的用例，您可能希望预测固有地高估或低估。**

# *影响的变化*

*![](img/01abd245ba6bba0d473d431a89f4492e.png)*

*[Gilles Lambert](https://unsplash.com/@gilleslambert?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄的照片。*

*现在，我们已经了解了错误的不同解释，我们可以看看之前的指标对业务的影响。例如，当查看不同货币区间的预测数量时，我们会考虑这对企业意味着什么。*

> ***以下是一些关于影响业务指标变化的示例和讨论:***

*   *高估会导致销量下降*

*例句:当我们高估或不高估时，超过两周的销售占多大比例？*

*   *低估导致卖家的钱更少*

*例:有百分之多少的出价被低估了？*

*正如您所看到的，这个用例特定于房屋销售，但是它可以应用于不同的场景。比如汽车销售，任何产品销售等等。它甚至不需要与销售有关，它可以是当你看到某组预测时，你公司应用程序的登录次数。您的预测的影响通常集中在 A/B 测试上，或者比较当您提出某些预测时会发生什么，您的业务的哪些方面对您或您的利益相关者最重要，以及这些指标是否发生了重大变化。*

# *摘要*

*有了这两个主要的商业度量类型的例子，我们可以看到如何讨论 MAE、MAPE、AUC 等。当将一个模型合并到一个商业的正式发布中时，这是不够的。从业务的角度来测试模型总是最好的，我们已经讨论了分时段的指标，以及它们对产品和用户行为的影响，可以更好地理解业务。*

> *总而言之，这里有两个你应该知道的重要业务指标:*

```
** Bucketed Unit Ranges* Change in Impact*
```

*我希望你觉得我的文章既有趣又有用。如果您同意或不同意这些重要业务指标的例子，请随时在下面发表评论。为什么或为什么不？您认为您可以使用或重要的其他指标有哪些？这些当然可以进一步澄清，但我希望我能够为数据科学家和利益相关者阐明一些更独特和具体的业务指标。感谢您的阅读！*

****我不属于这些公司中的任何一家。****

**请随时查看我的个人资料、* [Matt Przybyla](https://medium.com/u/abe5272eafd9?source=post_page-----c7ae905c076a--------------------------------) 、*和其他文章，并通过以下链接订阅接收我的博客的电子邮件通知，或通过点击屏幕顶部的订阅图标* *点击订阅图标* ***，如果您有任何问题或意见，请在 LinkedIn 上联系我。****

***订阅链接:**[https://datascience2.medium.com/subscribe](https://datascience2.medium.com/subscribe)*

# *参考*

*[1]照片由[路径数码](https://unsplash.com/@pathdigital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄(2020)*

*[2]由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2017)*

*[3]Gilles Lambert 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2015)*