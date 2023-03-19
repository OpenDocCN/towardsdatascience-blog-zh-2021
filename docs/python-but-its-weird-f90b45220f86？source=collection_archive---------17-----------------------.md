# Python 但是很奇怪

> 原文：<https://towardsdatascience.com/python-but-its-weird-f90b45220f86?source=collection_archive---------17----------------------->

## **质疑你的 Python 技能的代码片段**

![](img/9a5c977a57f4438e85859df7fc650a86.png)

由[vini cius“amnx”Amano](https://unsplash.com/@viniciusamano?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我们都爱 Python！长期使用这种语言后，如果深入概念，你会对这种语言的模块化感到惊讶。根据我的经验，我发现使用 Python、一点 PowerBI 和基于它的 SQL 的组合很容易实现我的大部分数据分析工作。

但是在这么多的特性中，有一些非常奇怪的 Python 概念和代码，您在编程过程中可能会遇到，也可能不会遇到。也许它被跳过了，或者你假设它对 Python 来说是显式正确的，没有任何解释。让我举一些例子。

# 递增/递减运算符在 Python 中有效吗？

![](img/4be1f34b515112ec8ac010d5208098f4.png)

沃洛季米尔·赫里先科在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在像 C/C++/Java/Go 这样的编程语言中，我们有增量(++)和减量运算符(-)，它们将变量的值更新 1。这些是在循环中迭代时常用的。

如果您尝试在 Python 中使用这些操作符，您将得到以下结果:

```
>>> a = 10
>>> print(++a)
>>> 10
>>> print(--a)
>>> 10
>>> print(a++)
>>> SyntaxError: invalid syntax
>>> print(a--)
>>> SyntaxError: invalid syntax
```

后递增/递减返回语法错误，因为 Python 语法中没有定义这种类型的语法。但是前递增/递减运算符呢？即使这些操作符返回概念上错误的答案，我们如何得到它们的结果？

实际上，双加号(++)被视为+(+a)，它被解析回(a ),这就是它没有引发任何错误的原因。而是返回“a”的原始值。同样的解释也适用于(- -a)。

# 是否正确使用了 round()函数？

![](img/e629a275b917e26497ca024d9017d4f9.png)

照片由 [Varun Gaba](https://unsplash.com/@varunkgaba?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

尝试在不使用 Python IDE 的情况下评估以下内容:

```
>>> print(round(12.5) - round(11.5))
```

很可能，您会将值 12.5 舍入为 13，将 11.5 舍入为 12，并将最终答案舍入为 1。这个答案在 Python 3.x 中是错误的！

Python 的 3.x round 函数是使用银行家或收敛舍入实现的，这与 5 的边缘情况不同。大于 5 的值向上舍入和小于 5 的值向下舍入的通常规则保持不变。对于 5 的边缘情况，它将数字舍入到最接近的偶数。

这意味着 12.5 将被舍入到 12，因为它是最接近的偶数，而 11.5 也将被舍入到 12，导致最终答案为 0。所以，在这种情况下使用 round 函数！

# 海象操作员行为

![](img/5fadbad8f51de07f48c1a988af158bd8.png)

杰伊·鲁泽斯基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

随着 Python 3.8 的发布，一个新的运营商进入了人们的视线，walrus operator。根据官方文档，它是一种赋值表达式，用于循环、表达式和更多实例中。它允许您在同一个表达式中赋值和返回值。

所有可能的实例都在官方版本中讨论过，你可以在 [PEP 572](https://www.python.org/dev/peps/pep-0572) 上找到。我在网上发现的一个有趣的用例将在下面讨论，在这里我输入数字，直到输入为-1。

在一般情况下，代码如下所示:

但是，使用 walrus 运算符，它将被简化为:

如果您试图在程序中使用 walrus 操作符作为赋值操作符，您将会得到一个语法错误:

```
>>> a := func(x)
>>> SyntaxError: invalid syntax
```

是因为海象“无粘性赋值表达式”被限制在顶级。通过将表达式括在括号中，仍然可以使用 walrus 运算符赋值。这将是一个有效的表达式，但不建议使用。

```
>>> (a := func(45))
```

# 循环引用

![](img/83abb2a9eeb06959198669cb0fdb0ec1.png)

马特·西摩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

这个概念真的让我大吃一惊，我都不知道 Python 语言里有没有这样的东西！

假设您将一个列表声明为:

```
>>> some_list = some_list[0] = [0, 1]
```

1.  这里，已经完成了三项任务。在这种情况下，先取最左边和最右边，然后取剩余的赋值。因此，some_list 变量保存值[0，1]。
2.  现在有趣的部分，some_list[0]将被分配给[0，1]。您可能认为它应该抛出一个错误，但是由于 some_list 变量已经被赋值，所以它不会引发任何错误。
3.  但是，我们也知道[0，1]首先被指定为 some_list。
4.  这意味着 some_list[0]只想引用自身。即使赋予了自我价值，它也会再次指向自身。
5.  这就产生了无限引用，称为循环引用。
6.  Python 通过将这个无限引用值指定为省略号(…)来处理这个问题。是啊！如果您打印出 some_list 变量，您将看到这个值。

```
>>> [[...], 1]
```

即使在我们的例子中尝试打印 some_list[0],也会得到如上所示的相同值。它将保持相同的价值为任何数量的水平！你可以尝试另一个非常著名的复杂例子:

```
>>> a, b = a[b] = {}, 5
>>> print(a)
```

这里也是，最初，a 和 b 被分配各自的值为{}和 b。对于 a[b]，它将 b 值 5 分配为字典的键，但是该值指向正在更新的字典 a。因此，key 5 将保存一组省略号和值 5。

```
{5: ({…}, 5)}
```

# 结论

Python 中有很多值得关注的东西。其中一些被设计成以定义的方式发生，其他的只是由社区发现的复活节彩蛋。诸如字符串实习、GIL 限制、基于等价的字典键散列、“is”操作符用法等等。您可以通过访问这个 [GitHub 资源库](https://github.com/satwikkansal/wtfpython)查看整个列表，它是本文中讨论的所有内容的主要来源。

我希望你喜欢这篇文章，如果你喜欢，一定要表示你的支持。如果有任何疑问、疑问或潜在的机会，您可以通过 [LinkedIn](https://www.linkedin.com/in/kaustubh-gupta/) 、 [Twitter](https://twitter.com/Kaustubh1828) 或 [GitHub](https://github.com/kaustubhgupta) 联系我。

**热门文章—**

*   **100k 观点俱乐部**

[](/building-android-apps-with-python-part-1-603820bebde8) [## 用 Python 构建 Android 应用程序:第 1 部分

### 使用 Python 构建 Android 应用程序的分步指南

towardsdatascience.com](/building-android-apps-with-python-part-1-603820bebde8) 

*   **50K 观点俱乐部**

[](/3-ways-to-convert-python-app-into-apk-77f4c9cd55af) [## 将 Python 应用程序转换为 APK 的 3 种方法

### 结束构建 Python 系列的 Android 应用程序！

towardsdatascience.com](/3-ways-to-convert-python-app-into-apk-77f4c9cd55af) [](/run-python-code-on-websites-exploring-brython-83c43fb7ac5f) [## 在网站上运行 Python 代码:探索 Brython

### Python 中的 JavaScript 等效脚本

towardsdatascience.com](/run-python-code-on-websites-exploring-brython-83c43fb7ac5f) 

**参考文献—**

1.  [https://github.com/satwikkansal/wtfpython](https://github.com/satwikkansal/wtfpython)
2.  [https://www.python.org/dev/peps/pep-0572/](https://www.python.org/dev/peps/pep-0572/)