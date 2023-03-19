# 何时使用可空的 NaN

> 原文：<https://towardsdatascience.com/when-to-use-the-nullable-nan-10c073b0261e?source=collection_archive---------30----------------------->

## 当 NA 不为 False、False 为 False 且 NA 不为 Null 时的布尔值。

![](img/74584768ae76f0bfbe294d9006d5a5e0.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Nerfee Mirandilla](https://unsplash.com/@nerfee?utm_source=medium&utm_medium=referral) 拍摄的照片

> 对于 Pandas 中的布尔数据来说，NaN、Null、NA 和 bools 之间有着至关重要的区别——简要介绍何时以及如何使用它们。

**任务:**清理包含布尔值(真/假)的熊猫数据帧以优化内存。约束是将所有空值保留为空值，即不要将空值变为 False，因为这是一个有意义的变化。

**动作:**显式转换列 ***dtypes*** ，即使用 ***float32*** 代替 ***float64*** 节省内存，使用 ***bool*** 代替 ***object*** 。

**问题:**将选定的列转换为 ***bool*** 时，所有行的计算结果要么全为真，要么全为假，并返回一个令人头疼的问题。

> 要在布尔值中保留类似 null 的值，请用 pd 显式替换 null 值。NA 并将 dtype 设置为‘布尔型’而不仅仅是‘布尔型’—这就是布尔型数组。

**要点:**当源列包含空值或非布尔值，例如像 *1.0* 这样的浮点数时，**应用 Pandas 'bool' dtype 可能会错误地将所有行评估为 True** 。相反，用 pd 显式替换空值。NA 并将 dtype 设置为“boolean”而不仅仅是“bool”

# 该项目

在构建和部署应用程序时，我变得非常专注于让事情尽可能快地运行。为此，我最初开始大幅度地将 Pandas dtypes 设置为*、 ***布尔*** 或 ***浮动 32*** ，只要有可能。然而，虽然我节省了大量内存，但我后来发现我的数据完整性受到了影响。*

*继续阅读，学习如何修正和避免这种错误。*

# *权衡*

*当强制一个列为 bool 数据类型时，几乎可以立即节省一半的内存。虽然这很诱人，但事情还有另外一面。当源数据不只有真/假值时，Pandas 将返回一些不想要但逻辑上正确的结果。*

*例如，假设在数据帧中有几个“标志”列，表示某种真/假组合。*

```
*# unique values of columns in a DataFrame# col1 -> False True
[False True]# col2 -> True True                     
[nan   True] # col3 -> True True              
[nan   1.]* 
```

*在 **col1** 中，唯一的值只有 False 和 True——这是理想的，应用一个 dtype 的 **bool** 可以很好地工作。*

*在**第二栏**中，我们遇到了一个问题。虽然我们有一个真值，但是我们也有一个 ***nan*** 值。“南”代表熊猫“不是一个数字”，这是计算机知道那里应该什么都没有的一种方式。虽然我不会深入到逻辑地狱( [TDS 已经在那里了](/navigating-the-hell-of-nans-in-python-71b12558895b))，但应该足以说明将 **col2** 设置为 dtype bool 会将每一行评估为 True。**这很糟糕，因为不应该为真的行现在为真，而应该为空的行现在有东西了。***

*在**列 3** 中，我们有与**列 2** 相同的问题，但是现在有一个浮点数 1。( *1。是电脑 1.0 的简称*。与 **col2** 非常相似，在没有任何干预的情况下，Pandas dtype bool 将对该列求值为 True，如果我们以后按此标志进行过滤，就会出现问题。*

# *失败的解决方案*

*要解决这种情况，您可以像我一样尝试用字典映射数据，然后将 dtype 设置为 bool。例如，如果 1。应该是真的，那就把它映射出来，然后忘掉它，对吗？没那么快。*

```
*# a tempting but spurious solution
# given a dataframe as dfimport pandas as pd
import numpy as npkey = {'nan': np.nan,
       1.: *True*} df['col1'] = df['col1].map(key)
df['col1'] = df['col1].astype(bool)# this will not work like you might think*
```

*上面的代码片段不起作用的主要原因是它试图将字符串“nan”映射到一种特殊类型的 NaN 值——这永远不会起作用。可以肯定的是，如果你字面上把' nan '作为一个字符串，字典映射返回 ***np.nan*** ，但是接下来呢？正如我们从 **col3** 例子中所知道的，它最终都是真值，我们又回到了起点。*

# *解决方案设置*

*为了说明如何让 Nulls、nan 和 bools 为您工作，考虑一个[示例表](https://raw.githubusercontent.com/justinhchae/medium/main/bools.csv)和通过 [GitHub Gist](https://gist.github.com/justinhchae/19d333984119e892dc29c17ff62baad0) 编写的代码，它用实际数据涵盖了到目前为止的问题。*

## *如何看待空值的条件匹配*

*通常，我们可能会尝试测试 if 语句的等价性，但是由于很多原因，传统的条件语句不能处理空值，这些原因我不会在本文中讨论。*

```
*a = 1
b = 1
if b == a:
   print('this will work but not for NaNs')*
```

> *虽然听起来不可思议，布尔数组允许你在一个布尔列中携带两个以上的值。*

# *可行的解决方案*

*相反，使用 ***isnull()*** 方法结合类似 ***np.where()*** 的方法来测试和查找数据。要替换数据，使用 **pd。NA** 而不是 **np.nan** 。接下来，在浮点 1.0 的情况下，我们可以嵌套第二个 ***np.where()*** 语句，将 1.0 映射为 True。最后，使用 dtype ' boolean '而不是' bool '来创建一个布尔数组。虽然听起来不可思议，布尔数组允许你在一个布尔列中携带两个以上的值。*

# *结论*

*优化是神奇的，将 Pandas 对象转换成 bools 以节省内存是一个关键的组成部分。然而，如果不小心，结果可能是痛苦的。在本文中，我描述了在将 null 值保留为 pd.NA 的同时转换 boolean 或 True/False 数据的一些缺陷。*

*虽然具有两个以上值的可空布尔数组的概念可能看起来很奇怪，但它完全有效，可以帮助您充分利用多个世界的优势！*

# *文件*

*   *[缺失数据的熊猫稳定文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/missing_data.html)*
*   *[熊猫布尔数据稳定文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/boolean.html)*

> *感谢阅读！请鼓掌或关注我，继续关注我在编程、数据科学和人工智能方面的故事。*