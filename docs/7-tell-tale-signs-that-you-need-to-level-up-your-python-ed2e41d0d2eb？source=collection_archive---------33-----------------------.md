# 你需要提高你的 Python 水平的 7 个信号

> 原文：<https://towardsdatascience.com/7-tell-tale-signs-that-you-need-to-level-up-your-python-ed2e41d0d2eb?source=collection_archive---------33----------------------->

## 避免这些常见的不良做法和新手错误

![](img/37250369c5506dc9a28d1004e46206fc.png)

[奥斯汀·陈](https://unsplash.com/@austinchan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在以下七种说法中，你能认出自己吗？那么这个故事是给你的！

1.  您正在使用`sys`或`os`进行文件路径管理
2.  你正在用`.format()`或`% operator`格式化字符串
3.  你没有使用`typing`类型提示
4.  你没有使用`lambda`的显式熊猫功能
5.  你是`printing`而不是`logging`用户反馈
6.  您没有使用 linters 或 formatters，也没有使用`pre-commit`
7.  你没有使用`ifmain`

是时候用下面的小技巧&把你的 Python 2.x 知识升级到 **Python 3.10** 了。

# Pathlib:一种更好的路径处理方式

> 忘记`sys`和`os`，用`pathlib`代替。

Python 3.4 引入了内置库`pathlib`，它将字符串路径转换为**面向对象的文件系统路径**，启用属性和方法功能。从 PEP519 开始，`pathlib`是公认的 Python 文件路径的抽象。

# 用 f 字符串格式化字符串

> 忘记`% operator`和`.format()`，用`*f''*`代替。

Python 3.6+引入了对字符串格式的最新更新:`f-strings`。

# 类型提示

> 使用类型提示增加源代码文档和可读性。

从 Python 3.5+开始，我们已经看到了下一代代码文档:*提示*哪些*变量的类型*在函数或类参数和返回语句中。这使得格式化程序、linters 和 IDE 能够为类型检查提供运行时支持。

类型提示可以从`typing`模块导入。格式为`<variable name> : <variable type>`，可以直接插入函数参数中。您可以用`-> <return variable>`包含返回声明。

请注意，类型提示是**可选的**:Python 解释器完全忽略它们，带有错误格式的类型提示的 Python 代码仍然会运行。像 Pycharm 这样的 ide 集成了类型检查，而像`mypy`这样的静态类型检查工具可以将类型错误作为 bug 挖掘出来。

# 熊猫:使用 lambda 函数

> 忘记隐式过滤和列赋值，改用`lambda`。

Pandas 是一个很棒的库，已经成为任何数据科学项目的标准。但是要确保明智地使用它的功能。避免常见的不良行为，例如:

1.  没有`.loc`和`lambda`函数的隐式滤波。
2.  没有`.assign`和`lambda`函数的隐式列赋值。

虽然这些方法有效，但是它们很难维护，尤其是在链接选项时。**显式比隐式好**:用指定的方法指定你在做什么，比如`.loc`或`.assign`和**如何用`lambda`函数**。`lambda`函数是未命名的函数，在这种情况下，迭代熊猫`DataFrames`的行。

# 日志:用于反馈的内置模块

> 忘记`print()`，使用`logging`进行错误、警告和调试。

打印错误可能对调试有用，但是使用 Python 内置的`[logging](https://docs.python.org/3/library/logging.html)`模块使**所有的** Python 模块都能够参与日志记录，因此您的应用程序日志可以包含您的消息以及来自第三方模块的消息。

`logging`模块提供了从最不严重到最严重的几个`LEVELS`:

*   `DEBUG` —详细信息，通常仅在诊断问题时感兴趣。
*   `INFO` —确认事情按预期运行。
*   `WARNING` —表示发生了或即将发生意想不到的事情(如“磁盘空间不足”)。该软件仍按预期工作。
*   `ERROR` —由于更严重的问题，软件无法执行某些功能。
*   `CRITICAL` —严重错误，表示程序本身可能无法继续运行。

`basicConfig`的级别表示要包含哪些日志。例如，如果您使用`info()`，但是日志级别被设置为`WARNING`，这些日志就不会显示出来。您也可以使用配置将日志输出到一个`example.log`文件，而不是将我们的日志流式传输到标准输出(相当于打印它们):

```
logging.basicConfig(filename='example.log',level=logging.DEBUG)
```

# 预提交:自动林挺和格式化

> 再也不要手动检查 Python 源代码中的样式错误。

通过 Git 挂钩，我们可以在每次提交和推送到我们的库之前运行林挺和格式化工具，比如`mypy`、`autoflake`、`flake8`、`isort`、`black`。这使我们能够在 Python 项目中自动创建**标准化的代码风格约定**。这些 Git 挂钩由`pre-commit`包提供。请在下面的故事中找到更多关于预提交 Git 挂钩的有用提示👇

[](https://betterprogramming.pub/4-tips-to-automate-clean-code-in-python-527f59b5fe4e) [## 在 Python 中自动化干净代码的 4 个技巧

### 通过这些林挺和格式化工具，使用预提交 Git 挂钩来自动化 Python 代码样式

better 编程. pub](https://betterprogramming.pub/4-tips-to-automate-clean-code-in-python-527f59b5fe4e) 

# Ifmain:模块化 Python 代码

> 通过`__main__`执行脚本，通过`__module_name__`执行模块。

当 Python 解释器解析 Python 脚本时，设置一些顶级变量**并执行源代码**。为了避免执行`.py`文件中的所有源代码，我们将`ifmain`与顶层`__name__`变量结合使用。该`__name__`变量是

1.  `__main__`执行源文件时，像这样:`$ python3 file.py`
2.  导入模块时的模块名称`__module_name__`如下:`import modular_function from file.py`

`if __name__ == '__main__':`下的`ifmain`代码是样板代码，保护用户在无意中调用脚本中的源代码。导入使用命令行参数和`argparse`的无保护脚本的一个问题是 CLI 参数会被覆盖。

An `ifmain`是 Python 中模块化编码原则的一部分:**不要重复自己**(干)。它使您能够编写独立的函数，这些函数独立于`.py`文件中的其余代码运行，同时也能够将源代码作为主程序运行。

在你的日常工作中使用这七个技巧，至少在接下来的几个月里，你就是未来的证明😉编码快乐！

[](https://medium.com/analytics-vidhya/seven-tips-to-clean-code-with-python-24930d35927f) [## 用 Python 清理代码的七个技巧

### 以下是我作为数据科学家在工作中每天使用的七个技巧和代码片段。

medium.com](https://medium.com/analytics-vidhya/seven-tips-to-clean-code-with-python-24930d35927f) [](/five-tips-for-automatic-python-documentation-7513825b760e) [## 自动化 Python 文档的五个技巧

### 用这五个自动化步骤和预提交 Git 挂钩在 MkDocs & Material 中创建漂亮的 Python 文档

towardsdatascience.com](/five-tips-for-automatic-python-documentation-7513825b760e)