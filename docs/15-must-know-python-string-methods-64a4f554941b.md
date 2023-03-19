# 15 个必须知道的 Python 字符串方法

> 原文：<https://towardsdatascience.com/15-must-know-python-string-methods-64a4f554941b?source=collection_archive---------3----------------------->

## 这并不总是关于数字。

![](img/37eada1c81825439c86a33af22fcf23d.png)

照片由[你好我是尼克](https://unsplash.com/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/text?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Python 是一门伟大的语言。它相对容易学习，语法直观。丰富的库选择也有助于 Python 的流行和成功。

然而，这不仅仅是关于第三方库。Base Python 还提供了许多方法和函数来加速和简化数据科学中的典型任务。

在本文中，我们将介绍 Python 中的 15 个内置字符串方法。你可能已经对其中一些很熟悉了，但我们也会看到一些罕见的。

这些方法是不言自明的，所以我将更多地关注示例来演示如何使用它们，而不是解释它们做什么。

## 1.大写

它使第一个字母大写。

```
txt = "python is awesome!"txt.capitalize()
'Python is awesome!'
```

## 2.上面的

它使所有的字母大写。

```
txt = "Python is awesome!"txt.upper()
'PYTHON IS AWESOME!'
```

## 3.降低

它让所有的字母都变成小写。

```
txt = "PYTHON IS AWESOME!"txt.lower()
'python is awesome!'
```

## 4.Isupper

它检查是否所有的字母都是大写的。

```
txt = "PYTHON IS AWESOME!"txt.isupper()
True
```

## 5.Islower

它检查是否所有的字母都是小写的

```
txt = "PYTHON IS AWESOME!"txt.islower()
False
```

下面的 3 个方法是相似的，所以我将做包括所有这些方法的例子。

## 6.Isnumeric

它检查是否所有的字符都是数字。

## 7.伊萨法

它检查是否所有的字符都在字母表中。

## 8.伊萨勒姆

它检查所有字符是否都是字母数字(即字母或数字)。

```
# Example 1
txt = "Python"print(txt.isnumeric())
Falseprint(txt.isalpha())
Trueprint(txt.isalnum())
True
```

```
# Example 2
txt = "2021"print(txt.isnumeric())
Trueprint(txt.isalpha())
Falseprint(txt.isalnum())
True
```

```
# Example 3
txt = "Python2021"print(txt.isnumeric())
Falseprint(txt.isalpha())
Falseprint(txt.isalnum())
True
```

```
# Example 4
txt = "Python-2021"print(txt.isnumeric())
Falseprint(txt.isalpha())
Falseprint(txt.isalnum())
False
```

## 9.数数

它计算给定字符在字符串中出现的次数。

```
txt = "Data science"txt.count("e")
2
```

## 10.发现

它返回给定字符在字符串中第一次出现的索引。

```
txt = "Data science"txt.find("a")
1
```

我们还可以找到一个字符的第二次或其他出现。

```
txt.find("a", 2)
3
```

如果我们传递一个字符序列，find 方法返回序列开始的索引。

```
txt.find("sci")
5
```

## 11.开始于

它检查字符串是否以给定的字符开始。我们可以在列表理解中使用这种方法作为过滤器。

```
mylist = ["John", "Jane", "Emily", "Jack", "Ashley"]j_list = [name for name in mylist if name.startswith("J")]j_list
['John', 'Jane', 'Jack']
```

## 12.末端

它检查字符串是否以给定的字符结尾。

```
txt = "Python"txt.endswith("n")
True
```

endswith 和 startswith 方法都区分大小写。

```
txt = "Python"txt.startswith("p")
Falsetxt.startswith("P")
True
```

## 13.替换

它用给定的字符集替换字符串或字符串的一部分。

```
txt = "Python is awesome!"txt = txt.replace("Python", "Data science")txt
'Data science is awesome!'
```

## 14.裂开

它在出现指定字符的位置拆分字符串，并返回包含拆分后每个部分的列表。

```
txt = 'Data science is awesome!'txt.split()
['Data', 'science', 'is', 'awesome!']
```

默认情况下，它在空格处分割，但我们可以根据任何字符或字符集来分割。

## 15.划分

它将一个字符串分成 3 部分，并返回包含这些部分的元组。

```
txt = "Python is awesome!"
txt.partition("is")
('Python ', 'is', ' awesome!')txt = "Python is awesome and it is easy to learn."
txt.partition("and")
('Python is awesome ', 'and', ' it is easy to learn.')
```

partition 方法正好返回 3 个部分。如果用于分区的字符多次出现，则考虑第一个字符。

```
txt = "Python and data science and machine learning"
txt.partition("and")
('Python ', 'and', ' data science and machine learning')
```

我们也可以通过限制拆分的数量来对拆分方法进行类似的操作。但是，也有一些不同之处。

*   split 方法返回一个列表
*   返回的列表不包括用于拆分的字符

```
txt = "Python and data science and machine learning"
txt.split("and", 1)
['Python ', ' data science and machine learning']
```

## 奖金

感谢 Matheus Ferreira 提醒我最伟大的字符串方法之一:join。我也使用 join 方法，但是我忘记在这里添加了。它理应作为额外奖励进入榜单。

join 方法将集合中的字符串组合成一个字符串。

```
mylist = ["Jane", "John", "Matt", "James"]"-".join(mylist)'Jane-John-Matt-James'
```

让我们也用一个元组来做一个例子。

```
mytuple = ("Data science", "Machine learning")" and ".join(mytuple)'Data science and Machine learning'
```

## 结论

在执行数据科学时，我们会大量处理文本数据。此外，文本数据比普通数字需要更多的预处理。幸运的是，Python 的内置字符串方法能够高效、流畅地执行这些任务。

最后但同样重要的是，如果你还不是[的中级会员](https://sonery.medium.com/membership)，并打算成为其中一员，我恳请你使用以下链接。我将从你的会员费中收取一部分，不增加你的额外费用。

<https://sonery.medium.com/membership>  

感谢您的阅读。如果您有任何反馈，请告诉我。