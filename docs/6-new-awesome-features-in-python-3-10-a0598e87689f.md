# Python 3.10 中 6 个令人惊叹的新特性

> 原文：<https://towardsdatascience.com/6-new-awesome-features-in-python-3-10-a0598e87689f?source=collection_archive---------1----------------------->

## [提示和技巧](https://towardsdatascience.com/tagged/tips-and-tricks)

## 新的 Python 版本推出了有趣的新特性。

![](img/cdf52c5ff75f03b4a7855093d6075829.png)

由[大卫·克劳德](https://unsplash.com/@davidclode?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 是当今最流行的编程语言之一。它被广泛用于各种领域和应用，从学习计算机科学的基础知识到执行复杂和简单的科学计算任务，再到构建游戏。它甚至被用于数据科学和量子计算等高级领域。

Python 的流行有很多原因。最重要的两个因素是 Python 的通用性，以及与其他编程语言相比，它的易学性。此外，Python 由 Python 软件基金会维护和开发，该基金会一直致力于改进 Python 的新方法。

一周前(2021 年 10 月 4 日)，Python 新版本发布，Python 3.10。在这个新版本中，Python 增加了独特而有价值的特性，同时删除了一些旧特性。我们可以将任何新软件版本中添加或删除的功能分为几类，如语法功能、添加到默认库中或对现有功能的改进。

[](/5-python-books-to-transfer-your-code-to-the-next-level-a5af0981e204) [## 5 本 Python 书籍，让你的代码更上一层楼

towardsdatascience.com](/5-python-books-to-transfer-your-code-to-the-next-level-a5af0981e204) 

Python 3.10 有几个新的很酷的特性，使得使用 Python 的体验更好。在本文中，我将与您分享 6 个新特性和插件，它们是我最感兴趣的，也是我最高兴看到添加到 Python 中的。

# №1:更好的错误跟踪

作为一个每天用 Python 写代码、教编码的人，我深知得到一个语法错误的挫败感。尽管一旦掌握了 Python 和编程，语法错误很容易修复，但有时我们希望有更好的错误消息，可以帮助我们更好地定位错误并节省调试时间。

在 Python 3.10 中，处理错误要好得多，因为有两个特性，更好的错误消息和用于调试的精确行号。例如，让我们考虑下面的代码，其中我们有一个字典和一个函数。然而，在这段代码中，我们忘记了关闭字典。

```
some_dict = {1: "jack", 2: "john", 3: "james" ,
a_results = a_useful_function()
```

在 Python 的早期版本中，错误消息如下所示:

```
File "amazing_code.py", line 3
    a_results = a_useful_function()
              ^
**SyntaxError**: invalid syntax
```

但是，随着新的错误消息和行号的改进，新的错误消息将具有更好的信息，比如错误的确切类型及其精确的行号。

```
File "amazing_code.py", line 1
    expected = {1: "jack", 2: "john", 3: "james" ,
                                                 ^
**SyntaxError**: '{' was never closed
```

这个新特性将有助于加快调试速度，减少初学 Python 的人的挫折感。

# №2:介绍结构模式匹配

如果你使用过 C++之类的其他编程语言，你可能会希望 Python 有 switch 语句，这样你就不用经历冗长的 If，elif，elif，…，else 语句。嗯，Python 3.10 的新特性之一是增加了他们所谓的结构模式匹配，或者换句话说，switch，case 语句，其语法如下。

```
match subject:
    case <patt1>:
        <act1>
    case <patt2>:
        <act2>
    case <patt3>:
        <act3>
    case _:
        <action_default>
```

[](/6-best-python-ides-and-text-editors-for-data-science-applications-6986c4522e61) [## 数据科学应用的 6 个最佳 Python IDEs 和文本编辑器

towardsdatascience.com](/6-best-python-ides-and-text-editors-for-data-science-applications-6986c4522e61) 

# №3:新型并集运算符

虽然 Python 是一种动态类型的编程语言，但是有一些方法可以使它的某些部分成为静态类型。例如，如果你正在编写一个函数，而属性的类型对于函数内部的交换是很重要的。在以前的版本中，您可以指定属性的类型，例如:

```
**def** func(num: int) -> int:
    **return** num + 5
```

但是，如果您想接受两种类型，那么您需要使用 Union 关键字。

```
**def** func(num: Union[int, float]) -> Union[int, float]:
    **return** num + 5
```

在 Python 的新版本中，您可以在两种类型之间进行选择，使用|操作符代替 Union 进行更直接的类型决定。

```
**def** func(num: int | float) -> int | float:
    **return** num + 5
```

# №4:其他酷功能

## 4.1 更严格的拉链设计

Python 中有趣的 Python 函数之一是`zip()`函数，这是 Python 中的一个内置函数，允许您组合和迭代来自几个序列的元素。在以前的版本中，您可以对不同长度的序列使用`zip` ，但是现在，引入了一个新的参数*strict*来检查传递给 zip 函数的所有 iterables 是否长度相同。

[](/9-free-quality-resources-to-learn-and-expand-your-python-skills-44e0fe920cf4) [## 9 个免费的优质资源来学习和拓展您的 Python 技能

### 不管你的技术背景如何，都要学习 Python。

towardsdatascience.com](/9-free-quality-resources-to-learn-and-expand-your-python-skills-44e0fe920cf4) 

## 4.2 自动文本编码

作为一名程序员，我们会遇到或说这样一句话“它在我的机器上工作！”。一个代码在一台机器上可以工作，而在另一台机器上却不行，原因有很多；文本编码会导致这样的错误。

在 Python 的早期版本中，如果没有明确声明编码类型，首选的本地编码可能会导致代码在其他机器上失败。在 Python 3.10 中，当用户打开没有特定编码类型的文本文件时，可以激活警告来通知用户。

## 4.3 异步迭代

一个强大而高级的编程范例是异步编程，从 3.5 版本开始，异步编程就是 Python 的一部分。在 Python 3.10 中，有两个新的异步内置函数`[aiter()](https://docs.python.org/3.10/library/functions.html#aiter)`和`[anext(](https://docs.python.org/3.10/library/functions.html#anext))`，让你的代码更具可读性。

# 最后的想法

当我在读本科学位时，我上过几门课，在这些课上我们使用 C++或 Java 来编写代码和实现应用程序。但是，到了写毕业论文的时候，我决定学习和使用 Python。那是快十年前的事了，我再也没有回头；此后，每当我处理问题时，Python 就成了我的首选编程语言。

然后，我开始教孩子们计算机科学，我意识到 Python 可以激励年轻一代去追求技术职业。除了用 Python 编写或阅读代码的简单性以及开始用 Python 实现代码的速度之外，我最喜欢这种编程语言的一点是 Python 软件基金会如何努力保持 Python 的相关性。

[](/the-5-certificate-to-prove-your-python-knowledge-level-c1b3b74e6d6b) [## 证明你 Python 知识水平的 5 个证书

### 有时候，拥有一个证书可能是你需要的验证

towardsdatascience.com](/the-5-certificate-to-prove-your-python-knowledge-level-c1b3b74e6d6b) 

随着 Python 的每一个新版本，都添加了令人难以置信的新特性，这些特性是大多数用户一直要求的，这些特性将使我们用 Python 编写代码更有效，并使人们更容易进入编程——一般而言——特别是 Python。在这篇文章中，我与您分享了 Python 3.10 中的 6 个新特性，这些特性是我最感兴趣的，我可以和我的学生一起使用。