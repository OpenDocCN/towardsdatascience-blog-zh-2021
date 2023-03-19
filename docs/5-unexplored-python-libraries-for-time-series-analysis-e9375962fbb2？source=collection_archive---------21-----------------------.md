# 用于时间序列分析的 5 个未开发的 Python 库

> 原文：<https://towardsdatascience.com/5-unexplored-python-libraries-for-time-series-analysis-e9375962fbb2?source=collection_archive---------21----------------------->

## 很高兴有这些宝石在你的桶里

![](img/240611d7caff9691f0983e22af26f595.png)

照片由 [**贾洛**发自](https://www.pexels.com/@giallo?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[像素 ](https://www.pexels.com/photo/assorted-silver-colored-pocket-watch-lot-selective-focus-photo-859895/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

时间序列分析是数据科学家最常遇到的问题之一。大多数时间序列解决方案处理经济预测、资源需求预测、股票市场分析和销售分析。

如果从头开始，为大量与时间相关的数据开发复杂的模型对程序员来说可能是一项艰巨的任务。这就是 Python 发挥作用的地方，它有专门为时间序列分析编写的令人惊叹的库。

本文将讨论五个这样的库，如果您有兴趣解决与时间序列相关的问题，它们可能会对您有所帮助。其中一些库正在使用深度学习方法来寻找数据中的最佳模式。

尽管如此，我还是建议用您的数据逐个尝试这些库，然后观察哪个模型可以帮助您以更好的方式捕捉模式。您还可以组合每个模型的结果来获得一个合并的结果，这有时会为我们提供一个更好的结果。

# **1。** **自动驾驶**

顾名思义，它是一个用于自动时间序列分析的 Python 库。AutoTS 允许我们用一行代码训练多个时间序列模型，这样我们就可以选择最适合我们问题的模型。

这个库是 autoML 的一部分，其目标是为了方便初学者而自动化库。

> 属国

*   Python 3.6 以上版本
*   Numpy
*   熊猫
*   Sklearn
*   统计模型

> **安装**

```
pip install autoTS
```

你可以在这里了解更多关于这个库的信息。

# **2。** **先知**

Prophet 是一个优秀的库，由脸书的数据科学团队开发，用于解决时间序列相关的问题，可以使用 R 和 python。

这对于处理具有强烈季节性影响的时间序列特别有用，如购买行为或销售预测。此外，它可以很好地处理杂乱的数据，无需任何手动操作。

> **安装**

```
pip install prophet
```

你可以在这里了解更多关于这个库的信息。

# **3。** **飞镖**

Darts 是一个 scikit-learn 友好的 Python 包，由 Unit8.co 开发，用于预测时间序列。它包含大量模型，从 ARIMA 到深度神经网络，用于处理与日期和时间相关的数据。

该库最大的优点是它还支持使用神经网络的多维类。

它还允许用户组合来自几个模型和外部回归的预测，这使得回测模型更容易。

> **安装**

```
pip install darts
```

你可以在这里了解更多关于这个图书馆[的信息。](https://github.com/unit8co/darts)

# **4。** **Pyflux**

Pyflux 是一个为 python 构建的开源时序库。Pyflux 选择了更多的概率方法来解决时间序列问题。这种方法对于像预测这样需要更全面的不确定性的任务尤其有利。

用户可以建立一个概率模型，通过联合概率将数据和潜在变量视为随机变量。

> **安装**

```
pip install pyflux
```

你可以在这里了解更多关于这个库[的信息。](https://pyflux.readthedocs.io/en/latest/)

# **5。** **Sktime**

Sktime 是一个 Python 库，带有与 scikit-learn 兼容的时序算法和工具。它还具有分类、回归和时间序列预测模型。该库的主要目标是制作可以与 scikit-learn 互操作的模型。

> **安装**

```
pip install sktime
```

你可以在这里了解更多关于这个库的信息。

# **结论**

这些是一些 Python 库/框架，可以在处理时间序列问题时使用。互联网上还有一些更酷的时间序列库，比如 tsfresh、atspy、kats(——你也可以去看看。

主要目标是根据您的需求选择一个库，即能够满足您的问题陈述要求的库。要了解更多关于这些库的信息，你可以查看它们各自提供的文档，因为大多数都是完全开源的。

> 在你走之前…

如果你喜欢这篇文章，并且想继续关注更多关于 **Python &数据科学**的**激动人心的**文章，请点击这里[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)考虑成为一名中等会员。

请考虑使用[我的推荐链接](https://pranjalai.medium.com/membership)注册。通过这种方式，会员费的一部分归我，这激励我写更多关于 Python 和数据科学的令人兴奋的东西。

还有，可以随时订阅我的免费简讯: [**普朗加尔的简讯**](https://pranjalai.medium.com/subscribe) 。