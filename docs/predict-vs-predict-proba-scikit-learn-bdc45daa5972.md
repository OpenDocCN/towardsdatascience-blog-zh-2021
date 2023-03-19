# scikit-learn 中的 predict()和 predict_proba()有什么区别？

> 原文：<https://towardsdatascience.com/predict-vs-predict-proba-scikit-learn-bdc45daa5972?source=collection_archive---------0----------------------->

## 如何对数据集使用`predict`和`predict_proba`方法来执行预测

![](img/2ae357736028dbbb9e1d79fde9bc2254.png)

[金伯利农民](https://unsplash.com/@kimberlyfarmer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 介绍

当用`sklearn`训练模型(更精确地说是监督估值器)时，我们有时需要预测实际类别，而在其他一些场合，我们可能希望预测类别概率。

在今天的文章中，我们将讨论如何在数据集上使用`predict`和`predict_proba`方法来执行预测。此外，我们将探索这些方法之间的差异，并讨论何时使用其中一种方法。

首先，让我们创建一个示例模型，我们将在本文中引用它来演示一些概念。在我们的示例中，我们将使用[虹膜数据集](https://scikit-learn.org/stable/auto_examples/datasets/plot_iris_dataset.html)，它也包含在`scikit-learn`的`sklearn.datasets`模块中。这将是一项分类任务，我们需要根据花瓣和萼片的尺寸(长度和宽度)识别并正确预测三种不同类型的鸢尾，即刚毛鸢尾、杂色鸢尾和海滨鸢尾。

```
import numpy as np
from sklearn import datasets
from sklearn.neighbors import KNeighborsClassifier # Load the Iris dataset
iris_X, iris_y = datasets.load_iris(return_X_y=True)# Split Iris dataset into train/test sets randomly
np.random.seed(0)
indices = np.random.permutation(len(iris_X))
iris_X_train = iris_X[indices[:-10]]
iris_y_train = iris_y[indices[:-10]]
iris_X_test = iris_X[indices[-10:]]
iris_y_test = iris_y[indices[-10:]]# Instantiate and fit a KNeighbors classifier
knn = KNeighborsClassifier()
knn.fit(iris_X_train, iris_y_train)
```

## predict()方法

`scikit-learn`中的所有监督估计器都实现了`predict()`方法，该方法可以在经过训练的模型上执行，以便**预测一组新数据的实际标签**(或类别)。

该方法接受与将对其进行预测的数据相对应的单个参数，并返回包含每个数据点的预测标签的数组。

```
**predictions = knn.predict(iris_X_test)**print(predictions)
***array([1, 2, 1, 0, 0, 0, 2, 1, 2, 0])***
```

## proba()方法

在分类任务的上下文中，一些`sklearn`评估器也实现了`predict_proba`方法，返回每个数据点的分类概率。

该方法接受与计算概率的数据相对应的单个参数，并返回包含输入数据点的类概率的列表数组。

```
**predictions = knn.predict_proba(iris_X_test)**print(predictions)
***array([[0\. , 1\. , 0\. ],
       [0\. , 0.4, 0.6],
       [0\. , 1\. , 0\. ],
       [1\. , 0\. , 0\. ],
       [1\. , 0\. , 0\. ],
       [1\. , 0\. , 0\. ],
       [0\. , 0\. , 1\. ],
       [0\. , 1\. , 0\. ],
       [0\. , 0\. , 1\. ],
       [1\. , 0\. , 0\. ]])***
```

## 最后的想法

在今天的文章中，我们讨论了如何使用预训练的`scikit-learn`模型对数据进行预测。此外，我们探讨了由`scikit-learn`的估计器实现的方法`predict`和`predict_proba`之间的主要差异。

`predict`方法用于预测实际类别，而`predict_proba`方法可用于推断类别概率(即特定数据点落入基础类别的概率)。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

</scikit-learn-vs-sklearn-6944b9dc1736>  </fit-vs-predict-vs-fit-predict-in-python-scikit-learn-f15a34a8d39f>  <https://medium.com/geekculture/fit-vs-transform-vs-fit-transform-in-python-scikit-learn-2623d5a691e3> 