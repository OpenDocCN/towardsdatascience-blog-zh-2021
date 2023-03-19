# 无监督机器学习解释

> 原文：<https://towardsdatascience.com/unsupervised-machine-learning-explained-1ccc5f20ca29?source=collection_archive---------18----------------------->

## 它是什么，方法和应用

![](img/f1df8c37fe25598aefd319c0ac0ac591.png)

Jase Bloor 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我们想要发现数据的底层结构时，无监督学习是一个很好的解决方案。与监督学习相比，我们不能将非监督方法应用于分类或回归类型的问题。这是因为无监督的 ML 算法从未标记的数据中学习模式，而我们需要知道输入-输出映射来执行分类或回归(在大多数情况下，我将在后面谈到这一点)。本质上，我们的无监督学习算法将发现数据中的隐藏模式或分组，而不需要人类(或任何人)标记数据或以任何其他方式干预。

</the-difference-between-classification-and-regression-in-machine-learning-4ccdb5b18fd3>  

## 接近无监督学习

当我们的数据未被标记时，这种学习方法通常被利用。例如，如果我们想要确定我们想要发布的新产品的目标市场是什么，我们将使用无监督学习，因为我们没有目标市场人口统计的历史数据。执行无监督学习时主要有三个任务(不分先后):1)聚类 2)关联规则 3)降维。让我们更深入地研究每一个。

**聚类**

从理论上讲，同一组中的实例将具有相似的属性和/或特征。聚类包括根据相似性和差异对未标记的数据进行分组，因此，当两个实例出现在不同的组中时，我们可以推断它们具有不同的属性和/或特征。

这种无监督学习的方法非常流行，并且可以进一步细分为不同类型的聚类，例如排他聚类、重叠聚类、层次聚类和概率聚类——这些方法的细节超出了本文的范围。一种流行的聚类算法是 [K 均值聚类](https://en.wikipedia.org/wiki/K-means_clustering)。

**关联**规则**规则**

关联规则学习是一种基于规则的机器学习方法，用于发现给定数据集中变量之间的有趣关系。该方法的目的是使用感兴趣的度量来识别在数据中发现的强规则。这些方法通常用于市场购物篮分析，使公司能够更好地了解各种产品之间的关系。有许多不同的算法用于关联规则学习，但最广泛使用的是 [Apriori 算法](https://en.wikipedia.org/wiki/Apriori_algorithm)。

**降维** **降维**

无监督学习的另一种形式是降维。这是指将数据从高维空间转换到低维空间，使得低维空间保留原始数据的含义属性。我们降低数据维度的一个原因是为了简化建模问题，因为更多的输入特征会使建模任务更具挑战性。这就是所谓的 [*维度诅咒*](https://en.wikipedia.org/wiki/Curse_of_dimensionality) 。我们这样做的另一个原因是为了可视化我们的数据，因为可视化超过 3 维的数据是困难的。

在机器学习工作流程的[的数据预处理或解释性数据分析(EDA)阶段，通常会采用降维技术。流行的算法有](/the-machine-learning-workflow-1d168cf93dea)[主成分分析](/algorithms-from-scratch-pca-cde10b835ebc)、[奇异值分解](https://en.wikipedia.org/wiki/Singular_value_decomposition)、[自动编码器](https://en.wikipedia.org/wiki/Autoencoder#:~:text=An%20autoencoder%20is%20a%20type,unlabeled%20data%20(unsupervised%20learning).&text=The%20autoencoder%20learns%20a%20representation,data%20(%E2%80%9Cnoise%E2%80%9D).)。

# 无监督学习的应用

当我们谈论机器学习时，很容易看出监督学习在商业中的位置，但在无监督学习中就不那么容易了。一个原因可能是，无监督学习自然会引入更高的不准确结果风险，因为没有什么可以衡量算法得出的结果——企业可能不愿意承担这种风险。

尽管如此，它仍然为查看数据提供了一个很好的探索途径，使企业能够发现数据中的模式，比他们手动观察数据要快。

无监督学习在现实世界中的常见应用包括:

*   **新闻分段**:众所周知，谷歌新闻利用无监督学习对来自不同新闻媒体的基于同一故事的文章进行分类。例如，足球(为我在大西洋彼岸困惑的朋友准备的足球)转会窗口的结果都可以归类到足球下。
*   **计算机视觉**:物体识别等视觉感知任务利用无监督学习。
*   **异常检测:**无监督学习用于识别偏离数据集正常行为的数据点、事件和/或观察值。
*   客户角色:可以使用无监督学习创建有趣的买家角色档案。这有助于企业了解客户的共同特征和购买习惯，从而使企业能够更好地调整产品。
*   **推荐引擎**:过去的购买行为与无监督学习相结合，可以帮助企业发现数据趋势，从而制定有效的交叉销售策略。

## 最后的想法

无监督学习是发现未标记数据的潜在模式的好方法。这些方法通常对分类和回归问题毫无用处，但我们可以使用无监督学习和监督学习的混合方法。这种方法被称为*半监督学习*——我将在另一篇文章中更深入地讨论这一点。

感谢阅读！

如果你喜欢这篇文章，请通过订阅我的免费**[每周简讯](https://mailchi.mp/ef1f7700a873/sign-up)与我联系。不要错过我写的关于人工智能、数据科学和自由职业的帖子。**

## **相关文章**

**</the-best-resources-to-learn-python-for-machine-learning-data-science-9fccd66fe943>  <https://medium.com/geekculture/how-to-get-paid-to-learn-data-science-machine-learning-37c9088ab925>  </the-best-machine-learning-blogs-resources-to-follow-f8177ab4f08f> **