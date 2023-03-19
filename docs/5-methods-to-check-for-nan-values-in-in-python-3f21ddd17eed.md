# Python 中检查 NaN 值的 5 种方法

> 原文：<https://towardsdatascience.com/5-methods-to-check-for-nan-values-in-in-python-3f21ddd17eed?source=collection_archive---------0----------------------->

## python 中如何检查单个值是否为 NaN？有使用库和不使用库的方法。

NaN 代表非数字，是表示数据中缺失值的常用方法之一。它是一个特殊的浮点值，不能转换为除 float 以外的任何其他类型。

NaN 值是数据分析中的主要问题之一。为了得到想要的结果，处理 NaN 是非常必要的。

在数组、系列或数据帧中查找和处理 *NaN* 很容易。然而，确定一个独立的 *NaN* 值是很棘手的。在这篇文章中我解释了在 python 中处理 *NaN* 的五种方法。前三种方法涉及库的内置函数。[后两个依赖于 *NaN* 的属性来寻找 *NaN* 值](#dd61) *。*

## 方法 1:使用熊猫图书馆

[*pandas* 库](https://pandas.pydata.org/docs/)中的 *isna()* 可用于检查值是否为 null/NaN。如果值为 NaN/null，它将返回 True。

```
import pandas as pd
x = float("nan")
print(f"It's pd.isna  : {pd.isna(x)}")**Output**It's pd.isna  : True
```

## 方法 2:使用 Numpy 库

[*numpy* 库](https://numpy.org/doc/stable/)中的 *isnan()* 可用于检查值是否为 null/NaN。类似于*熊猫*中的 *isna()* 。

```
import numpy as np
x = float("nan")
print(f"It's np.isnan  : {np.isnan(x)}")**Output**It's np.isnan  : True
```

## 方法 3:使用数学库

[*数学库*](https://docs.python.org/3/library/math.html) 提供了内置的数学函数。该库适用于所有实数。 [*处理复数的话可以用 cmath* 库](https://docs.python.org/3/library/cmath.html#module-cmath)。
数学库内置了函数 *isnan()* 来检查 null/NaN 值。

```
import math
x = float("nan")
print(f"It's math.isnan : {math.isnan(x)}")**Output**It's math.isnan  : True
```

## 方法四:与自身比较

当我开始在一家大型 IT 公司工作时，我必须接受第一个月的培训。培训师在介绍*南*价值观的概念时提到，他们就像*外星人*我们一无所知。这些外星人在不断变形，因此我们无法将 NaN 的价值与其自身进行比较。
检查 *NaN* 值的最常见方法是检查变量是否等于自身。如果不是，那么一定是 *NaN* 值。

```
def isNaN(num):
    return num!= numx=float("nan")
isNaN(x)**Output**True
```

## 方法 5:检查范围

NaN 的另一个可用于检查 NaN 的属性是范围。所有浮点值都在负无穷大到无穷大的范围内。

**无穷大<任意数<无穷大**

但是，*楠的*值不在这个范围内。因此，如果值不落在从负无穷大到无穷大的范围内，则可以识别出 *NaN* 。

这可以通过以下方式实现:

```
def isNaN(num):
    if float('-inf') < float(num) < float('inf'):
        return False 
    else:
        return Truex=float("nan")
isNaN(x)**Output**True
```

希望以上文章对你有所帮助。我确信会有许多其他技术来检查基于各种其他逻辑的 *NaN* 值。请分享你遇到过的检查 *NaN/ Null* 值的其他方法。

干杯！

## 成为会员

我希望你喜欢这篇文章，我强烈推荐 [**注册*中级会员***](https://abhijithchandradas.medium.com/membership) 来阅读更多我写的文章或成千上万其他作者写的各种主题的故事。
[你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。](https://abhijithchandradas.medium.com/membership)

## 你可能会感兴趣的我的其他文章:

</how-to-extract-key-from-python-dictionary-using-value-2b2f8dd2a995>  </how-to-add-text-labels-to-scatterplot-in-matplotlib-seaborn-ec5df6afed7a>  ![](img/df28a33352a2d4dd70882f06aad7f614.png)

sebastiaan stam 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片