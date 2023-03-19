# 掌握熊猫价值排序需要知道的 8 件事

> 原文：<https://towardsdatascience.com/8-things-to-know-to-master-value-sorting-in-pandas-2f4eb07f69d4?source=collection_archive---------19----------------------->

## 用熊猫有效地排序值

![](img/3fa6d4d90d2bc508b1e48e942787ea79.png)

由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当我们处理数据时，排序是直观检查数据质量的重要预处理步骤。对于 pandas，虽然有时我们可能会使用一个相关的方法— `sort_index`，但大多数时候我们使用`sort_values`方法对数据进行排序。在这篇文章中，我想分享 8 件对你完成这个预处理步骤至关重要的事情——重点是`sort_values`方法。

事不宜迟，让我们开始吧。

## 1.按单列排序

在本文中，我们将使用 flights 数据集，它记录了从 1949 年到 1960 年的每月乘客人数。出于本教程的目的，我们将选择一个随机子集，如下所示。

用于排序的数据集

当我们希望按单个列对数据进行排序时，我们直接将列名指定为函数调用的第一个参数。顺便说一下，你可能会看到我经常使用`head`，只是为了在不浪费空间的情况下向你展示最高值。

```
>>> df.sort_values("year").head()
    year     month  passengers
0   1949      July         148
13  1951  February         150
8   1951     April         163
19  1951  December         166
5   1952       May         183
```

## 2.就地排序值

在前面的排序中，您可能已经注意到了一件事，那就是`sort_values`方法将创建一个新的`DataFrame`对象，如下所示。

```
>>> df.sort_values("year") is df
False
```

为了避免创建新的数据帧，您可以通过设置`inplace`参数请求就地进行排序。当你这样做时，注意调用`sort_values`将返回`None`。

```
>>> df.sort_values("year", inplace=True)
>>> df.head()
    year     month  passengers
0   1949      July         148
13  1951  February         150
8   1951     April         163
19  1951  December         166
5   1952       May         183
```

## 3.排序后重置索引

在前面的排序中，您可能注意到索引伴随着每一个排序的行，当我希望排序的数据帧有一个有序的索引时，这有时让我困惑。在这种情况下，您可以在排序后重置索引，或者简单地利用`ignore_index`参数，如下所示。

```
>>> df.sort_values("year", ignore_index=True).head()
   year     month  passengers
0  1949      July         148
1  1951  February         150
2  1951     April         163
3  1951  December         166
4  1952       May         183
```

## 4.按多列排序

我们并不总是需要一列来排序。在许多情况下，我们需要按多列对数据框进行排序。使用 sort_values 也很简单，因为`by`不仅接受单个列，还接受没有任何特殊语法的列列表。

```
>>> df.sort_values(["year", "passengers"]).head()
    year     month  passengers
0   1949      July         148
13  1951  February         150
8   1951     April         163
19  1951  December         166
17  1952   January         171
```

## 5.按降序排序

正如我们到目前为止所看到的，每个排序都是使用升序来完成的，这是默认的行为。但是，我们通常希望数据按降序排序。我们可以利用`ascending`参数。

```
>>> df.sort_values("year", ascending=False).head()
    year    month  passengers
18  1960     June         535
6   1958    April         348
4   1958  October         359
1   1957     June         422
7   1957    March         356
```

按多列排序，对这些列有不同的升序要求，怎么办？在这种情况下，我们可以传递一个布尔值列表，每个值对应一列。

```
>>> df.sort_values(["year", "passengers"], ascending=[False, True]).head()
    year    month  passengers
18  1960     June         535
6   1958    April         348
4   1958  October         359
7   1957    March         356
1   1957     June         422
```

## 6.按自定义函数排序

如果我们想用当前数据集按年份和月份排序，该怎么办？不用想太多就试试吧。

```
>>> df.sort_values(["year", "month"]).head()
    year     month  passengers
0   1949      July         148
8   1951     April         163
19  1951  December         166
13  1951  February         150
9   1952    August         242
```

显然，排序后的数据不是我们所期望的——月份没有按照预期的顺序排列。为了实现这一点，我们可以利用`sort_method`获取一个`key`参数，我们可以向其传递一个自定义函数进行排序，就像 Python 的内置`sorted`函数一样。一个可能的解决方案如下所示。

```
>>> def _month_sorting(x):
...     if x.name == "year":
...         return x
...     months = ["January", "February", "March", "April", 
...         "May", "June", "July", "August", 
...         "Septempber", "October", "November", "December"]
...     return x.map(dict(zip(months, range(0, len(months)))))
... 
>>> df.sort_values(["year", "month"], key=_month_sorting).head()
    year     month  passengers
0   1949      July         148
13  1951  February         150
8   1951     April         163
19  1951  December         166
17  1952   January         171
```

*   `key`接受一个可调用函数，我们在这里使用一个自定义函数。而且这个参数只有熊猫 1.1.0+才有。
*   与`sorted()`中使用的`key`参数不同，`key`函数适用于`sort_values`方法中的每个排序列。因为我们只想自定义“月”列的排序，所以当列为“年”时，我们希望使用“年”列的原始值。

## 7.转换为分类后，按字典顺序对无序列进行排序

上面使用`key`参数的排序可能会让一些人感到困惑。有没有更干净的方法？Pandas 可以说是用于数据处理的最通用的库，并且您可以期待有一些简洁的东西来解决这个相对常见的问题——将这些字典上无序的列转换成分类数据。

按类别排序

*   我们通过指定月份的顺序来定义一个`CategoricalDtype`。
*   我们将 month 列转换为新定义的类别。
*   当我们对月份进行排序时，它将使用类别数据定义中月份的顺序。

## 8.不要忘了 NANs

请务必记住，您的数据集可以始终包含 nan。除非你已经检查过你的数据质量并且知道没有 NANs，否则你应该注意这一点。默认情况下，当我们对值进行排序时，这些 nan 被放在所有其他有效值的后面。如果我们想改变这个默认行为，我们设置`na_position`参数。

用 NANs 排序

*   我们首先将一个 NAN 注入到`DataFrame`对象中。
*   当我们对`na_position`不做任何事情时，NAN 值被放在排序组的末尾。
*   当我们将`“first”`设置为`na_position`时，NAN 值出现在顶部。

## 结论

在本文中，我们回顾了关于熊猫排序值的 8 件事/条件，这应该涵盖了大多数用例。如果你觉得我错过了什么重要的东西，请随时留下评论！