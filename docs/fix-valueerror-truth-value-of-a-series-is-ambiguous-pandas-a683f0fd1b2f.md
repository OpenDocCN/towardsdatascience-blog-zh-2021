# 如何修复值错误:熊猫系列的真值不明确

> 原文：<https://towardsdatascience.com/fix-valueerror-truth-value-of-a-series-is-ambiguous-pandas-a683f0fd1b2f?source=collection_archive---------1----------------------->

## 了解如何处理熊猫最棘手和最常见的错误之一

![](img/1b45fcbc601fe7da5f92405bd5c41eaa.png)

马特·阿特兹在 [Unsplash](https://unsplash.com/s/photos/fix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

熊猫最常报告的错误之一是

```
**ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() or a.all()**
```

而且有时处理起来可能相当棘手，尤其是如果你是熊猫库(甚至 Python)的新手。

在今天的文章中，我们将首先了解这个错误出现的原因和时间，并展示如何消除它。具体来说，我们将讨论如何通过使用

*   Python 的按位运算符，或
*   NumPy 的逻辑运算符方法

## 理解错误

在进入细节之前，让我们用一个例子重现这个错误，我们也将在本文中引用这个例子来演示一些概念，这些概念将最终帮助我们理解实际的错误以及如何消除它。

让我们开始在 pandas 中创建一个示例数据框架。

```
import pandas as pddf = pd.DataFrame(
    [
        (1, 531, True, 12.8),
        (2, 126, False, 74.2),
        (3, 175, False, 1.1),
        (4, 127, True, 45.8),
        (5, 543, True, 21.1),
        (6, 254, False, 98.1),
    ],
    columns=['colA', 'colB', 'colC', 'colD']
)print(df)
 *colA  colB   colC  colD
0     1   531   True  12.8
1     2   126  False  74.2
2     3   175  False   1.1
3     4   127   True  45.8
4     5   543   True  21.1
5     6   254  False  98.1*
```

现在让我们假设我们想要使用几个逻辑条件来过滤我们的熊猫数据帧。假设我们希望只保留列`colB`中的值大于`200`且列`colD`中的值小于或等于`50`的行。

```
df = df[(df['colB'] > 200) and (df['colD'] <= 50)]
```

上述表达式将失败，并出现以下错误:

```
File "/usr/local/lib/python3.7/site-packages/pandas/core/generic.py", line 1555, in __nonzero__ self.__class__.__name__ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() or a.all().
```

出现该错误是因为您使用逻辑运算符(如`and`、`or`、`not`)将多个条件链接在一起，导致了不明确的逻辑，因为对于每个指定的单独条件，返回的结果是基于列的。

换句话说，该错误告诉您，您正试图获取熊猫系列对象的布尔值。为了将它放入一个更简单的上下文中，考虑下面的表达式，它将再次引发这个特定的错误:

```
>>> import pandas as pd>>> s = pd.Series([1, 2, 3])
>>> s
0    1
1    2
2    3
dtype: int64>>> bool(s)
Traceback (most recent call last):
  File "/usr/local/lib/python3.7/site-packages/IPython/core/interactiveshell.py", line 3331, in run_code
    exec(code_obj, self.user_global_ns, self.user_ns)
  File "<ipython-input-5-68e48e81da14>", line 1, in <module>
    bool(s)
  File "/usr/local/lib/python3.7/site-packages/pandas/core/generic.py", line 1555, in __nonzero__
    self.__class__.__name__
**ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() or a.all().**
```

当多个条件被指定并使用逻辑操作符链接在一起时，每个单独的操作数被隐式地转换成一个`bool`对象，从而导致问题中的错误。

## 使用按位运算符

现在为了修复这个错误，你的第一个选择是使用 Python **位操作符**。

```
**Operator** |   **Meaning**
----------------------
   &     | Bitwise AND
   |     | Bitwise OR
   ~     | Bitwise NOT
   ^     | Bitwise XOR
```

现在，表达式应该像预期的那样工作，不会出现`ValueError`:

```
**df = df[(df['colB'] > 200) & (df['colD'] <= 50)]**print(df)
 *colA  colB  colC  colD
0     1   531  True  12.8
4     5   543  True  21.1*
```

## 使用 NumPy 的逻辑运算符

或者，您可以使用 NumPy 的逻辑运算符方法，即**按元素计算真值**，这样真值就不会有歧义。

```
Operator | Method
-----------------
AND      | [numpy.logical_and](https://numpy.org/doc/stable/reference/generated/numpy.logical_and.html)
OR       | [numpy.logical_or](https://numpy.org/doc/stable/reference/generated/numpy.logical_or.html)
NOT      | [numpy.logical_not](https://numpy.org/doc/stable/reference/generated/numpy.logical_not.html) 
XOR      | [numpy.logical_xor](https://numpy.org/doc/stable/reference/generated/numpy.logical_xor.html)
```

在我们的例子中，`numpy.logical_and`方法应该可以达到目的:

```
import numpy as np**df = df[np.logical_and(df['colB'] > 200, df['colD'] <= 50)]**print(df)
   colA  colB  colC  colD
*0     1   531  True  12.8
4     5   543  True  21.1*
```

## 最后的想法

在今天的指南中，我们讨论了 pandas 和 Python 中最常报告的错误之一，即`ValueError: The truth value of a Series is ambiguous`。

我们重现了这个错误，试图更好地理解为什么首先会出现这个错误，此外，我们还讨论了如何使用 Python 的按位运算符或 NumPy 的逻辑运算符方法来处理它。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒介上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

</how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  </whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351>  </automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb> 