# 制作 Python 包第 3 部分:设计您的类

> 原文：<https://towardsdatascience.com/making-python-packages-part-3-designing-your-classes-82b3a5786e30?source=collection_archive---------17----------------------->

## 为成功建立您的图书馆

你好！欢迎阅读制作 Python 包系列的第 3 部分！

![](img/d68ec17032319ff86bf0454e19787344.png)

高塔姆·克里希南在 [Unsplash](https://unsplash.com/s/photos/people-walking-in-a-park?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

前两部分探索了[如何打包你的代码](https://medium.com/@skylar.kerzner/publish-your-first-python-package-to-pypi-8d08d6399c2f)，以及[在 PyPI 上发布供公众使用](https://medium.com/towards-data-science/packages-part-2-how-to-publish-test-your-package-on-pypi-with-poetry-9fc7295df1a5)。

在这一部分，我们将讨论如何设置你的源代码:让你的包做一些有趣的事情的类和函数。

让我们直接开始吧。

**类别和功能**

如果您是一名数据科学家，您可能会花大量时间在笔记本上编写代码，或者编写一个 Python 脚本。

我们中的许多人倾向于在没有太多封装的情况下构建代码原型，只是一行一行地写我们想对我们的熊猫数据帧做的任何统计分析。

有时候你可能会写函数:也许是因为你需要给 pd 一个函数。DataFrame.apply()，或者将数据帧上的一系列类似 sql 的操作封装到一个函数中，或者将所有清理步骤封装到一个列表中。我们编写代码的风格几乎更符合函数式编程，在函数式编程中，像 Pandas DataFrame 或 list 这样的数据结构是通过一系列操作来传递的。

类似地，当我们第一次运行一些统计数据，或者用一个数据集创建一个初始的 ML 模型时，我们也倾向于不在那里写类。

但是我们当然使用*类——毕竟，Python 中的一切都是对象:从字符串和整数，到列表和数据帧，到 ML 模型类和 NLP 管道。*

我们很容易使用这些类对象——我们机械地记住我们可以运行。groupby()转换我们的熊猫系列。tolist()，。update()一个集合，获取。来自计数器的 most_common()。拟合()一个 ML 模型并得到它的。参数

但是，即使我们经常使用这种语法，我们也可能会忽略这样一个事实，即这些确实是某些人编写的类的方法和属性。

当我们编写自己的包时，我们需要生成相同类型的直观方法和属性。

**为什么上课？**

学习正确方式**的最好方法是看错误方式**。

写 Python，Pandas，scikit-learn 和 spaCy 的那些才华横溢的人在设计他们的包的时候，为什么不直接用函数呢？

至少对于 scikit-learn 来说，我们可以想象得到一个函数，它获取你的数据并输出预测和参数。

也许类似于:

```
from scikit-learn import run_logistic_regressionpredictions, parameters = run_logistic_regression(data)
```

这有什么不好？他们为什么使用类？

接下来会发生什么？一旦这个模型被训练，我们需要一个新的函数来做进一步的预测。我们需要将这些参数传递给它。

```
predictions = make_logistic_predictions(data, parameters)
```

这有点尴尬，因为我们必须保存和传递参数。

令人尴尬的是，这两个函数没有正式的关系，尽管它们是为了拟合和使用同一个模型。

如果您运行 dir()或查看源代码，您必须在 scikit-learn 的其他模型的 1000 个其他函数列表中找到这两个相关的函数。

但这可能会变得更糟。

当它是一个决策树或神经网络的集合体——具有复杂架构的东西——时会发生什么？

```
predictions, parameters = run_random_forest(data)
predictions = make_random_forest_predictions(data, parameters)
```

如果这些参数描述了我们所有不同形状的决策树上的规则，它们会采用什么结构？在列表列表中指定树的形状有点困难。

无论如何，我不会反复强调这一点。你可以看到问题。

那么，为什么要上课呢？

因为， ***一个类是状态和行为的容器。***

当我们希望允许用户获取一些数据，用这些数据做一些事情并保存结果时，类是实现这一点的自然方式。

一个类可以存储信息，给我们的用户一套很好的操作，他们可以对这些信息进行操作，并为他们保存结果。

我的意思是，这真的只是面向对象编程的基本论点。但是当你写一个库的时候不要忘记它。

使用类是有意义的！

**界面驱动开发**

所以我们知道我们应该使用类，我们也使用过其他库的类，但是如何为你的库开发正确的类结构呢？

您编写类名、这些类中的方法和属性名，并且还没有填充逻辑。

那是你的界面。接口是一个类的蓝图。它是 API，特别是您的用户将使用的类、方法和属性。

同样的建议也适用于开发函数:首先编写输入和输出，**使用函数的**。只有在这之后，您才应该实现逻辑。

让我们看一个例子。

在 spaCy 中，开发人员希望对文本字符串提供 NLP(自然语言处理)。因此，他们创建了一个类，该类接受文本字符串，并生成一个对象(一个“doc”)来提供对该文本字符串的各种 NLP 分析。

从根本上说，他们的库**获取输入数据**，**通过使用一些算法和数据库对数据进行操作来创建新的信息**，而**使新的信息易于访问**。

总的来说，他们希望直观地访问新信息。

```
doc = nlp(my_text)
token = doc[4] #an object derived from the word at index 4
token.pos_ #outputs the part of speech of that word at index 4
```

1.  他们想给你每个词的 NLP 分析。
2.  他们希望访问一个单词(令牌)看起来像是访问一个列表元素(如果它像内置 Python 对象或熟悉的库一样工作，会更直观)
3.  他们希望对单词(token)的 NLP 分析是简单的属性，如 word.pos_

**结论**

当你开发你的库时，关注你的**终端用户**的体验。

你想让 ***他们的*** 代码变成什么样子。 ***在你的自述文件中写出用例*** 的例子。什么会使他们的代码更直观？让他们的生活变得轻松。

概述一个提供这种体验的库:类、方法、属性。在你得到**用户** **体验**之前，不要写逻辑。

如果你优先考虑用户，那么用户会喜欢你的图书馆！

感谢阅读！

请关注我的 Medium，了解数据科学家的更多见解！

这是本系列的第 1 部分和第 2 部分:

<https://medium.com/@skylar.kerzner/publish-your-first-python-package-to-pypi-8d08d6399c2f>  <https://medium.com/@skylar.kerzner/packages-part-2-how-to-publish-test-your-package-on-pypi-with-poetry-9fc7295df1a5> 