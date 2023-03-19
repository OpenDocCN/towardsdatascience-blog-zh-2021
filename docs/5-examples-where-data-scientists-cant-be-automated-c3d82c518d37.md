# 数据科学家无法自动化的 5 个例子

> 原文：<https://towardsdatascience.com/5-examples-where-data-scientists-cant-be-automated-c3d82c518d37?source=collection_archive---------11----------------------->

## 意见

## 不，数据科学家不会失业，原因如下。

![](img/61629ad93c3400d7e2da5841e5aacd05.png)

[艾迪尔·华](https://unsplash.com/@aideal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/robot?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上的照片。

# 目录

1.  介绍
2.  形成业务需求
3.  数据探索
4.  特征创建
5.  行业理解
6.  将模型实施到业务和/或产品中
7.  摘要
8.  参考

# 介绍

随着数据科学越来越受欢迎，以及变得更加明确，有一种观点认为数据科学本身可以自动化。虽然，是的，数据科学家做的很多过程可以并且很可能会自动化，但这个过程的关键步骤几乎总是需要专家的干预。数据科学的某些方面，如模型比较、可视化创建和数据清理，可以实现自动化。然而，这些步骤中的一些并不是数据科学家最有价值的地方。虽然数据科学教育通常侧重于编码和模型开发，但一个人必须指导这一过程的主要原因是因为数据科学应该如何融入业务和产品。我将在下面更详细地讨论这个概念，以及数据科学家无法自动化的五个例子。

# 形成业务需求

![](img/56ec59a62eff6d023fa21c0544e0855b.png)

照片由[布鲁克·卡吉尔](https://unsplash.com/@brookecagle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上拍摄。

既然我们对数据科学可以自动化的领域有了大致的了解，我们就可以把重点放在不能自动化的领域。形成商业提问的最重要的步骤之一，也称为问题陈述。自动化无法在业务中找到问题并很好地定义它。数据科学自动化当然可以使执行解决方案的答案变得更容易，但创造力和业务理解是探索需要修复的业务漏洞所必需的。

> 话虽如此，以下是一些例子，说明为什么形成业务需求应该是人为的，而不是自动化的:

*   软件或自动化不能理解业务的问题，例如，它不知道存在的产品不是基于它们的历史被推荐的产品，因此，它不能提出推荐系统最终将解决的问题
*   自动化不知道如何区分业务需求的优先级，例如，它不知道如何自己评估工作、时间、金钱、受影响的应用程序等等
*   自动化不能满足产品经理，也不能理解业务中的痛点

> *从本质上讲，数据科学自动化首先要努力找出它需要自己的原因。*

# 数据探索

![](img/830b19bdf81691bb6cc4add297cd5af8.png)

照片由[卡伦·艾姆斯利](https://unsplash.com/@kalenemsley?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/explore?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【3】上拍摄。

类似于数据科学自动化或自动化机器学习(*AutoML——这个概念不是一家公司*)无法从业务需求开始，AutoML 也不知道要寻找哪些数据源。AutoML 可以合并、连接并最终联合一个最终数据集，但它无法在原始数据被转换之前找到它。

> 以下是数据探索的一些要点——为数据科学模型找到正确的数据，来自数据科学家与 AutoML:

*   要进行 AutoML 数据探索，首先需要数据，这是由数据科学家收集的
*   数据科学家将查看各种站点、来源和平台，以找到可用于模型的数据
*   AutoML 很难给公司发电子邮件，通常 ***很难知道要寻找什么数据***——无论是流量数据、消费者数据还是任何数据

就像上一节中突出显示的声明一样，数据探索必须知道要寻找什么类型的数据，要遵守什么行业，涉及的法规，以及何时停止寻找数据——这个过程将由数据科学家(*和数据工程师等——取决于公司*)来完成。

# 特征创建

![](img/f85cfbf31390a14d2e42ef7125a1cc10.png)

[Jr Korpa](https://unsplash.com/@korpa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍照。

是的，特征工程可以自动化，但是，这个术语经常被互换使用和混淆。出于文章的目的，我们将其定义为`feature creation`。数据科学流程中的这一步可以获得 AutoML 的好处，但是，它需要了解业务、产品及其消费者，才能知道要创建哪些新功能。

> 以下是 AutoML 与 data scientist 在特征创建方面的一些关键实例:

*   数据科学家知道如何通过相乘或相除来组合两个特征，例如在`clicks`中，`user`可以是`clicks per user`
*   数据科学家将知道在有意义的地方将某些功能组合在一起，也许 AutoML 会尝试创建一个功能为`clicks per house type`的功能，虽然它可以认识到划分现有功能很重要(*如果平台有此功能*)，但它可以创建一个完全没有意义的功能，因为它确实了解业务或行业。一个数据科学家会创造一个类似`clicks per user grouped by zipcode`的功能。这个特性将会是一个可操作的特性，而不仅仅是你输入到一个模型中的东西，就好像你知道这是最重要的特性一样，然后你就可以针对这个特性的某些价值创造一个营销活动。

# 行业理解

![](img/243e0196fc8fe4e4603b8447d468848e.png)

照片由 [Element5 数码](https://unsplash.com/@element5digital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/health-industry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【5】上拍摄。

这个例子遵循了上面例子的主题，数据科学需要一个了解业务的人。话虽如此，我将简短地谈这一点。

*   很难自动知道根据行业使用什么类型的数据科学模型
*   行业差异很大，医疗行业的推荐算法可能不如电影服务平台有用

# 将模型实施到业务和/或产品中

![](img/2802000c6d29688335b8b20145731538.png)

照片由[印尼 UX](https://unsplash.com/@uxindo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【6】上拍摄。

其他示例围绕数据科学过程的第一部分，而最后一个示例强调了数据科学过程的最后一部分。假设您拥有自动化程度最高的*型号选择平台，能够带来惊人的准确性，那么它接下来会做什么？这部分流程非常需要数据科学家。*

*   *自动化只能到此为止，相反，数据科学家需要知道将结果放在应用程序、网站等的什么地方*
*   *数据科学家将知道训练模型的频率、显示结果的频率或预测新数据的频率——通常在数据科学流程的范围界定部分进行讨论*
*   *数据科学家将知道如何更好地向利益相关者总结复杂的结果，即使 AutoML 很好地总结了结果，像数据科学家这样的人将更有助于回答利益相关者、客户和上层领导会提出的几个问题*

# *摘要*

*您可能已经注意到这些示例中的一个趋势，其中大多数情况是数据科学过程的开始部分，也是数据科学过程的结束部分。这一点意味着数据科学的中间部分可以自动化，这样做的平台非常有用。然而，这是您开始和结束数据科学的方式。这种开始和结束正是需要数据科学家的地方。*

> *总而言之，下面是自动化数据科学平台需要数据科学家的五个关键示例:*

```
** Forming the Business Ask* Data Exploration* Feature Creation* Industry Understanding* Model Implementation Into Business and/or Product*
```

*我相信数据科学职位不会被移除，相反，会随着时间的推移而更新。*

*我希望你喜欢这篇文章，并开始讨论什么时候我们真正需要数据科学家。你同意我上面讨论的例子吗？如果不是，那是为什么？您还能想到哪些自动化更有用、更值得信赖的例子，以及哪些数据科学家应该只被使用的例子？谢谢你的阅读，我很感激！*

**请随时查看我的个人资料和其他文章，**也可以在 LinkedIn 上联系我。**

**我不隶属于上述任何公司。**

# *参考*

*[1]照片由 [Aideal Hwa](https://unsplash.com/@aideal?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/robot?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)上拍摄*

*[2]Brooke Cagle 在 [Unsplash](https://unsplash.com/s/photos/business?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2018)*

*[3]照片由[卡伦·艾姆斯利](https://unsplash.com/@kalenemsley?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/explore?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2016)*

*[4]照片由 [Jr Korpa](https://unsplash.com/@korpa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在[Unsplash](https://unsplash.com/s/photos/data?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2019)拍摄*

*[5]照片由 [Element5 Digital](https://unsplash.com/@element5digital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/health-industry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2018)*

*[6]照片由[印尼 UX](https://unsplash.com/@uxindo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)(2020)拍摄*