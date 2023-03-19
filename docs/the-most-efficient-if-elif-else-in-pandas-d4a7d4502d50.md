# 熊猫中效率最高的 if-elif-else

> 原文：<https://towardsdatascience.com/the-most-efficient-if-elif-else-in-pandas-d4a7d4502d50?source=collection_archive---------4----------------------->

## 以及为什么我(和你一样)可能一直写得很低效

![](img/d9dcb92572ec91bd741d6235362f4e9c.png)

克里斯里德在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

简单的 if else 语句是每种编程语言的主要特点。Numpy 和 Pandas 为 Python 爱好者带来了无与伦比的速度，很明显，人们需要创建一个 if-elif-else 特性，该特性可以矢量化并有效地应用于数据集中的任何特定列。然而，即使看起来是这样，如果你真的在谷歌上搜索做这件事的最佳方法…真的没有明确的答案。有些用户声称使用 loc 和 iloc 最快，有些用户声称 [pandas.where()最快](https://stackoverflow.com/questions/43391591/if-else-function-in-pandas-dataframe)，有些用户甚至声称列表理解是最简单高效的方法。这些说法没有证据，只是用户分享了他们用来完成这个看似简单的任务的方法。正因为如此，我在互联网上搜寻了每一种可以完成任务的方法，最终的赢家完全出乎我的意料。

## 设置问题

为了测试这些方法，我想找到一个大小适中、易于使用的数据集。我选择了 Kaggle 的这个数据集，它包含 30，000 名信用卡客户及其相关的账单和支付。在那里，我将账单列重命名为“COL_1 ”,将付款列重命名为“COL_2 ”,这样我创建的这些函数就可以很容易地应用于任何问题。

对于这个数据集，我想创建一个新列“RESULT”，它基于一个简单的 if-elif-else 概念。

```
def new_column(row):
    if row['COL_1'] <= row['COL_2']:
        return 1
    elif row['COL_2'] == 0:
        return -1
    else:
        return 0
```

我们比较这两行，看看付款是否大于账单，如果是这样，返回 1。如果他们没有支付任何费用，则返回-1，而在任何其他情况下(他们支付的费用不足以支付账单)，则返回 0。

然后，我发现了将 if-elif-else 问题应用于我的数据集的 6 种不同方法。对于其中的每一个，我运行操作 100 次，存储结果，执行 3 次，并使用最低的分数作为我们的官方比较标准。

## 方法 1(最简单):将函数直接应用于数据帧

```
df['RESULT'] = df.apply(new_column, axis=1)28.503215789794922
28.901722192764282
29.452171087265015
---MIN---
28.503215789794922
```

Pandas 自带了一个内置方法(dataframe.apply ),直接将我们上面写的函数应用于每一列。对于不熟悉 Pandas 的人来说，这是迄今为止可读性最强、最容易理解的方法，尽管众所周知这种方法效率较低，但对于较小的数据集(少于 100，000 行)，您不太可能注意到速度下降，因为我能够在 28 秒内运行 100 次该函数的迭代，或者对于 30，000 行来说大约 0.3 秒。

## 方法 2:使用 lambda 和嵌套 if 语句进行应用

```
df['RESULT'] = df.apply(lambda x: 1 if x['COL_1'] <= x['COL_2'] 
               else (-1 if x['COL_2'] == 0 else 0), axis=1)28.910192251205444
29.083810329437256
28.666099071502686
---MIN---
28.666099071502686
```

这个测试背后的目标是看看使用 lambda 和嵌套 if 语句是否比只写出函数有任何显著的速度提升。如果这个测试证明了什么的话，那就是 Python 的编译器非常高效，写出完整的函数与 lambda 相比并没有真正的优势。因此，如果你打算使用 apply，只需写一个可读性函数。

## 方法 3(最简单有效的方法):使用 loc

```
df.loc[df['COL_1'] <= df['COL_2'], 'RESULT'] = 1
df.loc[df['COL_2'] == 0, 'RESULT'] = -1
df.loc[df['COL_1'] > df['COL_2'], 'RESULT'] = 00.267972469329834
0.24548792839050293
0.23981308937072754
---MIN---
0.23981308937072754
```

这是我一直用的方法。它简单、干净，比使用 apply 快 100 多倍。这种方法的唯一缺点是您可能会覆盖已经写好的值。例如，如果我交换第二个和第三个语句，全 1 将被替换为 0，因为两个条件都满足。我满以为这个方法是最有效的，然而，我错了。

## 方法 4(复杂但高效):链式 dataframe.where()

```
df["RESULT"] = 0
df["RESULT"] = df["RESULT"].where(df['COL_1'] > 
               df['COL_2'], 1).where(df['COL_2'] != 0, -1)0.19342803955078125
0.2046663761138916
0.2063906192779541
---MIN---
0.19342803955078125
```

当这比使用 loc 快 24%时，我很惊讶。使用 dataframe.where 显然是最快的方法之一，但是，由于它的语法很难学，所以很少使用。这是因为([from numpy . Where docs](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.where.html))“其中 cond 为真，保持原来的值。如果为 False，则替换为 other 中的相应值。在前面的所有例子中，我们都是在 True 时进行替换，所以本质上每个 where 条件都需要进行反转，以产生与使用 lambda 时相同的结果。在本例中，结果列也需要初始化为“else”条件才能正常工作。

## 方法 5(令人惊讶的赢家):Numpy.select

```
df[“RESULT”] = 0
df[“RESULT”] = np.select(condlist=[df[‘COL_1’] <= df[‘COL_2’],
               df[‘COL_2’] == 0],
               choicelist=[1,-1],
               default=0)0.12417435646057129
0.12804388999938965
0.12969422340393066
---MIN---
0.12417435646057129
```

没错。令人惊讶的最快结果来自 Numpy.select，它不仅快，而且比 where 快 56%，比 apply 快 230 倍。Numpy.select 也比我认为的 where 更直观一些。像 where 一样，我们将所有内容都设置为 else 条件，然后在 condlist 中为每个 if 提供一个列表，在 choicelist 中提供操作/结果。在写这篇文章之前，我甚至不知道这个功能的存在，现在，它是我的新工具，也应该是你的！

## 方法 6(失败者):用 iterrows()列出理解

```
df["RESULT"] = [1 if x[1]['COL_1'] <= x[1]['COL_2'] else (-1 if x[1]
               ['COL_2'] == 0 else 0) for x in df.iterrows()]124.71909689903259
124.28108930587769
123.89728021621704
---MIN---
123.89728021621704
```

请不要这样做。即使是一次迭代，它的速度也要慢 1 秒以上，而且不如 apply 直观，但速度却慢了 4 倍以上。在阅读了这篇文章后，我研究了这种方法，这篇文章声称列表理解比 where 快 50%。

公平地说，他们只是在寻找一个 if-else 命令，其中的数据只来自一个单独的列。这消除了使用 iterrows 的需要，因为当选择单个列时，默认的 python 行为是简单地迭代该列。为了测试这一点，我将问题简化为只在 COL_2 中查找 0，如果有 0，则设置为-1，否则设置为 0。

```
df["RESULT"] = [-1 if x == 0 else 0 for x in df['COL_2']]0.662632942199707
0.6409032344818115
0.6460835933685303
---MIN---
0.6409032344818115
```

这个函数看起来很简洁，比使用 iterrows 快 200 倍，但即使如此，在一个更困难的问题上使用 where 仍然快 3 倍以上，这意味着从来没有一个理解列表的好地方。

**但是如果我只需要一个简单的 if-else 呢？**

为了彻底起见，我在所有最快的方法(loc、where 和 select)上测试了简化的 if-else 问题，看看是否有相同的结果。我还在 Numpy.where 中添加了一个额外的竞争者(因为它只能用于 2 个选项问题)

```
#Dataframe.loc assignment
df['RESULT'] = 0
df.loc[df['COL_2'] == 0, 'RESULT'] = -10.08055353164672852
0.08478784561157227
0.08516192436218262
---MIN---
0.08055353164672852#Dataframe.where
df["RESULT"] = 0
df["RESULT"] = df["RESULT"].where(df['COL_2'] != 0, -1)0.11904525756835938
0.11285901069641113
0.13939785957336426
---MIN---
0.11285901069641113#Numpy.select
df["RESULT"] = 0
df["RESULT"] = np.select(condlist=[df['COL_2'] == 0],
               choicelist=[-1],
               default=0)0.07214689254760742
0.07320952415466309
0.07971072196960449
---MIN---
0.07214689254760742#Numpy.where
df["RESULT"] = 0
df["RESULT"] = np.where(df['COL_2'] == 0, 0, -1)0.06414675712585449
0.06172466278076172
0.06483030319213867
---MIN---
0.06172466278076172
```

这表明 np.select 和 np.where 都处于领先地位，在这种情况下，np.where 在可读性和性能方面都超过了 select。注意 Numpy 和 Pandas“where”命令是不同的，需要不同的参数。Numpy 的 where 是 np.where(条件，真时取值，假时取值)，比熊猫版直观多了。

## 结论

我仍然对这个测试的结果感到惊讶。我从来没有想到 numpy.select 会比一些主要的 Pandas 函数表现得更好，而 np.where 是简单 if-else 函数的最佳选择。

对于所有的数据科学家和熊猫爱好者，我希望你学到了一些东西，这有助于让你的熊猫之旅变得轻松一些。

如果你喜欢这篇文章，并想阅读我写的更多内容，请将你认为还会需要的文章加入书签，并考虑跟随的[或查看下面的其他文章！如果你正在寻找 Python/Pandas 的完整课程，我推荐](http://follow me)[这些家伙](https://datasciencedojo.com/python-for-data-science/?ref=jdibattista3)！

[](/the-most-efficient-way-to-merge-join-pandas-dataframes-7576e8b6c5c) [## 合并/连接熊猫数据帧的最有效方法

### 为什么几乎每个人都写得很低效

towardsdatascience.com](/the-most-efficient-way-to-merge-join-pandas-dataframes-7576e8b6c5c) [](/deep-learning-on-a-budget-450-egpu-vs-google-colab-494f9a2ff0db) [## 深度学习预算:450 美元 eGPU vs 谷歌 Colab

### Colab 对于开始深度学习来说是非凡的，但它如何与 eGPU +超极本相抗衡？

towardsdatascience.com](/deep-learning-on-a-budget-450-egpu-vs-google-colab-494f9a2ff0db) [](/building-a-budget-news-based-algorithmic-trader-well-then-you-need-hard-to-find-data-f7b4d6f3bb2) [## 建立一个基于预算新闻的算法交易者？那么你需要很难找到的数据

### 创建一个零美元的算法交易者，分析免费的 API，数据集和网页抓取器。

towardsdatascience.com](/building-a-budget-news-based-algorithmic-trader-well-then-you-need-hard-to-find-data-f7b4d6f3bb2)