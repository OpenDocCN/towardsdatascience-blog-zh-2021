# 你应该知道的关于 Python 函数的三个技巧

> 原文：<https://towardsdatascience.com/three-tricks-on-python-functions-that-you-should-know-e6badb26aac2?source=collection_archive---------20----------------------->

## Python 基础

## 快速浏览一些可以提高你编程技能的技巧:嵌套函数、可变参数和 lambda 函数。

![](img/82d64ad86b3d2ee52be37292ff9bb05e.png)

照片由[沙哈达特·拉赫曼](https://unsplash.com/@hishahadat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

本教程涵盖了 Python 函数的以下三个高级编程技巧:

*   嵌套函数
*   可变参数
*   λ函数

# 嵌套函数

嵌套函数是另一个函数中的函数。由于作用域规则，通常不能在容器函数之外调用嵌套函数。

**当重复操作应该在函数内部运行且只能在函数内部运行时，可以使用嵌套函数。**以下示例定义了一个函数，该函数接收两个字符串作为输入，对它们进行操作并返回它们。

```
def manipulate_strings(a,b):

    def inner(s):
        s = s.lower()
        return s[::-1]

    return inner(a), inner(b)
```

在两个字符串上测试函数:

```
a = "HELLO"
b = "WORLD"
manipulate_strings(a,b)
```

它给出了以下输出:

```
('olleh', 'dlrow')
```

**嵌套函数也可以由外部函数返回。**

考虑下面这个平凡的函数，它接收一个数字作为输入，并返回一个函数，该函数将一个字符串转换为小写，然后，如果字符串长度大于 n，它将该字符串截断为 n-1。函数返回内部函数。

```
def manipulate_string(n):

    def inner(a):
        a = a.lower()
        if n < len(a):
            a = a[:n]
        return a

    return inner
```

现在，我可以调用函数并将返回值赋给一个变量，该变量将包含内部函数。

```
manipulate = manipulate_string(3)
```

然后，我可以用不同的字符串调用内部函数:

```
a = "HELLO"
manipulate(a)
```

它给出了以下输出:

```
'hel'
```

前面的例子演示了当您有一些通用参数时，如何使用嵌套函数，这些参数可以由外部函数初始化，然后可以在内部函数中使用特定的参数。

# 可变参数

通常一个函数被调用时有固定数量的参数，包括默认参数。然而，Python 提供了一种机制，允许调用具有潜在无限数量参数的函数。

有两种类型的可变参数:

*   元组(项目列表)—作为*参数传递，例如`*args`
*   字典(键值对)—作为**参数传递，例如`**kargs`。

下面的例子展示了如何通过利用`*args`来连接可变数量的字符串:

```
def concatenate(*args):
    output = ''
    for item in args:
        output = output + item
    return output
```

现在我用可变数量的参数测试这个函数。有两个参数:

```
concatenate('dog', 'cat')
```

它给出了以下输出:

```
'dogcat'
```

有三个参数:

```
concatenate('green', 'red', 'yellow')
```

它给出了以下输出:

```
'greenredyellow'
```

下面的例子展示了如何利用`**kargs`参数。我定义了一个类`Configuration`，包含三个参数:alpha，beta，gamma。该类提供了一个名为`configure()`的方法，它可以接收数量可变的参数作为输入，这些参数对应于该类的配置参数。用户可以决定是设置所有配置参数还是仅设置其中的一部分。

```
class Configuration:

    def __init__(self):
        self.p = {}
        self.p['alpha'] = None
        self.p['beta'] = None
        self.p['gamma'] = None

    def configure(self,**kargs):
        for k,v in kargs.items():
            self.p[k] = v

    def print_configuration(self):
        for k,v in self.p.items():
            print(k + ': ' + str(v))
```

类`Configuration`还提供了一个名为`print_configuration()`的方法，它打印实例的当前状态。

我可以创建一个`Configuration()`对象，然后我可以决定，例如，只设置 alpha 参数:

```
config = Configuration()
config.configure(alpha = 2)
```

我打印当前配置以确保 alpha 参数已经设置好:

```
config.print_configuration()
```

它给出了以下输出:

```
alpha: 2
beta: None
gamma: None
```

现在我可以设置α和β参数:

```
config.configure(alpha = 2, beta = 4)
config.print_configuration()
```

它给出了以下输出:

```
alpha: 2
beta: 4
gamma: None
```

# λ函数

lambda 函数是一个内联函数，可用于运行简单和重复的运算，如众所周知的数学运算。

以下代码片段显示了如何通过 lambda 函数计算勾股定理:

```
from math import sqrt
pythagora = lambda x,y : sqrt(x**2 + y**2)
```

我可以如下测试该功能:

```
pythagora(3,4)
```

它给出了以下输出:

```
5.0
```

# 摘要

在本教程中，我展示了 Python 函数的一些技巧，包括嵌套函数、可变参数和 lambda 函数。

你可以从我的 [Github 库](https://github.com/alod83/data-science/tree/master/Basics)下载本教程的完整代码。

为了保持对我工作的更新，你可以在 [Twitter](https://twitter.com/alod83) 、 [Github](https://github.com/alod83) 、 [Youtube](https://www.youtube.com/alod83) 或我的[新网站](https://www.alod83.com/)上关注我。

# 相关文章

[](https://medium.com/analytics-vidhya/basic-statistics-with-python-pandas-ec7837438a62) [## python 熊猫的基本统计数据

### 入门指南

medium.com](https://medium.com/analytics-vidhya/basic-statistics-with-python-pandas-ec7837438a62) [](/how-to-load-huge-csv-datasets-in-python-pandas-d306e75ff276) [## 如何在 Python Pandas 中加载巨大的 CSV 数据集

### 可能会出现这样的情况，您的硬盘中有一个巨大的 CSV 数据集，占用了 4 或 5gb(甚至更多),而您…

towardsdatascience.com](/how-to-load-huge-csv-datasets-in-python-pandas-d306e75ff276) [](https://medium.com/geekculture/the-top-25-python-libraries-for-data-science-71c0eb58723d) [## 面向数据科学的 25 大 Python 库

### 你一生中至少应该尝试一次的 Python 库列表。

medium.com](https://medium.com/geekculture/the-top-25-python-libraries-for-data-science-71c0eb58723d) 

# 新到中？您可以每月订阅几美元，并解锁无限的文章— [单击此处](https://alod83.medium.com/membership)。