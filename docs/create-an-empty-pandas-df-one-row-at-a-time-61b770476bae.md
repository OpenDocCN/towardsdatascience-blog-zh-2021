# 你能创建一个空的熊猫数据框，然后填充它吗？

> 原文：<https://towardsdatascience.com/create-an-empty-pandas-df-one-row-at-a-time-61b770476bae?source=collection_archive---------26----------------------->

## 讨论为什么应该避免创建空的数据帧，然后一次追加一行

![](img/f12d7989ccd171f158c7f70c5946fc88.png)

照片由 [Glen Carrie](https://unsplash.com/@glencarrie?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/build?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

在 pandas 的上下文中，一个非常常见的问题是，您是否真的可以创建一个空的数据帧，然后通过一次追加一行来迭代地填充它。然而，这种方法往往**效率很低**，应该不惜一切代价避免。

在今天的文章中，我们将讨论一种替代方法，它将提供相同的结果，但比创建一个空的数据帧，然后使用循环在其中追加行要有效得多。

## 要避免什么

当然，实际上可以创建一个空的 pandas 数据帧，然后以迭代的方式追加行。这种方法看起来特别像下面这样:

```
import numpy as np
import pandas as pd
from numpy.random import randint# Make sure results are reproducible
np.random.seed(10)# Instantiate an empty pandas DF
df = pd.DataFrame(columns=['colA', 'colB', 'colC'])# Fill in the dataframe using random integers
**for i in range(7):
    df.loc[i] = [i] + list(randint(100, size=2))**print(df)
 *colA colB colC
0    0    9   15
1    1   64   28
2    2   89   93
3    3   29    8
4    4   73    0
5    5   40   36
6    6   16   11*
```

尽管上面的方法可以达到目的，但必须避免，因为它效率很低，而且肯定有更有效的方法，而不是创建一个空的数据帧，然后使用迭代循环来构建它。

一个**更糟糕的方法**，是在循环中使用`append()`或`concat()`方法。

> 值得注意的是，`[**concat()**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.concat.html#pandas.concat)`(因此也是`**append()**`)制作了数据的完整副本，不断地重用这个函数会对性能产生重大影响。如果需要在几个数据集上使用该操作，请使用列表理解。
> 
> — [熊猫文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html)

## 改为使用列表

不用使用`loc[]`属性或`append/concat`方法以迭代的方式追加行，您实际上可以将数据追加到一个列表中，最后直接从预先创建的列表中实例化一个新的 pandas DataFrame。这甚至在官方的熊猫文档中也有提及。

> 迭代地将行追加到数据帧可能比单个连接计算量更大。更好的解决方案是将这些行追加到一个列表中，然后将该列表与原始数据帧连接在一起。
> 
> — [熊猫文件](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.append.html)

```
import numpy as np
import pandas as pd
from numpy.random import randint# Make sure results are reproducible
np.random.seed(10)data = []
for i in range(7):
    data.append([i] + list(randint(100, size=2))df = pd.DataFrame(data, columns=['colA', 'colB', 'colC'])print(df)
 *colA  colB  colC
0     0     9    15
1     1    64    28
2     2    89    93
3     3    29     8
4     4    73     0
5     5    40    36
6     6    16    11*
```

使用列表(追加或删除元素)要高效得多，而且在迭代地将行追加到 pandas 数据帧时，您必须始终首选这种方法。

## 最后的想法

在今天的文章中，我们讨论了为什么避免创建空的 pandas 数据帧并迭代填充它们是重要的，因为这将显著影响性能。

相反，我们探索了如何使用列表迭代地构建这样的结构，并最终从创建的列表中创建新的 pandas 数据帧。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

**你可能也会喜欢**

[](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [## 加快 PySpark 和 Pandas 数据帧之间的转换

### 将大火花数据帧转换为熊猫时节省时间

towardsdatascience.com](/how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3) [](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) [## 使用预提交挂钩自动化 Python 工作流

### 什么是预提交钩子，它们如何给你的 Python 项目带来好处

towardsdatascience.com](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) [](/dynamic-typing-in-python-307f7c22b24e) [## Python 中的动态类型

### 探索 Python 中对象引用的工作方式

towardsdatascience.com](/dynamic-typing-in-python-307f7c22b24e)