# 十月版:数据科学遇上运动

> 原文：<https://towardsdatascience.com/october-edition-data-science-meets-sports-e66514480c62?source=collection_archive---------17----------------------->

## [月刊](https://towardsdatascience.com/tagged/monthly-edition)

## 探索体育界最容易接受数据科学解决方案的领域

![](img/c50589fa26100c01da1be91a7743f00f.png)

由[张秀坤·库恩](https://unsplash.com/@dominikkuhn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

数据科学是一个不断扩展到新行业的广阔领域，提供了大量有价值的产品和服务。体育产业——一个非常有利可图的产业——不能错过这个机会。

从体育数据中提取有意义的见解并不是最近的事。数据分析早在 20 世纪 90 年代初就进入了体育领域，从那时起，每个人——从运动员个人到大联盟——都在使用它来服务于表现、营销和其他目标。如今，当尖端技术以及最先进的机器和深度学习模型被广泛利用时，体育数据分析有时会承诺太多。尽管如此，到 2022 年底，它有望成长为一个 40 亿美元的产业！

为了更好地描述数据科学在体育领域的应用，我们可以将该领域分为两个主要领域:

1.  **数据分析** —操纵大量数据集的广阔领域，以生成关于比赛*结果*和球员*转会*等统计数据的有意义的见解。它主要由俱乐部、独立分析师甚至大学进行，服务于俱乐部本身或球迷(在博彩的背景下)。
2.  **体育科学**——一个特殊的领域，汇集了许多有助于提高俱乐部和运动员*效率的应用*。在这里，我们观察优化球队打法(球员位置等)的技巧。)、球员能力(如任意球)、健康或体能(如伤病预测)。所有这些都指向同一个目标:降低成本或收益最大化。

亲爱的 TDS 读者:这是你下一个项目的一些思考材料😉)，可以使用机器学习算法的一些典型情况包括:

1.  **分类**

将比赛相关数据的数据集加载到分类器(可能是神经网络)中，以预测未来比赛的结果(赢、输、平)——这本质上是一个多标签分类问题。

2.**回归**

操纵基于球员的数据集，解释他们的任何能力(如加速)，并预测其在比赛中各自的水平。球队的医疗和训练人员经常使用这种算法，以便更好地监控球员并制定他们的训练方案。

3.**集群**

聚集比赛中球员表现的数据集(例如，在足球比赛中，这可能意味着传球的准确性、覆盖的距离等。)，通过仅保留获胜实例来对其进行分段，并以最佳样本包含在一个或两个聚类中的方式对其进行聚类。在现在聚类的数据集上，训练分类器来预测任何感兴趣的玩家的过去游戏的标签。那些其性能将他们分配到最佳集群的人可以被认为是交易的主要目标！

4.**计算机视觉**

这一切都是为了让计算机能够从数字图像或视频中获得高层次的理解。作为一个相当新的跨学科科学领域，它几乎没有现有的文献，直到最近。但是大量的新论文处理这种类型的复杂事件识别。在体育领域，我们可以看到算法被用来检测球员在球场上的位置和动作，并推断出如何构建进攻和防守策略的见解。

总而言之，数据和体育是齐头并进的:球员、经理、教练和球迷基于分析做出决策。这就是为什么近年来出现了大量以体育为导向的数据工作(即体育分析师、数据球探等。)有一个共同的目标:揭开这个利润丰厚的行业的神秘面纱，一次一个数据集…

这里有一些你能在 TDS 档案中找到的最好的体育和数据相关的帖子。

[Gerasimos Plegas](https://medium.com/u/3ea2b50f5cb8?source=post_page-----e66514480c62--------------------------------) ，*TDS 的志愿编辑助理*

## [AI 能让你成为更好的运动员吗？](/can-ai-make-you-a-better-athlete-using-machine-learning-to-analyze-tennis-serves-and-penalty-kicks-f9dd225cea49)

利用机器学习分析网球发球和点球

戴尔·马科维茨 — 11 分钟

## [使用自然语言处理嵌入足球语言](/embedding-the-language-of-football-using-nlp-e52dc153afa6)

使用最先进的 NLP 算法构建体育分析领域未来机器学习解决方案的表示

到 [Ofir Magdaci](https://medium.com/u/a94f282199e5?source=post_page-----e66514480c62--------------------------------) — 13 分钟

## [用 SQL 为 2021 年东京奥运会做准备](/studying-up-for-the-tokyo-2021-olympics-with-sql-719a0ae3779b)

用 Python 查询 PostgreSQL 数据库，用 Pandas 显示结果，用 Matplotlib 可视化

由[塞加尔·杜瓦](https://medium.com/u/e353ddb0c125?source=post_page-----e66514480c62--------------------------------) — 18 分钟

## [使用 Python Pandas、Keras、Flask、Docker 和 Heroku 的端到端机器学习项目](/an-end-to-end-machine-learning-project-with-python-pandas-keras-flask-docker-and-heroku-c987018c42c7)

预测体育比分，从数据争论到模型部署

瑞安·兰姆 — 7 分钟

## [数据科学家能代替 NBA 球探吗？最佳转会建议的 ML 应用程序开发](/can-a-data-scientist-replace-a-nba-scout-ml-app-development-for-best-transfer-suggestion-f07066c2773)

使用 NBA API 创建自己的 ML 模型并预测最佳球员交易

由 [Gerasimos Plegas](https://medium.com/u/3ea2b50f5cb8?source=post_page-----e66514480c62--------------------------------) — 12 分钟

## [棒球迷统计:投球版](/stats-for-baseball-fans-the-single-metric-for-pitching-is-era-e615a6c0710d)

一名数据科学家表示，作为一名普通球迷，ERA 是最重要的统计数据

由考特尼·佩里戈——10 分钟

## [如何用 Python 可视化数据中的隐藏关系——分析 NBA 辅助](/how-to-visualize-hidden-relationships-in-data-with-python-analysing-nba-assists-e480de59db50)

使用交互式快照、气泡图和桑基图操纵和可视化数据，通过 Plotly 获得洞察力(代码和数据在我的 [GitLab repo](https://gitlab.com/jphwang/online_articles) 中)

由 [JP Hwang](https://medium.com/u/964fe0870229?source=post_page-----e66514480c62--------------------------------) — 12 分钟

## [通过数据分析理解网球比赛中一发的重要性](/understanding-the-importance-of-first-serve-in-tennis-with-data-analysis-4829ab088d36)

我们能根据一个网球运动员的第一次发球来判断他的表现吗？

由[安德里亚·卡扎罗](https://medium.com/u/e3cc32d07fb2?source=post_page-----e66514480c62--------------------------------) — 9 分钟

## [体育分析:国际足球比赛的探索性分析——第一部分](/sports-analytics-an-exploratory-analysis-of-international-football-matches-part-1-e133798295f7)

有可能通过强大的分析工具研究体育市场是一个巨大的附加值。

到[瓦伦蒂娜·阿尔托](https://medium.com/u/341264d69dd4?source=post_page-----e66514480c62--------------------------------) — 9 分钟

## [羽毛球比赛中的发球、得分领先和连续得分](/service-point-lead-and-consecutive-points-in-badminton-games-d6abb86ea5ab)

特定指标如何影响羽毛球运动员的心态和表现

由[马潇湘](https://medium.com/u/4916257b918e?source=post_page-----e66514480c62--------------------------------) — 8 分钟

## [美丽的游戏](/o-jogo-bonito-predicting-the-premier-league-with-a-random-model-1b02fa3a7e5a)

用随机模型预测英超联赛

作者:Tuan Nguyen Doan — 7 分钟

## [使用 Python 在 Peloton 中踩踏板](/python-pandas-and-the-peloton-aa024ca74fa5)

使用基础数据分析对环法自行车赛进行分析

到了[威尔·克劳利](https://medium.com/u/dc95dc921204?source=post_page-----e66514480c62--------------------------------) — 12 分钟

最后，这是一个欢迎所有在过去一个月加入我们的优秀作家的伟大时刻——有你加入 TDS 真是太好了！他们包括[迪维娅·戈皮纳特](https://medium.com/u/2519b46a1fe5?source=post_page-----e66514480c62--------------------------------)、[乔伊塔·巴塔查里亚](https://medium.com/u/896d7cce16e4?source=post_page-----e66514480c62--------------------------------)、[妮娜·斯威尼](https://medium.com/u/6e209ee7d400?source=post_page-----e66514480c62--------------------------------)、[阔克·蒂恩奥](https://medium.com/u/1cd979d1204f?source=post_page-----e66514480c62--------------------------------)、[米兰·伦纳德](https://medium.com/u/42b8ff0cf81f?source=post_page-----e66514480c62--------------------------------)、[大卫·恩杜克武博士](https://medium.com/u/2a977b6b20d3?source=post_page-----e66514480c62--------------------------------)、[凯丽·哈里里](https://medium.com/u/8846cdc5e839?source=post_page-----e66514480c62--------------------------------)、[阿肖克·奇拉卡帕蒂](https://medium.com/u/cc37b40eae29?source=post_page-----e66514480c62--------------------------------)、[丹尼尔·赫尔克特](https://medium.com/u/38e7a70a76e2?source=post_page-----e66514480c62--------------------------------)、[丹尼尔·古兹曼](https://medium.com/u/8c7d2efa99b?source=post_page-----e66514480c62--------------------------------) [](https://medium.com/u/8c7d2efa99b?source=post_page-----e66514480c62--------------------------------) [杰西卡·达弗伦](https://medium.com/u/f7d3236aa7d5?source=post_page-----e66514480c62--------------------------------)，[克里斯·贝克特](https://medium.com/u/84b900a5d79?source=post_page-----e66514480c62--------------------------------)，[埃米尔·里肯](https://medium.com/u/95ae6f4e7791?source=post_page-----e66514480c62--------------------------------)，[克利斯朵夫·布莱法里](https://medium.com/u/b8c2e41fcf16?source=post_page-----e66514480c62--------------------------------)，[克莱夫·西维欧](https://medium.com/u/3baa7f2e1e90?source=post_page-----e66514480c62--------------------------------)，[阿努克·杜特尔埃](https://medium.com/u/d153542b2be5?source=post_page-----e66514480c62--------------------------------)，[尼费西米·阿德莫耶](https://medium.com/u/3cf3dabffa0?source=post_page-----e66514480c62--------------------------------)，[莉莉·吴](https://medium.com/u/33299079c80d?source=post_page-----e66514480c62--------------------------------)，[托马斯·鲍姆加特纳](https://medium.com/u/2a9c94b128ae?source=post_page-----e66514480c62--------------------------------)，[拉韦娜·贾亚季耶夫](https://medium.com/u/cb567727317e?source=post_page-----e66514480c62--------------------------------) [我们邀请你看看他们的简介，看看他们的工作。](https://medium.com/u/cb567727317e?source=post_page-----e66514480c62--------------------------------)