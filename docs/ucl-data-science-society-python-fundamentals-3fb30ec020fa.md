# UCL 数据科学协会:Python 基础

> 原文：<https://towardsdatascience.com/ucl-data-science-society-python-fundamentals-3fb30ec020fa?source=collection_archive---------12----------------------->

## 研讨会 1: Jupyter 笔记本，变量，数据类型和操作

![](img/e61dfa428dd65ca735c16a78c7592222.png)

由 [Pakata Goh](https://unsplash.com/@pakata?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今年，作为 UCL 数据科学协会的科学负责人，该协会将在整个学年举办一系列 20 场研讨会，主题包括数据科学家工具包 Python 简介和机器学习方法。对于我介绍和交付的每一个，我的目标是创建一系列小型博客帖子，这些帖子将概述主要观点，并提供完整研讨会的链接，供任何希望跟进的人使用。所有这些都可以在我们的 [GitHub](https://github.com/UCL-DSS) 上找到，并将在全年更新新的研讨会和挑战。

接下来的第一个研讨会是 Python 基础知识的介绍，这是对成员可以使用的编程环境的介绍，同时涵盖了 Python 的基础知识，如*变量*、*数据类型*和*操作符*。虽然这里将分享一些亮点，但完整的研讨会，包括问题表，可以在[这里](https://github.com/UCL-DSS/fundamental-python-workshop)找到。

## **Jupyter 笔记本**

该协会提供的大多数研讨会将在 Jupyter 笔记本中介绍。这允许混合使用降价和代码单元格，这意味着我们可以有效地并排显示代码解释和实际代码本身。这种形式通常被用作新程序员的训练床，因为它允许他们能够一点一点地与较小的代码片段进行交互，而不是面对一个很难学习的大脚本。它们还经常用于数据探索或模型结果阶段，因为它允许数据科学家以不同的方式探索数据或结果，以便我们检查数据包含的内容或查看如何以不同的方式评估模型，而不必开发和等待大型脚本运行。

**降价单元格**

Jupyter 笔记本包含 markdown 单元格，允许您像在 word 文档中一样编辑和创建文本，但格式是用代码而不是点击操作创建的。在笔记本中这样做的好处是，除了笔记本中的代码之外，它们还允许对您创建的代码进行深入解释，从而使工作流更容易理解和记录。这对于你或者其他人以后回到你的代码上来是很重要的。

它的格式使用 markdown 语言，允许你改变文本和格式，就像在文字处理器中一样。主要区别是使用代码来格式化文本，比如用`#`创建标题和题目，`()[]`创建链接，`$ $`包含数学文本。了解更多信息的好地方是降价指南[这里](https://www.markdownguide.org/)。

**编码单元格**

Jupyter 笔记本中的另一种类型的单元格是代码单元格，顾名思义，这是您编写想要运行的代码的地方。只需单击单元格左侧的“play”按钮，单击顶部交互栏中的“run ”,或者在编写代码块时使用键盘上的`ctrl + enter`,即可完成此操作。笔记本的好处是双重的:第一，你可以一段一段地运行代码，而不是创建一个完整的程序；第二，代码单元的输出直接显示在下面，这样你就可以很快得到关于你的代码产生了什么的反馈。这对新学习者有快速反馈的好处，对更高级的代码有简单的交互，如随着程序的进展看到结果或可视化特定的输出。

## **注释**

学习编码的一个重要部分是学习如何注释你的代码。这可以简单地通过在一行代码的开头或者你希望注释在代码单元中开始的地方使用一个`#`来完成，这允许你在代码中提供正在发生的事情的解释。当您想要对您的代码所做的事情编写简短、简明的解释时，或者当您想要隐藏一段代码以查看发生了什么时，这非常有用。一般来说，在代码中自由地添加注释(只要每行不超过 70 个字符)是一个很好的习惯，这样可以确保你和其他人都能理解你的代码想要达到的目的。

## **打印报表**

我们首先要在代码中交互的是打印语句，这是代码中非常有用的基本工具。Python 的主要功能和优势之一是能够轻松地输出文本，无论是在代码单元的末尾、在终端中还是在日志文档中。它可以作为一种向计算机或用户输出代码结果的方式，这样我们就可以看到正在运行什么代码或者代码中发生了什么。为此，我们使用`print()`符号来打印代码中的文本，它可以接受多个参数。这方面的一个例子是:

```
print("The population of London is", 8.136, "million")#out:The population of London is 8.136 million
```

其中`#out:`显示了我们应该在代码块末尾看到的内容。我们在这里可以看到，我们为 print 语句输入了一些单词和数字，一旦代码运行，这些单词和数字就会显示在代码的末尾。我们可以在这个声明中加入更复杂的信息，但我们将把它留到以后。

## **变量**

在 Python 中要学习的第二件重要的事情是，它可以用来在称为变量的东西中存储信息，我们可以在稍后的代码中调用这些变量来查看存储了什么。本质上，这是一种在程序中存储数据和信息的方式，允许您在代码中使用它。我们通过使用`=`操作符将信息分配给它们来创建这些变量，这仅仅意味着我们将操作符右边的值分配给操作符左边的值。例如:

```
#store data in variables
x = 10
y = "Peter"#print the values held in the variables
print(x)
print(y)#Out:
10
'Peter'
```

这里我们可以看到，我们将`10`赋给了变量`x`，将字符串`"Peter"`赋给了变量`y`。然后，我们使用我们所知道的关于 print 语句的知识，在程序的后面部分打印包含在每个变量中的信息。

## **数据类型**

在变量中存储信息的一个重要部分是存储在这些变量中的数据的类型。这将影响我们将来对变量的处理，不管是加法、减法、乘法还是列表切片。您需要知道的主要变量类型有`str`、`int`、`float`和`boolean`，它们在代码中的定义如下:

```
a = 2
b = 2.0
c = "Hello World"
d = **True**
```

你能说出这些数据类型的区别吗？

在 Python 中:

*   `int`是一个整数值，即没有小数位
*   `float`是带小数位的数值
*   `str`是一个字符串值
*   `bool`只能承担`True`或`False`

这些在编程中有特定的用途，我们将在后面探讨。然而现在，你可以通过使用`type()`函数询问程序一个变量包含什么类型的数据。这可以通过以下方式完成:

```
print(type(a))
print(type(b))
print(type(c))
print(type(d))#out:<class 'int'>
<class 'float'>
<class 'str'>
<class 'bool'>
```

我们可以看到`a`是一个`int`，`b`是一个`float`，`c`是一个`str`，`d`是一个`bool`。

一旦变量已经通过一个称为造型的过程被创建，我们就可以改变这些数据类型。这使用`str`、`int`、`float`和`bool`函数来改变变量的数据类型(如果可以改变的话):

```
a = "2"
print(type(a))
b = float(a)
print(type(b))#out:<class 'str'>
<class 'float'>
```

例如，虽然“2”由于使用了引号而以字符串开始，但我们将该类型转换为浮点型，因为 2 也可以是浮点型。如果您尝试将一个值转换为另一种数据类型，这将会失败，因为它根本不可能，例如“二”在尝试转换为`int`、`float`或`bool`时将不起作用，因为编程语言不能识别它。

## **基本运算符**

根据我们拥有的数据类型，我们可以执行基本的操作。这通常包括:

*   算术运算符
*   赋值运算符
*   比较运算符
*   逻辑运算符
*   标识运算符
*   成员运算符
*   按位运算符

到目前为止，我们已经看到了有效的赋值操作符，通过它，数据被赋值给变量。然后，我们可以向您展示基本的算术和比较操作。

**算术运算符**

算术运算符可用于执行基本的数学计算，您可能会在学校中遇到过，并且会在计算器上执行。在 python 中，基本算术运算符包括:

*   `+`用于加法
*   `-`进行减法运算
*   `*`用于乘法运算
*   `/`为分部
*   `%`对于 modulos
*   `**`求幂运算
*   `//`为楼师

它可以用作:

```
*# Addition* 
print("Addition:", 2 + 2) *# Subtraction * 
print("Subtraction:", 5 - 2) *# Multiplication* 
print("Multiplication:", 2*4) *# Division* 
print("Division:", 6/3) *# Powers* 
print("Powers:", 5**3) *# Division without remainder * 
print("Divison without remainder:", 7//3) *# Returns remainder* 
print("Division returning the remainder:", 7%3)#out:
Addition: 4
Subtraction: 3
Multiplication: 8
Division: 2.0
Powers: 125
Divison without remainder: 2
Division returning the remainder: 1
```

**比较运算符**

除了算术运算符，python 中需要理解的一个重要的基本功能是比较运算符。这些用于比较两个不同的数据或变量，目的是程序返回`True`或`False`。在 Python 中，这包括:

*   `==`为相等
*   `!=`为不相等
*   `<`因不到
*   `>`对于大于
*   `>=`为大于或等于
*   `<=`小于或等于

例如:

```
#for equal
print("5 is equal to 5:", 5 == 5)#for not equal
print("5 is not equal to 4:", 5 != 4)#for less than
print("3 is less than 5:", 3 < 5)#for greater than
print("5 is greater than 3:", 5 > 3)#for greater than or equal to
print("5 is greater than or equal to 3:", 5 >= 3)#for less than or equal to
print("3 is less than or equal to 5:", 3 <= 5)#out:
5 is equal to 5: True
5 is not equal to 4: True
3 is less than 5: True
5 is greater than 3: True
5 is greater than or equal to 3: True
3 is less than or equal to 5: True
```

这些方法的使用将主要取决于您使用的数据类型，因为根据您可用的数据，比较的工作方式可能会有所不同。

至此，首届 UCL 数据科学学会研讨会结束！祝贺你完成这篇文章并继续学习。我们希望您在未来继续关注更多的研讨会。

完整的工作坊笔记，以及进一步的例子和挑战，可以在 [**这里**](https://github.com/UCL-DSS/fundamental-python-workshop) **找到。**如果您想了解更多关于该协会的信息，包括未来的研讨会，请随时关注我们的社交活动:

https://www.facebook.com/ucldata

Instagram:[https://www.instagram.com/ucl.datasci/](https://www.instagram.com/ucl.datasci/)

领英:[https://www.linkedin.com/company/ucldata/](https://www.linkedin.com/company/ucldata/)

<https://philip-wilkinson.medium.com/membership>  </ucl-data-science-society-python-sequences-e3ffa67604a0>  </ucl-data-science-society-python-logic-3eb847362a97>  </ucl-data-science-society-object-oriented-programming-d69cb7a7b0be> 