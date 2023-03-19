# 使用 Python 中的装饰模式包装 PySpark 数据帧

> 原文：<https://towardsdatascience.com/wrapping-pyspark-dataframe-using-the-decorator-pattern-in-python-7f55c91b2961?source=collection_archive---------17----------------------->

![](img/d662dec1b8d0b5761c4d64ec67568343.png)

图多尔·巴休在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## 如何包装 PySpark 数据帧？

在我的一个项目中，我需要增强现有的 DataFrame 功能。一种方法是实现实用程序方法，这些方法可以获取数据帧并根据需要实现附加功能。另一种方法是实现 decorator 模式，其中 Decorator 类将接受数据帧并实现其他方法。

## 让我拿 1

首先，让我们创建一个简单的 DataFrameDecorator 类，通过用常量参数 1 实现 take 方法来增强 DataFrame 的功能。让我们称这个方法为 take1。因此，如果修饰的数据帧不为空，该方法将返回一条记录。

上面的实现正是我们要做的。它实现了 *take1* 方法，该方法通过在修饰的 df 上调用 *take(1)* 来显式声明 take1 行。

上面的代码测试了我们刚刚实现的内容，两个打印命令返回相同的值。这是包装数据帧并获得一条记录的简单部分。如果我们想通过 DataFrameDecorator 访问 DataFrame 上所有现有的方法会怎么样？为了解决这个问题，我们将使用 __getattr__，但是在我们跳到这个问题之前，让我们激励一下我们为什么要使用它。

## 方法

实例对象理解两种属性名:数据和方法属性。方法只是实例对象的一个属性。对于本文来说，这可能没什么价值，但重要的是要提到，类有函数对象，而类实例有绑定到函数对象的方法对象。

> 如果你仍然不明白方法是如何工作的，看看实现也许可以澄清问题。当引用实例的非数据属性时，会搜索实例的类。如果名字表示一个有效的类属性，该类属性是一个函数对象，则通过将实例对象和**函数对象**打包(指向)**来创建一个方法对象，这是一个抽象对象**方法对象**。当用参数列表调用方法对象时，从实例对象和参数列表构造新的参数列表，并且用这个新的参数列表调用函数对象。**
> 
> 来源:[https://docs . python . org/3/tutorial/classes . html # class-objects](https://docs.python.org/3/tutorial/classes.html#method-objects)

要查看此操作，让我们看看下面的代码:

类上的 take1 是函数对象，而实例上的 take 1 是类函数对象的绑定方法。

## __getattr__

回到我们的问题，即 *DataFrameDecorator* 类不能处理所有的 *DataFrame* 函数。 *__getattr__* 是找不到属性时调用的方法。我们能做的就是在 *DataFrameDecorator* 类上实现 *__getattr__* 来处理 *DataFrame* 的所有功能。让我们首先看看下面的代码，以了解当我们在 *DataFrameDecorator* 上调用 *take* 时会发生什么，此时 *__getattr__* 被实现来为在 *DataFrameDecorator 上未找到的任何属性返回默认字符串“function not found”。*

理想情况下， *__getattr__* 返回属性，所以在这种情况下，我们返回一个函数 lambda，它在执行时只打印出没有找到原始函数。此外，显式打印 df_decorated.take 可以清楚地表明，它不是一个显式函数，而是 lambda 函数，是类 *DataFrameDecorator 上的 *__getattr__* 方法的一部分。*

现在，这给了我们一种方法来实现所有的 DataFrame 函数，只需在底层的 *df* 上调用 *DataFrameDecorator 中的方法。让我们看看那会是什么样子。*

上面的代码将所有这些放在一起。现在，即使没有在 *DataFrameDecorator 上定义方法 take，使用 *__getattr__* 我们也可以在底层 *df* 上调用 *DataFrame* 方法。*

## 结论

在这篇文章中，我讲述了如何使用装饰模式包装 DataFrame 以增强其功能。我希望你喜欢它。

在 LinkedIn 上与我联系或在 Medium 上关注我。如果你喜欢这个故事，你可能会喜欢我关于 python decorators 的其他故事:

<https://betterprogramming.pub/decorator-pattern-and-python-decorators-b0b573f4c1ce>  </python-decorators-from-simple-decorators-to-nesting-multiple-33bbab8c5a45> 