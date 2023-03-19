# 包装 numpy 的数组

> 原文：<https://towardsdatascience.com/wrapping-numpys-arrays-971e015e14bb?source=collection_archive---------20----------------------->

## 集装箱方法。

***记得使用右边的“关注”按钮来关注我→:你会收到新文章的通知，并帮助我达到 100 个关注者的目标:)***

Numpy 的数组是功能强大的对象，通常被用作更复杂对象的基础数据结构，如 [pandas](https://github.com/pandas-dev/pandas) 或 [xarray](https://github.com/pydata/xarray) 。也就是说，您当然也可以在自己的类中使用 numpy 的强大数组——为此，您基本上有两种方法:

*   **子类方法**:创建从 numpy.ndarray 继承的类
*   **容器方法**:创建属性为数组的类

## 在本文中，我们将看到如何使用容器方法包装 numpy 的数组来正确地创建自己的自定义类*。*

![](img/03e5fedec1fec78213522d230253eca3.png)

照片由 [Guillaume Bolduc](https://unsplash.com/@guibolduc?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

让我们以一个示例项目为例:我们想要创建一个简单的项目来处理物理单位和维度，创建长度类似于`[1, 2, 3] meter` 或重量类似于`[55 65 8] kilogram`的数组，然后使用这些数组来计算平均身高或【身体质量指数】([https://en.wikipedia.org/wiki/Body_mass_index](https://en.wikipedia.org/wiki/Body_mass_index))。我们希望依靠 numpy 来完成繁重的数字计算(如加、减、幂)，但我们也希望能够处理 numpy 数组之类的实例，如`np.sort(weights)`或`np.min(heights)`。

为此，我们将创建一个使用容器方法包装 numpy 数组的新类。数值将存储为普通的 numpy 数组，物理维度存储为字符串:

物理阵列的第一种实现

这将简单地打印:`[55.6 45.7 80.3] kilogram`。同样，这个字符串后面的数字列表是存储在`self.value`中的实际 numpy 数组。

现在这是完全无用的:我们不能让这个对象与任何其他东西交互，所以我们添加了基本的操作，比如与其他`Physical`实例的加法或乘法:

现在，物理阵列可以与其他物理阵列相加或相乘。

注意，在增加或减少物理量之前，我们首先检查它们是否有相同的单位:你不能用重量来增加长度(或用胡萝卜增加土豆，或用驴子增加马)。

这太棒了，我们现在可以计算一组体重指数(身体质量指数),给定一组以米为单位的身高和一组以千克为单位的体重。身体质量指数简单地通过将重量除以高度的平方给出，即:

BMI =weight(kg)/height(m)^2

万岁！我们用一个高度数组和一个高度数组计算体重指数数组，用后面的 numpy 数组进行实际的数值计算。但是 numpy 的阵列提供了更多的东西，这就是它真正有趣的地方。

# **实现 numpy 功能支持**

Numpy 提供了许多有用的函数用于数组。仅举几个例子:

*   `np.sin`、`np.cos`、`np.tan`等
*   `np.exp`、`np.log`、`np.log10`等
*   `np.add`、`np.multiply`、`np.divide`等
*   `np.min`、`np.max`、`np.argmin`、`np.argmax`等
*   `np.floor`、`np.ceil`、`np.trunc`等
*   `np.concatenate`、`np.vstack`等

诸如此类。你可以在 numpy 的网站上找到他们所有的东西:https://numpy.org/doc/stable/reference/routines.html。

让我们试着在课堂上使用其中一个:

试图在我们的物理实例`bmi`上调用`np.mean`会引发一个`AttributeError`，因为 numpy 依赖于整数的加法和除法，而我们的类不能正确地实现这种操作。所以我们必须在某个地方告诉 numpy，我们希望`np.mean(bmi)`如何表现。

这就是`__array_function__`接口发挥作用的地方。

接口只是一个规范化的过程，用来重载(某些)numpy 函数如何处理来自你的类的参数。

让我们看一个简单的例子来处理我们的`np.mean(bmi)`呼叫:

使用 __array_function__ 接口实现 np.mean 支持

再次欢呼，`np.mean(bmi)`返回我们的物理数组的“平均值”，它确实是一个物理量，单位为“kilogram/meter^2".”

让我们回顾一下为了实现这一点我们在代码中添加了什么。有 4 件事需要注意:

1.  首先，我们在类定义之上创建一个名为`HANDLED_FUNCTION = {}`的空字典。
2.  其次，我们向我们的类中添加了一个名为 `**__array_function__**`的**方法，该方法带有一个名为`func`的参数。我们一会儿将回到这个方法的内容。**
3.  第三，我们创建一个装饰器构造函数:这是一个返回装饰器的函数(即另一个接受函数作为参数的函数)。我们的`implements`装饰器只是在我们的`HANDLED_FUNCTION`字典中创建一个 numpy 函数和一个`func`函数之间的对应关系，这是我们的 numpy 函数版本。
4.  第四，当使用作为物理实例的`x`调用`np.mean(x)`时，我们实现了 numpy 的 mean 来处理物理实例。它具有与`np.mean`大致相同的签名，并执行以下操作:

*   使用 x 的值计算数值平均值，`x._value`，这是一个简单的数组。
*   然后使用平均值作为值，输入的单位作为单位，创建一个新的物理实例。
*   最后，我们在那个函数上使用`implements`装饰器。

那么当我们调用`np.mean(bmi)`时会发生什么呢？

嗯，因为 numpy 无法计算平均值，正如我们在上面看到的，它检查`bmi`是否有一个`__array_function__`方法，并用在`bmi`上使用的函数调用它，即`np.mean` : `bmi.__array_function__(np.mean, *args, **kwargs)`。

由于`np.mean`已经在`HANDELED_FUNCTIONS`中注册，我们用它来代替*来称呼`np.mean`的我们版本*:这里`HANDLED_FUNCTIONS[np.mean](*args, **kwargs)`相当于`np_mean_for_physical(*args, **kwargs)`。

这就是如何让 numpy 的函数与您的自定义类一起工作。

不幸的是，这并不完全正确。这个接口只适用于一些 numpy 函数，而不是所有的函数。

还记得上面的函数列表吗？我们可以将它们分为两个子列表:常规的 numpy 函数和 numpy 通用函数——或简称为“ufuncs ”:

*   数字功能:`np.min`、`np.max`、`np.argmin`、`np.argmax`、`np.concatenate`、`np.vstack.`
*   Numpy ufuncs : `np.sin`、`np.cos`、`np.tan`、`np.exp`、`np.log`、`np.log10`、`np.add`、`np.multiply`、`np.divide`、`np.floor`、`np.ceil`、`np.trunc`

我们看到了如何使用`__array_function__`实现 numpy 函数支持。在下一篇文章中，我们将看到如何使用`__array_ufunc__`接口添加对“ufuncs”的支持。

# 总结一下:

*   使用 numpy 数组的容器方法在于将数组设置为自定义类实例中的属性(与数组的子类化相反)。
*   要让你的类使用 numpy 函数调用，比如`np.mean(my_array_like_instance)`，你必须在你的类中实现`__array_function__`接口。
*   这基本上是通过在你的类中添加一个`__array_function__`方法，编写你自己的包装器(就像我们对`np_mean_for_physical`所做的那样)，并将它们链接在一起(就像我们对查找字典`HANDLED_FUNCTIONS`所做的那样)。
*   请注意，这只适用于“常规”numpy 函数。对于 numpy 的“通用”函数，您也需要实现`__array_ufunc__`接口。

这个主题非常广泛，因此您应该阅读以下几个链接，以便更好地了解什么是最重要的:

*   集装箱进场:[https://numpy.org/doc/stable/user/basics.dispatch.html](https://numpy.org/doc/stable/user/basics.dispatch.html)
*   `__array_function__`参考:[https://numpy . org/doc/stable/reference/arrays . classes . html # numpy . class . _ _ array _ function _ _](https://numpy.org/doc/stable/reference/arrays.classes.html#numpy.class.__array_function__)
*   ufuncs 参考:[https://numpy.org/doc/stable/reference/ufuncs.html](https://numpy.org/doc/stable/reference/ufuncs.html)

以下是我们在本文中编写的完整代码:

干杯！