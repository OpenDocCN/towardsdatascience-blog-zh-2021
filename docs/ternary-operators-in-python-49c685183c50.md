# Python 中的三元运算符

> 原文：<https://towardsdatascience.com/ternary-operators-in-python-49c685183c50?source=collection_archive---------14----------------------->

## 使用三元运算符改进 Python 代码

![](img/82981fb2f657b72e262003b77dc6feef.png)

阿诺·弗朗西斯卡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

对于任何 Python 开发人员来说，编写简短、干净、高效和可读的 Python 代码都应该是优先考虑的事情。三元运算符提供了一种在 Python 中编写条件语句的更短的方法，可以用来实现这一点。

在这个简短的教程中，我们将学习什么是三元运算符，并查看一些如何在您的代码中使用它们的示例。

# 使用 if/else

让我们从下面的 if/else 场景开始，找出哪个数字 **num1** 或 **num2** 是最小值:

```
num1, num2 = 5, 10
min = Noneif num1 > num2:
    min = num2
else:
    min = num1print(min)
# 5
```

> 我们首先将 **num1** 设置为 5，将 **num2** 设置为 10(注意我们如何在一行中将多个值赋给多个变量)。然后我们有一个 if/else 语句。如果 **num1** 大于 **num2** ，则 **min** 变量被赋给 **num2** 。否则， **min** 被分配给 **num1** 。显然，我们没有考虑到 **num1** 和 **num2** 在这里是相等的。

# 三元运算符

三元运算符提供了一种快速测试条件的方法，而不是使用多行 if 语句。因此，通过使用三元运算符，我们可以显著缩短代码(同时保持可读性)，三元运算符的格式如下:

> *x if C else y*

> **顾名思义(三元)，三元运算符由三个操作数(C、x、y)组成。c 是首先被评估为真或假的条件。如果 C 的计算结果为 True，则计算 x 并返回其值。如果 C 的计算结果为 False，则计算 y 并返回其值。**

因此，我们可以使用三元运算符来改进上面的代码，如下所示:

```
num1, num2 = 5, 10
min = num2 if num1 > num2 else num1
```

> 就是这样！ **C** 是我们的条件( **num1 > num2** )，先评估。如果它评估为真，那么 **x** 被评估并且它的值将被返回(并且被分配给变量 **min** )。否则， **y** 被求值并返回其值(并赋给变量 **min** )。在这种情况下， **num1 > num2** 的计算结果为假，因此返回 **num1** 的值。

# 使用带 Return 的三元运算符

我们也可以在函数的返回语句中使用三元运算符。例如，我们可以编写一个函数来检查一个数字是偶数还是奇数，并返回字符串“偶数”或“奇数”:

```
def even_or_odd(num):
    return 'even' if num%2==0 else 'odd'even_or_odd(2)
# 'even'even_or_odd(3)
# 'odd'
```

> 就是这样！ **num%2==0** 是首先评估的条件。如果计算结果为真，则返回“even”。否则，返回“奇数”。

# 额外收获—添加更多条件

我们甚至可以在三元运算符中添加更多的条件。例如，如果我们要返回两个数的最小值，但还要检查它们是否相等，我们可以使用下面的函数来实现:

```
def min(num1, num2):
    return num2 if num1>num2 else (num1 if num1<num2 else 'equal')min(5,10)
# 5min(10,5)
# 5min(5,5)
# 'equal'
```

> 首先评估条件 **num1 > num2** 。如果其评估为**真**，则返回 **num2** 。如果计算结果为 **False** ，将计算 else 后面括号内的表达式，这是另一个三元运算符。在这个 else 表达式中， **num1 < num2** 被求值。如果其评估为**真**，则返回 **num1** 。否则，返回“等于”。

**注意:这不一定是一个好的解决方案，因为它可能会令人困惑，难以阅读。因此，它不是 Pythonic 代码。**

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你注册使用我的 [***链接***](https://lmatalka90.medium.com/membership) *，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

我希望这篇关于三元运算符的简短教程对你有所帮助。感谢您的阅读！