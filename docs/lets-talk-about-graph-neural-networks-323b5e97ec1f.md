# 让我们来谈谈图形神经网络

> 原文：<https://towardsdatascience.com/lets-talk-about-graph-neural-networks-323b5e97ec1f?source=collection_archive---------19----------------------->

我们每周在 TDS 上发布几十篇新文章，涵盖令人眼花缭乱的主题。我们在这里帮助你避免决策瘫痪:在本周的变量中，我们关注 GNNs(图形神经网络),并邀请你通过三篇杰出的文章来探索机器学习的这个令人兴奋的子域。(如果你对 GNNs 不感兴趣，请向下滚动阅读我们每周的精彩内容。)

*   [**关于 GNNs**](/graph-neural-networks-through-the-lens-of-differential-geometry-and-algebraic-topology-3a7c3c22d5f) ，微分几何和代数拓扑能告诉我们什么？来自[的迈克尔布朗斯坦](https://medium.com/u/7b1129ddd572?source=post_page-----323b5e97ec1f--------------------------------)的新系列总是值得庆祝，他的新系列也不例外。在第一部分中，Michael 为接下来的工作奠定了基础，解决了 GNNs 的一些常见困境，[并建议通过微分几何和代数拓扑的透镜来处理它们](/graph-neural-networks-through-the-lens-of-differential-geometry-and-algebraic-topology-3a7c3c22d5f)可以“为图形机器学习中重要而具有挑战性的问题带来新的视角。”

如果你是 GNNs 的新手，在深入了解 Michael 帖子的本质之前，可以使用一个关于这个主题的可理解的解释器，不用担心！先从 [Shanon Hong](https://medium.com/u/f044fc057968?source=post_page-----323b5e97ec1f--------------------------------) 温柔[的图论介绍](/an-introduction-to-graph-neural-network-gnn-for-analysing-structured-data-afce79f4cfdc)说起，里面也涵盖了 GNNs 能做什么，不能做什么。接下来，直接前往 [Aishwarya Jadhav](https://medium.com/u/4f391d37f2d4?source=post_page-----323b5e97ec1f--------------------------------) 的常年最爱，在那里她[讨论图形神经网络的现实世界应用](/https-medium-com-aishwaryajadhav-applications-of-graph-neural-networks-1420576be574)。

![](img/e557fa448f180c6f97e498e6fa9bea13.png)

[铝烟灰](https://unsplash.com/@anspchee?utm_source=medium&utm_medium=referral)在[未飞溅](https://unsplash.com?utm_source=medium&utm_medium=referral)上的照片

在过去的一周里，我们一直忙于其他几个方面——如果你想了解一些我们最近最好的文章，就让点击和书签开始吧:

*   数据科学如何影响德国卫生部的法律和政策制定？阅读 [Elliot Gunn](https://medium.com/u/aad1101621dd?source=post_page-----323b5e97ec1f--------------------------------) 与 [Lars Roemheld](https://medium.com/u/ea4059de5473?source=post_page-----323b5e97ec1f--------------------------------) 在健康创新中心的 AI &数据主管的富有洞察力的 Q & A，在那里他们[讨论数据科学和政府创新的融合](/data-science-experiments-in-government-f61c692e2ac3)。
*   [该不该监控 ML 车型](/to-monitor-or-not-to-monitor-a-model-is-there-a-question-c0e312e19d03)？这是一个看似简单的问题，但德万西·维尔马(以及合著者马修·弗利吉尔、鲁帕·戈什和阿纳·博斯博士)深入探讨了它的利害关系(以及相关的复杂性)。
*   对于学习新技能的实践方法，请查看 [Parul Pandey](https://medium.com/u/7053de462a28?source=post_page-----323b5e97ec1f--------------------------------) 的最新文章，其中她向[展示了如何使用 GeoPandas](/interactive-geographical-maps-with-geopandas-4586a9d7cc10) 创建交互式地图。
*   你如何[赢得一场 Kaggle 比赛并帮助你的团队](/winning-the-kaggle-google-brain-ventilator-pressure-prediction-2d4c90d831ec)在 2650 名参赛者中脱颖而出？ [Gilles Vandewiele](https://medium.com/u/3e8cbc53806e?source=post_page-----323b5e97ec1f--------------------------------) 带我们回顾他(和他的队友)最近成功的起伏和突破时刻。
*   [人工智能、股票和医疗保健的交集](/healthcare-ais-equity-problem-fca16998a48e)目前是几个关键对话的中心，而[安吉拉·威尔金斯](https://medium.com/u/ced5aa52d6a?source=post_page-----323b5e97ec1f--------------------------------)最近的帖子充实了偏见蔓延和不平等数据的现实世界风险。
*   为了贴近人工智能对社会和社区的影响，[杰瑞米·哈里斯](https://medium.com/u/59564831d1eb?source=post_page-----323b5e97ec1f--------------------------------)与[吉莉安·哈德菲尔德](https://medium.com/u/477e725799c9?source=post_page-----323b5e97ec1f--------------------------------)的 TDS 播客节目报道了监管者和立法者当前在跟上技术进步方面面临的挑战，[指出了人工智能的好处，这些好处不仅可以解释，而且是合理的](/how-to-create-explainable-ai-regulations-that-actually-make-sense-8e6f35866bd8)。

感谢您加入我们为期一周的学习和探索，并参与我们发布的工作。

直到下一个变量，
TDS 编辑器