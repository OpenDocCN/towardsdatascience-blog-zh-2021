# 怎么知道自己的数据够不够好？

> 原文：<https://towardsdatascience.com/how-do-you-know-if-your-data-is-good-enough-174913ed2ad9?source=collection_archive---------26----------------------->

当然，可能会有一些例外，但我们中的大多数人处理数据是为了让人们能够做出好的决策。为了达到一个共同的目标，我们走了数百万条不同的道路:更好地了解这个世界，一次一点点。[罗摩·罗摩克里希南](https://medium.com/u/28748480e8bd?source=post_page-----174913ed2ad9--------------------------------)的“从预测到行动”系列将带领我们到达目的地的序列分解成有时很棘手的部分；最近一期文章聚焦于[为我们要解决的问题组装正确数据集的关键步骤](/from-making-predictions-to-optimizing-actions-an-introduction-to-policy-learning-2-4-9fc46ba8f3d0)。(既然你在这里，[你也不妨重温一下第一部](/from-prediction-to-action-how-to-learn-optimal-policies-from-data-part-1-1edbfdcb725d)。)

![](img/496d3cccd1673fd21a5b728612adc489.png)

照片由[蒂亚戈·加西亚](https://unsplash.com/@diegogarcia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

[定义要查看的正确数据，然后创建一个流程来从中得出可操作的见解](/product-analysts-what-is-that-936978e5c215)是作为产品分析师的 [Kate Gallo](https://medium.com/u/6461b8faf444?source=post_page-----174913ed2ad9--------------------------------) 的工作核心。与一些更常见的以数据为中心的工作描述相比，这是一个鲜为人知的角色，Kate 耐心地向我们介绍了它的来龙去脉。对她来说，产品分析师是作为用户的拥护者与多个合作伙伴一起工作的人:他的角色是“确保用户喜欢并从产品中获得价值”，并通过对精心选择的指标进行深思熟虑的分析来做到这一点。

Rama 和 Kate 的文章从商业决策的角度探讨了数据有效性的话题，但挑战在于知道我们是否在看正确的数据——我们有足够的数据吗？靠谱吗？它准确地代表了世界吗？—更深更广。正如[艾丹·佩平](https://medium.com/u/2a27c91b9110?source=post_page-----174913ed2ad9--------------------------------)在他的第一篇 TDS 文章中所写的，[使观察到的现象可测量的过程需要多种形式的简化](/make-measurable-what-galileo-didnt-say-about-the-subjectivity-of-algorithms-8d1d324253da)——“未能解释数据化的这种简化本质是许多算法伤害和失败的根本原因。”对于 Aidan 来说，如果我们想避免这些危险中最糟糕的情况，仔细的设计和注意我们在这个过程中做出的选择都是必要的。

观察、收集、展示和利用数据的方式有很多——如果你想在本周探索其他一些方向，我们将一如既往地为你提供帮助！):

*   [**发现结构方程建模的威力**](/structural-equations-models-3d8578952f87) 。Laura Castro-Schilo 分享了对 SEM 的有用而全面的介绍，并解释了为什么这种方法成为如此多的行为和社会科学家的最爱。
*   [**恶补最新深度学习研究**](/four-deep-learning-papers-to-read-in-august-2021-7d98385a378d) **。罗伯特·兰格备受期待的八月版学术论文推荐来了！这本书充满了对从视觉变形器到深度学习优化器基准等主题的热情阅读。**
*   [**了解人工智能**](/are-we-thinking-about-ai-wrong-4c826f9615c0) **中的转换范式。最近围绕人工智能的对话往往可以归结为一个简单的人类合作与人类对抗的二元对立。在最新的 TDS 播客中，Jeremie Harris 和嘉宾 Divya Siddarth 探讨了人工智能的其他想法和应用，并探讨了政府在该技术的下一个篇章中可能发挥积极作用的方式。**

我们希望你喜欢本周的阅读——感谢你花时间和我们在一起，感谢你的支持让这一切成为可能。

直到下一个变量，
TDS 编辑

## 我们策划主题的最新内容:

## [入门](https://towardsdatascience.com/tagged/getting-started)

*   [通过](/what-to-look-for-when-scaling-your-data-team-986e00024d6a) [Sheel Choksi](https://medium.com/u/337555da338d?source=post_page-----174913ed2ad9--------------------------------) 扩展您的数据团队时需要寻找什么
*   [数据可视化中颜色的力量](/the-power-of-color-in-data-visualizations-9868d661f2a0)作者[玛丽·勒菲弗尔](https://medium.com/u/2a04bf49928f?source=post_page-----174913ed2ad9--------------------------------)
*   [数据科学家 GitHub 桌面](/github-desktop-for-data-scientists-b9d8a3afc5ea)作者[德鲁·西沃德](https://medium.com/u/dff5f2854781?source=post_page-----174913ed2ad9--------------------------------)

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

*   [2021 年建立营销组合模式的完整指南](/an-complete-guide-to-building-a-marketing-mix-model-in-2021-fb53975be754)作者[特伦斯·申](https://medium.com/u/360a9d4d19ab?source=post_page-----174913ed2ad9--------------------------------)
*   [4 个预提交插件，由](/4-pre-commit-plugins-to-automate-code-reviewing-and-formatting-in-python-c80c6d2e9f5) [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----174913ed2ad9--------------------------------) 在 Python 中自动进行代码审查和格式化
*   [用熊猫很快生成假数据](/generating-fake-data-with-pandas-very-quickly-b99467d4c618)作者[胡安·路易斯·鲁伊斯-塔格勒](https://medium.com/u/c0cd05dc4084?source=post_page-----174913ed2ad9--------------------------------)

## [深潜](https://towardsdatascience.com/tagged/deep-dives)

*   [Thrill-K:下一代机器智能的蓝图](/thrill-k-a-blueprint-for-the-next-generation-of-machine-intelligence-7ddacddfa0fe)作者[加迪·辛格](https://medium.com/u/51de1f48d0b?source=post_page-----174913ed2ad9--------------------------------)
*   [应用贝叶斯推理(上)](/applied-bayesian-inference-pt-1-322b25093f62)由[阿尼·马杜尔卡](https://medium.com/u/c9b0adccc01d?source=post_page-----174913ed2ad9--------------------------------)
*   [单元 7)差异进化——自动机器学习](/unit-7-differential-evolution-automated-machine-learning-eb22014e592e)作者[布兰登·摩根](https://medium.com/u/422735e4a376?source=post_page-----174913ed2ad9--------------------------------)

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

*   [让模型更健壮更高效](/making-models-more-robust-more-efficiently-e8737178452c)作者[丹尼尔·安杰洛夫](https://medium.com/u/b47135f530bb?source=post_page-----174913ed2ad9--------------------------------)
*   [可解释的 K 均值:聚类特征重要性](/interpretable-k-means-clusters-feature-importances-7e516eeb8d3c)作者[尤瑟夫·阿尔霍法伊里](https://medium.com/u/f3cfbe3f803?source=post_page-----174913ed2ad9--------------------------------)
*   [正交系统和傅里叶级数](/orthogonal-system-and-fourier-series-bec96510db98)由[张希楚](https://medium.com/u/4bc88b1b8f22?source=post_page-----174913ed2ad9--------------------------------)