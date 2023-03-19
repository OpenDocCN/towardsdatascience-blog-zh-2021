# 实践知识比理论更重要吗？

> 原文：<https://towardsdatascience.com/is-hands-on-knowledge-more-important-than-theory-52352d2d7f6c?source=collection_archive---------20----------------------->

我们几乎每周都会在 TDS 上看到这些争论的潮涨潮落:数据科学家应该首先掌握高层次的概念并熟练掌握，比如说，概率论——还是直接进入(偶尔)模型调整和数据清理的混乱世界？事实上，我们阅读和分享的最好的帖子无缝地融合了数据科学的这两个方面。本周阵容也不例外；我们比平时更注重实践和动手操作，但是不要担心:我们的作者总是看到森林和树木。我们开始吧！

*   [**学习如何绕开自回归模型中的盲点**](/pixelcnns-blind-spot-84e19a3797b9) 。[杰西卡·达弗伦](https://medium.com/u/f7d3236aa7d5?source=post_page-----52352d2d7f6c--------------------------------)、[沃尔特·雨果·洛佩兹·皮纳亚](https://medium.com/u/a1dadbc02295?source=post_page-----52352d2d7f6c--------------------------------)和[佩德罗·费雷拉·达科斯塔](https://medium.com/u/11b4d70f7a77?source=post_page-----52352d2d7f6c--------------------------------)继续他们之前[在 DeepMind 的 PixelCNN 上的工作](/autoregressive-models-pixelcnn-e30734ede0c1)，“一种深度神经网络，可以捕捉其参数中像素之间的相关性分布。”在他们的最新帖子中，他们解决了该模型的最大限制之一——“盲点问题”——并展示了如何解决它。
*   [**结识(或再认识)SMOTE**](/smote-fdce2f605729) 。在他最近的解释者中， [Joos Korstanje](https://medium.com/u/8fa2918bdae8?source=post_page-----52352d2d7f6c--------------------------------) 向读者介绍了 SMOTE(合成少数过采样技术)，这是一种机器学习方法，可以消除因不平衡数据集而出现的问题。Joos 从大图开始，然后深入细节，用 Python 分享了一个完整的例子。

![](img/babc59531f45edd0a9783ed75c7a8393.png)

凯瑟琳·沃尔科夫斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

*   [**探索真实世界中隐马尔可夫模型的用例**](/hidden-markov-models-an-overview-98926404da0e) 。“有许多分析序列数据的工具，”T4·菲尔德卡迪说，“它们各有所长。”在本演练中，重点介绍了隐马尔可夫模型，菲尔德解释了它们如何工作，以及在什么情况下使用它们比使用 LSTM 模型更有意义。
*   [**给你的熊猫数据帧添加颜色**](/prettifying-pandas-dataframes-75c1a1a6877d) 。设计数据框架的样式不仅仅是美观——正如 Zolzaya Luvsandorj 在她便利的教程中所展示的，添加颜色、渐变和其他视觉提示会使它们更吸引人，更容易分析和记忆。
*   [**掌握一个强大的减方差方法(或几种)**](/online-experiments-tricks-variance-reduction-291b6032dcd7) 。分层，CUPED，方差加权估计量…你应该使用哪一个来确保你的 A/B 测试或在线实验有很高的统计能力？ [Sophia Yang](https://medium.com/u/ae9cae9cbcd2?source=post_page-----52352d2d7f6c--------------------------------) 向我们介绍了一些最有效的方法，并分享了足够多的代码示例，足以让您忙上一阵子。
*   [**了解一些我们最受欢迎的技巧文章**](/tips-tricks-and-tools-of-the-trade-7-popular-posts-you-should-read-6853d9fefa09) 。寻找关于工具、方法和学习策略的更可行的建议？不要错过我们最近由[拉希达·纳斯林·苏克伊](https://medium.com/u/8a36b941a136?source=post_page-----52352d2d7f6c--------------------------------)、[莎兰·库马尔·拉温德兰](https://medium.com/u/9fc8dfce153b?source=post_page-----52352d2d7f6c--------------------------------)、[萨拉·a·梅特沃利](https://medium.com/u/7938431b336a?source=post_page-----52352d2d7f6c--------------------------------)和[阿利亚克塞·米哈伊留克](https://medium.com/u/30bef13bba71?source=post_page-----52352d2d7f6c--------------------------------)等人撰写的热门帖子的综述。

我们希望你本周学到了一些新的和令人兴奋的东西，无论是在 TDS 上还是在你生活中一个完全不同的领域。感谢您阅读我们的帖子；如果你正在寻找一种亲身实践的方式来支持我们的工作，可以考虑[成为一名中级会员](https://medium.com/membership)。

直到下一个变量，
TDS 编辑

**附言**敬请关注——[Jeremie Harris](https://medium.com/u/59564831d1eb?source=post_page-----52352d2d7f6c--------------------------------)和 TDS 播客即将回归，带来新一季和令人兴奋的新嘉宾阵容。

# 我们策划主题的最新内容:

## [入门](https://towardsdatascience.com/tagged/getting-started)

*   [在 R 中制作精美的 3D 剧情——增强由](/make-beautiful-3d-plots-in-r-an-enhancement-on-the-story-telling-613ddd11e98)[西楚张](https://medium.com/u/4bc88b1b8f22?source=post_page-----52352d2d7f6c--------------------------------)创作的讲故事
*   [探索和理解你的数据与一个重要的关联网络](/explore-and-understand-your-data-with-a-network-of-significant-associations-9a03cf79d254)作者 [Erdogan Taskesen](https://medium.com/u/4e636e2ef813?source=post_page-----52352d2d7f6c--------------------------------)
*   [为什么马尔可夫决策过程在强化学习中很重要？](/why-does-malkov-decision-process-matter-in-reinforcement-learning-b111b46b41bd)由[真理子泽田](https://medium.com/u/505e57a528bb?source=post_page-----52352d2d7f6c--------------------------------)

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

*   [王思然·尼万](/particle-filter-localization-with-webots-and-ros2-619ecf0c5f08)[用 Webots 和 ROS2](https://medium.com/u/cfe9f4cd0e8f?source=post_page-----52352d2d7f6c--------------------------------) 进行粒子滤波定位
*   [如何调好 HDBSCAN](/tuning-with-hdbscan-149865ac2970) 作者[查尔斯·弗伦泽尔](https://medium.com/u/915eeebafbe7?source=post_page-----52352d2d7f6c--------------------------------)
*   [探索机器学习在地球科学中的使用案例](/exploring-use-cases-of-machine-learning-in-the-geosciences-b72ea7aafe2)作者[马丁·帕尔科维奇](https://medium.com/u/6c29787b0cc8?source=post_page-----52352d2d7f6c--------------------------------)

## [深潜](https://towardsdatascience.com/tagged/deep-dives)

*   [在野外寻找家庭](/families-in-the-wild-track-iii-b5651999385e)作者[约瑟夫·罗宾逊博士](https://medium.com/u/8049fa781539?source=post_page-----52352d2d7f6c--------------------------------)
*   [通过](/simulating-traffic-flow-in-python-ee1eab4dd20f) [Bilal Himite](https://medium.com/u/719b2acce22f?source=post_page-----52352d2d7f6c--------------------------------) 用 Python 模拟交通流
*   [AI-Tunes:用人工智能创作新歌](/ai-tunes-creating-new-songs-with-artificial-intelligence-4fb383218146)作者[罗伯特·a·贡萨尔维斯](https://medium.com/u/c97e6c73c13c?source=post_page-----52352d2d7f6c--------------------------------)

## [思想和理论](https://towardsdatascience.com/tagged/thoughts-and-theory)

*   [深度终身学习——从人脑中汲取灵感](/deep-lifelong-learning-drawing-inspiration-from-the-human-brain-c4518a2f4fb9)作者 [Aliaksei Mikhailiuk](https://medium.com/u/30bef13bba71?source=post_page-----52352d2d7f6c--------------------------------)
*   [Konstantin Kutzkov](/explicit-feature-maps-for-non-linear-kernel-functions-171a9043da38)[的非线性核函数](https://medium.com/u/e0e3fe10fe2d?source=post_page-----52352d2d7f6c--------------------------------)的显式特征图
*   [深度学习中的信息瓶颈与降维](/information-bottlenecks-c2ee67015065)作者[Tom bck strm](https://medium.com/u/fc86c596eca3?source=post_page-----52352d2d7f6c--------------------------------)