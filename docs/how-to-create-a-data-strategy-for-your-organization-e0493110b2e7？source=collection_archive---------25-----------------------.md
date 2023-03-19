# 如何为您的组织创建数据策略

> 原文：<https://towardsdatascience.com/how-to-create-a-data-strategy-for-your-organization-e0493110b2e7?source=collection_archive---------25----------------------->

## [人工智能笔记](https://pedram-ataee.medium.com/list/notes-on-ai-84e1081cf2dd)

## 创建可靠数据战略的三个基本支柱

![](img/02f0e1cae53c942af045a2bbe1e69dea.png)

照片由[赞·李](https://unsplash.com/@zane404?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在这里，我想用简单的话来定义数据策略，并帮助您为您的组织创建一个数据策略。近年来，数据已经成为公司的战略资源，每个公司都必须制定数据战略，以免输给竞争对手。主要问题是怎么做？要回答这个问题，你首先要了解什么是数据策略。然后，您必须创建一个适合您的组织的计划，了解其局限性和能力。**简而言之，数据策略有三个基本支柱:(1)价值，(2)收集，以及(3)架构。**数据策略确实有我在本文中没有涉及的其他方面。如果你在这些支柱中的任何一个失败了，你就达不到你的期望。

我决定写这篇文章，在收到许多关于我的前一篇文章的积极反馈之后，这篇文章的标题是[关于进行大规模数据收集的途径](https://medium.com/dataseries/on-the-path-to-conducting-a-large-scale-data-collection-8a8cd201543d)，在这篇文章中，我分享了一些见解，以防止在进行数据收集时的常见错误。

[](https://medium.com/dataseries/on-the-path-to-conducting-a-large-scale-data-collection-8a8cd201543d) [## 正在进行大规模数据收集

### 这是我多年来在管理风险方面学到的经验

medium.com](https://medium.com/dataseries/on-the-path-to-conducting-a-large-scale-data-collection-8a8cd201543d) 

# 价值

数据策略必须描述如何使用数据在组织中产生商业价值。有两种主要方式:(a) **构建基于数据的产品或服务(外部产品)**；以及(b) **创建报告并获得洞察力(内部产品)**。数据战略必须与企业战略(特别是数字化战略)保持一致，不能孤立进行。这就是为什么它必须由企业主决定和批准；否则永远不会超越婴儿期。

你可以建立一个基于数据的产品或服务，在现有的基础上创造收入。例如，一家开发电子邮件设计器的公司可以为其客户添加一个基于数据的推荐系统，以更容易地创建电子邮件，或者更好的是，使它们更具吸引力。您还可以使用数据来获得洞察力并创建报告，以便增强当前的业务流程。例如，一家食品公司可以在分拣阶段使用数据驱动的报告来衡量其产品的质量。这将有助于公司提高流程效率。

# 收藏品

我先举个例子。你想要抓取 RottenTomatoe 网站并收集数据。您运行 web scraper 一天，第二天，您会发现应该记录了另一个数据字段。这个过程必须重复。你经历过类似的场景吗？这可能发生在大规模的数据收集中，并且是有害的。大规模的数据收集是一个昂贵的过程，因此在进行之前，您必须回答以下问题，例如(a) **必须记录哪些领域的数据？** (b) **必须如何衡量数据质量？** (c) **收集干净数据的可扩展方式是什么？**

数据质量是决定实施数据策略成功几率的主要因素。这就是为什么**数据科学家的一个常见噩梦是收集大量无用的低质量数据**。根据 HBR 在 2017 年发表的一篇文章[,公司中只有 3%的数据符合基本质量标准。因此，在不知道如何衡量数据质量的情况下，强烈建议不要进行数据收集，至少不要进行大规模的数据收集。另外，除非现有数据的质量满足基本要求，否则你不应该对其寄予希望。](https://hbr.org/2017/09/only-3-of-companies-data-meets-basic-quality-standards)

# 体系结构

数据架构有两个主要阶段:(a) **存储**和(b) **分析。**每个阶段的要求不一样。例如，您必须创建一个数据管道，以最小的冗余快速接收和存储数据。NoSQL 数据库通常用于第一阶段，因为它们在接收新数据时执行速度很快。此外，它们是人类可读的，有助于理解数据。然后，您必须创建一个管道来检索和分析数据。SQL 数据库经常在这个阶段使用，因为 Scikit-Learn 等标准 ML 库接受表格数据作为输入。当你设计数据架构时，你必须总是考虑可伸缩性和效率。如果您不能在第一次尝试中设计出最好的数据架构，请根据需要修改它。性能不佳的数据架构将会给你带来沉重的打击。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [亚马逊](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为会员上* [*中*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium—Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)