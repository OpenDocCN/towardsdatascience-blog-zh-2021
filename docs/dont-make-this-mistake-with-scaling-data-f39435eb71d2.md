# 不要在缩放数据时犯这种错误

> 原文：<https://towardsdatascience.com/dont-make-this-mistake-with-scaling-data-f39435eb71d2?source=collection_archive---------14----------------------->

## MinMaxScaler 可以返回小于 0 且大于 1 的值。

![](img/dfb5fdcb00f48f80131e447ebbeb02c5.png)

凯利·西克玛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[MinMaxScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html) 是机器学习中最常用的缩放技术之一(紧随 StandardScaler 之后)。

> [来自 sklearns 的文档](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html):
> 
> 通过将每个要素缩放到给定范围来变换要素。
> 
> 该估计器单独缩放和翻译每个特征，使得它在训练集的给定范围内，例如在 0 和 1 之间。

通常，当我们使用 MinMaxScaler 时，我们在 0 和 1 之间缩放值。

您知道 MinMaxScaler 可以返回小于 0 且大于 1 的值吗？我不知道这件事，这让我很惊讶。

## 如果您感兴趣，Udacity 提供免费访问:

```
- [Intro to Machine Learning with PyTorch](https://imp.i115008.net/c/2402645/788201/11298)- [Deep Learning Nanodegree and more](https://imp.i115008.net/c/2402645/788202/11298)
```

# 问题是

让我们看一个例子。我用两个特性初始化 scaler。

```
import numpy as np
from sklearn.preprocessing import MinMaxScalerdata = [[-1, 2], [-0.5, 6], [0, 10], [1, 18]]
scaler = MinMaxScaler()
scaler.fit(data)
```

现在，让我们检查这两个特性的最小值和最大值:

```
scaler.data_min
# [-1\.  2.]scaler.data_max_
# [ 1\. 18.]
```

那些估计和预期的一样。

现在，让我们尝试输入大于最大值的值:

```
scaler.transform(np.array([[2, 20]]))# array([[1.5  , 1.125]])
```

**标量返回一个大于 1 的值。**

或更低的最小值:

```
scaler.transform(np.array([[-2, 1]]))# array([[-0.5   , -0.0625]])
```

**标量返回一个小于 0 的值。**

# 没什么大不了的，对吧？

当我们训练线性分类器时，这个问题可能发生，线性分类器将缩放特征与系数相乘。

分类器还没有看到某个特征的负值，它可以反转系数，这使得分类器工作不正确。

# 解决方案

我建议您将 MinMaxScaler 的输出限制在 0 到 1 之间。

```
scaler.transform(np.array([[-2, 1]]))# array([[0., 0.]])
```

# 在你走之前

```
- [Deploy Your 1st Machine Learning Model in the Cloud](https://gumroad.com/l/mjyDQ) [Ebook]- [Free skill tests for Data Scientists & Machine Learning Engineers](https://aigents.co/skills)
```

*上面的一些链接是附属链接，如果你通过它们购买，我会赚取佣金。请记住，我链接课程是因为它们的质量，而不是因为我从你的购买中获得的佣金。*

在 [Twitter](https://twitter.com/romanorac) 上关注我，在那里我定期[发布关于数据科学和机器学习的](https://twitter.com/romanorac/status/1328952374447267843)消息。

![](img/b5d426b68cc5a21b1a35d0a157ebc4f8.png)