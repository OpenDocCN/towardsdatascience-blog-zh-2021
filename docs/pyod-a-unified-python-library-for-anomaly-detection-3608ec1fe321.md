# PyOD:用于异常检测的统一 Python 库

> 原文：<https://towardsdatascience.com/pyod-a-unified-python-library-for-anomaly-detection-3608ec1fe321?source=collection_archive---------8----------------------->

## 用于异常检测机器学习任务的 scikit-learn

![](img/697a5c9e7b53c5a8756496352e83206b.png)

照片由[安妮塔·里泰诺尔](https://www.flickr.com/photos/puliarfanita/)在 [flickr](https://www.flickr.com/photos/puliarfanita/24860619467) 拍摄

`[PyOD](https://pyod.readthedocs.io/en/latest/index.html)`是一个 Python 库，具有一套全面的可扩展、最先进的(SOTA)算法，用于检测多元数据中的异常数据点。这项任务通常被称为[异常检测](https://en.wikipedia.org/wiki/Anomaly_detection)或[异常检测](https://en.wikipedia.org/wiki/Anomaly_detection)。

> *异常值检测任务旨在识别偏离给定数据的“正常”或一般分布的罕见项目、事件或观察值。*

我最喜欢的定义:**异常是指让人怀疑它是由不同的数据生成机制生成的**

离群点检测的常见应用包括欺诈检测、数据错误检测、网络安全中的入侵检测以及机械中的故障检测。

# 为什么要使用特定的异常检测算法？

实际上，异常检测最好是作为一种无监督或半监督的任务，在这种情况下，您试图识别数据中的异常观察。

**离群值(异常数据)与内标值(正常数据)的标签通常不可用且难以获得。**即使标签确实存在，它们通常也是不够的。一种异常可能只有少数几种标记。其他类型的异常可能以前从未发生过，因此您无法训练监督算法来发现它们。

**“离群值”的定义可能是主观的。**什么被视为“异常”取决于应用，但它通常意味着数据错误或欺诈或犯罪活动。

专用的异常值检测算法提供了一种可靠地对大量未标记数据执行模式识别的方法。

<https://pub.towardsai.net/why-outlier-detection-is-hard-94386578be6c>  

# **关键**特性

PyOD 拥有一套 30 多种检测算法，从隔离森林等经典算法到最新的深度学习方法，再到 [COPOD](/fast-accurate-anomaly-detection-based-on-copulas-copod-3133ce9041fa) ( [论文](https://arxiv.org/abs/2009.09463))等新兴算法。PyOD 算法已经得到很好的确立，在文献中被大量引用，并且非常有用。

PyOD 为所有算法、技术文档和示例提供了统一的 API，易于使用。

## 统一的 API

所有探测器都用`contamination`参数初始化。`contamination`是数据中异常值的预期比例，用于在模型拟合期间设置异常值阈值。默认情况下，`contamination=0.1`和必须在 0 和 0.5 之间。

探测器具有以下功能:

*   `fit(data)`:安装探测器。使用训练数据计算任何必要的统计数据。与`scikit-learn`中的监督模型不同，`PyOD`中的拟合方法不需要目标标签的`y`参数。
*   `decision_function(data)`:使用合适的检测器计算新数据的原始异常值。*(注意:一些探测器在没有预先安装的情况下仍然工作良好)*
*   `predict(data)`:返回每个输入样本对应的二进制标签(离群/正常)。在底层，该函数将拟合步骤中生成的阈值应用于`decision_function`返回的异常分数。
*   `predict_proba(data)`:使用标准化或统一化返回作为概率缩放的异常分数。

一旦安装，探测器包含属性`decision_scores_`、`labels_`和`threshold_`。

*   `decision_scores_` —训练数据的异常值分数。分数越高，表明数据越不规则
*   `threshold_` —根据拟合时初始化的`contamination`确定阈值。它根据`decision_scores_`中的`n_samples * contamination`最高分数设定。
*   `labels_` —训练数据的二进制异常标签。0 表示观察值是内部值，1 表示外部值。它是通过将`threshold_`应用于`decision_scores_`而生成的。

## API 演示

这里有一个 API 的快速说明。

首先，加载或生成异常值检测数据。

这里，我使用 PyOD 的`generate_data`函数生成一个包含 200 个训练样本和 100 个测试样本的合成数据集。*正态样本由多元高斯分布生成；异常样本是使用均匀分布生成的。*

训练和测试数据集都有 5 个特征，10%的行被标记为异常。我给数据添加了一点随机噪声，使其更难完美区分正常点和异常点。

接下来，我对训练数据拟合 ABOD(基于角度的离群点检测器),并对测试数据进行评估。

对相同的数据测试不同的 PyOD 算法很容易。在下一段代码中，我将展示如何训练和评估 COPOD。

在合成数据集上，COPOD 比 ABOD 有更好的 ROC-AUC 精度分数。

## 关于异常检测器评估的两点注意事项

1.  如果没有任何基本事实标签(内层与外层)，就无法测量检测机的性能。
2.  [ROC-AUC](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc) 得分对于阈值选择是稳健的——如果 AUC 较高，则您的模型性能对您选择的阈值不太敏感。

# 离群点检测算法的类型

有几种异常值检测算法。这里介绍的分类法反映了 PyOD 分类法。

## 线性模型

异常值检测的线性模型使用线性模型，如 [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis) 和[单类支持向量机](http://rvlasveld.github.io/blog/2013/07/12/introduction-to-one-class-support-vector-machines/)来分离正常和异常值观察值。

## 基于邻近的

用于离群点检测的基于邻近度的模型，例如 K- [最近邻居](https://scikit-learn.org/stable/modules/neighbors.html)和[局部离群因子](https://scikit-learn.org/stable/auto_examples/neighbors/plot_lof_outlier_detection.html?highlight=local%20outlier%20factor)，使用数据点之间的距离来度量相似性。彼此高度接近的观察值更可能是正常的。距离较远的观测值更有可能是异常值

## 盖然论的

异常值检测的概率模型依赖于统计分布来发现异常值。概率检测器包括[中位数绝对偏差](https://en.wikipedia.org/wiki/Median_absolute_deviation)(MAD)[基于 Copula 的离群点检测](/fast-accurate-anomaly-detection-based-on-copulas-copod-3133ce9041fa) (COPOD)，以及[基于角度的离群点检测](https://www.dbs.ifi.lmu.de/Publikationen/Papers/KDD2008.pdf) (ABOD)。

## 离群系综

离群值集成依赖于模型集成来隔离离群点。算法包括[隔离森林](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html)(也有`scikit-learn`)和 [LODA](https://link.springer.com/content/pdf/10.1007%2Fs10994-015-5521-0.pdf) (轻型异常在线检测器)。

## 神经网络

神经网络也可以被训练来识别异常。

[自动编码器](https://en.wikipedia.org/wiki/Autoencoder)(以及变化的自动编码器)网络架构可以被训练来识别异常，而无需标记实例。自动编码器学习压缩和重建数据中的信息。重建误差然后被用作异常分数。

最近，已经提出了几种用于异常检测的 GAN 架构(例如 [MO_GAAL](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.mo_gaal) )。

# 哪种算法最好？

没有一种检测算法能统治所有人。不同的算法会做得更好，这取决于任务。

## 算法基准

PyOD 针对来自 [ODDS](http://odds.cs.stonybrook.edu/#table1) 的 17 个异常检测基准数据集评估了其算法子集(10 种方法)。

我从最新基准测试结果中得出的结论在 PyOD [文档](https://pyod.readthedocs.io/en/latest/benchmark.html)中给出:

*   **所有算法都快！**即使是最大的数据集，运行时间也不超过 1 分钟(最长的数据集有 49k 行；最宽的数据集有 274 列)。
*   **在一个数据集上表现很好的算法可能在另一个数据集上表现很差**。例如，对于`vowels`数据集，ABOD 是第二好的检测器，但是对于`musk`数据集，它是最差的检测器。

## 定制探测器套装

构建更健壮的离群点检测模型(并且避免选择单个模型)的一种方法是将模型组合成定制的集成。这可以通过组合多个异常检测器的异常分数以及使用汇总分数对数据进行评分来完成。

PyOD 为`[pyod.models.combination](https://pyod.readthedocs.io/en/latest/pyod.models.html#module-pyod.models.combination)`模块中的模型组合提供了几个实用功能:

*   `[average](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.combination.average)` —对集合中检测器的得分进行平均
*   `[maximization](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.combination.maximization)` —集合中探测器的最高分
*   `[aom](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.combination.aom)`(最大值的平均值)—将基础检测器划分为子组，取每个子组的最大值，并对子组得分取平均值。
*   `[moa](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.combination.moa)`(平均值的最大值)—将基本检测器分成子组，取每个子组的平均分数，并返回最大子组分数。
*   `[majority_vote](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.combination.majority_vote)` —通过多数投票合并分数
*   `[median](https://pyod.readthedocs.io/en/latest/pyod.models.html#pyod.models.combination.median)` —来自集合中检测器的中值分数

请注意，异常分数在合并之前必须标准化，因为检测器不会返回相同级别的异常分数。

点击[此处](https://pyod.readthedocs.io/en/latest/example.html)查看探测器组合指南。

# 想了解更多？

如果您有兴趣了解更多关于异常检测的信息，请查看 PyOD Github 知识库的[异常检测资源](https://github.com/yzhao062/anomaly-detection-resources)页面。这个页面非常全面，并且随着新研究的发布而不断更新。

如果你喜欢这篇文章，你可能也会喜欢这些类似的帖子。

</fast-accurate-anomaly-detection-based-on-copulas-copod-3133ce9041fa> [## 基于 COPOD 的快速准确异常检测

towardsdatascience.com](/fast-accurate-anomaly-detection-based-on-copulas-copod-3133ce9041fa) <https://pub.towardsai.net/an-in-depth-guide-to-local-outlier-factor-lof-for-outlier-detection-in-python-5a6f128e5871>  <https://medium.com/geekculture/replace-outlier-detection-by-simple-statistics-with-ecod-f95a7d982f79>  </sktime-a-unified-python-library-for-time-series-machine-learning-3c103c139a55>  

## 不是中等会员？今天就加入！

  

# 参考

```
Zhao, Y., Nasrullah, Z. **and** Li, Z., 2019\. [PyOD: A Python Toolbox **for** Scalable Outlier Detection](https://www.jmlr.org/papers/volume20/19-011/19-011.pdf). Journal of machine learning research (JMLR), 20(96), pp.1-7.
```

【github.com】yzhao 062/pyod:(JMLR ' 19)一个用于可扩展异常值检测(异常检测)的 Python 工具箱

[欢迎使用 PyOD 文档！— pyod 0.8.9 文档](https://pyod.readthedocs.io/en/latest/index.html)