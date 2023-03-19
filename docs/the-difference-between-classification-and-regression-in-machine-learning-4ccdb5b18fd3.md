# 机器学习中分类和回归的区别

> 原文：<https://towardsdatascience.com/the-difference-between-classification-and-regression-in-machine-learning-4ccdb5b18fd3?source=collection_archive---------10----------------------->

## 函数逼近的 ***问题***

![](img/a9561d9ee7e83f93b7f67bdff4a96316.png)

[绿色变色龙](https://unsplash.com/@craftedbygc?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

在机器学习生涯的开始，区分回归和分类算法可能是一项挑战。当你已经在这个领域工作了 4 年，很容易忘记学习机器学习的挑战。有太多的术语，这使得你刚开始时很难找到自己的方向。

机器学习的一个重要部分是区分任务是回归问题还是分类问题。这种区别为从业者提供了更清晰的见解，即在处理问题时，什么样的机器学习算法可能最合适，因为一些模型对分类比对回归更有用——反之亦然。

这两个任务的相似之处在于，它们都是 ***监督学习*** *的一种形式。*

## 监督学习

回归和分类问题都属于监督学习范畴。每项任务都涉及开发一个从历史数据中学习的模型，使其能够对我们没有答案的新情况做出预测。更正式地说，所有监督学习任务都包括学习一个函数，该函数基于示例输入-输出对将输入映射到输出。

监督学习问题的目标是逼近从输入特征(X)到输出(y)的映射函数(f)——在数学上，这被称为函数逼近 的 ***问题。***

不管我们是试图解决分类问题还是回归问题，每当我们为监督学习问题开发学习算法时，算法的工作就是在给定可用资源的情况下找到最佳映射函数。

## 区别——分类与回归

尽管总体目标相似(基于输入-输出映射将输入映射到输出)，分类和回归问题是不同的。

在分类问题中，我们的学习算法学习将输入映射到输出的函数，其中输出值是离散的类别标签(即，猫或狗、恶性或良性、垃圾邮件或非垃圾邮件等)。流行的分类算法包括:

*   [逻辑回归](/algorithms-from-scratch-logistic-regression-7bacdfd9738e?source=collection_tagged---------5----------------------------)
*   [朴素贝叶斯](/algorithms-from-scratch-naive-bayes-classifier-8006cc691493?source=collection_tagged---------1----------------------------)
*   [K-最近邻](/algorithms-from-scratch-k-nearest-neighbors-fe19b431a57?source=collection_tagged---------2----------------------------)
*   [决策树](/algorithms-from-scratch-decision-tree-1898d37b02e0?source=collection_tagged---------4----------------------------)
*   [支持向量机](/algorithms-from-scratch-support-vector-machine-6f5eb72fce10?source=collection_tagged---------3----------------------------)

相反，回归问题关注的是输入到输出的映射，其中输出是一个连续的实数(例如，房子的价格)。流行的回归算法包括:

*   [线性回归](/algorithms-from-scratch-linear-regression-c654353d1e7c?source=collection_tagged---------6----------------------------)
*   里脊回归
*   套索回归
*   神经网络回归
*   [决策树](https://neptune.ai/blog/fighting-overfitting-with-l1-or-l2-regularization)

> **注**:在[用 L1 或 L2 正则化对抗过度拟合](https://neptune.ai/blog/fighting-overfitting-with-l1-or-l2-regularization)中了解更多关于脊和套索回归的信息

一些算法专门用于回归类型的问题，例如线性回归模型，而一些算法专门用于分类任务，例如逻辑回归。然而，有一些算法，如决策树、随机森林、XGBoost 和神经网络，一旦对它们进行小的修改，它们就可以重叠。

## 最后的想法

本质上，我们确定一个任务是分类问题还是回归问题的方法是通过输出。回归任务与预测连续值有关，而分类任务与预测离散值有关。此外，我们评估每种类型问题的方式是不同的，例如，均方差对于回归任务是一个有用的度量，但这对于分类来说是行不通的。类似地，精确度对于回归任务来说不是一个有效的度量标准，但是对于分类任务来说是有用的。

感谢阅读！

如果你喜欢这篇文章，请通过订阅我的免费**[每周简讯](https://mailchi.mp/ef1f7700a873/sign-up)与我联系。不要错过我写的关于人工智能、数据科学和自由职业的帖子。**

## **相关文章**

**</the-difference-between-data-scientists-and-ml-engineers-f6e797948003>  </how-to-become-a-machine-learning-engineer-e420e134c0a3>  </the-day-to-day-of-a-machine-learning-engineer-378b80bf1f6f> **