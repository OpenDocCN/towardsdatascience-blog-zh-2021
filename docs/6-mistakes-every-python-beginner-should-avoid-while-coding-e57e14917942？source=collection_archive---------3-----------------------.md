# 每个 Python 初学者在编码时应该避免的 6 个错误

> 原文：<https://towardsdatascience.com/6-mistakes-every-python-beginner-should-avoid-while-coding-e57e14917942?source=collection_archive---------3----------------------->

## 这些小的改进可能是至关重要的

![](img/ce7fb6661a58c2de3e42a8d5caf85107.png)

照片由[龟背竹 发自](https://www.pexels.com/@gabby-k?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[Pexels](https://www.pexels.com/photo/girl-showing-bright-brainteaser-in-hands-5063562/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

Python 是初学者中相当受欢迎的语言，因为它非常容易使用。但是在易于理解的语法和较短的代码背后，有一些每个人都必须注意的微小细节。因为忽略这些细节可能会导致您的代码被破坏，并可能成为您头疼的原因。

本文将讨论每个初学者在用 Python 编码时应该避免的 6 个错误。

# **1。****Python 的版本:**

这应该是 python 程序员主要关心的问题之一，因为全球程序员都在使用大量的 Python 版本。目前使用的两个主要版本是 Python 2。x 和 Python 3。x 它们之间有一些差异，比如:

**10 / 4**

> python 2 中的输出。x 是 2
> 
> python 3 中的输出。x 是 2.5

此外，在打印报表时:

> 打印‘hello world’**# python 2。X**
> 
> print(《hello world》)**# python 3。X**

甚至在 Python 3 的子版本之间也发现了一些差异，比如 Python 3.8 支持 Walrus 操作符(:=)，而 Python 3.7 不支持。Python 3.8 中的 f 字符串也支持“=”。

# **2。**操作系统**操作系统**

编写 python 代码时要记住的另一点是执行代码的操作系统。如果您正在为多个平台编写代码，这一点尤其重要。一个例子是在 Windows 和 Linux 上清除屏幕的命令:

> 对于 Windows:

`os.system(‘cls’)`

> 对于 Linux:

`os.system(“clear”)`

# **3。** **误解 Python 函数**

Python 有大量的函数，它们可能做同样的工作，但以不同的方式执行。所以每个程序员都需要理解一个特定的函数是如何工作的，因为使用错误的函数可能不会给你预期的结果。例如，在 Python 中，sort()和 sorted()函数做同样的事情，即对数组排序。但是它们的工作方式不同:

```
list1 = [6,3,8,1,5]
print(list1.sort())list2 = [9,2,7,6,8]
list2 = sorted(list2)
print(list2)
```

> 输出

```
**None****[2, 6, 7, 8, 9]**
```

在这里，虽然两个列表都是排序的，但是 list1 返回 none，因为排序是就地完成的，而在 sorted(list2)的情况下，会创建一个新列表，并正确打印该列表。

在处理列表时，reverse()和 reversed()函数以及 append()和 extend()函数也是如此。

# **4。** **缩进错误**

Python 对代码中的缩进非常严格，因为它不依赖括号来分隔代码块。这就是为什么即使一个缩进错误也会破坏代码并给你带来意想不到的结果。

当您在同一个代码文件中使用多个函数时，这变得更加严重，因为有时它可能不会给您带来缩进错误，但会成为代码中的一个严重错误。

# **5。** **迭代**时修改列表项

迭代时修改任何列表/集合都不是一个好主意。让我先给你举个例子:

```
a = [1,2,3,4,5,6,7,8,9]#here I want to remove all numbers those are >= 5 from the list
for b in a:
    if b >= 5:
        a.remove(b)
print(a)
```

> 输出

```
[1, 2, 3, 4, 6, 8]
```

这是因为在迭代语句的情况下，机器会多次遍历列表，当机器下次遍历列表时，原始列表可能会给你一个预期的结果。

这个错误可以通过理解列表来避免:

```
a = [1,2,3,4,5,6,7,8,9]
a = [b for b in a if b<5]
print(a)
```

> 输出

```
[1, 2, 3, 4]
```

# **6。** **命名不当**

Python 程序员应该养成在编码时遵循一些约定的习惯，以使代码更具可读性和无错误性。

*   在给一个变量命名时，必须非常小心，因为即使是一个单一的差异也会使变量不同。比如苹果和苹果是两个不同的变量。
*   许多初级程序员不小心命名了他们的函数，这成为其他程序员理解的噩梦。这就是为什么命名一个返回给定数字中最小值的函数而不是给定数字中的最小值是明智的。
*   有时程序员编写的模型名称与 Python 标准库模块冲突。当您试图导入同名的标准库模块时，这可能会导致一些严重的问题。

这里有一本[**关于 Python 编程的书**](https://amzn.to/3jmVmLk) ，我绝对推荐给所有初学者。

# **结论**

这些是初学 Python 的程序员应该知道并不惜一切代价避免的一些错误。虽然其中一些错误看起来很微不足道，但是如果不被注意，它们会在代码中造成大问题。

但好的一面是，如果你练习把这些要点记在脑子里——你可以在很短的时间内改掉这些错误，成为一名更好的开发者。

***注:*** *本文包含附属链接。这意味着，如果你点击它，并选择购买我上面链接的资源，你的订阅费的一小部分将归我所有。*

*然而，推荐的资源是我亲身经历的，并在我的数据科学职业生涯中帮助了我。*

> *在你走之前……*

如果你喜欢这篇文章，并希望**继续关注更多关于 **Python &数据科学**的**精彩文章**——请点击这里[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)考虑成为中级会员。**

请考虑使用[我的推荐链接](https://pranjalai.medium.com/membership)注册。通过这种方式，会员费的一部分归我，这激励我写更多关于 Python 和数据科学的令人兴奋的东西。

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。