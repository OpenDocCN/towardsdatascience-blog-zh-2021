# 常态？我们怎么检查呢？

> 原文：<https://towardsdatascience.com/normality-how-do-we-check-that-484a0478681b?source=collection_archive---------26----------------------->

## 你的数据正常吗，怎么检查？

在我提到如何检查正态性的几种方法之前，让我解释一下为什么“你的数据”是“正常的”是好消息。

正态分布具有某些其他分布没有的性质:

1.  它具有臭名昭著的钟形曲线，该曲线围绕平均值对称，这使得它成为线性回归模型的一个有吸引力的选择。
2.  然后因为[中心极限定理](https://en.wikipedia.org/wiki/Central_limit_theorem)，对于大量样本，正态分布可以逼近其他已知分布。
3.  平均值、中值和众数也是相等的。

上述性质使得正态分布在分析上更具吸引力和可解性。

![](img/cf983be0fb6955cb4588075bdfee89ef.png)

服从正态分布的数据是好消息。[图片由 [Felicia Buitenwerf](https://unsplash.com/@iamfelicia?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄]

但是我们如何检查正态性呢？根据您可获得的数据量，可以使用不同的测试，有时您可能有几个样本或很多样本，因此这取决于具体情况！

![](img/72909a4a194a13d9511eb819a5710a56.png)

正常测试是存在的，不要惊慌！[照片由 [Jasmin Sessler](https://unsplash.com/@jasmin_sessler?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄]

在我描述检查正态性的方法之前，让我们有一个示例数据集，它具有均值为 0.05、方差为 0.9 的正态分布。

```
>>> import numpy as np

>>> mu, sigma = 0.05, 0.90
>>> data = np.random.normal(mu, sigma, 10000)
```

a.)第一种测试可以是“*将数据*与给定的分布进行比较。这种测试的例子可以是:`Lilliefors test`(当估计参数已知时)、`Andrew-Darling test`等。

我们将看一下`Lilliefors test`，它计算你的数据的经验分布函数与参考分布的 CDF 之间的距离。请注意，这类似于`Kolmogorov-Smirnov`测试，但参数是估计的。

```
>>> **import statsmodels.api as sm**
>>> ## Return KS-distance, p-value>>> ks, p = sm.stats.lilliefors(data)
>>> print(p)
0.66190841161592895
```

现在，由于 p 值高于阈值(通常为 0.05)，我们说*零假设为真*。换句话说，这意味着`data`来自正态分布。

b.)第二种测试是“查看”数据的描述性统计数据，例如峰度(*尾部与标准正态分布的差异*)或偏斜度(*测量平均值周围的不对称性*)，或者结合峰度和偏斜度的测试，称为`omnibus test`。

```
>>> **from** **scipy** **import stats**>>> s, p = stats.normaltest(data)
>>> print(p)
0.6246248916944541
```

这与我们从(a)中得到的观察结果相似，因此我们可以说两个测试都预测数据来自正态分布。

如果你知道任何其他检查数据正态性的常用方法，请在评论中提出来。

我希望你觉得这篇文章有趣并且有用。

谢谢大家！