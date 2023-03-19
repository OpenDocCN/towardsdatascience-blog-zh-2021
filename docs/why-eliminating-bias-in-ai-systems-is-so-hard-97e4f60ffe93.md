# 为什么消除人工智能系统中的偏见如此困难

> 原文：<https://towardsdatascience.com/why-eliminating-bias-in-ai-systems-is-so-hard-97e4f60ffe93?source=collection_archive---------25----------------------->

每当我们决定使用什么数据集，选择什么特征，或者如何微调我们的模型时，我们的偏见就会发挥作用。其中一些是中立的，甚至可能是良性的——专业知识也是一种偏见。另一些则具有潜在的歧视性和有害性；我们如何确保这些人置身事外？

简而言之，这非常非常困难。在本周的变量中，我们从这个问题上的三个杰出贡献开始——让我们开始吧。(寻找其他，更技术性的话题？请继续阅读:我们也为您提供保险。)

*   [**了解在 ML/AI 管道中引入偏差的风险**](/eliminating-ai-bias-5b8462a84779) 。正如 [Sheenal Srivastava](https://medium.com/u/42bf180d6f14?source=post_page-----97e4f60ffe93--------------------------------) 在这个全景概览中解释的那样，当我们谈论偏见时，我们实际上想到了多种偏见 *es* ，每种偏见都以不同的方式影响着我们的项目、工作流程和结果。Sheenal 为我们提供了一个全面的路线图，用于识别、解决，并随着时间的推移，从一开始就防止它们发挥作用。
*   [**探索最有效的方法，确保你的模型是可解释的**](/picking-an-explainability-technique-48e807d687b9) 。许多偏见对话的核心是可解释性的问题——或者说缺乏可解释性:如此多的人工智能模型和人工智能系统之所以能够工作，仅仅是因为有一个不透明的过程将输入和输出分开。Divya Gopinath 分享了一个可解释技术的分类法，可以帮助实践者根据上下文和他们的特定需求选择正确的方法。

![](img/32df78b61f0bb6aa922fccc0212efceb.png)

Mick Haupt 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*   [**听一位著名的人工智能伦理研究员讨论公平和偏见的实际一面**](/practical-ai-ethics-639013a782a3) 。在主要科技公司实施人工智能道德多年后，玛格丽特·米歇尔是关于未被发现和未解决的偏见的危险的领先声音。你不会想错过她与 TDS 播客主持人 [Jeremie Harris](https://medium.com/u/59564831d1eb?source=post_page-----97e4f60ffe93--------------------------------) 的生动对话，其中涵盖了包容性、公平问题的分形性质，以及我们在模型中可以容忍的偏见水平和类型。

除了这些关于偏见及其后果的重要讨论之外，在过去的一周里，我们还发布了几十个指南和教程。这很难选择(总是如此)，但这里有三个我们认为你可能会特别喜欢的。

*   [**利用您的数据和 Python 技能解决供应链问题**](/production-planning-and-resource-management-of-manufacturing-systems-in-python-5458e9c04183) 。这些天来，关于产品短缺和运输延迟的讨论无处不在。然而，正如 Will Keefe 所展示的，当涉及到建模需求和约束、准备和安排生产周期以及可视化和交流计划时，您的 Python 知识可以成为一个重要的差异制造者。
*   [**让同事和利益相关者能够充分利用他们自己掌握的数据**](/what-helped-us-build-strong-self-service-analytics-in-a-fintech-startup-ef9f0333b94d) 。许多在行业中工作的数据科学家发现，他们花了太多时间为不太懂数据的同行提供基本的数据见解。姚文玲坚持认为有一个更好的方法:培训内部用户成为有效的数据消费者，甚至是数据创造者。
*   </overfitting-and-conceptual-soundness-3a1fd187a6d4>**对过度拟合(及其重要性)有了更细致的了解。 [Klas Leino](https://medium.com/u/d1ed71c899ca?source=post_page-----97e4f60ffe93--------------------------------) 的帖子对过度拟合提供了清晰而有用的指导，特别是在深度学习的背景下。Klas 向我们介绍了 TruLens 库，并展示了它如何帮助我们避免不健全功能的缺陷，并增加我们对模型在未来未知数据上的性能的信任。**

**我们希望你喜欢这个星期和我们一起度过的时光！如果你想支持我们作者(和我们)的工作，[今天就考虑成为一名中级会员](https://medium.com/membership)。**

**直到下一个变量，
TDS 编辑器**

# **我们策划主题的最新内容:**

## **[入门](https://towardsdatascience.com/tagged/getting-started)**

*   **[由](/build-your-first-mood-based-music-recommendation-system-in-python-26a427308d96) [Max Hilsdorf](https://medium.com/u/d0c085a74ae8?source=post_page-----97e4f60ffe93--------------------------------) 用 Python 构建你的第一个基于情绪的音乐推荐系统**
*   **[贝叶斯定理，由](/bayes-theorem-clearly-explained-with-visualization-5083ea5e9b14) [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----97e4f60ffe93--------------------------------) 用可视化清晰地解释**
*   **[美铁内部的机会:美国铁路](/the-opportunity-within-amtrak-americas-railroad-b9c38ee9b8b8)作者[尼娜·斯威尼](https://medium.com/u/6e209ee7d400?source=post_page-----97e4f60ffe93--------------------------------)**

## **[实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)**

*   **[Jonny Hofmeister](/take-your-python-visualizations-to-the-next-level-with-manim-ce9ad7ff66bf)[的 Manim](https://medium.com/u/4394de03cd56?source=post_page-----97e4f60ffe93--------------------------------) 让您的 Python 可视化更上一层楼**
*   **[我是如何赢得吴恩达首个以数据为中心的人工智能竞赛](/how-i-won-andrew-ngs-very-first-data-centric-ai-competition-e02001268bda)的 [Johnson Kuan](https://medium.com/u/b152ddf632a3?source=post_page-----97e4f60ffe93--------------------------------)**
*   **[Python 3.10:五个新特性和注意事项](/python-3-10-five-new-features-and-considerations-f775c9432887)作者 [Christopher Tao](https://medium.com/u/b8176fabf308?source=post_page-----97e4f60ffe93--------------------------------)**

## **[深潜](https://towardsdatascience.com/tagged/deep-dives)**

*   **[数据结构简介](/intro-to-data-structures-2615eadc343d)作者[马特·索斯纳](https://medium.com/u/f17fb22b897?source=post_page-----97e4f60ffe93--------------------------------)**
*   **[大卫·博雷利](/clustering-sentence-embeddings-to-identify-intents-in-short-text-48d22d3bf02e)[对句子嵌入进行聚类以识别短文本](https://medium.com/u/4ea40dd4f59c?source=post_page-----97e4f60ffe93--------------------------------)中的意图**
*   **[机器人最佳控制:第二部分](/optimal-control-for-robotics-part-2-e9f9acb4027b)作者[纪尧姆·克雷贝](https://medium.com/u/ea42d028f128?source=post_page-----97e4f60ffe93--------------------------------)**
*   **[在 B2B 产品创新中融合精益、设计思维和共同开发——团队回顾](/blending-lean-designing-thinking-and-co-development-in-b2b-product-innovation-a-team-7b9944c59302)**

## **[思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)**

*   **[Parul Pandey](/overcoming-imagenet-dataset-biases-with-pass-6e54c66e77a)[通过](https://medium.com/u/7053de462a28?source=post_page-----97e4f60ffe93--------------------------------)克服 ImageNet 数据集偏差**
*   **当社交距离成为规则时，人们如何移动？作者[马克西米利安·法尚](https://medium.com/u/5e10ab5fd7ce?source=post_page-----97e4f60ffe93--------------------------------)**
*   **[SVM 会说话的算法:用内点法训练 SVM](/svm-talking-algos-using-interior-point-methods-for-svm-training-d705cdf78c94)**
*   **[吉他效果的迁移学习](/transfer-learning-for-guitar-effects-4af50609dce1)作者 [Keith Bloemer](https://medium.com/u/744166a323a0?source=post_page-----97e4f60ffe93--------------------------------)**