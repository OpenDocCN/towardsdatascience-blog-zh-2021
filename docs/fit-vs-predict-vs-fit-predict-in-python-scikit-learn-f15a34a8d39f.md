# Python scikit 中的 fit()vs predict()vs fit _ predict()-learn

> 原文：<https://towardsdatascience.com/fit-vs-predict-vs-fit-predict-in-python-scikit-learn-f15a34a8d39f?source=collection_archive---------1----------------------->

## sklearn 中的 fit，predict 和 fit_predict 方法有什么区别

![](img/429c47837c3ff0a6d7a50843781067cf.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/scikit-learn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

**scikit-learn** (或俗称 *sklearn* )大概是 Python 中最强大、应用最广泛的机器学习库之一。它附带了一套全面的工具和现成的模型，从预处理实用程序到模型训练和模型评估实用程序。

许多 sklearn 对象，实现了三个具体的方法，即`fit()`、`predict()`和`fit_predict()`。本质上，它们是 scikit-learn 及其 API 中应用的约定。在这篇文章中，我们将探讨每一个是如何工作的，以及何时使用其中的一个。

请注意，在本文中，我们将使用特定的示例来探索上述函数，但是这里解释的概念适用于实现这些方法的大多数(如果不是全部)对象。

在解释`fit()`、`predict()`和`fit_predict()`背后的直觉之前，重要的是首先理解什么是 scikit-learn API 中的估计器。我们需要了解估计量的原因很简单，因为这样的对象实现了我们感兴趣的方法。

## **sci kit-learn 中的估计器是什么**

在 scikit-learn 中，估计器是一个对象，它根据输入数据(即训练数据)拟合模型，并执行与新的、看不见的数据的属性相对应的特定计算。换句话说，估计器可以是回归器或分类器。

这个库带有基类`[sklearn.base.BaseEstimator](https://scikit-learn.org/stable/modules/generated/sklearn.base.BaseEstimator.html#)`，所有的估算器都应该继承这个类。基类有两个方法，即`get_params()`和`set_params()`，分别用于获取和设置估计器的参数。注意估计器必须**明确**在构造器方法(即`__init__`方法)中提供它们的所有参数。

## fit()有什么作用

`fit()`由每个估计器实现，它接受样本数据的输入(`X`)，对于监督模型，它还接受标签的参数(即目标数据`y`)。或者，它还可以接受额外的样品属性，如重量等。

fit 方法通常负责许多操作。通常，他们应该首先清除已经存储在估计器上的任何属性，然后执行参数和数据验证。它们还负责从输入数据中估计属性，存储模型属性，并最终返回拟合的估计值。

现在作为一个例子，让我们考虑一个分类问题，我们需要训练一个 SVC 模型来识别手写图像。在下面的代码中，我们首先加载数据，然后将其分成训练集和测试集。然后我们实例化一个 SVC 分类器，最后调用`fit()`使用输入的训练和数据来训练模型。

> `[**fit**](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html#sklearn.svm.SVC.fit)` ( *X* ， *y* ， *sample_weight=None* ):根据给定的训练数据拟合 SVM 模型。
> 
> **X —** 训练向量，其中 n_samples 是样本数，n_features 是特征数。
> 
> **y —** 目标值(分类中的类别标签，回归中的实数)。
> 
> **样品重量—** 每个样品的重量。重新调整每个样本的 C。较高的权重迫使分类器更加强调这些点。

现在我们已经成功地训练了我们的模型，我们现在可以访问拟合的参数，如下所示:

请注意，每个估计量可能有不同的参数，一旦模型拟合，您就可以访问这些参数。您可以在官方文档和正在使用的特定评估器的“属性”部分找到可以访问的参数。通常，拟合参数使用下划线`_`作为后缀。特别是对于 SVC 分类器，您可以在文档的[章节](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)中找到可用的拟合参数。

## predict()做什么

既然我们已经训练了我们的模型，下一步通常涉及对测试集的预测。要做到这一点，我们需要调用方法`predict()`，该方法基本上使用`fit()`学习到的参数，以便对新的、看不见的测试数据点进行预测。

本质上，`predict()`将为每个测试实例执行一个预测，它通常只接受一个输入(`X`)。对于分类器和回归器，预测值将与在训练集中看到的值处于相同的空间。在聚类估计中，预测值将是一个整数。所提供的测试实例的预测值将以数组或稀疏矩阵的输出形式返回。

注意，如果您试图在没有首先执行`fit()`的情况下运行`predict()`，您将收到一个`[**exceptions.NotFittedError**](https://scikit-learn.org/stable/modules/generated/sklearn.exceptions.NotFittedError.html#sklearn.exceptions.NotFittedError)`，如下所示。

## fit_predict()是做什么的

展望未来，`fit_predict()`与无监督或直推式估值器更相关。本质上，这种方法将适合并执行对训练数据的预测，因此，在执行诸如聚类分析之类的操作时更合适。

> `[**fit_transform**](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html?highlight=k%20means#sklearn.cluster.KMeans.fit_transform)` ( *X* ，*y =无*，*sample _ weight =无* )
> 计算聚类，将 X 变换到聚类距离空间。相当于`fit(X).transform(X)`，但实现更高效。

注意到

*   scikit-learn 中的聚类估计器必须实现`fit_predict()`方法，但不是所有的估计器都这样做
*   传递给`fit_predict()`的参数与传递给`fit()`的参数相同

## 结论

在本文中，我们讨论了 sklearn 中最常实现的三个函数的用途，即`fit()`、`predict()`和`fit_predict()`。我们探讨了它们各自的功能和区别，以及在什么用例中应该使用其中的一个。

正如本文介绍中提到的，尽管我们使用了具体的例子来演示它们的行为，但本文中解释的概念几乎适用于 scikit-learn 中实现这些方法的所有估算器。

`fit()`方法将使模型适合输入训练实例，而`predict()`将根据在`fit`期间学习到的参数对测试实例进行预测。另一方面，`fit_predict()`与无监督学习更相关，在无监督学习中我们没有标记的输入。

一个非常相似的主题可能是比较由 scikit-learn 用于转换特性的**转换器**实现的`fit()`、`transform()`和`fit_transform()`方法。如果你想了解更多，你可以阅读我下面的文章。

<https://gmyrianthous.medium.com/fit-vs-transform-vs-fit-transform-in-python-scikit-learn-2623d5a691e3> 