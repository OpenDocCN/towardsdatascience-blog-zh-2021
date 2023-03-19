# 初学者的超参数优化

> 原文：<https://towardsdatascience.com/hyperparameter-optimization-for-beginners-32e3ab07b09c?source=collection_archive---------19----------------------->

## 科学家讨厌的任务数据

![](img/abff86449156100f9c00ad2425509a64.png)

照片由[德鲁·帕特里克·米勒](https://unsplash.com/@drewpatrickmiller?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

许多数据科学家忽略了超参数。超参数调整是一项高度实验性的活动，这种不确定性会导致任何正常人的严重不适，这是我们自然试图避免的。

> “超参数调整更多地依赖于实验结果，而不是理论，因此确定最佳设置的最佳方法是尝试许多不同的组合[…]。”— Will Koehrsen，超参数调整 Python 中的随机森林

不幸的是，应该这样做。我们不会走进商店，从货架上挑选一双运动鞋，然后买下来。我们首先选择一双我们认为能解决我们问题的鞋，不管是衣柜故障还是其他什么原因，我们已经失去了所有的运动鞋。接下来，我们在购买之前调整超参数，如鞋子的尺寸和我们想要的颜色。

如果我们愿意在现实世界中这样做，那么在数据科学中就不应该跳过它。

## 理解超参数

超参数优化是为学习算法选择最优超参数集的问题。通过确定超参数的正确组合，模型的性能得到了最大化——这意味着当提供看不见的实例时，我们的学习算法会做出更好的决策。

被选为超参数的值控制学习过程，因此，它们不同于正常参数，因为它们是在训练学习算法之前被选择的。

在形式上，模型超参数是在提供数据时不能由模型估计的参数，因此需要预先设置它们来估计模型的参数。相反，模型参数由学习模型根据提供的数据来估计。

## 方法

有许多方法可以有效地执行超参数优化——参见维基百科上的[超参数优化](https://en.wikipedia.org/wiki/Hyperparameter_optimization)获得完整的分类。Mind Foundry 在 Twitter 上进行了一项调查，以了解平台上从业者的情绪。

Mind Foundry 在推特[上进行的调查](https://twitter.com/MindFoundry/status/1042446256554033152?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1042446256554033152%7Ctwgr%5E%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fcdn.embedly.com%2Fwidgets%2Fmedia.html%3Ftype%3Dtext2Fhtmlkey%3Da19fcc184b9711e1b4764040d3dc5c07schema%3Dtwitterurl%3Dhttps3A%2F%2Ftwitter.com%2Fmindfoundry%2Fstatus%2F1042446256554033152image%3Dhttps3A%2F%2Fi.embed.ly%2F1%2Fimage3Furl3Dhttps253A252F252Fpbs.twimg.com252Fprofile_images252F720909614011838464252FoYcW2zb0_400x400.jpg26key3Da19fcc184b9711e1b4764040d3dc5c07)

让我们进一步了解它们，以及如何在 Python 中执行它们。

## 贝叶斯优化

维基百科将贝叶斯优化描述为“*，一种针对嘈杂的黑盒函数的全局优化方法。应用于超参数优化，贝叶斯优化建立了从超参数值到在验证集上评估的目标的函数映射的概率模型。通过基于当前模型迭代地评估有希望的超参数配置，然后更新它，贝叶斯优化旨在收集尽可能多地揭示关于该函数的信息的观察值，特别是最优值的位置。它试图平衡勘探(结果最不确定的超参数)和开发(预期接近最优的超参数)。在实践中，与网格搜索和随机搜索相比，贝叶斯优化已被证明可以在更少的评估中获得更好的结果，因为它能够在实验运行之前对实验的质量进行推理。***来源** : [维基](https://en.wikipedia.org/wiki/Bayesian_optimization)。

Scikit-Optimize (skopt)是一个优化库，它有一个[贝叶斯优化实现](https://scikit-optimize.github.io/stable/auto_examples/sklearn-gridsearchcv-replacement.html)。我建议使用这种实现，而不是尝试实现自己的解决方案——尽管如果您想更深入地了解贝叶斯优化是如何工作的，实现自己的解决方案也是有价值的。有关示例，请参见下面的代码。

使用 Python 的贝叶斯优化示例

## 网格搜索

网格搜索是我学习的第一个执行超参数优化的技术。它包括在学习算法中彻底搜索超参数空间的特定值的手动子集。执行网格搜索意味着必须有一个性能指标来指导我们的算法。

强烈建议您使用 [Sklearn 实现](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)，而不是从头开始实现网格搜索。有关示例，请参见下面的代码。

使用 Python 进行网格搜索的示例

## 随机搜索

随机搜索随机选择组合，而不是在网格搜索中穷举所有列出的组合。当少量的超参数对最终的模型性能有影响时，随机搜索可以胜过网格搜索——尽管它在上面的调查中评级很低，但随机搜索仍然是您的工具包中非常重要的技术。

像网格搜索一样，随机搜索也有一个 [Scikit Learn 实现](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)，比你自己的解决方案更好用。参见下面的代码。

使用 Python 的随机搜索示例

## 最后的想法

根据我的经验，在执行超参数优化时，很好地理解您正在使用的学习算法以及超参数如何影响它的行为会有所帮助。虽然这是最重要的任务之一，但我觉得超参数调优没有得到应有的重视，或者可能是我看得不够多。然而，这是你的项目中极其重要的一部分，不应该被忽视。

感谢阅读！

如果你喜欢这篇文章，请通过订阅我的**免费** [每周简讯](https://mailchi.mp/ef1f7700a873/sign-up)与我联系。不要错过我写的关于人工智能、数据科学和自由职业的帖子。

## 相关文章

[](/cross-validation-c4fae714f1c5) [## 交叉验证

### 验证机器学习模型的性能

towardsdatascience.com](/cross-validation-c4fae714f1c5) [](/oversampling-and-undersampling-5e2bbaf56dcf) [## 过采样和欠采样

### 一种不平衡分类技术

towardsdatascience.com](/oversampling-and-undersampling-5e2bbaf56dcf) [](/gradient-descent-811efcc9f1d5) [## 梯度下降

### 机器学习中梯度下降的不同变体的利弊

towardsdatascience.com](/gradient-descent-811efcc9f1d5)