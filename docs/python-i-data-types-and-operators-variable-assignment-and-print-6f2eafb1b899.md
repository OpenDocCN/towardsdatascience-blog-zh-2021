# 用于数据科学的 Python:初学者指南

> 原文：<https://towardsdatascience.com/python-i-data-types-and-operators-variable-assignment-and-print-6f2eafb1b899?source=collection_archive---------37----------------------->

## 学习 Python 基础知识，以便在数据科学项目中使用它。

![](img/fc5fe4c63fe65aba248f7ea8930c77cf.png)

图片来自[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1084923)的[约翰逊·马丁](https://pixabay.com/users/johnsonmartin-724525/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1084923)

# 一、关于 Python🐍

由荷兰程序员[吉多·范·罗苏姆](https://en.wikipedia.org/wiki/Guido_van_Rossum)在[Centrum wisk unde&Informatica 创建的](https://en.wikipedia.org/wiki/Centrum_Wiskunde_%26_Informatica) Python 于 1991 年首次亮相。三十多年来，它广受欢迎，赢得了“编程语言的瑞士军刀”的美誉。以下是几个原因:

*   一个强大的社区已经开发了大量的免费库和包，允许 Python 与不同领域的技术相结合。Python 在数据科学方面很受欢迎，[但是你也可以用它来进行 web 开发、金融、设计游戏等等。](https://realpython.com/what-can-i-do-with-python/)
*   Python 也是一种多范例语言——换句话说，它既允许[结构化/功能性的](https://realpython.com/python-functional-programming/)编程，也允许[面向对象的](https://realpython.com/python3-object-oriented-programming/)编程。
*   作为一种高级语言，语法被设计得直观易读。这一特性对初学者以及 Instagram 和谷歌这样的大公司都很有吸引力。

在数据科学、人工智能和机器学习等新兴领域，强大的社区、丰富的包、范式灵活性和语法简单性使初学者和专业人士能够专注于见解和创新。

# 三。数据类型

Python 大约有十二种不同的数据类型，在本教程中我将向您介绍其中的五种: ***string*** ， ***integer*** ， ***float*** ， ***boolean*** ，以及 ***list*** 。我还将向您展示一些可以在 Python 中用于您的项目的基本函数和方法。这应该足以让你开始，并为下一个教程做好准备:[熊猫 I](https://christineegan42.medium.com/pandas-i-read-csv-head-tail-info-and-describe-43b9b2736490) 。

## 1.Python 字符串

字符串是一系列字符。它们可以是数字和非数字字符。在 Python 中，它们用引号括起来。你会注意到在字符串的上方，有一个 hashtag 后面的描述。这叫做**评论**。标签后面的任何内容都是评论。

```
# This is a comment. The line below is a string in Python.
“This is a string.”   # you can also use them inline
```

在 Python 中，我们将任何类型的值赋给 ***变量*** 以便使用它们。看起来像这样:

```
the_string = “This is a string.”
```

**单等号(=)** 用于给变量赋值。以下是一些关于变量的注意事项:

*   命名约定表明变量应该是小写的
*   字母和数字都可以，但是数字不能没有字母(例如“a1”可以，而“1”不行)。
*   不允许使用特殊字符和空格。下划线**(" _ "**应该用来代替空格
*   你可以给一个变量起任何你想要的名字，但是有几个名字你不能用，因为它们已经是 Python 关键字了。请参见下面的列表:

```
False      class      finally    is         return
None       continue   for        lambda     try
True       def        from       nonlocal   while
and        del        global     not        with
as         elif       if         or         yield
pass       else       import     assert
break      except     in         raise
```

要打印所有输出，包括 Python 中的字符串，我们使用 **print()** :

```
[in]  print(the_string)
[out] "This is a string."
```

你可以**连接**或者像这样把字符串放在一起:

```
[in]  another_string = “This is another string.”
      a_combo_string = the_string + another string
      print(a_combo_string)
[out] "This is a string. This is another string."
```

字符串是不可变的，这意味着它们不能被改变。它们需要被重新分配来改变(如果你愿意，你可以回收这个变量)。

如果想知道任意值或变量的数据类型，可以使用 **type()** 。

```
[in]  print(type(the_string))
[out]  <class 'str'>
```

## 2.Python 整数

Python 中的整数是不带小数点的数字。它们可以是积极的，也可以是消极的。

```
[in]  int_num = 1
      print(int_num)
[out] 1
```

您可以使用算术运算符对数字数据类型整数执行运算。

```
[in]  print(1 + 2)
[out] 3
```

您可以将它们赋给变量，也可以执行操作。

```
[in]  a = 5
      b = 6
      c = a — b 
      print(c)
[out] -1
```

你也可以乘除:

```
[in]  print(a * b / c)
[out] -30.0
```

## 3.Python 浮动

像整数一样，浮点数也是一种数字数据类型。它们也可以是积极的和消极的。然而，他们是浮点小数，这意味着他们有一个小数点，而不是整数。然而，我们可以在整数上执行的大多数操作都可以在浮点数上执行。

```
[in]  int_float = 2.5
      print(num_float)
      d = 1.33
      e = 4.67
      f = d + e
      g = f — 7
      print(g)
[out] 2.5
      1.0
```

## 4.Python 布尔值

布尔值不是**真**就是**假**。

```
[in]  print(type(True))
      print(type(False))
[out] <class 'bool'>
      <class 'bool'>
```

您可以使用布尔运算符来比较值。

```
[in]  print(d > e)   # d greater than g 
[out] False [in]  print(c >= g)  # c greater than or equal to g 
[out] True[in]  print(a < e)   # a less than e
[out] False[in]  print(d <= g)  # d less than or equal to g 
[out] False[in]  print(g == c)  # g equals c
[out] True
```

## 5.Python 列表

Python 中的列表是存储值的容器。它们用括号括起来，我们一般把它们赋给变量。

```
[in]  our_list = [a, b, c, d, e, f, g]
      print(our_list)
[out] [5, 6, -1, 1.33, 4.67, 6.0, -1.0]
```

它们可以是数值，也可以是非数值。

```
[in]  i = “Ice”
      j = “Jewel”
      k = “Karate”
      l = “Lemon”
      another_list = [i, j, k, l]
```

它们可以是**排序()**也可以。

```
[in]  sorted(our_list)
[out] [-1, -1.0, 1.33, 4.67, 5, 6, 6.0]
```

但是，如果我们希望列表在操作时保持排序，我们需要重新分配它:

```
[in] our_sorted_list = sorted(our_list)
```

我们可以用 **append()** 添加列表:

```
[in]  h = 7
      our_list.append(h)
      print(our_list)
[out] [5, 6, -1, 1.33, 4.67, 6.0, -1.0, 7]
```

我们可以用 **remove()** 删除东西:

```
[in]   our_list.remove(a)
       print(our_list)
[out]  [6, -1, 1.33, 4.67, 6.0, -1.0, 7]
```

我们可以*连接*(放在一起)两个列表:

```
[in]  combined_list = our_list + another_list
      print(combined_list)
[out] [6, -1, 1.33, 4.67, 6.0, -1.0, 7, 'Ice', 'Jewel', 'Karate', 'Lemon']
```

我们也可以使用 **add-assign 操作符** **(+=)** 将列表放在一起:

```
[in]  another_combo_list = []
      another_combo_list += combined_list
      print(another_combo_list)
[out] [6, -1, 1.33, 4.67, 6.0, -1.0, 7, 'Ice', 'Jewel', 'Karate', 'Lemon']
```

最后，我们可以使用**等式操作符(" ==")** 来比较列表，以获得一个布尔输出。

```
[in]  print(combined_list == another_combo_list)
[out] True
```

# 二。我们做了什么？

1.  在 Python 中发现了五种不同的数据类型:整型、浮点型、布尔型、字符串型和列表型。
2.  讨论了变量赋值、print()、type()和注释。
3.  学习算术运算符和布尔运算符。
4.  尝试列表。

# 四。下一步是什么？

在 [Pandas I](https://christineegan42.medium.com/pandas-i-read-csv-head-tail-info-and-describe-43b9b2736490) 中，您将学习如何使用 Python 和 Pandas 来分析 [Metal Bands by Nation](https://www.kaggle.com/mrpantherson/metal-by-nation) 数据集。