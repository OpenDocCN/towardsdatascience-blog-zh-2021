# 释放 Scikit-learn 管道的力量

> 原文：<https://towardsdatascience.com/unleash-the-power-of-scikit-learns-pipelines-b5f03f9196de?source=collection_archive---------7----------------------->

## scikit-learn 管道中级指南

![](img/8b90c4bff358528ba913bb4bdb5a6eaf.png)

克里斯托夫·迪翁在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在我的上一篇文章中，我写了一篇介绍 scikit-learn 管道的文章。如果您尚未阅读，可以通过下面的链接访问:

[](/introduction-to-scikit-learns-pipelines-565cc549754a) [## Scikit-learn 管道介绍

### 使用 scikit-learn 构建端到端的机器学习管道

towardsdatascience.com](/introduction-to-scikit-learns-pipelines-565cc549754a) 

在这篇文章中，我想扩展以前的帖子，展示一些很酷的功能，以创建更强大的管道，在帖子的最后，我将向您展示如何微调这些管道，以提高准确性。

正如我们在上一篇文章中看到的，管道可能非常有用，但是我们在介绍中仅仅抓住了表面。

如果我们想做一些考虑到特征类型的特征转换呢？

假设我们有一个包含数字和分类特征的熊猫数据帧，我们希望以不同的方式处理这两种类型的特征。在这种情况下，我们可以使用 scikit-learn pipeline 中的 ColumnTransformer 组件。我们开始吧！

出于教育目的，我们将使用 Kaggle 的成人人口普查收入数据集:

[](https://www.kaggle.com/uciml/adult-census-income) [## 成人人口普查收入

### 根据人口普查数据预测收入是否超过 5 万美元/年

www.kaggle.com](https://www.kaggle.com/uciml/adult-census-income) 

该数据集包含 6 个数字特征和 9 个分类特征，因此它似乎适合我们想要构建的管道。jupyter 笔记本将在我的 github 中提供，您可以通过以下链接下载:

[](https://github.com/unaiLopez/towards-data-science-posts-notebooks/blob/master/pipelines/Unleash%20the%20Power%20of%20Scikit-learn%27s%20Pipelines.ipynb) [## 走向-数据-科学-帖子-笔记本/释放 Scikit 的力量-学习的管道。ipynb at master…

### 通过在 GitHub 上创建一个帐户，为 unaiLopez/forward-data-science-posts-notebooks 的开发做出贡献。

github.com](https://github.com/unaiLopez/towards-data-science-posts-notebooks/blob/master/pipelines/Unleash%20the%20Power%20of%20Scikit-learn%27s%20Pipelines.ipynb) 

在实现管道之前，我们必须对数据集做一点转换，因为所有未知的值都保存为“？”而不是 NaN，收入(目标列)是一个字符串，我们想把它编码成实数。

一旦我们完成了这些转换，我们就可以开始实现我们的管道了。为此，我们将使用 scikit-learn 的 ColumnTransformer 组件，并以不同的方式选择和处理分类和数字特征:

Make_column_selector 将帮助我们按类型选择列。在我们的例子中，这些类型对于数字特性是 int 和 float，对于分类特性是 object。如果我们仔细观察，ColumnTransformer 组件也使用了两个变量，称为 numerical _ pipe 和 categorical _ pipe。这些管道的定义如下:

一旦我们定义了 ColumnTransformer 的组件及其所有元素，我们将使用该组件来创建我们的主管道:

就是这样！我们的管道完工了！让我们用它来训练，并测量它的准确性:

我们使用 33%的数据作为测试集的准确率是 83.2%。那一点也不差！

如何才能提高这条流水线的精度？

为了改善这个管道的结果，我们可以对每个组件执行微调技术。Scikit-learn 使使用 GridSearchCV 进行微调变得容易。该组件将进行广泛的微调，在我们定义的范围内尝试所有可能的超参数组合。这里我们必须小心，因为如果我们尝试太多的组合，与此过程相关的计算复杂性可能会呈指数级增长:

如果我们仔细观察代码，我们已经为管道定义了一些超参数，然后在定义了我们想要优化的指标之后，我们已经开始了拟合所有可能组合的过程。经过这一过程，我们将准确度提高了 0.9%，总准确度达到 84.1%。这听起来不多，但这是一个非常简单的优化，它可以进一步改善定义一个更大的超参数空间，尝试更强大的模型，进行一些贝叶斯或遗传优化，而不是网格搜索优化，等等。

## 结论

当我们一起使用管道和超参数微调时，它们工作得非常好。这篇文章展示了管道不可思议的潜力，以及它们如何在数据科学家的日常工作中非常有用。虽然这个管道比我第一篇文章中的管道更复杂，但是这些管道可以更复杂。例如，我们可以创建自定义转换，并在管道中使用它们。然而，这超出了这篇文章的范围，但是如果你想要一篇关于定制变形金刚的文章，请告诉我。

## 参考

[](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) [## sklearn.model_selection。GridSearchCV

### 对估计量的特定参数值的穷举搜索。重要成员是适合的，预测。GridSearchCV…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) [](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.make_scorer.html) [## sklearn.metrics.make_scorer

### 根据绩效指标或损失函数进行评分。这个工厂函数包装了评分函数，用于和…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.make_scorer.html) [](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html) [## sk learn . compose . column transformer

### 将转换器应用于数组或 pandas 数据框架的列。该估计器允许不同的列或列…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html) [](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html) [## sklearn.pipeline.Pipeline

### 使用 sklearn.pipeline.Pipeline 的示例:特征聚集与单变量选择特征聚集与…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)