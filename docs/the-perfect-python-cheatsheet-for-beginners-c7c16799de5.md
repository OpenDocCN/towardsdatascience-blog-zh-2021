# 初学者的完美 Python 备忘单

> 原文：<https://towardsdatascience.com/the-perfect-python-cheatsheet-for-beginners-c7c16799de5?source=collection_archive---------19----------------------->

## 备忘单是有抱负的早期职业数据科学家的救命稻草。

![](img/349c2b7f27f2190e9070c96a60a35b6c.png)

照片由[蔡文旭](https://unsplash.com/@tsaiwen_hsu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/life-buoy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

Python 是成为数据科学家可以学习的最简单的编程语言之一(另一种语言是 R)，有大量免费的在线资源可以帮助您入门。学习 Python 成为数据科学家的过程可以**大致**分为三个科目:学习编程语言(通常是 Python)、数据分析(数据可视化、数学和统计)和机器学习(通过经验自动改进的算法)。

然而，第一步，学习如何编程可能需要太长时间，一些有抱负的数据科学家可能会对实现他们的专业目标感到气馁。为什么？他们希望通过数据分析和洞察来解决问题。你不想花几百个小时在重复的 Python 练习上来达到完美；你想直接投入行动。那么，如何在不完善每一个 Python 命令的情况下推进和解决现实生活中的数据问题呢？

嗯，最常见的方法是依靠**堆栈溢出**。但是，在数据科学之旅的开始，人们可能并不总能找到他们期望的答案。这是因为 Stack Overflow 上发布的大多数解决方案都是由专家或经验丰富的数据科学家提供的，他们忘记了从头开始是什么样子。不幸的是，这是一种被称为知识诅咒的认知偏见，在试图与不同背景的人交流的专家中很常见。我经常看到一些简单问题的不必要的高级编程答案。因此，数据科学专业的学生将花费大量时间寻找一个初学者友好的解决方案，而不是致力于最重要的事情；解决问题。

因此，为了帮助你[变得更有效率](/increase-productivity-data-cleaning-using-python-and-pandas-5e369f898012?sk=937d5e144e877ae6e73cb72ba395e5f0)和[花更少的时间](https://medium.com/better-programming/save-time-using-the-command-line-glob-patterns-and-wildcards-17befd6a02c8?sk=ba92dbe1a1cab10e7bca14aabaff3b07)寻找简单的答案，我决定为数据科学学生和早期职业数据专业人员分享一份全面的 Python 备忘单。我希望下面的备忘单能让你把时间投入到解决问题和产生数据洞察力上。

# **内容:**

1.  **集合**(列表、字典、范围&列举)
2.  **类型**(字符串、正则表达式、数字、日期时间)
3.  **语法** (Lambda，Comprehension，Map Filter & Reduce，If-Else)
4.  图书馆(熊猫)
5.  **下一步去哪里？**

# **1。收藏**

## **列表**

列表是一个**有序**和**可变**容器；它可以说是 Python 中最常见的数据结构。当您处理数据清理和创建 for 循环时，理解列表的工作方式变得更加重要。

## **字典**

字典是 Python 中的一种数据结构，它使用键进行索引。字典是一个无序的条目序列(键值对)，对于数据科学家来说至关重要，尤其是那些对网络搜集感兴趣的人。例如:从 YouTube 频道中提取数据。

## **范围&枚举**

Range 函数返回一系列数字，默认情况下递增 1，并在指定的数字前停止。enumerate 函数接受一个集合(一个元组),并向枚举对象添加一个计数器。这两个函数在 for 循环中都很有用。

# **2。类型**

## **字符串**

字符串是包含一系列字符的对象。字符串方法总是返回新值，并且**而不是**会改变原始字符串。

不要忘记使用一些其他的基本方法，如`lower()`、`upper()`、`capitalize()`和`title()`。

## **正则表达式(Regex)**

正则表达式是描述搜索模式的字符序列。在 Pandas 中，正则表达式与矢量化字符串方法集成在一起，使得查找和提取字符模式变得更加容易。学习如何使用 [Regex](https://docs.python.org/3.4/library/re.html) 让数据科学家减少数据清理的耗时。

除非使用`**flags=re.ASCII**`参数，否则默认情况下，任何字母表中的空格、数字和字母数字字符都将匹配。同样，用大写字母表示否定。

## **数字**

**数学&基础统计**

Python 有一个内置模块，数据科学家可以将其用于数学任务和基本统计。然而，`**describe()**`函数计算 DataFrame 列的统计信息的汇总。

## **日期时间**

模块**日期时间**提供日期`**d**`、时间`**t**`、日期时间`**dt**`和时间增量`**td**`类。这些类是不可变的和可散列的。这意味着它的值不会改变。因此，它允许 Python 创建一个惟一的哈希值，并被字典用来跟踪惟一的键。Datetime 模块对于经常遇到显示“购买时间”或“用户在特定页面上花了多长时间”的数据集的数据分析师来说至关重要

# **3。语法**

**λ**

Python lambda 函数是一个匿名函数，其工作方式就像普通的带参数的函数一样。当数据科学家只需要使用一次函数**而不想编写整个 Python 函数`**def**` **时，它们非常方便。****

****理解****

**理解是一行代码，允许数据专业人员从可重复的源(如列表)创建列表。它们非常适合于简化 for 循环和`**map()**`，同时解决数据科学中的一个基本前提:“可读性很重要。”所以，尽量不要让你的清单/字典理解过于复杂。**

****如果-否则****

**数据科学家使用 **if-else** 语句，仅在满足特定条件时执行代码。**

# ****4。库****

**图书馆是数据科学家的救星。其中一些库非常庞大，是专门为满足数据专业人员的需求而创建的。因为 Numpy 和 Pandas 都允许数据科学家使用多种基本功能，下面，您会发现一些对初学者和早期职业数据科学家有用的基本功能。**

**![](img/2cc9fdd44c96b68473528ea960ed5bd8.png)**

**迈克尔·D·贝克与在 [Unsplash](https://unsplash.com/s/photos/libraries?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的合影**

## ****NumPy****

**Python 是一种高级语言，因为不需要手动分配内存或指定 CPU 如何执行某些操作。一种低级语言，如 ***C*** 为我们提供了这种控制，并提高了特定代码的性能(在处理大数据时至关重要)。NumPy 让 Python 变得高效的原因之一是因为矢量化，它利用了**单指令多数据** ( [SIMD](https://en.wikipedia.org/wiki/SIMD) )来更快速地处理数据。当您将线性代数应用到您的机器学习项目时，NumPy 将变得非常有用。**

**请记住，NumPy 中的列表被称为 1D Ndarray，而 2D Ndarray 是列表的列表。NumPy ndarrays 使用行和列的索引，这是选择和分割值的方式。**

## **熊猫**

**尽管 NumPy 提供了重要的结构和工具，使数据处理变得更加容易，但它也有一些限制:**

*   **因为不支持列名，NumPy 强迫你框架问题，这样你的答案永远是多维数组操作。**
*   **如果每个 ndarray 只支持一种数据类型，那么处理既包含数字数据又包含字符串数据的数据就更具挑战性了。**
*   **有许多低级的方法——然而，有许多普通的分析模式没有预先构建的方法。**

**幸运的是，熊猫图书馆提供了上述每个问题的解决方案。熊猫不是 NumPy 的替代品，而是 NumPy 的大规模延伸。我不想陈述显而易见的东西，但是为了完整起见:pandas 中的主要对象是**系列**和**数据帧**。前者相当于 1D 恩达拉雷，而后者相当于 2D 恩达拉雷。Pandas 对于数据清理和分析至关重要。**

## ****系列****

**Pandas **系列**是一个一维数组，可以保存任何类型的数据(整数、浮点、字符串、python 对象)。轴标签称为索引。**

## ****数据帧****

**熊猫数据帧是一个二维标签数据结构，其中每一列都有一个唯一的名称。您可以将 Pandas 数据帧看作一个 SQL 表或一个简单的电子表格。**

****合并，加入&串联****

**这些方法使数据科学家能够通过将多个数据集合并到单个数据框架中来扩展他们的分析。**

****分组依据****

**`**groupby()**`方法对于研究根据给定标准分组的数据集很有用。**

## ****那么，接下来去哪里？****

**寻求关于基本 Python 特性、库和语法的帮助是数据科学家学习曲线的一部分。然而，不幸的是，你会发现有些答案并不像你想象的那么简单。你可以在我的 GitHub 中找到[完整的‘备忘单’。此外，DataCamp 还为 Python 数据科学家发布了一份长长的清单。我希望你的一些直截了当的问题能很快得到回答，现在你已经准备好开始数据科学有趣的部分了:解决问题和产生见解。](https://github.com/boemer00/python-cheatsheet#numbers-1)**

****感谢阅读。以下是你可能喜欢的其他文章:****

**[](/increase-productivity-data-cleaning-using-python-and-pandas-5e369f898012) [## 提高生产力:使用 Python 和 Pandas 进行数据清理

### 数据清理可能很耗时，但了解不同类型的丢失值，以及如何处理…

towardsdatascience.com](/increase-productivity-data-cleaning-using-python-and-pandas-5e369f898012) [](/switching-career-to-data-science-in-your-30s-6122e51a18a3) [## 30 多岁转行做数据科学。

### 不要纠结于已经有答案的问题。以下是我希望在开始职业生涯前知道的三件事…

towardsdatascience.com](/switching-career-to-data-science-in-your-30s-6122e51a18a3) [](https://medium.com/better-programming/save-time-using-the-command-line-glob-patterns-and-wildcards-17befd6a02c8) [## 使用命令行节省时间:Glob 模式和通配符

### 使用模式匹配文件名

medium.com](https://medium.com/better-programming/save-time-using-the-command-line-glob-patterns-and-wildcards-17befd6a02c8) [](/what-makes-a-data-scientist-stand-out-e8822f466d4c) [## 是什么让一个数据科学家脱颖而出？

### 越来越多的人痴迷于硬技能，但软技能可以让你脱颖而出。

towardsdatascience.com](/what-makes-a-data-scientist-stand-out-e8822f466d4c) 

**参考文献:**

******知识的诅咒**[https://en.wikipedia.org/wiki/Curse_of_knowledge](https://en.wikipedia.org/wiki/Curse_of_knowledge)****

****【2】****data camp**网站[https://www . data camp . com/community/data-science-cheat sheets？page=2](https://www.datacamp.com/community/data-science-cheatsheets?page=2)****