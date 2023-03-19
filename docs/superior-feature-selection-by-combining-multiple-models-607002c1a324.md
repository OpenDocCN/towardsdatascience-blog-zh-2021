# 通过组合多种模型实现卓越的功能选择

> 原文：<https://towardsdatascience.com/superior-feature-selection-by-combining-multiple-models-607002c1a324?source=collection_archive---------10----------------------->

## 你最喜欢的模特自己选择特色

![](img/4f1f97a1c48e13591b8ecb867b41f9f5.png)

**图片由** [**安德里亚**](https://www.pexels.com/@olly?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) **上** [**像素**](https://www.pexels.com/photo/crop-multiracial-people-joining-hands-together-during-break-in-modern-workplace-3931562/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

## 介绍

机器学习中有很多特征选择方法。每一种方法都可能给出不同的结果，这取决于你如何使用它们，所以很难完全信任一种方法。让多种方法投票决定我们是否应该保留一个特性，这不是很酷吗？这就像随机森林算法一样，它将多个弱学习者的预测结合起来，形成一个强学习者。事实证明，Sklearn 已经给了我们工具来自己制作这样的特征选择器。

通过使用这些工具，我们将一起构建一个特性选择器，它可以接受任意数量的 Sklearn 模型。所有这些模型将投票决定我们应该保留哪些功能，我们通过收集所有模型(民主)的投票来做出决定。

<https://ibexorigin.medium.com/membership>  

获得由强大的 AI-Alpha 信号选择和总结的最佳和最新的 ML 和 AI 论文:

<https://alphasignal.ai/?referrer=Bex>  

## 构建选择器的必备知识:权重和系数

在我们继续构建选择器之前，让我们回顾一些必要的主题。首先，几乎所有产生预测的 Sklearn 估计器在被拟合到训练数据之后都具有`.coef_`和`.feature_importances_`属性。

`.coef_`属性主要出现在`sklearn.linear_model`子模块下给出的模型中:

顾名思义，以上是通过拟合线性回归的最佳拟合直线计算出的*系数*。其他模型遵循类似的模式，并产生其内部方程的系数:

`sklearn.tree`和`sklearn.ensemble`中的模型工作方式不同，它们计算`.feature_importances_`属性下每个特征的*重要性*或*权重*:

与线性模型的系数不同，权重之和为 1:

```
np.sum(dt.feature_importances_)1.0
```

无论模型如何，随着特征权重或系数的降低，特征对整体预测的贡献越来越小。这意味着我们可以丢弃系数或权重接近 0 的特征。

## RFECV 概述

递归特征消除(RFE)是一种流行的特征选择算法。它会自动找到要保留的最佳特征数量，以实现给定模型的最佳性能。下面是一个简单的例子:

上述代码用于查找最少数量的特征，以实现[套索回归](/intro-to-regularization-with-ridge-and-lasso-regression-with-sklearn-edcf4c117b7a?source=your_stories_page-------------------------------------)模型的最佳性能。

在拟合到训练数据之后，RFECV 具有`.support_`属性，该属性给出布尔掩码，具有应该保留的特征的真值:

然后，我们可以使用该掩码对原始数据进行子集划分:

```
X.loc[:, rfecv.support_]
```

定制特性选择器的核心将是这个`RFECV`类。我没有详细介绍它是如何工作的，但我以前的文章重点介绍了它。我建议在继续之前阅读:

</powerful-feature-selection-with-recursive-feature-elimination-rfe-of-sklearn-23efb2cdb54e>  

## 第一部分:选择模型

我们将使用 [Ansur Male](https://www.kaggle.com/seshadrikolluri/ansur-ii) 数据集，主要是因为它包含了 6000 名美国陆军人员的许多身体测量特征(98 个数字):

![](img/ca73576cecbbdae72b2145079ae26f58.png)

我们将尝试预测以磅为单位的体重，为了做到这一点，我们需要降低模型的复杂性——也就是说，使用尽可能少的特征创建一个具有尽可能多的预测能力的模型。目前有 98 个，我们将努力减少这个数字。此外，我们将删除以千克为单位记录重量的列。

```
ansur.drop("weightkg", axis=1, inplace=True)
```

我们的第一个模型将是 Lasso Regressor，我们将把它插入`RFECV`:

我们将从套索生成的布尔遮罩存储在`lasso_mask`中，稍后你会看到原因。

接下来，我们将对另外两个模型进行同样的操作:线性回归和 GradientBoostingRegressor:

## 第二部分:合并投票

现在，我们将投票作为布尔掩码放在三个数组中:`lasso_mask`、`gb_mask`和`lr_mask`。由于真/假值代表 1 和 0，我们可以添加三个数组:

结果将是一个数组，其中记录了所有模型选择每个特征的次数。现在，我们可以设置一个投票阈值来决定是否保留该特性。这个门槛取决于我们想保守到什么程度。我们可以设置一个严格的阈值，我们希望所有 3 个成员都选择该特性，或者我们可以选择 1 作为安全阈值:

现在，`final_mask`是一个具有真值的布尔数组，如果所有 3 个估计器都选择了一个特性。我们可以用它来划分原始数据的子集:

```
>>> X.loc[:, final_mask].shape(4082, 39)
```

如您所见，最终的掩码选择了 39 列，而没有选择 98 列。您可以使用数据集的这个子集来创建一个不太复杂的模型。例如，我们将选择线性回归模型，因为我们可以预期身体测量值是线性相关的:

## 步骤总结

即使需要一些工作才能得到最终结果，这也是值得的，因为多个模型的组合能力可以超过任何其他特征选择方法。

我们在示例中只选择了 3 个模型，但是您可以包含任意多的模型，以使结果更加可靠和可信。

此时，合乎逻辑的一步是将所有这些代码包装在一个函数中，甚至是一个定制的 Sklearn 转换器，但是定制转换器是另一篇文章的主题。为了加强上述观点并给你一个大纲，让我们回顾一下我们已经采取的步骤:

1.  选择任意数量的具有`.coef_`或`.feature_importnances_`属性的 Sklearn 估计器。估计量越多，结果就越稳健。然而，多个模型是有代价的——因为`RFECV`在幕后使用[交叉验证](/how-to-master-the-subtle-art-of-train-test-set-generation-7a8408bcd578),对于集合模型和大型数据集，训练时间在计算上将是昂贵的。此外，确保根据问题的类型选择估计器——记住只传递分类或回归估计器，以便`RFECV`工作。
2.  将所有选择的模型插入`RFECV`类，并确保保存每一轮的布尔掩码(通过`.support_`访问)。为了加快速度，您可以调整`step`参数，以便在每轮淘汰中丢弃任意数量的特征。
3.  对所有估计量的掩码求和。
4.  设置投票计数的阈值。这个门槛取决于你想保守到什么程度。使用此阈值将投票数组转换为布尔掩码。
5.  使用最终掩膜对原始数据进行子集化，以进行最终模型评估。

## 与特征选择相关的进一步阅读

*   [如何使用方差阈值进行鲁棒特征选择](/how-to-use-variance-thresholding-for-robust-feature-selection-a4503f2b5c3f?source=your_stories_page-------------------------------------)
*   [如何使用成对相关性进行稳健的特征选择](/how-to-use-pairwise-correlation-for-robust-feature-selection-20a60ef7d10?source=your_stories_page-------------------------------------)
*   [强大的功能选择和递归功能消除](/powerful-feature-selection-with-recursive-feature-elimination-rfe-of-sklearn-23efb2cdb54e)
*   [RFECV Sklearn 文档](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFECV.html)
*   [Sklearn 官方功能选择用户指南](https://scikit-learn.org/stable/modules/feature_selection.html#feature-selection)

## 您可能也会感兴趣…

*   [面向数据科学家的面向对象编程简介](https://towardsdev.com/intro-to-object-oriented-programming-for-data-scientists-9308e6b726a2?source=your_stories_page-------------------------------------)
*   [我的 6 部分强大 EDA 模板，讲述终极技能](/my-6-part-powerful-eda-template-that-speaks-of-ultimate-skill-6bdde3c91431?source=your_stories_page-------------------------------------)
*   [如何将 Sklearn 管道用于极其简洁的代码](/how-to-use-sklearn-pipelines-for-ridiculously-neat-code-a61ab66ca90d?source=your_stories_page-------------------------------------)