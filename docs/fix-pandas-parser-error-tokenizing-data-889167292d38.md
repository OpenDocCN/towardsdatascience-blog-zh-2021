# 如何修复 pandas.parser.CParserError:标记数据时出错

> 原文：<https://towardsdatascience.com/fix-pandas-parser-error-tokenizing-data-889167292d38?source=collection_archive---------0----------------------->

## 了解在 pandas 中读取 CSV 文件时出现错误的原因以及如何处理该错误

![](img/89dfc44c7d48830a77d886fba38a8ce8.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/token?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

从 csv 文件导入数据可能是实例化 pandas 数据帧最常用的方式。然而，很多时候这可能有点棘手，尤其是当文件中包含的数据不是预期的形式时。在这些情况下，pandas parser 可能会引发类似下面报告的错误:

```
pandas.errors.ParserError: Error tokenizing data. C error: Expected 5 fields in line 2, saw 6
```

在今天的简短指南中，我们将首先讨论为什么会出现这个错误，此外，我们还将讨论一些最终可以帮助您处理它的方法。

## 重现错误

首先，让我们使用我在本教程中准备的一个小数据集来重现这个错误。

包含不一致格式数据的示例文件— [来源:作者](https://gmyrianthous.medium.com/)

现在，如果我们尝试使用`read_csv`读取文件:

```
import pandas as pddf = pd.read_csv('test.txt')
```

我们将得到下面的错误

```
pandas.errors.ParserError: Error tokenizing data. C error: Expected 4 fields in line 4, saw 6
```

这个错误非常明显，因为它表明在第 4 行而不是 4 行，观察到了 6 个字段(顺便说一下，同样的问题也出现在最后一行)。

默认情况下，`read_csv`使用逗号(`,`)作为分隔符，但是很明显，文件中两行的分隔符比预期的多。在这种情况下，预期的数字实际上是 4，因为我们的头(即文件的第一行)包含 4 个用逗号分隔的字段。

## 手动修复文件

这个问题最明显的解决方案，是通过删除行中给我们带来麻烦的额外分隔符来手动修复数据文件。这实际上是最佳解决方案(假设您已经指定了正确的分隔符、标题等)。调用`read_csv`函数时)。然而，当您需要处理包含数千行的大文件时，这可能是相当棘手和痛苦的。

## 指定行结束符

此错误的另一个原因可能与数据中的某些回车符(即`'\r’`)有关。在某些场合这实际上是通过`pandas.to_csv()`方法引入的。将 pandas 数据帧写入 CSV 文件时，会在列名中添加一个回车，然后该方法会将后续的列名写入 pandas 数据帧的第一列。因此我们将在第一行中得到不同数量的列。

如果是这种情况，那么您可以在调用`read_csv()`时使用相应的参数显式地指定`'\n'` 的行结束符:

```
import pandas as pd df = pd.read_csv('test.csv', **lineterminator='\n'**)
```

## 指定正确的分隔符和标题

该错误也可能与调用`read_csv`时指定的分隔符和/或头(非)有关。确保传递正确的分隔符和标题。

例如，下面的参数指定`;`是用于分隔列的分隔符(默认情况下逗号用作分隔符),并且文件不包含任何标题。

```
import pandas as pddf = pd.read_csv('test.csv', sep=';', header=None)
```

## 跳过行

跳过导致错误的行应该是您最后的手段，我个人不鼓励您这样做，但是我想在某些用例中这是可以接受的。

如果是这种情况，那么在调用`read_csv`函数时，可以通过将`error_bad_lines`设置为`False`来实现:

```
import pandas as pd df = pd.read_csv('test.txt', **error_bad_lines=False**)print(df)
 *colA colB  colC  colD
0     1    A   100   1.0
1     2    B   121   2.1
2     4    D   164   3.1
3     5    E    55   4.5*
```

正如您从上面的输出中看到的，导致错误的行实际上被跳过了，现在我们可以继续处理我们的 pandas 数据帧了。

## 最后的想法

在今天的简短指南中，我们讨论了在将 csv 文件读入 pandas 数据帧时，pandas 解析器引发`pandas.errors.ParserError: Error tokenizing data`的几种情况。

此外，我们展示了如何通过修复数据文件本身的错误或打字错误，或者通过指定适当的行结束符来处理错误。最后，我们还讨论了如何跳过导致错误的行，但是请记住，在大多数情况下，这是应该避免的。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](/dynamic-typing-in-python-307f7c22b24e) [## Python 中的动态类型

### 探索 Python 中对象引用的工作方式

towardsdatascience.com](/dynamic-typing-in-python-307f7c22b24e) [](/mastering-indexing-and-slicing-in-python-443e23457125) [## 掌握 Python 中的索引和切片

### 深入研究有序集合的索引和切片

towardsdatascience.com](/mastering-indexing-and-slicing-in-python-443e23457125)