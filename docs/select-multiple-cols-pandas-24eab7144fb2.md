# 从 Pandas 数据框架中选择多个列

> 原文：<https://towardsdatascience.com/select-multiple-cols-pandas-24eab7144fb2?source=collection_archive---------32----------------------->

## 讨论如何在 pandas 中从数据帧中选择多列

![](img/7c896491fa51a54ed1039e2337e3ce5f.png)

照片由 [Aviv Ben 或](https://unsplash.com/@vivanebro?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/vertical?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

多列选择是最常见和最简单的任务之一。在今天的简短指南中，我们将讨论从 pandas 数据框架中选择多列的几种可能方法。具体来说，我们将探索如何做到这一点

*   使用**基准分度**
*   用`**loc**`
*   使用`**iloc**`
*   通过创建一个**新数据帧**

此外，我们将根据您的特定用例以及您是否需要生成原始 DataFrame 对象的视图或副本，讨论何时使用一种方法而不是另一种方法。

首先，让我们创建一个示例数据框架，我们将在本文中引用它来演示一些概念。

```
import pandas pd df = pd.DataFrame({
    'colA':[1, 2, 3], 
    'colB': ['a', 'b', 'c'],
    'colC': [True, False, True],
    'colD': [1.0, 2.0, 3.0],
})print(df)
 *colA colB   colC  colD
0     1    a   True   1.0
1     2    b  False   2.0
2     3    c   True   3.0*
```

## 使用基本索引

当从现有的 pandas 数据框架中选择多个列时，您的第一个选择是使用**基本索引**。当您确切知道要保留哪些列时，这种方法通常很有用。

因此，您可以通过使用`[]`符号(相当于 Python 类中的`[__getitem__](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#basics)`实现)传递带有名称的列表，获得只包含这些列的原始数据帧的副本。

```
df_result = **df[['colA', 'colD']]**print(df_result)
 *colA  colD
0     1   1.0
1     2   2.0
2     3   3.0*
```

如果您想了解更多关于 Python 中索引和切片的工作原理，请务必阅读下面的文章。

</mastering-indexing-and-slicing-in-python-443e23457125>  

## 使用 iloc

或者，如果您想**引用列索引而不是列名**并分割原始数据帧(例如，如果您想保留前两列，但您并不真正知道列名)，您可以使用`**iloc**`。

```
df_result = **df.iloc[:, 0:2]**print(df_result)
 *colA colB
0     1    a
1     2    b
2     3    c*
```

注意，上面的操作，**将返回原始数据帧的视图**。这意味着视图将只包含原始数据帧的一部分，但它仍将指向内存中的相同位置。因此，如果您修改切片对象(`df_result`)，那么这也可能影响原始对象(`df`)。

如果您希望分割数据帧，但却得到原始对象的副本，那么只需调用`copy()`:

```
df_result = **df.iloc[:, 0:2].copy()**
```

## 使用 loc

现在，如果您想使用实际的列名对原始数据帧进行切片，那么您可以使用`loc`方法。例如，如果您想要获得前三列，您可以通过引用您想要保留的列的名字和姓氏，使用`loc`来实现:

```
df_result = df.loc[:, 'colA':'colC']print(df_result)
 *colA colB   colC
0     1    a   True
1     2    b  False
2     3    c   True*
```

此时，您可能想要了解 Pandas 中`loc`和`iloc`之间的差异，并根据您的具体需求和使用案例阐明使用哪一个。

</loc-vs-iloc-in-pandas-92fc125ed8eb> [## 熊猫中的 loc 与 iloc

towardsdatascience.com](/loc-vs-iloc-in-pandas-92fc125ed8eb) 

## 创建新的熊猫数据框架

最后，您甚至可以只使用原始数据帧中包含的列的子集来创建新的数据帧，如下所示。

```
df_result = **pd.DataFrame(df, columns=['colA', 'colC'])**print(df_result)
 *colA   colC
0     1   True
1     2  False
2     3   True*
```

## 最后的想法

在今天的简短指南中，我们展示了从 pandas 数据框架中选择多个列的几种可能的方法。我们讨论了如何使用简单的索引、`iloc`、`loc`以及通过创建新的数据帧来实现这一点。请注意，本文中讨论的一些方法可能会生成原始数据帧的视图，因此您应该格外小心。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</how-to-iterate-over-rows-in-a-pandas-dataframe-6aa173fc6c84>  </how-to-auto-adjust-the-width-of-excel-columns-with-pandas-excelwriter-60cee36e175e>  </how-to-get-the-row-count-of-a-pandas-dataframe-be67232ad5de> 