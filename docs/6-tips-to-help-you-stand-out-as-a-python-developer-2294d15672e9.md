# 帮助你脱颖而出成为 Python 开发者的 6 个技巧

> 原文：<https://towardsdatascience.com/6-tips-to-help-you-stand-out-as-a-python-developer-2294d15672e9?source=collection_archive---------28----------------------->

## Python 开发人员和数据科学家的实用技巧

![](img/db3f45f5e01a437ddbc7ed89a1364ceb.png)

[粘土银行](https://unsplash.com/@claybanks)在 [Unsplash](https://unsplash.com/photos/NiYS_ExTdg8) 拍摄的照片

Python 是使用最广泛的编程语言之一，随着时间的推移，它还在不断流行。这个领域的竞争越来越激烈，因为越来越多的人想要精通这门语言，并在这个领域找到一份好工作。因此，为了能够与其他人竞争并在 Python 开发人员或数据科学家的职业生涯中取得进步，不断学习和提高您的编码技能是非常重要的。

在本文中，我将讨论一些技巧，这些技巧最终会帮助你成为一名出色的 Python 程序员，方法是使用有助于你的代码看起来更 Python 化、更专业的技术。此外，我们将讨论一些您可以合并到您的工作流中的工具，这些工具将帮助您以及您当前工作的整个开发团队提高开发速度。

## 尽可能遵循 PEP-8 风格指南

**P**ython**E**n 增强 **P** roposal (PEP) **8** 介绍 Python 代码的风格指南。该风格指南包括鼓励开发人员遵循的编码约定，以便编写符合特定标准的更简洁的代码。

您应该意识到，源代码被读取的频率比它实际被编写的频率要高得多。因此，以一致的方式编写清晰可读的代码非常重要，这将帮助您、更广泛的团队以及未来的程序员理解、修改或发展代码库。

PEP-8 风格指南包含许多建议，主要集中在缩进、最大行长度、模块导入、间距、注释、命名约定等方面。有关完整的建议列表，您可以参考[特定 PEP 页面](https://www.python.org/dev/peps/pep-0008/)。

值得一提的是，项目本身可以有自己的指南。如果项目特定指南与 PEP-8 风格指南相冲突，则前者优先。此外，请务必关注 PEP-8 风格指南，因为它会随着时间的推移引入更多的约定，并删除由于语言本身的进步而被认为过时的过去的约定。

## 更频繁地使用上下文管理器

Python 通过让您处理资源(例如文件)的`with`语句来实现上下文管理器。每个上下文管理器实现两部分功能；`__enter__()`在语句体之前执行，而`__exit__()`在语句结束后立即执行。

上下文管理器用于简化与资源管理相关的常见操作，因此开发人员只需编写较少的代码。此外，它们还有助于避免 Python 应用程序中的资源泄漏。

举个例子，假设你想在一个文件中写一个字符串。如果不使用上下文管理器，代码可能类似于下面给出的代码:

使用 Python 将字符串“Hello World”写入 test.txt 文件

下面提供了使用`with`上下文管理器的等价代码。这里的主要优点是`with`语句将确保文件被正确关闭。正如您所看到的，上下文管理器可以通过消除样板代码来帮助我们编写更干净、更 Pythonic 化的代码，否则我们必须自己编写样板代码。

使用上下文管理器写入文本文件

在适用的地方使用上下文管理器是一个很好的实践。您还应该注意，如果您的项目上下文需要，您甚至可以编写自己的上下文管理器。

## 使用理解

你可能必须考虑掌握的另一个好的实践是在 for 循环中使用理解。理解是可以用来从现有的序列中创建新序列的结构。Python 2 引入了列表理解，而 Python 3 扩展了这个概念，因此理解也可以用于集合和字典。

一般来说，理解被认为是 Pythonic 式的，必须优先于传统的 for 循环。除了创建新的序列，理解也可以用于其他上下文，如过滤或映射。

为了证明理解的强大，让我们考虑下面的例子，我们想要创建一个包含 0 到 10 范围内的偶数整数的列表。传统的 for 循环如下图所示。

使用列表理解的等效情况如下

正如你所看到的，列表理解的符号更加清晰，并且使事情变得简单。

## 保持简单！

Python 最强大的地方可能是它能够让开发人员编写简单干净的代码。这可能是具有其他语言背景的开发人员最容易陷入的陷阱之一。

![](img/4fe2425178514045c4732acc464b854c.png)

照片由[阿曼达·琼斯](https://unsplash.com/@amandagraphc)在 [Unsplash](https://unsplash.com/photos/oHVdj31R3F4) 上拍摄

它的动态典型模型提供了灵活性，让您不必编写冗长的代码就能保持简单。[**P**ython**E**enhancement**P**roposal(PEP)20](https://www.python.org/dev/peps/pep-0020/)是关于 Python 设计的指导原则，以格言的形式表达出来。所谓 Python 的**禅**，蒂姆·彼得斯，下面给出。**一定要坚持！**

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

如果您想了解更多关于 Python 的动态类型模型，请阅读下面的文章。

[](/dynamic-typing-in-python-307f7c22b24e) [## Python 中的动态类型

### ..是语言灵活性的根源

towardsdatascience.com](/dynamic-typing-in-python-307f7c22b24e) 

## 使用 sphinx 自动生成文档

优秀的开发人员还必须编写高质量的文档——句号。如果您不能正确地记录您的工作，那么即使您花费了巨大的努力来编写最先进的源代码，它也只是变得毫无用处。

高质量的文档帮助其他人理解如何使用你的作品。这使得更广泛的团队在将来需要时更容易维护甚至重构代码。因此，使用能够帮助您自动生成文档的工具是非常重要的，这样您可以在这样做的时候节省时间，但同时不要牺牲文档的质量。

Sphinx 是一个强大的工具，它可以帮助您根据添加到源代码中的注释自动生成文档。它有许多很棒的特性，而且它还支持几个扩展，可以让你的生活更加轻松。

## 使用提交前挂钩

如果你是一个有经验的开发人员，我想当然地认为你已经使用了版本控制系统(VCS ),比如 Git。**预提交挂钩**肯定是在创建提交之前触发的动作集合。它们经常被用来确保代码符合某些标准。例如，您可以触发一个预提交钩子，确保您的 Python 源代码的代码风格与我们之前讨论的 PEP-8 风格指南保持一致。

预提交挂钩是方便的工具，可以帮助您确保您交付的代码是高质量的，最重要的是一致的。它们还可以通过自动化某些原本需要手动执行的操作来帮助您提高开发速度。

要更全面地了解 Python 中的预提交钩子，请阅读我下面的文章。

[](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) [## 使用预提交挂钩自动化 Python 工作流

### 什么是预提交钩子，它们如何给你的 Python 项目带来好处

towardsdatascience.com](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) 

## 结论

在本文中，我们介绍了一些技巧，这些技巧有可能帮助您成为一名出色的 Python 开发人员。由于越来越多的人热衷于精通 Python 编程语言，因此了解最新的实践和工具非常重要。这样做，将有助于你在职场中与人竞争，在职业生涯中取得进步，并获得你想要的工作。

具体来说，我们讨论了在编写代码或从事项目时应该考虑的六个有用的行为。讨论主要集中在:

*   PEP-8 风格指南
*   什么是上下文管理器，为什么要使用它们
*   理解的重要性
*   编写简单代码的重要性
*   如何为你的源代码自动生成文档
*   最后，预提交钩子的作用和重要性