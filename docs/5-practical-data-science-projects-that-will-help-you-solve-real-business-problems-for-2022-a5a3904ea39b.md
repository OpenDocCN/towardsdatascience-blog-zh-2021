# 5 个实用的数据科学项目将帮助您解决 2022 年的实际业务问题

> 原文：<https://towardsdatascience.com/5-practical-data-science-projects-that-will-help-you-solve-real-business-problems-for-2022-a5a3904ea39b?source=collection_archive---------2----------------------->

## 模拟现实生活问题的数据科学项目精选列表

![](img/4f7ee3ecf778fa18df6bec17250782d7.png)

照片由 [XPS](https://unsplash.com/@xps?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/project?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

> ***一定要*** [***订阅***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学指南、诀窍和技巧、生活经验等的文章！***

> "告别无用的副业."

我开始写文章已经快两年了——相当于刚刚超过 175 篇文章！我以前的一些文章中的一个错误是，我建议的数据科学项目很有趣，**但不实用。**

获得一份数据科学家工作的最简单的方法之一是展示你已经完成了类似的项目，并且在工作岗位上工作过。因此，我想和你分享一些**实用的**数据科学项目，这些项目是我个人在职业生涯中完成的，将会丰富你的经验和你的投资组合。

话虽如此，让我们深入探讨一下:

# 1.客户倾向建模

## 什么？

倾向模型是预测某人做某事的可能性的模型。举几个例子:

*   **网站访问者**将**注册账户**的可能性
*   一个**注册用户**将**付费并订阅**的可能性
*   一个**用户**将**推荐另一个用户**的可能性

此外，倾向建模不仅包括“谁”和“什么”，还包括“什么时候”(你应该什么时候锁定你已经确定的用户)和“如何”(你应该如何向你的目标用户传递你的信息？).

## 为什么？

倾向建模允许你更明智地分配资源，从而提高效率，同时取得更好的结果。举个例子，想想这个:不是发送一封用户点击几率为 0%-100%的电子邮件广告，而是通过倾向建模，你可以锁定点击几率超过 50%的用户。更少的邮件，更多的转化！

## 如何:

下面是演示如何构建基本倾向模型的两个代码演练:

[](https://www.kaggle.com/benpowis/customer-propensity-to-purchase) [## 客户购买倾向

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自客户购买倾向数据集的数据

www.kaggle.com](https://www.kaggle.com/benpowis/customer-propensity-to-purchase) [](https://www.kaggle.com/jalenguzman/marketing-analytics-classification-and-eda#Classification-Algorithms) [## 营销分析、分类和 EDA

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自营销活动的数据

www.kaggle.com](https://www.kaggle.com/jalenguzman/marketing-analytics-classification-and-eda#Classification-Algorithms) 

这里有两个数据集，你可以用来建立一个倾向模型。记下每个数据集中提供的要素类型:

[](https://www.kaggle.com/benpowis/customer-propensity-to-purchase-data) [## 客户购买倾向数据集

### 记录网上商店购物者互动的数据集

www.kaggle.com](https://www.kaggle.com/benpowis/customer-propensity-to-purchase-data) [](https://www.kaggle.com/rodsaldanha/arketing-campaign?select=marketing_campaign.csv) [## 营销活动

### 提高营销活动的利润

www.kaggle.com](https://www.kaggle.com/rodsaldanha/arketing-campaign?select=marketing_campaign.csv) 

> ***一定要*** [***订阅***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学的文章，包括指南、诀窍和技巧、生活经验等！***

# 2.度量预测

## 什么？

指标预测是不言自明的，它指的是在短期内预测给定的指标，如收入或用户总数。

具体来说，预测涉及使用历史数据作为输入来生成预测输出的技术。即使输出本身不完全准确，预测也可以用来衡量特定指标走向的总体趋势。

## 为什么？

预测基本上就像是展望未来。通过预测(带着一定程度的自信)未来会发生什么，你可以更主动地做出更明智的决定。这样做的结果是，你将有更多的时间来做决定，并最终减少失败的可能性。

## 如何:

第一个资源提供了几个时间序列模型的概要:

[](/an-overview-of-time-series-forecasting-models-a2fa7a358fcb) [## 时间序列预测模型综述

### 我们描述了 10 个预测模型，并应用它们来预测工业生产指数的演变

towardsdatascience.com](/an-overview-of-time-series-forecasting-models-a2fa7a358fcb) 

第二个资源提供了使用 Prophet 创建时序模型的分步演练，Prophet 是脸书专门为时序建模构建的 Python 库:

[](https://www.kaggle.com/elenapetrova/time-series-analysis-and-forecasts-with-prophet) [## 使用 Prophet 进行时间序列分析和预测

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用 Rossmann 商店销售数据

www.kaggle.com](https://www.kaggle.com/elenapetrova/time-series-analysis-and-forecasts-with-prophet) 

# 3.推荐系统

## 什么？

推荐系统是一种算法，其目标是向用户建议最相关的信息，无论是亚马逊上的类似产品，网飞上的类似电视节目，还是 Spotify 上的类似歌曲。

推荐系统主要有两种类型:协同过滤和基于内容的过滤。

*   **基于内容的**推荐系统根据先前选择的项目的特征推荐特定的项目。例如，如果我以前看了很多动作片，它会将其他动作片排在更高的位置。
*   **协同过滤**另一方面，基于相似用户的反应过滤用户可能喜欢的项目。比如我喜欢宋 A，别人喜欢宋 A **和**宋 C，那么我就会被推荐宋 C。

## 为什么？

推荐系统是最广泛使用和最实用的数据科学应用之一。不仅如此，就数据产品而言，它还拥有最高的投资回报率。[据估计，亚马逊在 2019 年的销售额增长了 29%，这主要归功于其推荐系统。](https://rejoiner.com/resources/amazon-recommendations-secret-selling-online/#:~:text=%E2%80%9CJudging%20by%20Amazon's%20success,%20the,the%20same%20time%20last%20year.)同样，[网飞声称其推荐系统在 2016 年价值惊人的 10 亿美元](https://www.businessinsider.com/netflix-recommendation-engine-worth-1-billion-per-year-2016-6)！

但是是什么让它如此有利可图呢？正如我前面提到的，这是关于一件事:**相关性**。通过向用户提供更多**相关的**产品、节目或歌曲，你最终会增加他们购买更多和/或保持更长时间参与的可能性。

## 如何:

## 资源和数据集

[](/introduction-to-recommender-systems-1-971bd274f421) [## 推荐系统介绍- 1:基于内容的过滤和协同过滤

### 像网飞、亚马逊和 Youtube 这样的服务是如何向用户推荐商品的？

towardsdatascience.com](/introduction-to-recommender-systems-1-971bd274f421) [](https://www.kaggle.com/shivamb/netflix-shows) [## 网飞电影和电视节目

### 网飞上的电影和电视节目列表

www.kaggle.com](https://www.kaggle.com/shivamb/netflix-shows) [](https://www.kaggle.com/mrmorj/restaurant-recommendation-challenge?select=orders.csv) [## 餐厅推荐挑战

### 建议质询数据

www.kaggle.com](https://www.kaggle.com/mrmorj/restaurant-recommendation-challenge?select=orders.csv) [](https://www.kaggle.com/bricevergnou/spotify-recommendation?select=yes.py) [## Spotify 推荐

### 200 首歌曲及其统计

www.kaggle.com](https://www.kaggle.com/bricevergnou/spotify-recommendation?select=yes.py) 

# 4.深潜分析

## 什么？

深入分析就是对特定问题或主题的深入分析。他们本质上可以是探索性的，以发现新的信息和见解，或者是调查性的，以了解问题的原因。

这不是一个被广泛谈论的技能，部分是因为它来自经验，但这并不意味着你不能提高它！和其他事情一样，这只是一个练习的问题。

## 为什么？

对于任何与数据相关的专业人士来说，深度挖掘都是必不可少的。能够找出某些东西不工作的原因，或者能够找到银弹，是区分伟大和优秀的标准。

## 资源和数据集

以下是几个你可以自己尝试的深度潜水任务:

[](https://www.kaggle.com/ravichaubey1506/healthcare-cost) [## 医疗费用

### 分析威斯康星医院的医疗成本和利用

www.kaggle.com](https://www.kaggle.com/ravichaubey1506/healthcare-cost) [](https://www.kaggle.com/pavansubhasht/ibm-hr-analytics-attrition-dataset) [## IBM HR Analytics 员工流失和绩效

### 预测你有价值的员工的流失

www.kaggle.com](https://www.kaggle.com/pavansubhasht/ibm-hr-analytics-attrition-dataset) [](https://www.kaggle.com/jackdaoud/marketing-data/tasks?taskId=2986) [## 营销分析

### 运用市场数据进行探索性和统计性分析

www.kaggle.com](https://www.kaggle.com/jackdaoud/marketing-data/tasks?taskId=2986) 

# 5.客户细分

## 什么？

客户细分是将客户群分成几个部分的实践。

最常见的细分类型是人口统计，但也有许多其他类型的细分，包括地理、心理、需求和价值。

## 为什么？

细分对企业来说非常有价值，原因有几个:

*   它可以让您进行更有针对性的营销，并向每个细分市场传递更个性化的信息。年轻的青少年比几个孩子的父母更看重不同的东西。
*   当资源有限时，它允许您优先考虑特定的细分市场，尤其是那些利润更高的细分市场。
*   细分也是追加销售和交叉销售等其他应用的基础。

## 如何:

[](https://www.kaggle.com/fabiendaniel/customer-segmentation) [## 客户细分

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自电子商务数据的数据

www.kaggle.com](https://www.kaggle.com/fabiendaniel/customer-segmentation) [](https://www.kaggle.com/kushal1996/customer-segmentation-k-means-analysis) [## 客户细分(K 均值)|分析

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自商场客户细分数据的数据

www.kaggle.com](https://www.kaggle.com/kushal1996/customer-segmentation-k-means-analysis) 

## 数据集

[](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python) [## 商场客户细分数据

### 市场篮子分析

www.kaggle.com](https://www.kaggle.com/vjchoudhary7/customer-segmentation-tutorial-in-python) [](https://www.kaggle.com/kaushiksuresh147/customer-segmentation) [## 客户细分分类

### 将顾客分为四类

www.kaggle.com](https://www.kaggle.com/kaushiksuresh147/customer-segmentation) 

# 感谢阅读！

我希望这对您的数据科学之旅有所帮助！掌握这些技能不仅表明你在技术上是可靠的，还表明你知道如何开展增加商业价值的数据科学项目。对任何一位招聘经理说这句话，我保证他们会印象深刻；).

一如既往，我祝你学习一切顺利！

> ***如果您喜欢这篇文章，请务必*** [***订阅***](https://terenceshin.medium.com/membership) ***千万不要错过另一篇关于数据科学指南、技巧和提示、生活经验等的文章！***

不确定接下来要读什么？我为你挑选了另一篇文章:

[](/10-most-practical-data-science-skills-you-should-know-in-2022-9487d7750e8a) [## 2022 年你应该知道的 10 个最实用的数据科学技能

### 实际上能让你就业的技能

towardsdatascience.com](/10-most-practical-data-science-skills-you-should-know-in-2022-9487d7750e8a) 

**还有一个:**

[](/all-probability-distributions-explained-in-six-minutes-fe57b1d49600) [## 六分钟内解释所有概率分布

towardsdatascience.com](/all-probability-distributions-explained-in-six-minutes-fe57b1d49600) 

# 特伦斯·申

*   ***如果你喜欢这个，*** [***订阅我的媒介***](https://terenceshin.medium.com/membership) ***获取独家内容！***
*   ***同样，你也可以*** [***关注我上媒***](https://medium.com/@terenceshin)
*   ***跟我上***[***LinkedIn***](https://www.linkedin.com/in/terenceshin/)***其他内容***