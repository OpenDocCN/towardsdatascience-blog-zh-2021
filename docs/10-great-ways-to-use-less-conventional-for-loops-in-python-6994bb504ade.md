# 在 Python 中使用非常规 For 循环的 5 个好方法

> 原文：<https://towardsdatascience.com/10-great-ways-to-use-less-conventional-for-loops-in-python-6994bb504ade?source=collection_archive---------6----------------------->

## 避免基本循环和创建更快算法的简单技术

![](img/320ef53919e8199cb264c962e5e5f2f5.png)

(图片由[mikkekylitt](https://pixabay.com/images/id-4201970/)在 [Unsplash](http://unsplash.com) **)**

# 介绍

for 循环；这通常是我们介绍计算艺术的一个关键部分。for 循环是一个多用途的工具，通常用于操作和处理数据结构。对于许多操作，您可以使用 For 循环来获得相当好的性能，同时仍然完成一些重要的操作。然而，在现代 Python 中，有一些方法可以用来练习典型的 for 循环。这比 Python 中传统的 for 循环要快。也就是说，这些选项是可用的，这当然是一件好事，在某些情况下，它们可以用来加速 Python 代码！此外，如果您想查看本文的源代码，可以在这里查看:

[](https://github.com/emmettgb/Emmetts-DS-NoteBooks) [## GitHub-emmettgb/Emmetts-DS-NoteBooks:各种项目的随机笔记本。

### 这些是我的开源笔记本，随意打开阅读，下载，和分叉等。我的投资组合您也可以…

github.com](https://github.com/emmettgb/Emmetts-DS-NoteBooks) 

# 有关 for 循环的更多信息…

在我们深入研究一些不使用 for 循环的好方法之前，让我们先来看看如何在 Python 中解决一些使用 for 循环的问题。这将使我们注意到循环在典型的编程场景中是如何使用的。此外，我们可以看看 for 循环可能导致的性能问题。让我们来看看最传统的 Pythonic for 循环，我们很多人在学习这门语言时可能都学过:

```
data = [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]for i in data:
    print(i)
```

这种方法有几个问题。虽然对于像这样的例子，对于这种少量的数据，这肯定会工作得很好——并且在大多数情况下可能是这样，有一些更好的——更 Pythonic 化的方法我们可以用来加速代码。这种传统意义上的 For 循环几乎可以完全避免。在某些情况下，这种语法可以压缩成一个方法调用。for 循环的问题是，它们可能会大大增加处理时间。这绝不是说“完全抛弃 for 循环”，就像有些人从他们的编程工具箱中那样。相反，我会说，拥抱目标——这是一个人在任何技术栈组件上应该有的立场。

for 循环有一个特殊的目的，但是这个列表中的一些选项也是如此。让程序员变得伟大的一点是能够选择适合他们当前团队的堆栈。这归结为选择正确的模块、函数和类似的东西。世界上没有人有足够的时间来学习每个模块和每个电话，所以权衡一下可以学习的模块，并阅读概述新选项的文章，肯定是确保一个人的技能足够多样化的好方法。

for 循环可能有问题的原因通常与处理大量数据或对所述数据执行许多步骤有关。这个列表中的一些工具在这方面或那方面特别出色，这就是这些技术的优势所在。

# №1:1 行 for 循环。

单行 for 循环是我们都应该利用的语法黑客的经典例子。单行 for 循环有几个特征使它不同于常规的 for 循环。其中最明显的是它包含在一行中。

```
[print(i) for i in data]
```

关于这种循环的另一个重要的事情是，它也将提供一个回报。对于打印示例，由于每个示例都是标准输出，所以我们实际上返回的是一个空数组。这个特性值得注意，因为它使得这种循环的应用非常明显。此循环最适合对一组值执行小型运算。让我们编写一个快速函数，将一些统计数据应用到我们的值中。这里有两个支持函数，其中一个实际上使用了一行 for 循环，这是我为了演示而创建的:

```
import math
"""
mean(x : list)
Returns the mean of a list.
"""
def mean(x : list):
    return(sum(x) / len(x))"""
std(x : list)
Returns the standard deviation (std) of a list.
"""
def std(x : list):
    m = mean(x)
    x = [(i - m) ** 2 for i in x]
    m = mean(x)
    m = math.sqrt(m)
    return(m)
```

第一个函数是简单的均值函数，然后在下面的标准差函数中使用。这使用一行 for 循环对数据求平方，收集其平均值，然后收集该平均值的平方根。现在，对于我们的最后一个组件，我们将编写一个正态分布函数，它将对这些数据进行标准缩放。我们将在单行 for 循环中缩放每个值。

```
def normal(x : list):
    mu = mean(x)
    sigma = std(x)
```

我们的单行 for 循环需要这些值。语法的工作原理是在空的 iterable 中创建一个迭代器，然后将数组复制到新的数组中。另一种方法是追加或推送。让我们来看看一行代码:

```
return([(i - mu) / sigma for i in x])
```

让我们使用%timeit 来检查这样做需要多长时间。在本文的最后，我将比较这个应用程序中的所有时间，以衡量哪个选项可能是最好的。

```
import numpy.random as rd
x = rd.randn(10000)
%timeit normal(data)3.37 µs ± 136 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

# 2 号:λ

我们要研究的下一项技术是 Lambda。Lambda 是一种简单的技术，我们可以在 Python 内部使用它来创建表达式。然后可以使用 apply()方法在 iterable 上计算这些表达式。当然，使用 lambda 还有很多其他方式。对于今天的例子，我们将把 lambda 应用到我们的数组中，以便正常分布我们的数据。实际上，我不久前写了一篇文章，谈论 Lambda 的伟大之处。如果你想更深入地了解这项技术，你可以在这里做:

[](/scientific-python-with-lambda-b207b1ddfcd1) [## 带 Lambda 的科学 Python

### Python Lambda 函数的正确用法:Python 科学编程的最佳语法。

towardsdatascience.com](/scientific-python-with-lambda-b207b1ddfcd1) 

Lambda 非常容易使用，只需要几秒钟就可以学会。然而，当一个人刚刚开始时，很容易明白为什么各种各样的 lambda 知识会变得令人困惑。让我们看看如何将 lambda 应用到我们的函数中。我们将面临的问题是，最终 lambda 在这种实现中并不能很好地工作。然而，尽管如此，λ更多的是一个组成部分；幸运的是，在有些应用程序中，我们可以将这个列表中的另一个组件与 lambda 结合起来，以形成一个使用 lambda 来应用不同操作的工作循环。在我们函数的例子中，例如:

```
def normallambda(x : list):
    mu = mean(x)
    sigma = std(x)
```

首先我们定义一个 lambda 表达式:

```
ex = lambda x: x - mu / sigma
```

然后，我们使用一行 for 循环将表达式应用于我们的数据:

```
return([ex(y) for y in x])
```

# №3:应用

鉴于我们中许多从事 Python 工作的人都是数据科学家，很可能我们中的许多人都与熊猫打交道。如果情况确实如此，我希望向您介绍来自 Pandas 的 apply()方法。这个函数包含在 Pandas DataFrames 中，允许使用 Lambda 表达式来完成各种令人惊叹的事情。当然，为了实际使用它，我们首先需要使用熊猫图书馆。让我们快速地将数据放入一个数据框架中:

```
import pandas as pd
df = pd.DataFrame({"X" : x})
```

现在我们将编写我们的新函数，请注意，类型更改为 pd。数据帧，调用略有改变:

```
def normaldfapply(x : pd.DataFrame):
    mu = mean(x["X"])
    sigma = std(x["X"])
```

现在让我们使用我们的 lambda 调用。从循环到 apply 方法没有任何变化:

```
ex = lambda x: x - mu / sigma
```

使用 apply()方法时，可以从 Series 和 DataFrame 类型调用该方法。因此，在这种情况下，由于我们使用的是一维序列，不需要将其应用于此数据帧的整个范围，因此我们将使用该序列。我们可以通过用[]索引数据帧来调用该系列。

```
x["X"].apply(ex)
```

对于最终的函数，如下所示:

```
def normaldfapply(x : pd.DataFrame):
    mu = mean(x["X"])
    sigma = std(x["X"])
    ex = lambda x: x - mu / sigma
    x["X"].apply(ex)
```

# №4: Itertools

从一个更基本的实现角度来看，解决这个问题的一个好方法是使用 itertools。itertools 模块包含在 Python 标准库中，是一个非常棒的工具，我建议一直使用它。它是在 Python 中实现流行的、快速的数据处理算法，这些算法可以用更少的 Python 来完成工作。我有一整篇文章详细介绍了 itertools 的神奇之处，如果您愿意，可以点击这里查看:

[](/wicked-fast-python-with-itertools-55c77443f84c) [## 使用 Itertools 的超快速 Python

### 快速浏览一种简单的方法，通过使用 itertools 让 Python 更快、更有效地进行机器学习…

towardsdatascience.com](/wicked-fast-python-with-itertools-55c77443f84c) 

问题是，这个库提供了很多东西——所以我很高兴有人能在这里更深入地研究那篇文章——因为现在我只打算写这个函数，到此为止。我肯定地认为，在大多数情况下，对这个模块进行更多的阅读是有保证的，尽管它确实是你的武器库中一个令人敬畏的多功能工具。

```
import itertools as its
```

我们在这个例子中使用的主要函数是 itertools.cycle，这个方法为这个数组创建一个新的迭代器。我们还可以在其中添加算法，这使得它非常适合这个实现。

```
def itsnorm(x : list):
    count = 1
    newx = []
    mu = mean(x["X"])
    sigma = std(x["X"]) 
    z = its.cycle(x * std * m)
    return(z)
```

# №5:当

最后，也可能是意想不到的，避免在代码中使用传统 for 循环的方法是使用 while。当然，也会有这是一个糟糕的选择的情况。同样，也有这是最佳选择的例子。通常，当涉及到 iterables 时，很少使用循环。

然而，让我们想一想为什么 while 循环不用于这样的事情。首先，必须打破 while 循环。对于 iterable 循环来说也是如此，但仅仅是因为 iterable 已经完成了迭代(或者在条件之外有一些 break 设置之类的)。)也就是说，在某些实现中，while 循环确实在做一些非常迭代的事情。

```
def normalwhile(x : list):
    count = 1
    newx = []
    mu = mean(x["X"])
    sigma = std(x["X"])
    while count <= len(x):
        newx.append((x[count] - mu) / sigma)
        count += 1
```

真正拖累 while 循环的是使它运行得更像 for 循环的所有计算。当然，在某些情况下这可能会派上用场，但是在这个例子中，我并不认为这比传统的 for 循环写得更好。有很多初始化工作，就像我们需要一个常规的 for 循环一样。

# 结论

当然，人们可以有更多的方法来解决这类问题。在所有应用中，没有一种解决方案比另一种更好，我认为这些不同的工具各有所长。程序员使用循环和与循环交互的方式绝对是影响代码最终结果的重要因素。让我们看看所有这些技术，以及它们在我们的分配问题中的应用，然后看看在这个特定的场景中哪种技术做得最好。这是用标准、直接的 for-loop 风格编写函数的方式:

```
def normalfor(x : list):
    mu = mean(x)
    sigma = std(x)
    newx = []
    for i in x:
        newx.append((i - mu) / std)
    return(newx)
```

经过快速比较，在这个实例中胜出的是 Pandas 的 df.apply()方法。这对 Python 来说意味着什么？在许多情况下，尽管使用正则 Pythonic 表达式似乎更合理，但有时你就是无法打败基于 C 的库。Pandas 可以超越我们编写的任何 Python 代码，这既展示了 Pandas 有多棒，也展示了使用 Python 中的 C 语言有多棒。然而，紧随其后的是内联 for 循环。在该选项可能需要替换的情况下，肯定会推荐使用该技术。另一点需要注意的是，没有包括实际创建所使用的类型的时间，这对于 Apply()方法来说可能是一个小小的缺点，因为您的数据必须在 DataFrame 中。非常感谢你看我的文章！我希望这是有见地的，理想的启发你的 Python 代码！编程快乐！