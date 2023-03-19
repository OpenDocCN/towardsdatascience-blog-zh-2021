# 为初学者解码 10 大数据科学术语(在面试中经常被问到)

> 原文：<https://towardsdatascience.com/decoding-the-top-10-data-science-jargons-for-beginners-commonly-asked-in-interviews-436b5afbe3c0?source=collection_archive---------3----------------------->

## 用简单的英语解释并附有参考

![](img/f19759fbbfe62d429d19d0b6fecc6340.png)

在 [Unsplash](https://unsplash.com/s/photos/job-interview?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上[猎人赛跑](https://unsplash.com/@huntersrace?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)的照片

这篇文章是关于解码数据科学中使用的一些流行术语。更好地理解这些概念很重要。他们通常在数据科学工作面试中被问到。让我们进入主题。

# 因变量和自变量

因变量(目标变量)由研究中的自变量驱动。例如，零售商店的收入取决于走进商店的顾客数量。这里商店收入是因变量。走进商店的顾客数量是自变量。因变量之所以这么叫，是因为它的值依赖于自变量。此外，独立变量之所以如此称呼，是因为它们独立于可能影响因变量的其他变量。就像，降雨量(自变量)与走进商店的顾客数量无关。这两个独立变量都有助于做出更好的预测。

在研究预测数据科学问题时。通常会有一个因变量和多个自变量。下面是一个很好的资源，可以更好地理解因变量和自变量。

[](https://www.scribbr.com/methodology/independent-and-dependent-variables/) [## 自变量和因变量

### 在研究中，变量是可以取不同值的任何特征，如身高、年龄、物种或考试…

www.scribbr.com](https://www.scribbr.com/methodology/independent-and-dependent-variables/) 

# 极端值

异常值是不在变量正常范围内的值。例如，平均寿命在 70 岁左右。119 岁的个体被认为是异常值，因为他的年龄大大超出了正常范围。在处理数据科学问题时，通常会检查数据集中的异常值。如果出现预测问题，数据中的异常值可能会影响算法的选择。

这里有一篇详细的文章，讨论了异常值检测中常用的技术。

[](/a-brief-overview-of-outlier-detection-techniques-1e0b2c19e561) [## 离群点检测技术概述

### 什么是离群值，如何处理？

towardsdatascience.com](/a-brief-overview-of-outlier-detection-techniques-1e0b2c19e561) 

# 序数数据

当分类数据中包含推断序列时，它就是序数数据。例如，机票的类别是有序数据。有一个类似头等舱和二等舱的顺序。

当我们有有序分类数据时，最好使用整数编码。只需将它们转换成与推断序列对齐的整数表示。通过这种方式，算法将能够寻找模式。比如，当变量的值增加或减少时，它如何影响结果。

下面是一篇非常好的文章，可以学习更多关于分类数据编码的最佳方法

[](/smarter-ways-to-encode-categorical-data-for-machine-learning-part-1-of-3-6dca2f71b159) [## 为机器学习编码分类数据的更智能方法

### 探索类别编码器

towardsdatascience.com](/smarter-ways-to-encode-categorical-data-for-machine-learning-part-1-of-3-6dca2f71b159) 

# 一键编码

一键编码是一种数据转换技术，有助于将分类属性转换为数字表示。独热编码的主要优点是它有助于避免对 ML 模型的任何混淆。

用更简单的术语来解释，诸如性别、城市、国家之类的属性是非顺序的。非序数意味着它们内部没有顺序，也就是说所有的性别都是一样的。当我们将这种非顺序属性转换成整数时，许多算法认为较高的值更重要/不太重要。虽然没有任何这样的关系。这个问题可以通过使用一键编码将非序数属性转换成二进制表示来解决。

要了解有关实现热编码的更多信息，请阅读下面的内容。

[](https://www.educative.io/blog/one-hot-encoding) [## 5 分钟内的数据科学:什么是热门编码？

### 一种热门的编码是将分类数据变量转换成数值的过程。学习如何热…

www.educative.io](https://www.educative.io/blog/one-hot-encoding) 

# 偏斜度和峰度

偏斜度是理解数据分布的一种度量。当数据的偏斜度接近 0 时，意味着数据接近对称分布。当分布的左侧与右侧完全相同时，该分布是对称的。当数据呈负偏态时，意味着大多数数据点大于平均值。在正偏态数据中，大多数数据点小于平均值。

峰度也是一种更好地理解数据分布的度量。当数据具有正峰度时，意味着与正态分布相比，该分布具有更高的峰值。这实际上意味着可能有许多异常值。

下面是一篇非常好的文章，用可视化的表示方式来更好的理解偏度和峰度。

[](https://www.analyticsvidhya.com/blog/2021/05/shape-of-data-skewness-and-kurtosis/) [## 偏度和峰度|数据的形状:偏度和峰度

### 理解数据的形状是一个至关重要的行动。这有助于了解大部分信息在哪里，以及…

www.analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2021/05/shape-of-data-skewness-and-kurtosis/) 

# 不平衡数据集

不平衡数据集是指目标属性(待预测的属性)分布不均匀的数据集。在处理数据科学问题时，这种情况并不少见。例如，预测欺诈性信用卡交易是不平衡数据集的一个很好的例子。因为大多数信用卡交易都是真实的。然而，也有一些欺诈交易。

不平衡的数据集需要特别注意，因为构建模型或评估性能的常规方法不起作用。这里有一篇文章详细讨论了不平衡数据集以及更好地处理它们的最佳方法。

[](/handling-imbalanced-datasets-in-machine-learning-7a0e84220f28) [## 机器学习中不平衡数据集的处理

### 面对不平衡的班级问题，应该做什么，不应该做什么？

towardsdatascience.com](/handling-imbalanced-datasets-in-machine-learning-7a0e84220f28) 

# 缩放比例

缩放要素是一种通常用于将数据集的所有要素(独立变量)调整到一致比例的技术。用一个例子来解释这个概念。让我们来看一个问题，我们有年龄和工资这样的特征。年龄在 20-75 岁之间，薪水在 5 万到 50 万之间。当我们使用基于梯度下降或任何基于距离的算法时。在将特征传递给算法之前，将特征缩放到一致的范围是很重要的。如果未对特征进行缩放，则更高比例的特征将影响预测。

要了解什么是 it 扩展以及它为什么重要，请阅读下面的文章。

[](/what-is-feature-scaling-why-is-it-important-in-machine-learning-2854ae877048) [## 什么是特征缩放&为什么它在机器学习中很重要？

### 最小最大分频器与标准分频器与鲁棒分频器

towardsdatascience.com](/what-is-feature-scaling-why-is-it-important-in-machine-learning-2854ae877048) 

# 相互关系

相关性是解释两个特征之间关系的统计度量。假设我们有两个特征 A 和 B，如果 A 和 B 彼此正相关，这意味着随着 A 的增加，B 也会增加。如果 A 和 B 是负相关的，那么当其中一个增加时，另一个减少。

建立模型时，相关性通常用于特征选择。当存在彼此高度相关的特征时，这意味着它们相互依赖。它们不是真正独立的，因此在构建模型时，通常会从特征列表中删除其中一个。

通过一个工作示例了解更多关于相关性以及如何在特征选择中使用它们的信息。阅读下面的文章，

[](/feature-selection-correlation-and-p-value-da8921bfb3cf) [## 要素选择-相关性和 P 值

### 通常，当我们获得一个数据集时，我们可能会在数据集中发现过多的要素。我们在中发现的所有功能…

towardsdatascience.com](/feature-selection-correlation-and-p-value-da8921bfb3cf) 

# 置信区间和置信水平

置信区间和置信水平容易混淆，尤其是新手。一旦你理解了这个概念，它就不会被混淆。

让我们考虑一个简单的现实世界的例子。一家电子商务公司希望在最终购买之前了解商品的平均浏览次数。跟踪每个用户的点击流数据并不简单。因此，最好的方法是计算样本的平均值，并得出一个估计值。当我们分析样本用户数据时，我们希望得出一个估计范围。比如，用户在最终购买前平均浏览 4 到 9 件商品。这个区间就是置信区间。每 100 个用户中落入该范围的用户数量的确定性是置信水平。

要了解有关置信区间和置信水平检查背后的计算的更多信息，请参阅下面的文章

[](/confidence-intervals-explained-simply-for-data-scientists-8354a6e2266b) [## 为数据科学家简单解释置信区间

### 没有沉重的术语

towardsdatascience.com](/confidence-intervals-explained-simply-for-data-scientists-8354a6e2266b) 

# 同方差和异方差

同方差是线性回归中的一个重要假设。这是求职面试中常见的问题。同方差意味着自变量和因变量之间的残差在自变量的不同值上是相同的。

让我们举一个简单的例子，我们有一个自变量“财产大小”和因变量是“财产价值”。意思是我们用‘财产大小’来预测‘财产价值’，误差就是残差。如果误差不随“属性大小”的不同值而变化，则它满足同质性。如果与较小的属性相比，较大的属性的残差较高，则它是异方差的。

为了更好地理解这个概念。此外，了解为什么同方差是解决回归问题的一个重要假设。读下面这篇文章，

[](https://www.statisticssolutions.com/free-resources/directory-of-statistical-analyses/homoscedasticity/) [## 同方差统计解决方案

### 同方差假设(意思是“相同的方差”)是线性回归模型的核心。同质性…

www.statisticssolutions.com](https://www.statisticssolutions.com/free-resources/directory-of-statistical-analyses/homoscedasticity/) 

# 准备数据科学面试？

这是我的 YouTube 频道上的一段视频，讲述了准备数据科学面试的步骤。这不是面试前一晚的准备，而是长期的准备。

# 保持联系

*   如果你喜欢这篇文章，并对类似的文章感兴趣，[在 Medium](https://rsharankumar.medium.com/) 上关注我。成为[的中级会员](https://rsharankumar.medium.com/membership)，访问数千篇与职业、金钱等相关的文章。
*   我在我的 YouTube 频道上教授和谈论各种数据科学主题。[在这里订阅我的频道](https://www.youtube.com/c/DataSciencewithSharan)。
*   在这里注册[我的电子邮件列表](https://chipper-leader-6081.ck.page/50934fd077)获取更多数据科学技巧，并与我的工作保持联系