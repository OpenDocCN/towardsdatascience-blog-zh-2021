# 如何在 Python f-strings 中添加新行

> 原文：<https://towardsdatascience.com/how-to-add-new-line-in-python-f-strings-7b4ccc605f4a?source=collection_archive---------4----------------------->

## 如何修复语法错误:f 字符串表达式部分不能包含反斜杠

![](img/dfc8c185fd47be6693c5227d8e0840bd.png)

由 [Kevin Mak](https://unsplash.com/@kivenage?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/lines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

在 Python 中，不可能在 f 字符串的花括号`{}`中包含反斜杠。这样做会导致一个`SyntaxError`:

```
>>> f'{\}'
SyntaxError: f-string expression part cannot include a backslash
```

这种行为完全符合 [PEP-0498](https://www.python.org/dev/peps/pep-0498/#escape-sequences) 关于文字字符串插值的要求:

> 反斜杠不能出现在 f 字符串的表达式部分中，所以你不能使用它们，例如，在 f 字符串中转义引号

在接下来的几节中，我们将探索几个选项，你可以使用这些选项在 f 弦中添加反斜杠(包括新行)。

## 在 f 弦中使用反斜杠

正如我们已经讨论过的，反斜杠不能直接在 Python f 字符串中使用。但是，有一个简单的解决方法，允许我们使用反斜杠来表示新行、制表符等。

假设我们有一个想要输出到标准输出的整数列表。

```
nums = [10, 20, 30] 
print(f"Numbers:\n {'\n'.join(map(str, nums))}")
```

不出所料，上述操作将会失败

```
SyntaxError: f-string expression part cannot include a backslash
```

一种解决方法是将新的行字符(即`'\n'`)添加到一个字符串变量中，然后在打印出数字时引用该变量。举个例子，

```
new_line = '\n'
nums = [10, 20, 30] 
print(f"Numbers:{new_line}{new_line.join(map(str, nums))}")
```

输出应该是

```
Numbers:
10
20
30
```

请注意，您可以对几乎所有需要反斜杠的字符都这样做。例如，让我们假设您想要在 f 字符串中使用制表符。以下应该是窍门:

```
tab = '\t'
print(f'Hello {tab} World!')# Output
Hello   World!
```

## 如何在 f 弦中添加新的一行

特别是对于新行，我们还可以遵循另一种方法在 Python f-strings 中添加新的行字符。

`os`包附带了一些关于其他系统信息的功能。这包括返回新行字符的`[os.linesep](https://docs.python.org/3/library/os.html#os.linesep)`。

> 用于在当前平台上分隔(或者说，终止)行的字符串。这可以是单个字符，比如 POSIX 的`'\n'`，也可以是多个字符，比如 Windows 的`'\r\n'`。

举个例子，

```
import osnums = [10, 20, 30] 
print(f"Numbers:{os.linesep}{os.linesep.join(map(str, nums))}")
```

输出将再次是

```
Numbers:
10
20
30
```

请注意，在编写以文本模式打开的文件时，必须避免使用`os.linesep`作为行终止符。相反，你必须使用单个的`'\n'`。

另一种可能，是用`chr()`功能生成一个新的行字符。Python 中的`chr()`函数返回对应于 Unicode 字符的输入整数的字符串表示。Unicode 中`10`的十进制值相当于换行符(即新行)。

```
>>> chr(10)
'\n'
```

因此，为了使用`chr()`在 f 弦中添加新的线条，您需要做的就是

```
nums = [10, 20, 30] 
print(f"Numbers:{chr(10)}{chr(10).join(map(str, nums))}")# Numbers:
# 10
# 20
# 30
```

## 最后的想法

在今天的文章中，我们讨论了 Python 的一个限制，它阻止我们在 f 字符串中使用反斜杠。我们介绍了一些可能的解决方法，并且我们还特别探索了如何在 Python f-strings 中隐式地添加新行。

本质上，你有三个选择；第一个是定义一个新行作为字符串变量，并在 f-string 花括号中引用该变量。第二种解决方法是使用返回新行字符的`os.linesep`，最后一种方法是使用对应于 Unicode 新行字符的`chr(10)`。