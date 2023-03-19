# Python 现在支持 Switch 语句——下面是入门方法

> 原文：<https://towardsdatascience.com/python-now-supports-switch-statements-heres-how-to-get-started-fe7da830236a?source=collection_archive---------3----------------------->

## 了解如何在 Python 3.10 中使用结构化模式匹配实现 Switch 语句

![](img/4f439120254d7857c2c8143db4d4e624.png)

照片由[罗伯特·威德曼](https://unsplash.com/@antilumen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/bulb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Python 3.10 仍处于 alpha 阶段，但会带来一些令人兴奋的新特性。我们今天将看看其中的一个——switch 语句——正式名称为*结构模式匹配*。

Switch 语句在大多数编程语言中都很常见，它提供了一种更简洁的实现条件逻辑的方式。当有很多条件需要评估时，它们就派上用场了。

今天，我们将看看如何使用它们，并比较与更传统的方法的代码差异。

这篇文章的结构如下:

*   实现条件运算的旧方法
*   python 3.10 Way-Switch 语句
*   最后的想法

# 实现条件运算的旧方法

我们会掩护两个。第一个是标准的 if-elif-else 语句，另一个将使用字典键值映射来完全避免 if 语句。

首先，我们需要一些代码。我们将声明一个名为`get_mood(day: str) -> str`的函数，它返回一个值取决于输入参数的字符串。随着周末的临近，返回值变得更加令人兴奋。愚蠢的小功能，但将做演示的目的。

让我们用经典的`if`方法来实现它。代码如下:

没有什么新的或开创性的。代码很容易理解，但是太冗长，尤其是当多个值可以满足一个条件时。

我们可以通过完全避免 if 语句并以键值映射的形式编写函数来“改进”或混合一些东西。这还包括一个用于设置默认返回值的`try except`块。

下面是代码片段:

如您所见，结果是相同的，但是我们没有使用条件运算符。这两种方法都可以很好地工作，但是 Python 一直缺少的是一个专用的`switch`语句。

3.10 版本解决了这个问题。

# python 3.10 Way-Switch 语句

根据[官方文件](https://docs.python.org/3.10/whatsnew/3.10.html):

> 增加了结构模式匹配，形式为带有关联动作的模式的*匹配语句*和 *case 语句*。模式由序列、映射、原始数据类型以及类实例组成。模式匹配使程序能够从复杂的数据类型中提取信息，对数据结构进行分支，并基于不同形式的数据应用特定的操作。

我们今天将坚持基础知识，并在其他时间探索结构模式匹配必须提供的一切。

让我们回到我们的`get_mood()`函数，用类似于`switch`语句的语法重写它。与许多其他编程语言不同，Python 使用了`match`关键字，而不是`switch`。`case`关键字是相同的。

下面是代码片段:

概括一下:

*   使用`case`关键字评估条件(`case ‘Monday’`与`if day == ‘Monday’`相同)
*   使用管道运算符`|`分隔多个条件，例如，如果两个输入值应该产生相同的返回值
*   使用下划线运算符— `_` —指定默认大小写。

这就是你的结构模式匹配——至少是最基本的。这只是 Python 3.10 即将推出的令人兴奋的新特性之一。

# 最后的想法

Python 3.10 将带来许多令人兴奋的特性，但它仍处于 alpha 阶段。因为它还不能用于生产，所以将其作为默认 Python 版本安装可能不是一个好主意。

今天我们已经介绍了可能是最激动人心的新特性，但是其他的也将在接下来的文章中介绍。请继续关注博客，了解更多信息。

*喜欢这篇文章吗？成为* [*中等会员*](https://medium.com/@radecicdario/membership) *继续无限制学习。如果你使用下面的链接，我会收到你的一部分会员费，不需要你额外付费。*

[](https://medium.com/@radecicdario/membership) [## 通过我的推荐链接加入 Medium-Dario rade ci

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@radecicdario/membership) 

# 了解更多信息

*   [我作为数据科学家卖掉我的 M1 Macbook Pro 的三大理由](/top-3-reasons-why-i-sold-my-m1-macbook-pro-as-a-data-scientist-abad1226f52a)
*   [如何用 Cron 调度 Python 脚本——你需要的唯一指南](/how-to-schedule-python-scripts-with-cron-the-only-guide-youll-ever-need-deea2df63b4e)
*   [Dask 延迟—如何轻松并行化您的 Python 代码](/dask-delayed-how-to-parallelize-your-python-code-with-ease-19382e159849)
*   [如何用 Python 创建 PDF 报告——基本指南](/how-to-create-pdf-reports-with-python-the-essential-guide-c08dd3ebf2ee)
*   [2021 年即使没有大学文凭也能成为数据科学家](/become-a-data-scientist-in-2021-even-without-a-college-degree-e43fa934e55)

# 保持联系

*   在 [Medium](https://medium.com/@radecicdario) 上关注我，了解更多类似的故事
*   注册我的[简讯](https://mailchi.mp/46a3d2989d9b/bdssubscribe)
*   在 [LinkedIn](https://www.linkedin.com/in/darioradecic/) 上连接