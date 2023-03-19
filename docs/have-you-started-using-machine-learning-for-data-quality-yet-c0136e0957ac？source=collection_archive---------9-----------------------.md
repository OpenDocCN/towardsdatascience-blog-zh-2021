# 机器学习是数据质量的未来吗？

> 原文：<https://towardsdatascience.com/have-you-started-using-machine-learning-for-data-quality-yet-c0136e0957ac?source=collection_archive---------9----------------------->

## 提高数据质量的一些机器学习技术

![](img/abbce95462e5b864c7dbb644ddcead5c.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/check?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

“垃圾进，垃圾出”，在数据世界中，我们经常听到这个短语，意思是如果你的数据是“坏的”，你就永远无法做出“好的”决策(*打赌你没有看到这一天的到来:P* )。

从“差”到“好”的旅程就是数据质量。现在，坏数据可能意味着很多事情，例如:

*   数据不是最新的，*及时性*
*   *数据不准确，准确性*
*   数据对不同的用户有不同的价值，或者没有单一的真实来源，*一致性*
*   数据不可访问。*可用性*
*   数据不可用，可用性

[这篇](http://web.mit.edu/tdqm/www/winter/StrongLeeWangCACMMay97.pdf)文章很好地定义了数据的各个维度，请继续阅读，了解更多信息。

数据质量对所有领域的工作都很重要，也很关键，但作为一名数据工程师，我们在交付数据的同时也要承担主要责任，我们要交付“好”的数据。

**我的经历:**

为了确保数据质量，我还实施了基于规则的解决方案来处理:

*   错误的模式
*   重复数据
*   后期数据
*   异常数据

这主要围绕着清楚地了解我将向系统提供什么样的数据，当然反过来对整个数据管道框架进行概括。

虽然自动化系统有助于从被动方法转变为主动方法，但基于规则的系统的问题是

*   对于高基数、多维数据，它可能有太多的规则。
*   对于每一个新的错误，每一个新的异常，数据质量框架需要一些定制的实现，即在这样的解决方案中，人工干预是不可避免的

为了克服基于规则的场景中的人为干预，我们需要寻找一个完全自动化的系统。随着许多最近的发展，ML 是可能有助于实现这一目标的领域之一。

让我们看看机器是如何帮助我们确保自动化数据质量或超越表面现象的？

在讨论如何之前，我们先讨论为什么？

## 为什么机器学习是为了数据质量？

*   ML 模型可以从大量的数据中学习，并且可以发现其中隐藏的模式。
*   *能处理重复性任务*
*   不需要维护规则
*   可以随着数据的发展而发展

但是我还想指出，虽然上面的列表看起来像是 ML 作为候选人的选举横幅，但使用它取决于用例与用例之间的关系，而且 ML 通常不适合小型数据集或不显示任何模式的数据集。

说到这里，让我们看看一些 ML 应用程序的数据质量:

*   识别错误数据
*   识别不完整的数据
*   识别敏感数据以实现法规遵从性(可能是 PII 识别)
*   通过使用模糊匹配技术进行重复数据删除(有时只对数据进行唯一性处理不起作用)
*   通过评估历史模式来填补缺失的数据
*   通过使用历史信息对 SLA 的潜在违反发出警报(假设使用历史信息，系统检测到可能影响 SLA 的数据量急剧增加)
*   可以帮助有效地开发新的业务规则(定义 apt 阈值)

## 可以用于数据质量的一些 ML 技术

## 降维

降维是用于识别数据中的模式和处理计算量大的问题的 ML 方法。该方法包括一组算法，旨在通过确定每列的重要性来减少数据集中的输入变量数量。

它可以用来识别和删除在过多的列中带来很少或没有信息的列，从而消除维数灾难([阅读更多信息](/the-curse-of-dimensionality-50dc6e49aa1e))

在为任何其他数据质量算法馈送数据之前，它通常可以用作第一算法。在处理涉及语音、视频或图像压缩的视频和音频数据时，降维非常有用。

例如:UMAP、 [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis)

**聚类**

聚类根据数据的相似性和不相似性将数据组织成组(聚类)。

示例:DBSCAN

**异常检测**

异常检测算法本身并不独立。它通常伴随着降维和聚类算法。通过使用降维算法作为异常检测的前置阶段，我们首先将高维空间转换到低维空间。然后我们可以计算出这个低维空间中主要数据点的密度，这可能被确定为“正常”那些远离“正常”空间的数据点是异常值或“异常值”

例如:ARIMA

## *关联规则挖掘*

关联挖掘是一种无监督的 ML 算法，用于识别大型数据集中频繁出现的隐藏关系。

这种算法通常用于识别事务数据库、关系数据库或任何类似数据库中的模式和关联。例如，可以构建 ML 算法，该算法将通过处理来自条形码扫描仪的数据来分析购物篮，并定义一起购买的商品。

嗯，大概就是这样吧！讨论了使用机器学习提高数据质量的各种方法后，我想强调的是，没有一种适合所有 DQ 需求的解决方案。它可能因用例而异，在某些情况下，基于规则的系统可能是完美的，但随着数据的增长和*的变化*最终转向机器学习方法来提高数据质量可能会帮助我们超越显而易见的东西。欢迎来到未来！

我还会写一篇关于科技巨头如何利用机器学习提高数据质量的后续文章。

下次再见，
JD

**推荐人:**

[](https://eng.uber.com/monitoring-data-quality-at-scale/) [## 利用统计建模大规模监控数据质量

### 糟糕的数据无法做出好的商业决策。在优步，我们使用汇总和匿名数据来指导…

eng.uber.com](https://eng.uber.com/monitoring-data-quality-at-scale/) [](https://netflixtechblog.com/tracking-down-the-villains-outlier-detection-at-netflix-40360b31732) [## 追踪坏人

### 网飞的异常值检测

netflixtechblog.com](https://netflixtechblog.com/tracking-down-the-villains-outlier-detection-at-netflix-40360b31732) 

[https://engineering . LinkedIn . com/blog/2020/data-sentinel-automating-data-validation](https://engineering.linkedin.com/blog/2020/data-sentinel-automating-data-validation)

[](https://medium.com/data-science-at-microsoft/partnering-for-data-quality-dc9123557f8b) [## 合作提高数据质量

### 微软的两个小组如何在数据质量项目上合作。

medium.com](https://medium.com/data-science-at-microsoft/partnering-for-data-quality-dc9123557f8b) [](https://medium.com/airbnb-engineering/data-quality-at-airbnb-870d03080469) [## Airbnb 的数据质量

### 第 2 部分—新的黄金标准

medium.com](https://medium.com/airbnb-engineering/data-quality-at-airbnb-870d03080469) [](https://medium.com/airbnb-engineering/anomaly-detection-for-airbnb-s-payment-platform-e3b0ec513199) [## Airbnb 支付平台的异常检测

### 作者陆

medium.com](https://medium.com/airbnb-engineering/anomaly-detection-for-airbnb-s-payment-platform-e3b0ec513199)