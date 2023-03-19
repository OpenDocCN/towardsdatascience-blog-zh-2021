# 数据输入，预测输出

> 原文：<https://towardsdatascience.com/data-in-predictions-out-8be0812269ed?source=collection_archive---------33----------------------->

## 数据科学项目的生命周期

![](img/fe71c8595dc9dd9f961477c02005f51b.png)

照片由 [Pexels](https://www.pexels.com/photo/cutout-paper-illustration-of-human-hand-with-magnifier-and-chart-5841845/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 Gabby K 拍摄

数据科学家在处理大数据时，遵循定义良好的数据科学工作流程总是有利的。无论数据科学家是希望以通过数据可视化传达故事的动机来执行分析，还是希望构建数据模型，数据科学工作流程都很重要。数据科学项目的标准工作流可确保组织内的各个团队保持同步，从而避免任何进一步的延迟。

任何数据科学项目的最终目标都是产生有效的数据产品。在数据科学项目结束时产生的可用结果被称为数据产品。数据产品可以是任何东西——仪表板、推荐引擎或任何有助于商业决策的东西)来解决商业问题。然而，为了达到生产数据产品的最终目标，数据科学家必须遵循正式的逐步工作流程。数据产品应该有助于回答业务问题。同样，数据科学项目的生命周期不应该仅仅关注过程，而应该更加重视数据产品。这篇文章概述了数据科学家遵循的数据科学项目的标准工作流程。解决任何分析问题的全球公认结构被称为跨行业数据挖掘标准流程或 CRISP-DM 框架。以下是典型数据科学项目生命周期中的各个阶段。

![](img/82489dae5629411eaf285e0ea87424e0.png)

CRISP-DM 架构

## 1.商业理解

![](img/f5ae2f79a68f5a8bed39a06f33d1b0f7.png)

照片由[娜塔莉亚·瓦特凯维奇](https://www.pexels.com/@n-voitkevich?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/tax-documents-on-the-table-6863335/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

整个循环围绕着业务目标。如果你没有一个精确的问题，你会解决什么？清楚地理解业务目标是极其重要的，因为这将是你分析的最终目标。只有在正确理解之后，我们才能设定与业务目标同步的具体分析目标。你需要知道客户是想减少信用损失，还是想预测商品价格等等。

## 2.数据理解

![](img/3eb98c7044bf93d5ff6a975bc5a9ef2b.png)

照片由 [Pexels](https://www.pexels.com/photo/charts-on-black-wooden-table-669622/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Lukas](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

业务理解之后，下一步就是数据理解。这包括收集所有可用的数据。在这里，您需要与业务团队密切合作，因为他们实际上知道存在什么数据，什么数据可以用于这个业务问题以及其他信息。这一步包括描述数据、它们的结构、它们的相关性、它们的数据类型。使用图形绘制来浏览数据，并提取通过浏览数据可以获得的任何信息。

## 3.数据准备

![](img/f860383dfe59a939665746ba15834e24.png)

照片由[卢卡斯](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/person-holding-pen-pointing-at-graph-590020/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

接下来是数据准备阶段。这包括选择相关数据、通过合并数据集整合数据、清理数据、通过移除或输入缺失值来处理缺失值、通过移除错误数据来处理错误数据、使用箱线图检查异常值并处理它们等步骤。构建新数据，从现有数据中获取新特征。将数据格式化为所需的结构，移除不需要的列和要素。数据准备是整个生命周期中最耗时也是最重要的一步。你的模型会和你的数据一样好。

## 4.探索性数据分析

![](img/9d1d28c93ffb27473af9e167961a1488.png)

照片由[像素](https://www.pexels.com/photo/clear-light-bulb-355948/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的 [Pixabay](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

这一步包括在构建实际模型之前，对解决方案和影响它的因素有所了解。数据在一个特征的不同变量中的分布使用条形图进行图形探索，不同特征之间的关系通过散点图和热图等图形表示来捕捉。许多其他数据可视化技术被广泛用于单独探索每个要素，以及将它们与其他要素相结合。

## 5.数据建模

![](img/d7768b6c1cfb80bb6a6074299eecfd7d.png)

照片由来自[佩克斯](https://www.pexels.com/photo/black-and-gray-laptop-computer-546819/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[路易斯·戈麦斯](https://www.pexels.com/@luis-gomes-166706?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

数据建模是数据分析的核心。模型将准备好的数据作为输入，并提供所需的输出。这一步包括选择适当的模型类型，无论问题是分类问题、回归问题还是聚类问题。在选择了模型系列之后，在该系列的各种算法中，我们需要仔细选择算法来实现它们。我们需要调整每个模型的超参数，以实现所需的性能。我们还需要确保在性能和可推广性之间有一个正确的平衡。我们不希望模型学习数据，而在新数据上表现不佳。

## 6.模型评估

![](img/e30ac629994429de9b8553e484f701ef.png)

照片由[维塔利·弗拉索夫](https://www.pexels.com/@ihasdslr?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[派克斯](https://www.pexels.com/photo/person-pointing-numeric-print-1342460/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

在这里，评估模型是为了检查它是否准备好被部署。该模型在一个看不见的数据上被测试，在一组仔细考虑过的评估标准上被评估。我们还需要确保模型符合现实。如果我们在评估中没有获得满意的结果，我们必须重复整个建模过程，直到达到期望的度量级别。任何数据科学解决方案，一个[机器学习模型](https://www.educba.com/machine-learning-models/)，就像人类一样，应该进化，应该能够用新数据改进自己，适应新的评估指标。我们可以为某个现象建立多个模型，但很多模型可能并不完美。模型评估帮助我们选择和建立一个完美的模型。

## 7.模型部署

![](img/6d5bac80d07c9bf9ed5cfbcc1a776dca.png)

照片由[克里斯蒂娜·莫里洛](https://www.pexels.com/@divinetechygirl?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从 [Pexels](https://www.pexels.com/photo/woman-standing-while-carrying-laptop-1181354/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 拍摄

经过严格评估的模型最终以期望的格式和渠道部署。这是数据科学生命周期的最后一步。上面解释的数据科学生命周期中的每一步都应该认真对待。如果任何一步执行不当，都会影响下一步，整个努力都白费了。例如，如果数据收集不当，您将会丢失信息，并且无法构建完美的模型。如果数据清理不当，模型将不起作用。如果模型评估不当，它将在现实世界中失败。从业务理解到模型部署，每一步都应该给予适当的关注、时间和努力。

人们经常混淆数据科学项目和软件工程项目的生命周期。情况不应该是这样的，因为数据科学更多的是科学，而不是工程。没有适用于所有数据科学项目的通用工作流流程，数据科学家必须确定哪个工作流最符合业务要求。上面描述的工作流程是基于一个最古老和最流行的工具——CRISP DM。它是为数据挖掘项目开发的，但现在也被大多数数据科学家采用，并根据数据科学项目的要求进行了修改。CRISP-DM 仍然是数据挖掘和数据科学项目的顶级方法/工作流，43%的项目使用它。

数据科学项目生命周期中的每一步都依赖于各种数据科学家技能和数据科学工具。它从提出一个有趣的业务问题开始，这个问题指导着数据科学项目的整个工作流程。数据科学项目生命周期是一个研究和发现的迭代过程，为使用预测模型所需的任务提供指导。从业务理解到模型部署，每一步都应该给予适当的关注、时间和努力。

感谢阅读！

如果你喜欢这篇文章，请随意查看<https://seekingwithin.substack.com/>****——**每周一期的时事通讯，涵盖了广泛的话题，探讨了这样一个信念:我们寻求的大多数答案就在我们自己身上。**

**想和我交流吗？通过[电子邮件](http://mailto:reuben3699@gmail.com/)、 [LinkedIn](https://linkedin.com.in/reubence) 或 [Twitter](https://twitter.com/reuben_rapose) 给我发消息！**