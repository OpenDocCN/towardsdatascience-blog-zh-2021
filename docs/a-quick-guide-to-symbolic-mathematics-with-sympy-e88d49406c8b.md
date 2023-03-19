# SymPy 符号数学快速指南

> 原文：<https://towardsdatascience.com/a-quick-guide-to-symbolic-mathematics-with-sympy-e88d49406c8b?source=collection_archive---------3----------------------->

## 了解如何在 Python 中操作数学表达式

![](img/4546caf8cf1fd6c3d75eaec3c90216ef.png)

米歇尔·玛特隆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 您的旅程概述

1.  [设置舞台](#d5ae)
2.  [安装症状](#d9f5)
3.  [创建 SymPy 符号](#fbb6)
4.  [解方程](#82bc)
5.  [有用的功能](#c8ab)
6.  [你可以用 SymPy 做的其他事情](#ccca)
7.  [包装](#4e28)

# 1 —搭建舞台

无论你是大学的学生，还是从事数据科学的工作，都没有绕过数学的路。有人甚至会说，数据科学(简单地说)是应用数学/统计学的一种形式。在 Python 中，有很多处理数学数值的库，像 **NumPy** 、 **SciPy** 、 **Scikit-Learn** 和 **Tensorflow** 。然而，要明确地处理数学符号，只有一个竞争者:SymPy。

SymPy 在 Python 中代表*符号数学，是一个处理数学的 Python 库。在 NumPy、Pandas 和 Matplotlib 等巨头中，它是 SciPy 生态系统的核心库之一。有了 SymPy，你可以操纵数学表达式。此外，你可以解决大学水平的数学问题，需要微分，积分和线性代数。*

在这篇博文中，我将向您展示如何使用 SymPy 中的基础知识，以便您可以开始象征性地解决数学问题。我之前在 SymPy 上制作了一个视频系列，所以如果你是一个视觉学习者，也可以随意看看😃

**前提:**你应该大概知道 Python 的基础知识。除此之外，你不需要任何关于 SymPy 的经验😌

# 2 —安装 SymPy

SymPy 的开发者推荐通过 [Anaconda](https://www.anaconda.com/) 安装 SymPy。如果您使用的是 Anaconda，那么您应该已经安装了 SymPy。为了确保您拥有最新的版本，请在 Anaconda 提示符下运行以下命令:

```
conda update sympy
```

如果您没有使用 Anaconda，而是使用 PIP，那么您可以运行以下命令:

```
pip install sympy
```

对于安装 SymPy 的其他方式，请查看[安装选项](https://docs.sympy.org/latest/install.html)。

# 3 —创建交响乐符号

现在让我们从 SymPy 开始吧！SymPy 的基本对象是一个*符号*。要在 SymPy 中创建一个符号`x`,您可以写:

```
# Import the package sympy with the alias sp
import sympy as sp# Create a symbol x
x = sp.symbols("x")
```

上面的代码创建了符号`x`。SymPy 中的符号是用来模拟代表未知量的数学符号。因此，下面的计算就不足为奇了:

```
print(x + 3*x)
**Output:** 4*x
```

正如你从上面看到的，符号`x`就像一个未知数。如果你想创建一个以上的符号，那么你可以这样写:

```
y, z = sp.symbols("y z")
```

这里你同时做了两个符号，`y`和`z`。现在，您可以随意加减乘除这些符号:

```
print((3*x + 2*y - 3*z)/x)
**Output:** (3*x + 2*y - 3*z)/x
```

在下一节中，我将向你展示如何用 SymPy 解方程🔥

> **Pro 提示:**如果你看 SymPy 的文档，你有时会看到他们使用 import 语句`from sympy import *`。除了快速测试之外，这是一个坏主意(即使在这种情况下，我也要说它会养成坏习惯)。问题是这样做会覆盖其他包中的函数和类。所以我一直推荐写`import sympy as sp`。

# 4 —解方程

![](img/a31be00cf1ab805c8b56820f95e662a5.png)

照片由[在](https://unsplash.com/@thisisengineering?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上绘制

现在让我们使用 SymPy 符号来解方程:假设你有一个二次多项式`x**2 + 5*x + 6`。怎么能解方程`x**2 + 5*x + 6 = 0`？我们大多数人对于如何手工操作都有一些模糊的记忆，但是你是如何让 SymPy 完成这项工作的呢？以下代码首先创建一个符号`x`，然后创建问题中的等式，最后求解该等式:

```
import sympy as sp# Creating the symbol x
x = sp.symbols("x")# Creates the equation in question
equation = sp.Eq(x**2 + 5*x + 6, 0)# Solving the equation
solution = sp.solveset(equation, x)
```

让我们看看上面的代码创建的变量`solution`:

```
print(type(solution))
**Output:** <class 'sympy.sets.sets.FiniteSet'>print(solution)
**Output:** FiniteSet(-3, -2)
```

如您所见，`solution`变量为我们提供了方程`x**2 + 5*x + 6 = 0`的两个解。SymPy 的 FiniteSet 类型是一个 iterable 类型，所以如果您愿意，您可以遍历它:

```
for num in solution:
    print(num)**Output:** 
-3 
-2
```

如果您想从`solution`变量中提取数字-3 和-2，您可以首先将`solution`转换成一个列表，然后像往常一样进行索引:

```
# Turn solution into a list
solution_list = list(solution)# Access the individual solutions
print(solution_list[0])
**Output:** -3
```

我们刚刚完成了一个简单的二次多项式，如果你愿意的话，你可以用手算出来。但是，SymPy 解方程远比你我强(抱歉不抱歉)。自己尝试更难的东西，看看 SymPy 能承受多少😁

# 5 —有用的功能

关于 SymPy 的一个有趣的事情是它包含了有用的数学函数。这些函数可以开箱即用，您应该熟悉一些最常用的函数。

以下脚本使用 SymPy 中的函数创建了三个不同的方程:

```
import sympy as spx = sp.symbols("x")equation1 = sp.Eq(sp.cos(x) + 17*x, 0)equation2 = sp.Eq(sp.exp(x ** 2) - 5*sp.log(x), 0)equation3 = sp.Eq(sp.sinh(x) ** 2 + sp.cosh(x) ** 2, 0)
```

如果你仔细阅读上面的代码，你会发现

*   `cos`(余弦函数)
*   `sin`(正弦函数)
*   `exp`(指数函数)
*   `sinh`(双曲正弦函数)
*   `cosh`(双曲余弦函数)

配备了这些函数，您可以例如使激活函数用于深度神经网络。例如，众所周知的 *sigmoid 激活函数*可以写成如下形式:

```
sigmoid = 1/(1 + sp.exp(-x))
```

如果要将值代入 sigmoid 函数，可以使用`.subs()`方法。让我们试试这个:

```
print(sigmoid.subs(x, 2))
**Output:** 1/(exp(-2) + 1)
```

嗯。虽然从技术上来说是正确的，但是将该值作为浮点数会更好。为此，您可以使用函数`sp.N()`(N 代表数字):

```
print(sp.N(sigmoid.subs(x, 2)))
**Output:** 0.880797077977882
```

![](img/37ac405221de38b32535702bb20d64b4.png)

照片由 [Aziz Acharki](https://unsplash.com/@acharki95?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 6 —您可以使用 SymPy 做的其他事情

与其详细地漫谈更多的话题，不如让我给你一个清单，你可以检查一下来提高你的交响乐技巧:

*   **导数和积分:** SymPy 可以做你在微积分入门课上学到的大部分事情(除了思考😅).您可以从检查 SymPy 中函数的[微分](https://docs.sympy.org/latest/tutorial/calculus.html)开始。
*   **矩阵和线性代数:** SymPy 可以处理矩阵，从线性代数开始做基本运算。该语法有点类似于 NumPy 使用的语法，但也有区别。首先，请查看 Sympy 中的[预科课程](https://docs.sympy.org/latest/tutorial/matrices.html)
*   简化: SymPy 足够聪明，可以自动对表达式进行一些简化。然而，如果你想对此有更好的控制，那么看看 SymPy 中的[简化](https://docs.sympy.org/latest/tutorial/simplification.html)。
*   **深入探究 SymPy:** 在本质上，SymPy 使用一种基于树的结构来跟踪被称为*表达式树*的表达式。如果你想深入了解 SymPy 的内部工作原理，可以查看一下[表达式树](https://docs.sympy.org/latest/tutorial/manipulation.html)。
*   **与 NumPy 的关系:** NumPy 和 SymPy 都是可以处理数学的库。但是，它们根本不同！NumPy 用数字运算，而 SymPy 用符号表达式。这两种方法都有优点和缺点。幸运的是，有一种聪明的方法可以将 SymPy 表达式“导出”到 NumPy 数组。查看 SymPy 中的 [lambdify 函数](https://docs.sympy.org/latest/tutorial/basic_operations.html#lambdify)了解其工作原理。

# 7 —总结

如果你需要了解更多关于 SymPy 的信息，请查看 SymPy 的文档或我在 SymPy 上的视频系列。

**喜欢我写的？**查看我的博客文章[类型提示](/modernize-your-sinful-python-code-with-beautiful-type-hints-4e72e98f6bf1)、[Python 中的下划线](https://medium.com/geekculture/master-the-5-ways-to-use-underscores-in-python-cfcc7fa53734)、 [5 个很棒的 NumPy 函数](/5-awesome-numpy-functions-that-can-save-you-in-a-pinch-ba349af5ac47)和 [5 个字典提示](/5-expert-tips-to-skyrocket-your-dictionary-skills-in-python-1cf54b7d920d)以获得更多 Python 内容。如果你对数据科学、编程或任何介于两者之间的东西感兴趣，那么请随意在 LinkedIn 上加我，并向✋问好