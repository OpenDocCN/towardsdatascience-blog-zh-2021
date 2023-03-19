# 选择正确的深度学习框架的 7 个要点

> 原文：<https://towardsdatascience.com/7-points-to-choose-the-right-deep-learning-framework-815978c5c327?source=collection_archive---------32----------------------->

## 是 Pytorch，Keras，还是 Tensorflow？

![](img/48496be672cf9f63d058ed5c258942c1.png)

照片由 [**瓦伦丁·安东努奇**](https://www.pexels.com/@valentinantonucci?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**Pexels**](https://www.pexels.com/photo/person-holding-compass-841286/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

每一个打算在数据科学领域开始职业生涯的技术爱好者，都会在旅程中的任何时候在谷歌上搜索这个。但是，大多数时候我们得不到明确的答案，而且我们经常以困惑告终。

在这篇文章中，我将尝试帮助您选择更好的深度学习库。我们将对不同的因素进行分析，如架构、速度、用户友好性、受欢迎程度等等。

但是，在开始分析部分之前，让我们简要了解一下它们。

# 克拉斯

神经网络库是开源的。在这个符号数学库中，我们解决与深度学习和机器学习相关的问题。

这个工具的目的是使用深度学习进行快速实验。Keras 就是一个高级编程 API。该应用由 Francois Chollet 于 2015 年 3 月 27 日开发

# Pytorch

这是一个用 Python & C++编写的机器学习 torch 库，可以作为开源程序下载。

脸书的研究小组在 2016 年 10 月开发了这项技术，用于自然语言处理、计算机视觉等应用。

当比较高水平和低水平时，这个程序介于 TensorFlow 和 Keras 之间。

# 张量流

该库很容易与 C++、Java 和其他代码语言集成。它为开发人员和公司提供了全面的工具来构建基于机器学习的应用程序。在这个符号数学库中，解决了深度学习和机器学习问题。

在编程中，TensorFlow 被称为底层 API。它是由谷歌于 2015 年 11 月 9 日创建的。

# **1。架构**

大而复杂的模型训练起来最耗时，所以对于大而复杂的模型处理速度会少一些。

与 Keras 相比，PyTorch 具有更复杂的架构，这导致可读性较差。

这场比赛的获胜者是 TensorFlow 和 PyTorch，它们都是实用的低级模拟框架，速度和时间都很快。

# **2。速度**

Keras 不会在最低速度以上运行。TensorFlow 和 Pytorch 都以最大速度工作，从而带来高性能。

# **3。API 级别**

Keras 的 API 提供了对 Theano 和 CNTK 的访问，所以 Keras 可以在这两个平台上执行。

由于 PyTorch 的底层 API，它只支持数组表达式。最近，它获得了极大的关注，并成为学术研究和需要自定义表达式优化的深度学习应用程序的首选解决方案。

除了提供低级 API，TensorFlow 还提供高级 API。

# **4。初学者友好型**

Keras 中设计了快速原型功能，使深度学习模型易于测试。该程序有一个对初学者非常友好的界面，用户可以像使用乐高积木一样轻松地构建神经网络。

调试 Python 错误就像调试 Python 代码一样简单。使用任何流行的 Python 调试器都可以调试这些错误。

可以使用 Tensorflow 的调试模块来调试其中的错误。

# **5。调试**

使用 Keras 通常很简单，您不太可能遇到任何困难。然而，由于它在后端平台上有太多的抽象层次，调试通常会很困难。

使用 Pytorch 可以比使用 Keras 或 TensorFlow 更容易地进行调试。

TensorFlow 的调试过程可能具有挑战性。

# **6。流行趋势**

Keras 广泛使用基于内核的神经网络，包括卷积层和效用层。Keras 最常用于 Nvidia、优步、亚马逊、苹果和网飞等公司。

它在内部对谷歌的使用以及它用来自动捕捉图像的软件都使它出名。除了谷歌、LinkedIn、Snap、AMD、彭博、Paypal 和高通，Tensorflow 也被许多其他公司使用。

凭借其 NN 模块、优化模块和自动签名模块，Keras 支持高功率 GPU 应用，其在深度学习网络上的自动微分使其广受欢迎。使用 Pytorch 的主要公司有脸书、富国银行、Salesforce、基因泰克、微软和摩根大通。

# **7。数据集**

在最初的版本中，Keras 的速度很慢，是为快速原型设计的。因此，该框架不太适合处理大型数据集。它在较小的数据集中工作良好的原因是它的执行速度很快。

虽然 TensorFlow 和 PyTorch 是底层框架，但它们能够很好地处理大型数据集，因为它们速度很快。

使用此工具可以在高维数据集上执行高性能任务。

# **结论**

为了比较这三个框架，我们考察了各种参数。虽然 PyTorch 用户友好且简单，但 TensorFlow 因其单薄的 API 而不尽人意。

Keras 和 TensorFlow 拥有由砖块和砂浆建造的墙，但它们为通信留下了微小的开口，而 PyTorch 与 Python 紧密绑定，可以应用于许多不同的平台。

> *走之前……*

如果你喜欢这篇文章，并且想继续关注更多关于 **Python &数据科学**的**精彩文章**——请点击这里[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)考虑成为中级会员。

请考虑使用[我的推荐链接](https://pranjalai.medium.com/membership)注册。通过这种方式，会员费的一部分归我，这激励我写更多关于 Python 和数据科学的令人兴奋的东西。

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。