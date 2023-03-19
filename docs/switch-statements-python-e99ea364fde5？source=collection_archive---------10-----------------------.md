# 如何用 Python 编写 Switch 语句

> 原文：<https://towardsdatascience.com/switch-statements-python-e99ea364fde5?source=collection_archive---------10----------------------->

## 编程；编排

## 了解如何使用模式匹配或字典在 Python 中编写 switch 语句

![](img/fd522880f7eef9bff9239a96a950672a.png)

[斯蒂夫·约翰森](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/switch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

## 介绍

在编程语言中处理多路分支的典型方法是 if-else 子句。当我们需要对许多场景进行编码时，另一种选择是大多数现代语言都支持的所谓的 **switch** 或 **case** 语句。

**然而，对于 Python 版本< 3.10，没有这样的语句**能够基于特定变量的值选择指定的动作。相反，我们通常必须编写一个包含多个 if-else 语句的语句，甚至创建一个字典，然后根据特定的变量值对其进行索引。

在今天的简短指南中，我们将演示如何以 if-else 系列或字典的形式编写 switch 或 case 语句。此外，我们将展示 Python 3.10 版本中引入的新的**模式匹配**特性。

## 编写 if-else 语句

让我们假设我们有一个名为`choice`的变量，它接受一些字符串值，然后根据变量值打印一个浮点数。

使用一系列 if-else 语句，如下面的代码片段所示:

```
if choice == 'optionA':
    print(1.25)
elif choice == 'optionB':
    print(2.25)
elif choice == 'optionC':
    print(1.75)
elif choice == 'optionD':
    print(2.5)
else:
    print(3.25)
```

在传统的 if-else 语句中，我们在 else 子句中包含默认选项(即，当变量值与任何可用选项都不匹配时应该选择的选项)。

## 使用字典编写 switch 语句

另一种可能性(如果您使用的是 Python < 3.10)是字典，因为它们可以方便有效地建立索引。例如，我们可以创建一个映射，其中我们的选项对应于字典的键，值对应于期望的结果或动作。

```
choices = {
    'optionA': 1.25,
    'optionB': 2.25,
    'optionC': 1.75,
    'optionD': 2.5,
}
```

最后，我们可以通过提供相应的键来挑选所需的选项:

```
choice = 'optionA'
print(choices[choice])
```

现在我们需要处理我们的选择没有包含在指定字典中的情况。如果我们试图提供一个不存在的密钥，我们将得到一个`KeyError`。基本上有两种方法可以处理这种情况。

第一个选项要求在从字典中选择值时使用`get()`方法，这也允许我们在没有找到键时指定默认值。举个例子，

```
>>> choice = 'optionE'
>>> print(choices.get(choice, 3.25)
3.25
```

或者，我们可以使用`try`语句，这是一种通过捕捉`KeyError`来处理缺省值的通用方法。

```
choice = 'optionE'try:
    print(choices[choice])
except KeyError:
    print(3.25)
```

## Python ≥ 3.10 中的模式匹配

Python 3.10 引入了新的**结构模式匹配** ( [PEP 634](https://docs.python.org/3.10/whatsnew/3.10.html#pep-634-structural-pattern-matching) ):

> 结构模式匹配以模式的*匹配语句*和 *case 语句*的形式添加了相关动作。模式由序列、映射、原始数据类型以及类实例组成。模式匹配使程序能够从复杂的数据类型中提取信息，对数据结构进行分支，并基于不同形式的数据应用特定的操作。
> 
> 来源— [Python 文档](https://docs.python.org/3.10/whatsnew/3.10.html#pep-634-structural-pattern-matching)

因此，我们现在可以将代码简化如下:

```
choice:
    case 'optionA':
        print(1.25)
    case 'optionB':
        print(2.25)
    case 'optionC':
        print(1.75)
    case 'optionD':
        print(2.5)
    case _:
        print(3.25)
```

注意，我们甚至可以使用如下所示的`|`(‘或’)操作符将几个选择组合在一个案例或模式中:

```
case 'optionA' | 'optionB':
    print(1.25)
```

## 最后的想法

在今天的文章中，我们讨论了在 Python 中实现 switch 语句的几种替代方法，因为在 3.10 版本之前，Python 中没有像许多其他编程语言那样的内置结构。

因此，您可以编写传统的 if-else 语句，甚至初始化一个字典，其中的键对应于 if/else if 语句中使用的条件，当特定条件(键)成立时，值对应于所需的值。

此外，我们展示了如何使用 Python 3.10 中最近引入的新模式匹配 switch 语句。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](/augmented-assignments-python-caa4990811a0) [## Python 中的扩充赋值

### 了解增强赋值表达式在 Python 中的工作方式，以及为什么在使用它们时要小心…

towardsdatascience.com](/augmented-assignments-python-caa4990811a0) [](/real-time-speech-recognition-python-assemblyai-13d35eeed226) [## 如何用 Python 执行实时语音识别

### 使用 Python 中的 AssemblyAI API 执行实时语音转文本

towardsdatascience.com](/real-time-speech-recognition-python-assemblyai-13d35eeed226) [](/apply-vs-map-vs-applymap-pandas-529acdf6d744) [## 熊猫中的 apply() vs map() vs applymap()

### 讨论 Python 和 Pandas 中 apply()、map()和 applymap()的区别

towardsdatascience.com](/apply-vs-map-vs-applymap-pandas-529acdf6d744)