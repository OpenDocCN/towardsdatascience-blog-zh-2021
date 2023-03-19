# 高效的熊猫:应用与矢量化操作

> 原文：<https://towardsdatascience.com/efficient-pandas-apply-vs-vectorized-operations-91ca17669e84?source=collection_archive---------12----------------------->

## 时间和效率很重要

![](img/3f696494afd19791646888b0368e5e7f.png)

Marc-Olivier Jodoin 在 [Unsplash](https://unsplash.com/s/photos/fast?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Pandas 是数据科学生态系统中最常用的数据分析和操作库之一。它提供了大量的函数和方法来执行高效的操作。

我最喜欢熊猫的一点是，完成一项既定任务几乎总是有多种方式。然而，当从可用选项中选择一个方法时，我们应该考虑时间和计算复杂性。

仅仅完成给定的任务是不够的。我们应该尽可能提高效率。因此，全面理解函数和方法是如何工作的至关重要。

在本文中，我们将举例比较 pandas 的 apply 和 applymap 功能与矢量化操作。apply 和 applymap 函数可用于许多任务。然而，随着数据量的增加，时间成为一个问题。

让我们创建一个包含 100k 行的示例数据框架。我们首先需要导入所需的库。

```
import numpy as np
import pandas as pd
import timeit df = pd.DataFrame({
    'cola':np.random.randint(1,100, size=100000),
    'colb':np.random.randint(100,1000, size=100000),
    'colc':np.random.random(100000)
})df.shape
(100000, 3)
```

我们将使用 timeit 库来测量执行一个操作所需的时间。

apply 函数通过遍历元素来执行按行或按列的操作。applymap 函数以类似的方式工作，但是对数据帧中的所有元素执行给定的任务。

下面的代码将对“cola”列中的每个数字求平方。

```
%%timeitdf['cola'].apply(lambda x: x**2)best of 3: 54.4 ms per loop
```

需要 54.4 毫秒。让我们用矢量化运算来完成同样的任务。

```
%%timeitdf['cola'] ** 2best of 3: 1.15 ms per loop
```

在 100k 行上大约快 50 倍。

下一个示例包括一个在数据帧的每一行中查找最小值的任务。

```
%%timeitdf.apply(lambda x: x.min(), axis=1)best of 3: 3.01 s per loop
```

执行应用功能需要 3 秒钟。

```
%%timeitdf.min(axis=1)best of 3: 1.58 ms per loop
```

用矢量化运算还是毫秒级的。差别巨大。差异显著增加的原因是 apply 函数在每行的每列中循环。

让我们稍微增加一下操作的复杂度。我们想找出一行中最高值和最低值之间的差异。

该操作通过应用功能完成，如下所示:

```
%%timeitdf.apply(lambda x: x.max() - x.min(), axis=1)best of 3: 5.29 s per loop
```

我们使用 lambda 表达式来计算最高值和最低值之间的差值。轴设置为 1，表示对行的操作已经完成。该操作执行需要 5.29 秒。

同样任务可以通过两个矢量化运算来完成。

```
%%timeitdf.max(axis=1) - df.min(axis=1)best of 3: 3.86 ms per loop
```

我们使用 max 和 min 函数作为矢量化运算。axis 参数为 1，表示我们需要一行中的最小值或最大值。如果我们不将轴参数指定为 1，则返回列中的最小值或最大值。

矢量化版本的执行时间为 3.86 毫秒，快了一千多倍。

下一个示例将 applymap 函数与矢量化运算进行了比较。以下代码将 dataframe 中的每个元素加倍。

```
%%timeitdf.applymap(lambda x: x * 2)best of 3: 93.6 ms per loop
```

需要 93.6 毫秒。

```
%%timeitdf * 2best of 3: 1.03 ms per loop
```

同样的操作大约需要 1 毫秒，比 applymap 函数快 90 倍。

## 结论

我们已经介绍了一些例子来比较矢量化运算和 apply 和 applymap 函数。

对于小数据，时间差通常可以忽略不计。然而，随着大小的增加，差异开始变得明显。我们可能会处理大量的数据，所以时间应该总是被考虑在内。

apply 和 applymap 函数的成功之处在于它们执行给定任务的方式。给定的操作是通过循环遍历元素来完成的，随着数据变大，执行速度会变慢。

感谢您的阅读。如果您有任何反馈，请告诉我。