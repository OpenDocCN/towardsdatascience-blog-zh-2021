# Python 初学者突破(python 风格)

> 原文：<https://towardsdatascience.com/python-beginner-breakthroughs-pythonic-style-dbf543e5e117?source=collection_archive---------26----------------------->

## 正如已经说过的，代码被阅读的次数远远多于它被编写的次数。在编写代码时养成良好的格式化习惯几乎和让它工作一样重要。

![](img/578e813e6cb0d57ff8fd5f0816d2a777.png)

乔丹·昆斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

代码的主要功能是工作，其次是如果你打算不止一次地使用它，让它可以被解释。养成良好的习惯并遵循 PEP-8 中推荐的通用格式惯例([见这里](https://www.python.org/dev/peps/pep-0008/))是标准化代码外观的一种简单快捷的方法。PEP-8 (Python 增强提案#8)是作为一种尝试和创建通用实践模板的手段而创建的，目的是增强可读性和标准化。在本文中，我将重点介绍一些您可以对代码进行的更改，以使其更实用、更时尚。

# 代码布局

## 刻痕

PEP-8 提供了关于何时以及如何正确使用缩进的广泛指南。我将强调两个对于新手程序员来说可能不太直观的例子。

*   制表符或空格——Python 3 解决了混合制表符和空格的问题，但是理论上,*空格是缩进的首选方法*,但是如果制表符已经在现有代码中被一致地使用，它也是合适的。
*   最大行长度— *将任何一行长而复杂的代码限制在 79 个字符以内*。这更多是为了美观，也是为了让你的 IDE 的其他窗口的屏幕空间最大化。*文档字符串和注释应限制在 72 个字符以内*。
*   与操作符相关的换行符——这实际上会使理解代码变得简单得多，一旦你看到并比较了不同之处，但是换行符应该插在操作符之前，请看下面的代码块。第一个例子是当换行符在前，第二个是在后。第二种方法使得从视觉角度理解表达式简单得多，因为它类似于手写算术问题。

```
total_cost = (retail price + 
              total_taxes +
              shipping_handling -
              discount)# Preferred option
total_cost = (retail price 
              + total_taxes
              + shipping_handling
              - discount)
```

## 进口

尽可能地使用绝对导入，以使代码更具可读性，并使读者能够理解哪个模块来自哪个库。如果第二部分不太重要或者使用了库中的公共模块，那么只从库中导入模块是可以的。

*   一次仅导入一个库。(唯一的例外是在一行中调用导入库中的多个模块，如下所示)

```
#Good only calling a single library
import statistics#exception for multiple modules
from scipy import optimize, stats
```

## 空白

正如 PEP-8 中所写的那样，过多的空白会让你看起来很不舒服，而且还会让你不得不花更多的时间去修改。

1.  紧接着左括号:

2.在尾随普通括号和右括号之间

3.在逗号、分号或冒号之前

4.请注意，在顺序数据类型的切片中，冒号的作用类似于一个运算符。你应该用相等的空间对待冒号(见下面的例子)

5.紧接在函数调用的左括号之前

6.将工作分配与另一个工作分配对齐的附加空格

```
1\. spam(ham[1], {eggs: 2}) # Good
   spam( ham[ 1 ], { eggs: 2 } ) # Bad
2\. foo = (0,) # Good
   bar = (0, ) # Not so good
3\. if x == 4: print x, y; x, y = y, x # Thumbs up!
   if x == 4 : print x , y ; x , y = y , x # Ewww!
4\. ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:] # Good
   ham[lower+offset : upper+offset] # Good too
   ham[1: 9], ham[1 :9], ham[1:9 :3] # Ouch
   ham[lower : : upper] # Eyes hurting
5\. spam(1) # Good
   spam (1) # Why??
6\. x = 1
   y = 2
   long_variable = 3 # Good
   x             = 1 # Excessive spaces in code block
   y             = 2
   long_variable = 3
```

空白的关键是只在需要的时候使用它们。请注意上面不必要的空白造成了多少额外的空间。不要走极端，确保代码的清晰和流畅不被打断。

![](img/141588b850203af60418b66da890ac41.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) 拍摄的照片

**评论**

我个人反对在代码中使用大量的注释，除非你是为了让别人从中学习而写的。变量赋值、函数开发以及代码中的其他一切都应该具有可读性，以便有编码经验的人理解正在发生的事情。这并不是说不应该有任何注释，而是(像空格一样)在必要时使用它们。例如，当代码的后续读者可能不清楚某些内容时。

*   随着代码的修改更新注释(必须！！！)
*   为了可读性，注释应该是完整的句子
*   块注释中的段落应该用#开头的一行隔开
*   应该避免行内注释，因为它们通常被认为是不必要的和分散注意力的。

**命名风格**

当您在代码中为项目指定名称时，以下示例是普遍接受的形式:

```
b (single lowercase letter)
B (single uppercase letter)
lowercase
lower_case_with_underscores
UPPERCASE
UPPER_CASE_WITH_UNDERSCORES
CapitalizedWords (or CapWords, CamelCase5, StudlyCaps)
mixedCase (differs from CapitalizedWords by initial lowercase character!)
Capitalized_Words_With_Underscores (ugly!)
```

## 加载项

有各种各样的插件可以安装到您的 ide 环境中，这些插件将根据 PEP-8 和其他格式标准提出建议和/或自动更正您的代码。我使用的一个插件叫做 PyLint，但是还有许多其他类似的插件。

希望这个关于编码风格的小提示能让你的代码更简洁，更重要的是更易读！非常感谢您的任何评论或反馈，如果您有任何想要探索的内容，请对本文做出回应。