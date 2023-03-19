# 使用四分位间距的异常值识别

> 原文：<https://towardsdatascience.com/outlier-identification-using-interquartile-range-74f5de12932a?source=collection_archive---------29----------------------->

## 一种识别异常值的简单方法

![](img/7aa50dbefb43638d68fc2fbd78e1486f.png)

*照片由* [*艾萨克*](https://unsplash.com/@isaacmsmith?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *上* [*下*](https://unsplash.com/s/photos/chart?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

识别异常值是数据预处理中非常常见的任务。它们可以通过模型改变样品的感知重要性，如果处理不当，可以改变任何分析的结果。识别它们的一个简单方法是使用四分位数范围。

# 四分位间距是多少？

IQR(四分位数范围)是分布的第三个四分位数和第一个四分位数之间的差值(或第 75 个百分位数减去第 25 个百分位数)。这是对我们的分布范围的度量，因为这个范围包含了数据集的一半点。了解分布的形状是非常有用的。例如，它是箱线图中盒子的宽度。

# 使用 IQR 的异常值定义

一旦我们计算出来，我们就可以用 IQR 来识别异常值。如果一个点满足以下条件之一，我们将该点标记为异常值:

*   它大于第 75 百分位+ 1.5 IQR
*   它不到 25%的百分之一点五 IQR

应用这个简单的公式，我们可以很容易地检测出分布中的异常值。Boxplot 使用相同的方法将异常值绘制为胡须外部的点。

1.5 系数背后的原因依赖于正态分布，但一般的想法是在不使用可能受异常值影响的一些度量的情况下计算异常值。这就是为什么，举例来说，使用标准差，可能会导致我们糟糕的结果。四分位数和百分位数是基于计数的，因此它们不太容易受到异常值的影响。

这个想法是，如果一个点离第 75 百分位(或第 25 百分位)太远，它就是一个“奇怪的”点，可以被标记为异常值。这个距离的数量级就是 IQR 本身。

让我们看一个 Python 编程语言中的简单例子。

# Python 中的离群点检测

在本例中，我们将根据正态分布生成一些随机分布的点，然后我们将人工添加两个异常值，以查看算法是否能够发现它们。

首先，让我们导入 NumPy 并设置随机数生成器的种子。

```
import numpy as np 
np.random.seed(0)
```

现在，让我们创建我们的正态分布数据集。

```
x = np.random.normal(size=100)
```

让我们添加两个异常值，例如，-10 和 5。

```
x = np.append(x,[5,-10])
```

由于正态分布的均值为 0，方差等于 1，所以这两个数字离均值非常远，非常罕见。我们可以使用正态分布的累积分布函数显式计算它们的频率，这可以使用 scipy 来计算。

```
from scipy.stats import norm
```

值小于-10 的概率为:

```
norm.cdf(-10) 
# 7.61985302416047e-24
```

值大于 5 的概率为:

```
1-norm.cdf(5) 
# 2.866515719235352e-07
```

因此，这些值是如此罕见，远离平均值，他们可以被视为离群值。

现在，让我们来计算 IQR:

```
iqr = np.percentile(x,75) - np.percentile(x,25)
```

最后，我们可以根据原始公式创建真/假数组掩码来识别异常值:

```
outliers_mask = (x > np.percentile(x,75) + 1.5*iqr) | (x < np.percentile(x,25) - 1.5*iqr)
```

正如所料，它们被完美地识别出来:

```
x[outliers_mask] # array([ 5., -10.])
```

# 结论

对于数据科学家来说，处理离群值始终是一个问题。我们可以使用适当的[探索性数据分析](https://www.yourdatateacher.com/exploratory-data-analysis-in-python-free-online-course/)来检测异常值的存在，但是如果我们想要正确地标记它们，我们必须应用合适的算法。尽管 IQR 异常值检测只适用于单变量，但它对任何数据科学家和分析师来说都是一个简单而强大的帮助。

www.yourdatateacher.com*上* [*教授机器学习和数据科学的数据科学家吉安卢卡·马拉托。*](http://www.yourdatateacher.com/)

*原载于 2021 年 11 月 1 日 https://www.yourdatateacher.com*<https://www.yourdatateacher.com/2021/11/01/outlier-identification-using-interquartile-range/>**。**