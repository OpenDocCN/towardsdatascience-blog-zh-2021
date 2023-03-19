# 停止使用 XGBoost…

> 原文：<https://towardsdatascience.com/stop-using-xgboost-660ed6718845?source=collection_archive---------20----------------------->

## 意见

## 而是使用 CatBoost。

![](img/2437c46bfd35d74a931380b6c8dbf4a1.png)

由 [Pacto 视觉](https://unsplash.com/@pactovisual?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【1】上拍摄。

# 目录

1.  介绍
2.  XGBoost 不利
3.  CatBoost 优势
4.  摘要
5.  参考

# 介绍

我首先要说的是，XGBoost 是一种非常强大的机器学习算法，已经被证明赢得了无数的数据科学比赛，并且很可能处于数据科学中最专业的用例的最前沿。出于这个原因，这种算法在过去几年中一直受到关注，这是一件好事。也就是说，这意味着它的缺点也受到了关注，因此，产生了开发类似算法的动机，这种算法在 XGBoost 没有的地方表现出色。让我们讨论 XGBoost 及其竞争对手 CatBoost，并强调为什么您会想要使用这种新兴的算法，无论您是在学校，还是作为专业数据科学家和/或机器学习工程师工作。请继续阅读下面的内容，了解 XGBoost 的缺点以及 CatBoost 的优点。

# XGBoost 不利

![](img/b75f8ac208cacf55a46b060eae1618f8.png)

Sebastian Unrau 在[Unsplash](https://unsplash.com/s/photos/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【2】上的照片。

这个[算法](https://xgboost.readthedocs.io/en/latest/)【3】是基于决策树的，利用优化的分布式梯度推进。

> 仅数值

XGBoost 很棒——有数值。这意味着它不能处理分类特征，除非它们被转换以使模型工作。大多数数据科学家会使用 one-hot-encoding，它会将特征值转置为列本身，并将一个`0`或`1`作为值。这也很棒，但是会导致另一个问题。

> 稀疏数据集

这里我所说的稀疏是指，因为一个分类特性不能有一个单独的列，所以该分类特性中的唯一值有多少列就有多少列。例如，如果有 100 个唯一值，您将有 100 多列追加到您的数据集，充满 0 和 1。当然，这会导致另一个问题。

> 长期培训

随着越来越多的分类要素转换为数值要素，这可能意味着您的模型将需要更长的时间来运行更大的数据集。

> 部署

当您想要部署模型并对新数据进行推断或预测时，这些数据必须与您的训练数据具有相同的结构。这个问题意味着您将不得不转换您的预测数据，这将导致更多的代码、更多的出错空间以及更多的麻烦。

> 因素

XGBoost 非常强大，但是参数太多，也很难调整。理解所有参数并对它们进行调优可能需要一些时间。

如您所见，这些缺点大多与分类特征有关。所以，你可以看到为什么会有一个类似的模型与 XGBoost 有很多相同的好处，但是专注于修复 XGBoost 的类别问题。因此，命名为 CatBoost——分类增强。

# CatBoost 优势

![](img/4f8b435a4ceab9a7c6619d06c265c130.png)

Ludemeula Fernandes 在[Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)【4】上拍摄的照片。

这个[算法](https://catboost.ai/#:~:text=CatBoost%20-%20open-source%20gradient%20boosting%20library)【5】也是一个基于梯度提升无决策树的高置换算法，重点是处理分类特征，这在过去常常被忽略。以下是 CatBoost 的优势:

> 处理分类特征

*   不需要将分类特征转换成数字特征
*   这意味着您不必对数据进行太多的预处理(*——取决于您的用例*)
*   您的数据集可以保持其原始格式，如原始列，而不必制作一次性热编码列，这更容易/更简单，更有利于部署，并向利益相关者解释您的功能

> 简单参数

*   利用 CatBoost 的默认参数最有可能获得最佳结果
*   也就是说，花在调整参数上的时间更少

> 准确度提高

*   虽然 XGBoost 在大多数算法中往往具有最好的准确性，但 CatBoost 已经在严重的流行数据集上被证明可以击败它

这两种算法都很棒，但是如果你有分类特征，那么 CatBoost 是一个很好的选择。

# 摘要

这篇文章是片面的，因为我强调了一种算法的优点，同时讨论了另一种算法的缺点。然而，这就是为什么 CatBoost 是这样的。但是，没有 XGBoost 就没有 CatBoost，所以总而言之，算法和各自的库都是强大的、有益的，最终由您决定使用哪一个。还可能出现其他因素，比如文档。XGBoost 有更多的在线文档和示例，这是 CatBoost 努力的地方，但随着时间的推移，我可以看到 CatBoost 在这方面有所改进。或许，会出现另一种甚至比 CatBoost 更好的算法。

> 总而言之，以下是使用 CatBoost 的主要原因:

```
* Ease of using categorical features* Implementation of dataset processing* Improved accuracy
```

我希望你觉得我的文章既有趣又有用。如果你同意这些比较，请在下面随意评论——为什么或为什么不同意？你更喜欢 XGBoost 还是 CatBoost？如果你有，请随意评论你喜欢其中一个的原因。CatBoost 有哪些缺点？你是否知道一种更新的机器学习算法和库，在简单性、易用性和准确性方面可以最好地超越 CatBoost？谢谢你看我的文章！

*请随时查看我的个人资料和其他文章，* [*马特·普日比拉*](https://medium.com/u/abe5272eafd9?source=post_page-----660ed6718845--------------------------------) ，*也可以在 LinkedIn 上联系我。*

# 参考

[1]图片由 [Pacto Visual](https://unsplash.com/@pactovisual?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄，(2016)

[2]Sebastian Unrau 在 [Unsplash](https://unsplash.com/s/photos/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片，(2015)

[3] xgboost 开发人员， [XGBoost 文档](https://xgboost.readthedocs.io/en/latest/)，(2020)

[4]lude meula Fernandes 在 [Unsplash](https://unsplash.com/s/photos/cat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片，(2017)

[5] Yandex， [CatBoost 文档](https://catboost.ai/#:~:text=CatBoost%20-%20open-source%20gradient%20boosting%20library)，(2021)