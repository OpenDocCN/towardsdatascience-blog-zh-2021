# 如何迭代熊猫数据框架中的行

> 原文：<https://towardsdatascience.com/how-to-iterate-over-rows-in-a-pandas-dataframe-6aa173fc6c84?source=collection_archive---------3----------------------->

## 讨论如何在 pandas 中迭代行，以及为什么最好避免(如果可能的话)

![](img/7d717235834920cfe3ecf7e4fdcf6ddf.png)

由[迪尔](https://unsplash.com/@thevisualiza?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/repetition?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 介绍

对 pandas 数据帧进行迭代绝对不是一个最佳实践，你应该只在绝对必要的时候，并且当你已经尝试了所有其他可能的更优雅和更有效的选择时，才考虑这样做。

> 遍历熊猫对象一般**慢**。在许多情况下，不需要手动迭代这些行，并且可以避免
> 
> — [熊猫文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/basics.html#iteration)

在今天的文章中，我们将讨论如何避免在 pandas 中遍历数据帧。在选择迭代方法之前，我们还将浏览一份“清单”,您可能每次都需要参考它。此外，我们将探讨在没有其他选项适合您的特定用例的情况下如何做到这一点。最后，我们将讨论为什么在迭代熊猫对象时应该避免修改它们。

## 真的需要遍历行吗？

正如 pandas 官方文档中所强调的，通过数据帧的迭代是非常低效的，并且通常可以避免。通常，pandas 新成员不熟悉矢量化的概念，也不知道 pandas 中的大多数操作应该(也可以)在非迭代环境中执行。

在尝试遍历 pandas 对象之前，您必须首先确保以下选项都不符合您的用例需求:

*   **迭代矢量化** : pandas 提供了丰富的内置方法，其性能得到了优化。大多数操作都可以使用这些方法中的一种来执行。此外，您甚至可以查看一下`numpy`并检查它的任何功能是否可以在您的上下文中使用。
*   **将函数应用于行:**一个常见的需求是将函数应用于每一行，比如说，一次只能应用于一行，而不能应用于整个数据帧或序列。在这种情况下，最好使用`[apply()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas.DataFrame.apply)`方法，而不是遍历 pandas 对象。要了解更多细节，你可以参考 pandas 文档的这一部分，它解释了如何将你自己的或另一个库的函数应用于 pandas 对象。
*   **迭代操作:**如果您需要执行迭代操作，同时性能也是一个问题，那么您可能需要考虑 cython 或 numba。关于这些概念的更多细节，你可以阅读熊猫文档的这一部分。
*   **打印一个数据帧:**如果你想打印一个数据帧，那么只需使用`[DataFrame.to_string()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_string.html)`方法，以便将数据帧呈现为一个控制台友好的表格输出。

## 迭代数据帧的行

如果以上选项都不适合您，那么您可能仍然希望遍历熊猫对象。您可以使用`iterrows()`或`itertuples()`内置方法来实现。

在查看这两种方法的运行之前，让我们创建一个示例数据帧，我们将使用它进行迭代。

```
import pandas as pd df  = pd.DataFrame({
    'colA': [1, 2, 3, 4, 5],
    'colB': ['a', 'b', 'c', 'd', 'e'],
    'colC': [True, True, False, True, False],
})print(df)
 *colA colB   colC
0     1    a   True
1     2    b   True
2     3    c  False
3     4    d   True
4     5    e  False*
```

*   `[**pandas.DataFrame.iterrows(**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iterrows.html#pandas.DataFrame.iterrows)**)**`方法用于将 DataFrame 行作为`(index, Series)`对进行迭代。请注意，该方法不会跨行保存`dtypes`，因为该方法会将每一行转换为`Series`。如果您需要保留 pandas 对象的 dtypes，那么您应该使用`itertuples()`方法。

```
for index, row in **df.iterrows**():
    print(row['colA'], row['colB'], row['colC'])*1 a True
2 b True
3 c False
4 d True
5 e False*
```

*   `[**pandas.DataFrame.itertuples()**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.itertuples.html#pandas.DataFrame.itertuples)`方法用于以命名元组的形式迭代数据帧行。**一般来说，** `**itertuples()**` **预计会比** `**iterrows()**` **快。**

```
for row in **df.itertuples**():
    print(row.colA, row.colB, row.colC)*1 a True
2 b True
3 c False
4 d True
5 e False*
```

关于 Python 中**命名元组的更多细节，你可以阅读下面的文章。**

</what-are-named-tuples-in-python-59dc7bd15680>  

## 迭代行时修改

在这一点上，重要的是要强调**你不应该修改你正在迭代的 pandas 数据帧或系列**。根据 pandas 对象的数据类型，迭代器可能返回对象的副本，而不是视图。在这种情况下，向副本中写入任何内容都不会达到预期的效果。

例如，假设我们想要将`colA`中每一行的值加倍。**迭代方法不会奏效**:

```
for index, row in df.iterrows():
  row['colA'] = row['colA'] * 2print(df)
 *colA colB   colC
0     1    a   True
1     2    b   True
2     3    c  False
3     4    d   True
4     5    e  False*
```

在类似的用例中，你应该使用`apply()`方法。

```
df['colA'] = df['colA'].apply(lambda x: x * 2)print(df)
 *colA colB   colC
0     2    a   True
1     4    b   True
2     6    c  False
3     8    d   True
4    10    e  False*
```

## 最后的想法

在今天的文章中，我们讨论了为什么在处理 pandas 对象时避免迭代方法是重要的，而更喜欢矢量化或任何其他适合您特定用例的方法。

pandas 提供了一组丰富的内置方法，这些方法针对大型 pandas 对象进行了优化，您应该始终优先选择这些方法，而不是其他迭代解决方案。如果您仍然想要/必须迭代一个数据帧或系列，您可以使用`iterrows()`或`itertuples()`方法。

最后，我们讨论了为什么必须避免修改正在迭代的 pandas 对象，因为这可能不会像预期的那样工作。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒体上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</how-to-efficiently-convert-a-pyspark-dataframe-to-pandas-8bda2c3875c3>  </how-to-drop-rows-in-pandas-dataframes-with-nan-values-in-certain-columns-7613ad1a7f25>  </what-is-machine-learning-e67043a3a30c> 