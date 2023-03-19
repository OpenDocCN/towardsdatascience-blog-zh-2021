# 通过 easy-ht Python 包简化了假设检验

> 原文：<https://towardsdatascience.com/hypothesis-testing-made-easy-through-the-easy-ht-python-package-2ee395b95fe2?source=collection_archive---------14----------------------->

## 编码教程，统计

## 你应该使用哪种假设检验？皮尔逊还是斯皮尔曼？t 检验还是 Z 检验？卡方？easy-ht 没问题。

![](img/2b287526c6c0120297c86af524636017.png)

由 [Edge2Edge 媒体](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

一个新的数据科学家可能会遇到的主要困难之一是关于**统计基础知识**。特别是，数据科学家可能很难理解在特定情况下使用哪种假设检验，例如何时可以使用卡方检验，或者皮尔逊相关系数和斯皮尔曼等级相关之间有什么区别。

> *为此，我实现了一个名为 easy-ht 的 Python 包，它允许执行一些统计测试，如相关性、正态性、随机性、均值等，而不关心要使用的具体测试。*

**软件包将根据提供的数据自动配置自身。**

在本文中，我通过以下步骤描述了`easy-ht` Python 包:

*   包的概述
*   单个输入数据集的使用示例
*   两个输入数据集的使用示例

# 1 easy-ht 封装概述

easy-ht 软件包可以通过 pip 轻松安装:

`pip install easy-ht`

其完整文档可在[此链接](https://pypi.org/project/easy-ht/)获得。在一个或两个数据集的情况下，该软件包允许计算以下假设检验:

*   **正态性** —检查样本是否遵循正态分布
*   **相关性** —检查样本是否相关。它只能用于两个样本的测试。
*   **随机性** —检查样本是否以随机方式构建。
*   **表示—** 在一个样本测试中，将样本与期望值进行比较。在两个样本测试中，比较两个样本的平均值。
*   **分布** —在一个样本测试中，将样本与分布进行比较。在两个样本测试中，比较两个样本的分布。

# 2 一个样本测试

在第一个示例中，只考虑了一个数据集。我生成一个随机的正态分布数据集。同样的过程也可以应用于一般数据。

# 2.1 生成数据

首先，我导入所需的库:

```
from easy_ht import HypothesisTest
import random
import numpy as np
```

然后，我生成正态分布的数据:

```
mu, sigma = 0, 0.1
X = np.random.normal(mu, sigma, 100)
```

现在，我创建一个`HypothersisTest`对象，它将用于进一步的分析。我将数据集`X`作为输入参数传递:

```
test = HypothesisTest(x = X)
```

一旦创建了对象，我就可以运行一些测试，而不用关心要使用的具体测试。

# 2.1 将数据平均值与理论值进行比较

```
value = 50
result = test.compare_means(value = value)
if result:
   print("Test is True: There is no difference")
else:
   print("Test is False: There is difference")
```

# 2.3 检查随机性

检查数据集是否以随机方式生成:

```
result = test.check_randomness()
if result:
   print("Test is True: Data is random")
else:
   print("Test is False: Data is not random")
```

# 2.4 比较分布

用`mean = mu` 和`sigma = sigma`比较正态分布的数据

```
result = test.compare_distributions(cdf = 'norm', args=(mu,sigma))
if result:
   print("Test is True: Data follows a normal distribution",mu, sigma)
else:
   print("Test is False: Data does not follow a normal distribution", mu, sigma)
```

# 2 双样本检验

在第二个例子中，我考虑两个数据集。我生成了两个正态分布的数据集，但是同样的过程也可以用于一般数据集。

# 3.1 生成数据

和前面的例子一样，首先我导入所需的库:

```
from easy_ht import HypothesisTest
import random
import numpy as np
```

然后，我生成两个正态分布的数据集，具有相同的平均值和不同的 sigma:

```
mu, sigma1, sigma2 = 0, 0.1, 0.4
X = np.random.normal(mu, sigma1, 100)
Y = np.random.normal(mu, sigma2, 100)
```

现在，我创建一个`HypothersisTest`对象:

`test = HypothesisTest(x = X, y = Y)`

我利用它做进一步的测试。对于一个示例情况，参数`y`也被作为输入传递。

# 3.2 比较手段

比较两个数据集的平均值:

```
result = test.compare_means()
if result:
   print("Test is True: There is no difference")
else:
   print("Test is False: There is difference")
```

不带参数调用`compare_means()`函数。

# 3.2 比较分布

比较两个数据集的分布。与`compare_means()`函数类似，is 函数不传递任何参数。

```
result = test.compare_distributions()
if result:
   print("Test is True: Samples follow the same distribution")
else:
  print("Test is False: Samples do not follow the same distribution")
```

# 3.3 检查相关性

检查这两个数据集是否相关。

```
result = test.check_correlation()
if result:
   print("Test is True: Samples are correlated")
else:
   print("Test is False: Samples are not correlated")
```

# 摘要

在本文中，我展示了用于假设测试的新的 easy-ht Python 包。这个包正在开发中，因此如果你在测试它的时候遇到了一些问题，请不要犹豫联系我，或者在[这个链接](https://github.com/alod83/easy-ht/issues)提出问题。

本教程的完整代码可以从这个链接下载[。](https://github.com/alod83/easy-ht/tree/main/examples)

最初发布于[初学者数据科学](https://alod83.altervista.org/hypothesis-testing-made-easy-through-the-easy-ht-python-package/)。

# 相关文章

[](https://medium.com/analytics-vidhya/a-gentle-introduction-to-descriptive-analytics-8b4e8e1ad238) [## 描述性分析的简明介绍

### 关于集中趋势、频率和分散指数的一些基本概念。

medium.com](https://medium.com/analytics-vidhya/a-gentle-introduction-to-descriptive-analytics-8b4e8e1ad238) [](/data-normalization-with-python-scikit-learn-e9c5640fed58) [## 使用 Python scikit 进行数据规范化-学习

### 继关于数据预处理的系列文章之后，在本教程中，我将讨论 Python 中的数据规范化…

towardsdatascience.com](/data-normalization-with-python-scikit-learn-e9c5640fed58) [](https://medium.com/analytics-vidhya/basic-statistics-with-python-pandas-ec7837438a62) [## python 熊猫的基本统计数据

### 入门指南

medium.com](https://medium.com/analytics-vidhya/basic-statistics-with-python-pandas-ec7837438a62)