# 如何用 Numba 提升你的 Python 速度🚀

> 原文：<https://towardsdatascience.com/how-to-skyrocket-your-python-speed-with-numba-89fd16362e47?source=collection_archive---------9----------------------->

## 快速介绍

## 学习如何使用 Numba Decorators 让你的代码更快

![](img/6d226e044eabee415758fd23111abc9e.png)

Marc-Olivier Jodoin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 您的旅程概述

1.  [设置舞台](#3846)
2.  [了解数字和安装](#31ff)
3.  [使用 Jit-Decorator](#6cf5)
4.  [三个陷阱](#2df5)
5.  【Numba 闪耀的地方
6.  [包装](#500b)

# 1 —搭建舞台

在数据科学和数据工程中，许多从业者每天都在编写代码。为一个问题制定一个可行的解决方案绝对是最重要的事情。然而，有时代码的执行速度也很重要。

在**实时分析**和**预测**中尤其如此。如果代码太慢，那么这可能会给整个系统造成瓶颈。随着时间的推移，系统通常会变慢。由于要处理的数据越来越多，在数据学科中尤其如此。最坏的情况是，你构建的实时系统可能太慢而没有用😮

很多编译型编程语言，比如 C++，一般都比 Python 快。这是否意味着您应该将整个 Python 管道连根拔起？不。这通常不值得付出巨大的努力。

另一种方法是让您的 Python 代码更快。这就是 Numba 介入的地方:

> Numba 是一个 Python 库，旨在提高你的 Python 代码的速度。Numba 的目的是在运行时检查你的代码，看看是否有部分代码可以被翻译成快速机器码。

听起来很复杂，对吧？确实是。然而，对于最终用户(也就是你)来说，使用 Numba 非常简单。通过几行额外的 Python 代码，您可以显著增加代码库的主要部分。你并不真的需要理解 Numba 是如何工作的才能看到结果。

在这篇博文中，我将向你展示 Numba 的基础知识，让你开始学习。如果你需要了解更多，那么我推荐 [Numba 文档](https://numba.pydata.org/)。如果你更喜欢视觉学习，那么我也制作了一个关于这个主题的视频:

# 2 —了解 Numba 和安装

让我先给你一个 Numba 的高层次概述👍

Numba 这样描述自己:

> Numba 是一个开源的 JIT 编译器，它将 Python 和 NumPy 代码的子集翻译成快速的机器代码。— Numba 文档

我们来解开上面的说法。Numba 是一个开源的轻量级 Python 库，它试图让你的代码更快。其方式是使用行业标准的 [LLVM](https://llvm.org/) 编译器库。你不需要理解 LLVM 编译器库来使用 Numba。

在实践中，您将添加某些 [Python 装饰器](https://realpython.com/primer-on-python-decorators/)来告诉 Numba 应该优化正在讨论的装饰函数。然后，在运行时，Numba 会检查您的函数，并尝试将部分函数编译成快速机器码。

术语 **JIT 编译**是[即时编译](https://en.wikipedia.org/wiki/Just-in-time_compilation)的缩写。因此，编译步骤发生在代码执行期间，而不是预先编译代码(例如 C++)。实际的区别？您不会生成难以共享的二进制文件，而只会得到 Python 文件！

让我从 Numba 的主页向您展示一个代码示例，以演示 Numba 是多么容易使用。下面的代码是一个[蒙特卡罗方法](https://en.wikipedia.org/wiki/Monte_Carlo_method)用于近似圆周率的值。

```
import random

def monte_carlo_pi(nsamples):
    acc = 0
    for i in range(nsamples):
        x = random.random()
        y = random.random()
        if (x ** 2 + y ** 2) < 1.0:
            acc += 1
    return 4.0 * acc / nsamples
```

Numba 尚未应用于上述代码。如果变量`nsamples`很大，那么函数`monte_carlo_pi`相当慢。但是，添加下面两行代码会使它快很多:

```
from numba import jit # <-- importing jit from numba
import random

@jit(nopython=True) # <-- The only difference
def monte_carlo_pi(nsamples):
    acc = 0
    for i in range(nsamples):
        x = random.random()
        y = random.random()
        if (x ** 2 + y ** 2) < 1.0:
            acc += 1
    return 4.0 * acc / nsamples
```

没那么糟，对吧？😃

如果您通过 Anaconda 在 Jupyter 笔记本中工作，那么在 Anaconda 提示符下运行以下命令来安装 Numba:

```
conda install numba
```

如果你在 IDE 中编写代码，比如 Visual Studio Code[或者 py charm](https://code.visualstudio.com/)[或者 PIP，那么你可能想通过 PIP 安装 Numba:](https://www.jetbrains.com/pycharm/)

```
$ pip install numba
```

更多高级选项，如从源代码编译 Numba，可以在[安装页面](https://numba.readthedocs.io/en/stable/user/5minguide.html)中找到。

# 3—使用 Jit 装饰器

现在 Numba 已经安装好了，你可以试用了。我创建了一个 Python 函数，它执行一些 NumPy 操作:

```
import numpy
from numba import jitdef numpy_features(matrix: np.array) -> None:
    """Illustrates some common features of NumPy."""
    cosine_trace = 0.0
    for i in range(matrix.shape[0]):
        cosine_trace += np.cos(matrix[i, i])
    matrix = matrix + cosine_trace
```

上面的代码不用想太多。该功能的唯一目的是使用 NumPy 中的几个不同功能，如[通用功能](https://numpy.org/doc/stable/reference/ufuncs.html)和[广播](https://numpy.org/doc/stable/user/basics.broadcasting.html)。让我用 Jupyter 笔记本中的以下神奇命令为上面的代码计时:

```
x = np.arange(1000000).reshape(1000, 1000)
%time numpy_features(x)**Output:**
Wall time: 32.3 ms
```

如果您运行上面的代码，根据您的硬件和其他因素，您将获得稍微不同的速度。这可能不是你见过的最慢的 Python 代码。然而，整个代码库中像这样的代码确实会降低整个应用程序的速度。

现在让我们将`@jit(nopython = true)`装饰器添加到函数中，看看会发生什么。代码现在应该如下所示:

```
import numpy
from numba import jit@jit(nopython=True)
def numpy_features(matrix: np.array) -> None:
    """Illustrates some common features of NumPy."""
    cosine_trace = 0.0
    for i in range(matrix.shape[0]):
        cosine_trace += np.cos(matrix[i, i])
    matrix = matrix + cosine_trace
```

编写代码的方式没有太大变化，但是速度不同了。如果您再次对代码计时，您会得到以下结果:

```
x = np.arange(1000000).reshape(1000, 1000)
%time numpy_features(x)**Output:**
Wall time: 543 ms
```

什么？代码变得比原始代码慢 10 倍😧

不要气馁。尝试再次运行代码:

```
x = np.arange(1000000).reshape(1000, 1000)
%time numpy_features(x)**Output:**
Wall time: 3.32 ms
```

现在的代码比原来的代码快了 10 倍。

这是怎么回事？😵

# 4—三个陷阱

## 编译的陷阱

我给你看的这个奇怪的东西不是一个 bug，而是一个特性。当你第一次用`@jit(nopython=True)`装饰器运行函数时，代码会变慢。为什么？

第一次，Numba 必须检查函数中的代码，并找出要优化的代码。这增加了**额外的开销**，因此该功能运行缓慢。然而，每次后续的功能会快得多。

这最初看起来像是一种权衡，但实际上并非如此。在数据分析和数据工程中，函数要运行很多次。

例如，考虑一个在预测之前对进入数据管道的新数据进行归一化的函数。在实时系统中，新数据一直在不断到达，函数每分钟被使用数百次甚至数千次。在这种系统中，最初较慢的运行可以在几秒钟内完成。

## 不将参数 nopython 设置为 True

装饰器`@jit(nopython=True)`可以不带参数`nopython`使用。如果你这样做，那么默认情况下 Numba 将设置`nopython=False`。

这不是一个好主意！

如果`nopython=False`那么 Numba 不会在无法优化代码时提醒你。在实践中，您只需将 Numba 开销添加到代码中，而无需任何优化。这会降低代码的速度😠

如果 Numba 没有成功优化您的代码，那么您希望被告知。最好完全移除 Numba 装饰器。因此你应该总是使用参数`@jit(nopython=True)`。

> 专业提示:装饰者`@njit`是`@jit(nopython=True)`的简写，很多人用它来代替。

## 不要过度优化你的代码

过度优化代码意味着在不需要的时候花费大量时间来获得最佳性能。

**不要过度优化你的代码！**

在许多情况下，代码速度并不那么重要(例如，批量处理中等数量的数据)。降低代码速度几乎总是会增加开发时间。仔细权衡是否应该将 Numba 整合到代码库中。

# Numba 闪耀的地方

![](img/0a368c0f53b2279c2051a3ef093dd73e.png)

照片由[穆罕默德·诺哈西](https://unsplash.com/@coopery?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

假装 Numba 擅长优化任何类型的 Python 代码对任何人都没有帮助。Numba 擅长优化的只是一些 Python 代码。

Numba 非常擅长优化任何涉及循环或 NumPy 代码的东西。由于许多机器学习库(如 [Scikit-Learn](https://scikit-learn.org/stable/) )大量使用 Numpy，这是使用 Numba 的好地方🔥

然而，Numba 不理解例如熊猫。将`@jit(nopython=True)`添加到一个纯粹处理熊猫数据帧的函数中可能不会产生很好的性能。参见 [Numba 文档](https://numba.readthedocs.io/en/stable/user/5minguide.html)中的示例。

我的建议如下:

> 总是通过测试代码的速度来检查添加的 Numba decorator 是否增加了价值(在第一个编译步骤之后)。不要只是为了好玩而使用 Numba decorators，必要时这样做可以加快代码的速度。

# 6—总结

如果你需要了解更多关于 Numba 的信息，那么查看 Numba 文档[或我在 Numba 上的 YouTube 视频](https://numba.pydata.org/)。

**喜欢我写的？**查看我的其他帖子，了解更多 Python 内容:

*   用漂亮的类型提示使你罪恶的 Python 代码现代化
*   [用 Python 可视化缺失值非常简单](/visualizing-missing-values-in-python-is-shockingly-easy-56ed5bc2e7ea)
*   【SymPy 符号数学快速指南
*   [5 个超赞的数字功能，能在紧要关头救你一命](/5-awesome-numpy-functions-that-can-save-you-in-a-pinch-ba349af5ac47)
*   [5 个专家提示，让你的 Python 字典技能突飞猛进🚀](/5-expert-tips-to-skyrocket-your-dictionary-skills-in-python-1cf54b7d920d)

如果你对数据科学、编程或任何介于两者之间的东西感兴趣，那么请随意在 LinkedIn 上加我，并向✋问好