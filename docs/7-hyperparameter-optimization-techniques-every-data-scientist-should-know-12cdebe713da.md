# 每个数据科学家都应该知道的 7 种超参数优化技术

> 原文：<https://towardsdatascience.com/7-hyperparameter-optimization-techniques-every-data-scientist-should-know-12cdebe713da?source=collection_archive---------14----------------------->

## 从手动调整到自动超参数优化—带实践示例

![](img/a501ccfd1bc1ddb829e530d446db0e3b.png)

图片由[穆罕默德·哈桑](https://pixabay.com/users/mohamed_hassan-5229782/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3679741)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3679741)

选择正确的机器学习模型和相应的正确的超参数集对于训练健壮的机器学习模型是必不可少的。机器学习模型的性能随着超参数调整而提高。

超参数是指模型无法学习，需要在训练前提供的参数。超参数调优基本上是指微调模型的参数，这基本上是一个漫长的过程。

在本文中，我们将讨论 7 种超参数优化技术，并给出一些实际例子。

```
**Hyperparameter Optimization Checklist:
*1) Manual Search
2) Grid Search
3) Randomized Search
4) Halving Grid Search
5) Halving Randomized Search
6) HyperOpt-Sklearn
7) Bayes Search***
```

# 预热:

信用卡欺诈检测数据集将用于训练基线逻辑回归模型。逻辑回归模型的其他超参数将使用各种技术进行调整，以提高性能。

(作者代码)，信用卡欺诈检测数据集的处理

# 手动搜索:

不需要专门的库来手动调整超参数，相反，开发人员需要为模型尝试不同的超参数组合，并选择性能最佳的模型。`**max_depth, gamma, reg_lambda, scale_pos_weight**`是一些可以为 XBGClassifer 模型调整的超参数。

人们可以尝试超参数值的所有组合并为每个组合训练模型，并挑选具有最佳性能的模型。遍历超参数的不同值并评估每个组合也可以是另一种用于超参数优化的手动搜索方法。

(作者代码)，手动搜索超参数优化

手动搜索是一个有点繁琐的过程，因此需要不必要的人力。

# 网格搜索:

网格搜索可以被称为手动搜索超参数优化的自动化版本。Scikit-Learn 库附带了一个 GridSearchCV 实现。GridSearch 不是计算友好的，因为它需要大量的时间来优化，但人们可以不必编写多行代码。

可以将字典格式的训练模型和超参数列表提供给 GridSeachCV 函数，并且其返回执行模型及其得分度量。

(作者代码)，网格搜索 CV

# 随机搜索:

网格搜索尝试超参数的所有组合，因此增加了计算的时间复杂度。基于随机超参数组合的随机搜索训练模型。与网格搜索相比，随机搜索训练若干模型的组合总数较少。

Scikit-Learn 包还附带了 RandomSearchCV 实现。

(作者代码)，随机搜索简历

# 减半网格搜索:

减半网格搜索是网格搜索超参数优化的优化版本。等分网格搜索使用连续等分方法在指定的超参数列表中进行搜索。搜索策略开始在数据的一个小样本上评估所有候选，并使用越来越大的样本迭代地选择最佳候选。

与网格搜索方法相比，减半网格搜索的计算成本更低。Scikit-Learn 库实现了 HalvingGridSearch。

> 阅读下面提到的文章中的[，了解将网格搜索 CV 减半如何将超参数优化速度提高 20 倍。](/20x-times-faster-grid-search-cross-validation-19ef01409b7c)

</20x-times-faster-grid-search-cross-validation-19ef01409b7c> [## 网格搜索交叉验证速度提高 20 倍

towardsdatascience.com](/20x-times-faster-grid-search-cross-validation-19ef01409b7c) 

(作者代码)，减半网格搜索 CV

# 减半随机搜索:

减半随机搜索使用相同的连续减半方法，并且与减半网格搜索相比进一步优化。与对半网格搜索不同，它不在所有超参数组合上训练，而是随机选取一组超参数组合。Scikit-Learn 库还提供了 HalvingRandomizedSeachCV。

(作者代码)，减半随机搜索简历

# Hyperopt-Sklearn:

Hyperopt 是一个用于贝叶斯优化的开源 Python 库，专为具有数百个参数的模型的大规模优化而设计。它允许超参数优化跨 CPU 的多个内核进行扩展。

Hyperopt-Sklearn 是 Hyperopt 库的扩展，它允许自动搜索机器学习算法，并为分类和回归任务建立超参数模型。

> 阅读[这篇文章](https://machinelearningmastery.com/hyperopt-for-automated-machine-learning-with-scikit-learn/)以了解更多关于 Hyperopt-Sklearn 包的用法和实现。

# 贝叶斯网格搜索:

贝叶斯网格搜索使用贝叶斯优化技术来模拟搜索空间，以尽快达到优化的参数值。它利用搜索空间的结构来优化搜索时间。贝叶斯搜索方法使用过去的评估结果来采样最有可能给出更好结果的新候选。

[Scikit-Optimize](https://pypi.org/project/scikit-optimize/) 库附带了 BayesSearchCV 实现。

(作者代码)，贝叶斯搜索简历

# 结论:

在本文中，我们讨论了 7 种超参数优化技术，可以用来获得最佳的超参数集，从而训练一个健壮的机器学习模型。最佳模型性能和最佳优化技术之间的权衡是影响某人选择的一个因素。

对于一些组件，可以使用 GridSearchCV 技术。但是当组件数量增加时，可以尝试将网格搜索 CV 或随机搜索 CV 减半，因为它们在计算上并不昂贵。

# 参考资料:

[1] Scikit-Learn 文档:[https://scikit-learn.org/stable/modules/grid_search.html](https://scikit-learn.org/stable/modules/grid_search.html)

> 感谢您的阅读