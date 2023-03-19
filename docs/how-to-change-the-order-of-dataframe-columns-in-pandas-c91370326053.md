# 如何改变熊猫数据框架中列的顺序

> 原文：<https://towardsdatascience.com/how-to-change-the-order-of-dataframe-columns-in-pandas-c91370326053?source=collection_archive---------15----------------------->

## 在 pandas 数据框架中更改列顺序并将列移到前面

![](img/88a2af456ffbf973836b5afdf57a9eb8.png)

[斯通王](https://unsplash.com/@stonewyq?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 介绍

对 pandas 数据帧中的列重新排序是我们想要执行的最常见的操作之一。当涉及到向其他人展示结果时，这通常是有用的，因为我们需要以某种逻辑顺序对(至少几个)列进行排序。

在今天的文章中，我们将讨论如何使用

*   原始帧的切片——当您需要**重新排序大多数列**时最相关
*   `insert()`方法——如果你想**将一个单独的列插入到指定的索引中**
*   `set_index()` —如果需要**将一列移动到数据框的前面**
*   和，`reindex()`方法——主要与您可以**按照您希望的顺序指定列索引的情况相关**(例如，按照字母顺序)

首先，让我们创建一个我们将在本指南中引用的示例数据帧。

```
import pandas as pddf = pd.DataFrame({
    'colA':[1, 2, 3], 
    'colB': ['a', 'b', 'c'],
    'colC': [True, False, False],
    'colD': [10, 20, 30],
})print(df)
#    colA colB   colC  colD
# 0     1    a   True    10
# 1     2    b  False    20
# 2     3    c  False    30
```

## 使用切片

最简单的方法是使用一个列表分割原始数据帧，该列表包含您希望它们遵循的新顺序的列名:

```
**df = df[['colD', 'colB', 'colC', 'colA']]**print(df)
#    colD colB   colC  colA
# 0    10    a   True     1
# 1    20    b  False     2
# 2    30    c  False     3
```

如果您想对大多数列的名称进行重新排序，这种方法可能已经足够好了(而且您的数据框架可能有太多的列)。

## 使用 insert()方法

如果您需要**将列插入数据帧的指定位置**，那么`[pandas.DataFrame.insert()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.insert.html)`应该可以做到。但是，您应该确保首先将该列从原始数据帧中取出，否则将出现一个带有以下消息的`ValueError`:

```
ValueError: cannot insert column_name, already exists
```

因此，在调用`insert()`之前，我们首先需要对数据帧执行`[pop()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.pop.html)`操作，以便从原始数据帧中删除该列并保留其信息。例如，如果我们想将`colD`作为框架的第一列，我们首先需要`pop()`该列，然后将其插回，这次是插入到所需的索引。

```
**col = df.pop("colD")
df.insert(0, col.name, col)** print(df)
#    colD  colA colB   colC
# 0    10     1    a   True
# 1    20     2    b  False
# 2    30     3    c  False
```

## 使用 set_index()方法

如果你想把一列移到熊猫数据框的前面，那么`[set_index()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.set_index.html)`就是你的朋友。

首先，您指定我们希望移到前面的列作为数据帧的索引，然后重置索引，以便旧的索引作为一列添加，并使用新的顺序索引。同样，请注意我们如何`pop()`在将列添加为索引之前删除它。这是必需的，否则当试图使旧索引成为数据帧的第一列时会发生名称冲突。

```
**df.set_index(df.pop('colD'), inplace=True)**#       colA colB   colC 
# colD
# 10       1    a   True
# 20       2    b  False
# 30       3    c  False**df.reset_index(inplace=True)**#    colD  colA colB   colC
# 0    10     1    a   True
# 1    20     2    b  False
# 2    30     3    c  False
```

## 使用 reindex()方法

最后，如果您想要**按照您希望的顺序指定列索引**(例如按字母顺序)，您可以使用`[reindex()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.reindex.html)`方法使数据帧符合新的索引。

例如，假设我们需要按字母降序排列列名

```
**df = df.reindex(columns=sorted(df.columns, reverse=True))**#    colD   colC colB  colA
# 0    10   True    a     1
# 1    20  False    b     2
# 2    30  False    c     3
```

请注意，上述内容相当于

```
df.reindex(sorted(df.columns, reverse=True), axis='columns')
```

## 最后的想法

在今天的简短指南中，我们讨论了如何以多种不同的方式改变熊猫数据帧中的列顺序。确保根据您的具体要求选择正确的方法。

**你可能也会喜欢**

</how-to-delete-a-column-in-pandas-cc4120dbfc25>  </easter-eggs-in-python-f32b284ef0c5>  </whats-the-difference-between-shallow-and-deep-copies-in-python-ceee1e061926> 