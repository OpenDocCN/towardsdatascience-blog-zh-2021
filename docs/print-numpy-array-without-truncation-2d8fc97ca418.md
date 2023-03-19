# 如何在不截断的情况下打印完整的 NumPy 数组

> 原文：<https://towardsdatascience.com/print-numpy-array-without-truncation-2d8fc97ca418?source=collection_archive---------15----------------------->

## 探索在不截断的情况下打印 NumPy 数组的几种不同方法

![](img/ebb0954988ef942e531a300b831255cd.png)

[阳光摄影](https://unsplash.com/@sstoppo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/eraser?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

当打印出 NumPy 数组时，由于大量的元素，默认情况下输出可能会被截断。虽然在大多数情况下这不是问题，但有时您可能必须完整地打印出数组，以便能够检查完整的内容。

在今天的简短指南中，我们将探索将 NumPy 数组打印到标准输出而不进行任何截断的几种不同方法。具体来说，我们将展示如何做到这一点

*   使用`set_printoptions`方法
*   使用`printoptions`上下文管理器
*   通过将 NumPy 数组转换成列表的列表

首先，让我们创建一个示例 NumPy 数组，我们将在这篇短文中引用它来演示一些概念。

```
import numpy as np# Create a dummy NumPy array consisting of 250 rows and 40 columns
my_arr = np.arange(10000).reshape(250, 40)
print(my_arr)*array([[   0,    1,    2, ...,   37,   38,   39],
       [  40,   41,   42, ...,   77,   78,   79],
       [  80,   81,   82, ...,  117,  118,  119],
       ...,
       [9880, 9881, 9882, ..., 9917, 9918, 9919],
       [9920, 9921, 9922, ..., 9957, 9958, 9959],
       [9960, 9961, 9962, ..., 9997, 9998, 9999]])*
```

正如您在上面看到的，当输出到标准输出时，numpy 数组的列和行都被截断。

## 使用`set_printoptions`方法

`[numpy.set_printoptions](https://numpy.org/doc/stable/reference/generated/numpy.set_printoptions.html)`是一种用于配置显示选项的方法，例如数组、浮点数和其他`numpy`对象显示到标准输出的方式。

在`set_printoptions`中接受的参数之一是`threshold`，它对应于触发全表示汇总的数组元素的总数。默认情况下，该值设置为`1000`。为了在没有总结的情况下触发完整的表示，您应该将`threshold`设置为`[**sys.maxsize**](https://docs.python.org/dev/library/sys.html#sys.maxsize)`。

```
import sys
import numpy as np**np****.set_printoptions(threshold=sys.maxsize)**
```

此外，如果您还想通过在调用`set_printoptions()`时指定`precision`参数来调整浮点输出的精度。

## 使用 printoptions 上下文管理器

在许多情况下，您可能希望完全打印一个或几个 numpy 数组，然后使用默认选项。在这种情况下，上下文管理器是一个非常有用的构造，可以帮助您做到这一点。

NumPy 附带了一个名为`[np.printoptions](https://numpy.org/doc/stable/reference/generated/numpy.printoptions.html#numpy.printoptions)`的上下文管理器，帮助您在`with`块的范围内设置打印选项，然后在最后恢复旧的选项。

```
import sys
import numpy as np**with np.printoptions(threshold=sys.maxsize):
    print(my_arr)**
```

## 将数组转换为列表

前一种方法的另一种替代方法是使用`[numpy.ndarray.tolist()](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.tolist.html)`方法将 NumPy 数组转换成一个列表。

```
import numpy as np **print(my_arr.tolist())**
```

## 最后的想法

在今天的简短指南中，我们探索了几种完全打印 NumPy 数组的不同方法。默认情况下，在标准输出中打印的数组可能会被截断(取决于它们的大小/元素)。

在许多情况下，您可能希望完整地查看所有元素，因此本文中介绍的选项是一个很好的参考，可以帮助您做到这一点。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒体上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership>  

**你可能也会喜欢**

</16-must-know-bash-commands-for-data-scientists-d8263e990e0e>  </whats-the-difference-between-shallow-and-deep-copies-in-python-ceee1e061926>  </8-must-know-venv-commands-for-data-scientists-and-engineers-dd81fbac0b38> 