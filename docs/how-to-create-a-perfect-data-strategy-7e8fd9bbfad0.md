# 如何创建完美的数据策略

> 原文：<https://towardsdatascience.com/how-to-create-a-perfect-data-strategy-7e8fd9bbfad0?source=collection_archive---------33----------------------->

## [人工智能笔记](https://pedram-ataee.medium.com/list/notes-on-ai-84e1081cf2dd)

## 深入了解数据战略的四大支柱

![](img/344f281d27da9431d87654834c10235f.png)

由[伊万杰洛斯·姆皮卡基斯](https://unsplash.com/@mpikman?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如今，公司的目标是通过使用他们的数据建立人工智能解决方案来增加利润。但是，由于他们通常没有完善的数据策略，因此无法成功完成任务。数据策略有四个主要支柱:**价值**、**集合**、**架构、**和**治理。**我在[如何为您的组织创建数据策略](/how-to-create-a-data-strategy-for-your-organization-e0493110b2e7)中描述了数据策略。我见过很多公司在制定数据策略时犯的常见错误。在本文中，我想描述构建数据策略的最佳实践，并解释我从一些真实场景中学到的经验。

[](/how-to-create-a-data-strategy-for-your-organization-e0493110b2e7) [## 如何为您的组织创建数据策略

### 创建可靠数据战略的三个基本支柱

towardsdatascience.com](/how-to-create-a-data-strategy-for-your-organization-e0493110b2e7) 

# 价值——它真的创造了商业价值吗？

一个大型组织必须找到一个可以被人工智能解决方案很好服务的商业目标。例如，一家大型连锁杂货店可以分析来自其客户忠诚度计划、供应链、商店交易和商店流量的数据。**问题是使用人工智能解决方案可以显著增强哪个业务目标。**对于这些组织来说，开始构建人工智能解决方案的成本效益分析没有意义是很常见的。失败可能会阻止该组织追求其他有前途的道路。因此，强烈建议在使用人工智能确定业务目标之前，进行成本效益或复杂性预期分析。要阅读更多关于复杂性期望分析的内容，你可以看看下面的文章。

[](https://medium.com/swlh/if-you-consider-using-ai-in-your-business-read-this-5e666e6eca23) [## 如果你考虑在你的业务中使用人工智能，请阅读这篇文章。

### 期望-复杂性框架来评估人工智能在你的企业中的可行性

medium.com](https://medium.com/swlh/if-you-consider-using-ai-in-your-business-read-this-5e666e6eca23) 

> 在使用人工智能确定商业目标之前，你必须进行成本效益或复杂性预期分析。

# 收集—收集的数据与要解决的问题相关吗？

众所周知，如果大量数据或各种数据类型不存在，人工智能解决方案就无法工作。然而，数据和问题之间的关联经常被忽视。那些不是人工智能专家的人，期待人工智能解决方案的魔力。他们认为，如果大量数据输入到人工智能解决方案中，这就足够了。不对！领域专家可以简单地识别数据与业务目标的相关性。他们知道问题背后的物理原理，并且能够很容易地确定需要收集的相关数据。然而，他们的意见有时会受到损害。要了解大规模数据收集中存在的其他挑战，您可以阅读下面的文章。

[](https://medium.com/dataseries/on-the-path-to-conducting-a-large-scale-data-collection-8a8cd201543d) [## 正在进行大规模数据收集

### 这是我多年来在管理风险方面学到的经验

medium.com](https://medium.com/dataseries/on-the-path-to-conducting-a-large-scale-data-collection-8a8cd201543d) 

> 没有多样、庞大、相关的数据，AI 会让你失望。然而，数据和问题之间的关联经常被忽视。

# 架构—数据集的设计使用、扩展和维护效率如何？

随着人工智能项目从单一类型数据转换到多模态数据，数据架构变得更加重要。此外，近年来，数据集的规模和用途呈指数级增长。指数级增长导致了数据架构中必须解决的可扩展性问题。最后，但同样重要的是，数据架构会显著影响性能和开发速度。**因此，如果你不建立一个高性能的数据架构，你将会遇到普遍的技术挑战。**有一次，我正在咨询一家大型芯片制造商，以构建一个人工智能解决方案来检测其芯片的故障率。他们给了我大量的数据集，这些数据集不仅包含相同的数据，而且每个数据集只包含一两个有用的数据字段。不用说，这些数据集也经常更新。他们的低质量数据架构让我花了很多时间来创建一个干净、可靠、易于监控和更新的数据集。

> 您必须分析数据架构如何影响项目，否则技术挑战很快就会出现。

# 治理——各种团队是否可以访问所需的数据？

在大型组织中，由于团队经常在竖井中工作，因此在不同的团队之间收集和共享数据存在许多挑战。例如，数据库团队负责创建数据存储和检索的基础设施，而人工智能团队负责分析和处理数据。另一方面，数据收集发生在人工智能和数据库团队不在场的领域。团队之间没有那么多的互动，他们也没有意识到双方存在的挑战。例如，人工智能团队可能会要求提供重要的缺失数据以提高人工智能模型的性能，但数据团队不会合作收集这些数据。或者，一个部门可能拥有一个有价值的数据集，但由于技术或政治原因，他们没有与其他团队共享它。问题出现在这里，嘣！一个好的数据策略必须帮助不同的团队访问所需的数据，并且能够在需要时收集一组新的数据。

> 你必须创建一个计划，让数据收集、数据库、AI 等各个团队高效地协同工作。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium—Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)