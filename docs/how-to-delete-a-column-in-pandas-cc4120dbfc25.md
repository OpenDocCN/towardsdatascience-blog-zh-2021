# 如何删除熊猫中的一个栏目

> 原文：<https://towardsdatascience.com/how-to-delete-a-column-in-pandas-cc4120dbfc25?source=collection_archive---------14----------------------->

## 从 pandas 数据框架中删除列

![](img/cc59e54a9631f37ad1d05c5c1eff22dc.png)

照片由[夏羽·亚伊奇](https://unsplash.com/@stefyaich?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

从 pandas 数据帧中删除列是一项相当常见的任务。在今天的简短指南中，我们将探讨如何通过名称删除特定的列。更具体地说，我们将讨论如何使用以下命令删除列:

*   `del`命令
*   `drop()`方法
*   `pop()`法

此外，我们将讨论如何通过指定索引而不是名称来删除列。

首先，让我们创建一个示例 pandas 数据框架，我们将在本指南中使用它来演示一些概念。

```
import pandas as pd df = pd.DataFrame({
    'colA':[1, 2, 3], 
    'colB': ['a', 'b', 'c'],
    'colC': [True, False, False],
})print(df)#    colA colB   colC
# 0     1    a   True
# 1     2    b  False
# 2     3    c  False
```

## 使用 del 删除列

如果您想要删除特定的列，您的第一个选项是调用如下所示的`del`:

```
**del df['colC']**print(df)#    colA colB
# 0     1    a
# 1     2    b
# 2     3    c
```

只有当您希望删除一列时，这种方法才有效。如果您需要一次删除多个列，请阅读以下部分。

另外，需要注意的是`del df.colC`T16 不能用！语法必须与上面示例中显示的语法相同。

## 使用删除多个列。丢弃()

`[pandas.DataFrame.drop](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html)`方法用于从行或列中删除指定的标签。

为了删除列，您需要指定如下所示的`axis=1`:

```
**df = df.drop(['colA', 'colC'], axis=1)**print(df)#   colB
# 0    a
# 1    b
# 2    c
```

注意，如果您想删除列，而不必将结果重新分配给`df`，您需要做的就是将`inplace`指定给`True`:

```
**df.drop(['colA', 'colC'], axis=1, inplace=True)**
```

## 使用删除列。流行()

另一个选项是`[pandas.DataFrame.pop](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pop.html)`，它返回被删除的列，并最终将其从原始数据帧中删除。

```
**df.pop('colB')**
```

将返回我们希望从 DataFrame 中删除的列`colB`:

```
0    a
1    b
2    c
```

我们可以验证该列确实已经从原始框架中删除:

```
print(df) colA   colC
0     1   True
1     2  False
2     3  False
```

## 按索引删除列

在上面的小节中，我们探讨了如何通过名称删除特定的列。但是，您可能还需要通过引用特定列的索引来删除该列。

为此，可以使用`[pandas.DataFrame.drop](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html)`方法。假设我们想要删除索引分别为`0`和`2`的列`colA`和`colC`，我们可以这样做，如下所示:

```
**df.drop(df.columns[[0, 2]], axis=1, inplace=True)**
```

## 最后的想法

在今天的简短指南中，我们探讨了如何通过名称或索引以各种方式删除 pandas 数据帧中的特定列。

您可能也对阅读如何重命名熊猫数据帧的列感兴趣。如果是这样，请不要错过下面的文章。

[](/how-to-rename-columns-in-pandas-d35d13262c4f) [## 如何重命名 Pandas 中的列

### 重命名熊猫数据框架中的列

towardsdatascience.com](/how-to-rename-columns-in-pandas-d35d13262c4f) 

此外，下面的文章讨论了如何根据特定条件执行正确的行选择。

[](/how-to-select-rows-from-pandas-dataframe-based-on-column-values-d3f5da421e93) [## 如何根据列值从 Pandas 数据框架中选择行

### 探索如何根据 pandas 数据框架中的条件选择行

towardsdatascience.com](/how-to-select-rows-from-pandas-dataframe-based-on-column-values-d3f5da421e93)