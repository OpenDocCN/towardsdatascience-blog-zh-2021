# 熊猫中的 apply() vs map() vs applymap()

> 原文：<https://towardsdatascience.com/apply-vs-map-vs-applymap-pandas-529acdf6d744?source=collection_archive---------10----------------------->

## 讨论 Python 和 Pandas 中 apply()、map()和 applymap()的区别

![](img/27d45edb1e5cdcaf75a9680baef3ddd4.png)

[木丹](https://unsplash.com/@wudan3551?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 介绍

通常，我们需要对 DataFrame 的列或行应用某些函数，以便更新值或者甚至创建新的列。熊猫最常用的手术是`apply`、`map`和`applymap`方法。

在今天的指南中，我们将探索所有三种方法，并了解它们各自的工作原理。此外，我们将讨论何时使用其中一个。

首先，让我们创建一个示例数据框架，我们将在本文中使用它来演示一些概念，这些概念将有助于我们强调`apply()`、`map()`和`applymap()`之间的区别。

```
import pandas as pd df = pd.DataFrame(
    [
        (1, 521, True, 10.1, 'Hello'),
        (2, 723, False, 54.2, 'Hey'),
        (3, 123, False, 33.2, 'Howdy'),
        (4, 641, True, 48.6, 'Hi'),
        (5, 467, False, 98.1, 'Hey'),
    ],
    columns=['colA', 'colB', 'colC', 'colD', 'colE']
)print(df)
 *colA  colB   colC  colD   colE
0     1   521   True  10.1  Hello
1     2   723  False  54.2    Hey
2     3   123  False  33.2  Howdy
3     4   641   True  48.6     Hi
4     5   467  False  98.1    Hey*
```

## apply()方法

`[pandas.DataFrame.apply](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html)`方法用于沿着 pandas 数据帧的指定轴应用一个函数。`apply()`方法一次对整行或整列进行操作，最适用于**应用** **函数**而**无法矢量化的情况。**注意方法的输入必须是一个*可调用的*。

```
import numpy as np**df[['colA', 'colB']] = df[['colA', 'colB']].apply(np.sqrt)**print(df)
 *colA       colB   colC  colD   colE
0  1.000000  22.825424   True  10.1  Hello
1  1.414214  26.888659  False  54.2    Hey
2  1.732051  11.090537  False  33.2  Howdy
3  2.000000  25.317978   True  48.6     Hi
4  2.236068  21.610183  False  98.1    Hey*
```

此外，该方法也适用于熊猫系列(见`[pandas.Series.apply](https://pandas.pydata.org/docs/reference/api/pandas.Series.apply.html)`)。

## map()方法

`[pandas.Series.map](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.map.html)`方法只能应用于 pandas 系列对象，用于根据输入映射系列的值，该输入用于将每个值替换为从字典、函数甚至另一个`Series`对象中导出的指定值。

注意，该方法一次对一个元素进行操作，丢失的值将在输出中表示为`NaN`。

```
**df['colE'] = df['colE'].map({'Hello': 'Good Bye', 'Hey': 'Bye'})**print(df)
 *colA  colB   colC  colD      colE
0     1   521   True  10.1  Good Bye
1     2   723  False  54.2       Bye
2     3   123  False  33.2       NaN
3     4   641   True  48.6       NaN
4     5   467  False  98.1       Bye*
```

## applymap()方法

最后，`[pandas.DataFrame.applymap](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.applymap.html)`方法只能应用于 pandas DataFrame 对象，并用于在元素方面应用指定的函数**。该方法只接受*调用*，最适合转换多行或多列中的值。**

```
**df[['colA', 'colD']] = df[['colA', 'colD']].applymap(lambda x: x**2)**print(df)
 *colA  colB   colC     colD   colE
0     1   521   True   102.01  Hello
1     4   723  False  2937.64    Hey
2     9   123  False  1102.24  Howdy
3    16   641   True  2361.96     Hi
4    25   467  False  9623.61    Hey*
```

## **最后的想法**

**在今天的简短指南中，我们讨论了`apply()`、`map()`和`applymap()`方法在熊猫身上的作用。此外，我们展示了如何使用这些方法，并探讨了它们的主要区别。**

**有时，可能不清楚您是否需要使用`apply()`或`applymap()`方法，因此，最好的方法是简单地测试两种方法，然后选择更有效/更快的方法。**

**[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。****

**[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership)** 

****你可能也会喜欢****

**[](/whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351) [## Python 中静态方法和类方法有什么区别？

### 关于 classmethod 和 staticmethod，您只需要知道

towardsdatascience.com](/whats-the-difference-between-static-and-class-methods-in-python-1ef581de4351)** **[](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) [## 使用预提交挂钩自动化 Python 工作流

### 什么是预提交钩子，它们如何给你的 Python 项目带来好处

towardsdatascience.com](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb)** **[](/mastering-indexing-and-slicing-in-python-443e23457125) [## 掌握 Python 中的索引和切片

### 深入研究有序集合的索引和切片

towardsdatascience.com](/mastering-indexing-and-slicing-in-python-443e23457125)**