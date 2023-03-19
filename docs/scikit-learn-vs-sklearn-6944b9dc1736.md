# Scikit-Learn 和 Sklearn 有区别吗？

> 原文：<https://towardsdatascience.com/scikit-learn-vs-sklearn-6944b9dc1736?source=collection_archive---------6----------------------->

## Python 中的 scikit-learn vs sklearn

![](img/80e779420265ece96bc325ab18ea8481.png)

照片由 [Anastasia Zhenina](https://unsplash.com/@disguise_truth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/learn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

**scikit-learn** 绝对是机器学习和 Python 最常用的包之一。然而，许多新手对包本身的命名感到困惑，因为它看起来有两个不同的名字；`scikit-learn` 和`sklearn`。

在今天的短文中，我们将首先讨论这两个包之间是否有任何区别。此外，我们将讨论在源代码中安装和导入哪一个是否重要。

## 什么是 scikit-learn

该项目最初开始于 2007 年，是谷歌代码之夏的一部分，而第一次公开发布是在 2010 年初。

[**scikit-learn**](https://scikit-learn.org/) 是一个开源的机器学习 Python 包，提供支持监督和非监督学习的功能。此外，它还提供了模型开发、选择和评估工具，以及许多其他实用工具，包括数据预处理功能。

更具体地说，scikit-learn 的主要功能包括分类、回归、聚类、降维、模型选择和预处理。这个库使用起来非常简单，最重要的是非常高效，因为它是基于 **NumPy** 、 **SciPy** 和 **matplotlib 构建的。**

## scikit-learn 和 sklearn 有区别吗？

简短的回答是**没有**。`scikit-learn`和`sklearn`都是指同一个包，但是，有一些事情你需要知道。

首先，您可以使用`scikit-learn`或`sklearn`标识符安装软件包，但是**建议使用** `**skikit-learn**` **标识符安装** `**scikit-learn**` **到** `**pip**` **。**

如果您使用`sklearn`标识符安装软件包，然后运行`pip list`，您会注意到令人讨厌的`sklearn 0.0`条目:

```
$ pip install sklearn
$ pip list
Package       Version
------------- -------
joblib        1.0.1
numpy         1.21.2
pip           19.2.3
**scikit-learn  0.24.2**
scipy         1.7.1
setuptools    41.2.0
**sklearn       0.0**
threadpoolctl 2.2.0
```

此外，如果您现在尝试卸载`sklearn`，该软件包将不会被卸载:

```
$ pip uninstall sklearn
$ pip list
Package       Version
------------- -------
joblib        1.0.1
numpy         1.21.2
pip           19.2.3
**scikit-learn  0.24.2**
scipy         1.7.1
setuptools    41.2.0
threadpoolctl 2.2.0
```

本质上，`sklearn`是 PyPi 上的一个[虚拟项目，它将依次安装`scikit-learn`。因此，如果你卸载`sklearn`，你只是卸载虚拟包，而不是真正的包本身。](https://pypi.org/project/sklearn/)

现在**不管你如何安装 scikit-learn，你必须使用** `**sklearn**` **标识符**在你的代码中导入它:

```
import sklearn
```

如果您试图使用`scikit-learn`标识符导入包，您将会以`SyntaxError`结束:

```
>>> import sklearn
>>> import scikit-learn
File "<stdin>", line 1
import scikit-learn
             ^
SyntaxError: invalid syntax
```

即使您试图用`__import__()`导入它以便处理包名中的连字符，您仍然会得到一个`ModuleNotFoundError`:

```
>>> __import__('scikit-learn')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'scikit-learn'
```

## 最后的想法

在今天的短文中，我们试图阐明关于`scikit-learn`和`sklearn`的一些问题，因为很多初学者似乎对在 Python 中开发 ML 功能时使用哪个术语感到困惑。

一般来说，建议您使用`scikit-learn`标识符(即`pip install scikit-learn`)来安装库，但是在您的源代码中，您必须使用`sklearn`标识符(即`import sklearn`)来导入它。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

[](/what-is-machine-learning-e67043a3a30c) [## 什么是机器学习

### 讨论机器学习的学习方法、类型和实际应用

towardsdatascience.com](/what-is-machine-learning-e67043a3a30c) [](/how-to-split-a-dataset-into-training-and-testing-sets-b146b1649830) [## 如何用 Python 将数据集分割成训练集和测试集

### 探索从建模数据集创建训练和测试样本的三种方法

towardsdatascience.com](/how-to-split-a-dataset-into-training-and-testing-sets-b146b1649830) [](/fit-vs-predict-vs-fit-predict-in-python-scikit-learn-f15a34a8d39f) [## Python scikit 中的 fit()vs predict()vs fit _ predict()-learn

### sklearn 中的 fit，predict 和 fit_predict 方法有什么区别

towardsdatascience.com](/fit-vs-predict-vs-fit-predict-in-python-scikit-learn-f15a34a8d39f)