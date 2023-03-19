# 如何创建自定义 scikit-学习分类和回归模型

> 原文：<https://towardsdatascience.com/how-to-create-custom-scikit-learn-classification-and-regression-models-70db7e76addd?source=collection_archive---------21----------------------->

![](img/2ea4c419afe9ab1a05d93e382ea87fa1.png)

让-菲利普·德尔伯格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

[Scikit learn](https://scikit-learn.org/stable/) 是 Python 中标准机器学习模型的首选包。它不仅提供了您在实践中想要使用的大多数核心算法(即 GBMs、随机森林、逻辑/线性回归)，而且还提供了用于特征预处理的各种转换(例如 Onehot 编码、标签编码)以及用于跟踪模型性能的度量和其他便利功能。但是，仍然会有你需要做一些稍微不同的事情的时候。在这些情况下，我经常**仍然希望在 scikit learn 的通用 API 内工作**。坚持使用 scikit learn API 有两个主要好处:

1.  您可以与其他数据科学家共享您的模型，他们可能知道如何使用它，而无需您提供重要的文档。
2.  您可以在包含其他 scikit-learn 操作的管道中使用它——至少对我来说，这是主要的好处。

幸运的是，实现定制模型非常简单，因此它可以像标准的 scikit-learn 模型一样工作。

## 实现自定义模型🚀

在 scikit-learn API 中实现定制模型可以使用下面的框架类来完成——一个用于分类，一个用于回归。基本上，您想做的任何事情都可以添加到`init`、`fit`、`predict`和`predict_proba`(用于分类器)方法中。通过继承`BaseEstimator`、`ClassifierMixin/RegressorMixin`，该类将与其他 scikit-learn 类具有互操作性，并且可以在 scikit-learn 管道中使用。

在这篇[帖子](/lesser-known-data-science-techniques-you-should-add-to-your-toolkit-a96578113350)中，你可以看到我是如何在实践中使用这个结构来定义定制的 scikit-learn 模型的。例如，下面的模型创建了一个树空间逻辑回归分类器:

在这种情况下，我将几个 scikit-learn 模型包装在一起。您可以看到,`fit`方法将在输入上拟合一个`GradientBoostedClassifier`,使用这个拟合的模型来转换输入，一个热编码这个转换，然后在输出上拟合一个`LogisticRegression`。将它包装在一个单独的定制模型中比单独定义这些步骤要整洁得多。

感谢您的阅读——希望这对您有所帮助。