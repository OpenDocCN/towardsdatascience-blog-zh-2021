# 在熊猫数据帧中重排行

> 原文：<https://towardsdatascience.com/shuffling-rows-in-pandas-dataframes-eda052275635?source=collection_archive---------15----------------------->

## 讨论如何重排熊猫数据帧的行

![](img/e34caf2d8b7403e0b8120e1063c74c1e.png)

瑞安·昆塔尔在 [Unsplash](https://unsplash.com/s/photos/shuffle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

数据混洗是一项常见任务，通常在模型训练之前执行，以便创建更具代表性的训练和测试集。例如，假设您的原始数据集是基于特定列排序的。

如果分割数据，那么得到的集合将不会代表数据集的真实分布。因此，我们必须调整原始数据集，以最小化差异，并确保模型能够很好地推广到新的、看不见的数据点。

在今天的简短指南中，我们将讨论如何以各种方式重排熊猫数据帧的行。具体来说，我们将探索如何使用

*   `pandas`中的`sample()`方法
*   `scikit-learn`中的`shuffle()`方法
*   `numpy`中的`random.permutation()`方法

首先，让我们创建一个示例 pandas DataFrame，我们将在本文中引用它来演示如何以多种不同的方式重排行。

```
import pandas as pd df = pd.DataFrame({
    'colA': [10, 20, 30, 40, 50, 60],
    'colB': ['a', 'b', 'c', 'd', 'e', 'f'],
    'colC': [True, False, False, True, False, True],
    'colD': [0.5, 1.2, 2.4, 3.3, 5.5, 8.9],
})print(df)
 *colA colB   colC  colD
0    10    a   True   0.5
1    20    b  False   1.2
2    30    c  False   2.4
3    40    d   True   3.3
4    50    e  False   5.5
5    60    f   True   8.9*
```

## 在熊猫中使用 sample()方法

对 pandas 数据帧进行混排的第一个选择是`[panads.DataFrame.sample](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.sample.html)`方法，它返回一个随机的项目样本。在这种方法中，您可以指定想要采样的记录的确切数量或部分。因为我们想要打乱整个数据帧，所以我们将使用`frac=1`来返回所有记录。

```
**df = df.sample(frac=1)**print(df)
 *colA colB   colC  colD
4    50    e  False   5.5
0    10    a   True   0.5
2    30    c  False   2.4
1    20    b  False   1.2
5    60    f   True   8.9
3    40    d   True   3.3*
```

如果你想让洗牌在重现**，那么**也要确保为** `**random_state**` **参数**指定一个有效值。此外，有时您可能希望重置返回的数据帧的索引。在这种情况下，以下方法应该可以解决问题:**

```
df = df.sample(frac=1).reset_index(drop=True)
```

## 使用 scikit-learn 的 shuffle()方法

另一个可以用来混洗数据帧的函数是如下所示的`[sklearn.utils.shuffle()](https://scikit-learn.org/stable/modules/generated/sklearn.utils.shuffle.html)`:

```
**from sklearn.utils import shuffle****df = shuffle(df)**print(df)
 *colA colB   colC  colD
4    50    e  False   5.5
5    60    f   True   8.9
1    20    b  False   1.2
2    30    c  False   2.4
0    10    a   True   0.5
3    40    d   True   3.3*
```

同样，如果您希望结果是可重复的，请确保配置`random_state`参数。

## 在 numpy 中使用 random.permutations()

我们的另一个选择是随机排列序列的`[numpy.random.permutation](https://numpy.org/doc/stable/reference/random/generated/numpy.random.permutation.html)`方法。

```
import numpy as np**df = df.iloc[np.random.permutation(len(df))]**print(df)
 *colA colB   colC  colD
3    40    d   True   3.3
0    10    a   True   0.5
5    60    f   True   8.9
1    20    b  False   1.2
2    30    c  False   2.4
4    50    e  False   5.5*
```

同样，如果你希望结果是可重复的，你必须设置`numpy`的随机种子。举个例子，

```
np.random.seed(100)
```

## 最后的想法

在今天的简短指南中，我们讨论了数据洗牌在机器学习模型环境中的重要性。此外，我们还探索了如何使用`sample()`方法、`sklearn`中的`shuffle()`方法和`numpy`中的`random.permutations()`方法来洗牌。

如果你想了解更多关于**如何将数据分成训练集、测试集和验证集**的信息，那么请务必阅读下面的文章。

</how-to-split-a-dataset-into-training-and-testing-sets-b146b1649830>  

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</how-to-iterate-over-rows-in-a-pandas-dataframe-6aa173fc6c84>  <https://medium.com/geekculture/use-performance-visualization-to-monitor-your-python-code-f6470592a1cb>  </how-to-suppress-settingwithcopywarning-in-pandas-c0c759bd0f10> 