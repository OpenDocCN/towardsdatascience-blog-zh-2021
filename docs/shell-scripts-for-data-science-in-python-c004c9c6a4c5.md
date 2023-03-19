# Python 中数据科学的 Shell 脚本

> 原文：<https://towardsdatascience.com/shell-scripts-for-data-science-in-python-c004c9c6a4c5?source=collection_archive---------19----------------------->

## 学习 Bash 中命令行参数的基础知识，以便作为数据科学家在工作中充分利用 shell 脚本。

![](img/e444635d9adae29a9ae7ab57db3120c9.png)

[Jethro Carullo](https://unsplash.com/@kumuriph?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

让我们学习一点 shell 脚本，因为这将极大地提高您为 Python 脚本提供和解析命令行参数(CLA)的能力。

shell 脚本是一种计算机程序，设计用于运行 Unix shell，一种命令行解释程序。shell 脚本执行的典型操作包括文件操作和程序执行，这正是作为一名数据科学家学习基础知识的原因。

*   为什么使用 shell 脚本？
*   shell 脚本中带有`getopts`的单划线 CLA
*   shell 脚本中的双破折号 CLA
*   Python 的内置`argparse`
*   `argparse`布尔值的教训:`bool`和`store_true`
*   把所有的放在一起

# 为什么使用 shell 脚本？

Shell 脚本使您能够在单个脚本中自动执行一组命令行操作，一行一行地执行它们。您的项目已经为这些自动化步骤做好了准备，学习 shell 越早越好。

我在作为数据科学家的工作中使用它的几个例子:虚拟环境管理、PySpark 表的实例化、Python linters 的执行、静态文档网站的更新和构建、带有特定参数的生产管道的执行等。

# shell 脚本中带有`getopts`的单划线 CLA

Bash 脚本有一个内置选项`getopts`来处理布尔标志和有可选参数的标志。`getopts`只能对付单杠 CLA，比如`-a`。它读取 while 语句中的`:ab:`如下

*   `:`第一个冒号启用**静默错误报告模式** : `getopts`不会产生任何关于缺少选项参数或无效选项的诊断消息。通过抑制自动错误消息，您可以使用自定义消息来处理错误。
*   `a` case 代表一个布尔类`-a`。
*   `b:` case 代表可选参数 CLA `-b some_value`。

在 while 循环中，我们可以用`<character>)`语法捕获命令行参数；然后我们可以将 CLA 赋给一个变量，比如`first_variable`。有两种特殊情况

*   `:`上述案例抓住了缺失的论证错误。这仅在所有参数都是强制的情况下才有意义。
*   `/?`案例代表任何无效选项错误。注意，`?`需要被转义或加引号来匹配文字`?`，否则它会像正则表达式一样匹配任何单个字符。

我们可以用下面的命令行参数调用我们的脚本

```
**$** sh run.sh -a -b hello
```

# shell 脚本中的双破折号 CLA

双破折号参数比看起来要困难得多，因为没有处理双破折号的内置方法:我们必须迭代每个 CLA 并匹配参数。这对于像`--help`这样的布尔参数来说相当简单。

我们对在`$@`中捕获的所有命令行参数使用 for 循环迭代。If-elif-else 语句以`fi`结束。这个特定的函数为每个参数寻找在双管`||` OR 语句中捕获的两个字符串比较:`--help`和`-h`。

我使用 bash 支持的双括号`[[ ... ]]`，但是您也可以使用单括号。点击阅读更多关于这两个[的区别。](https://stackoverflow.com/questions/669452/is-double-square-brackets-preferable-over-single-square-brackets-in-ba#:~:text=What%20is%20the%20difference%20between%20test%2C%20%5B%20and%20%5B%5B%20%3F%20and%20Bash%20Tests)

如果我们想要提供可选的参数，比如`--arg1 some_var`，这将变得更加复杂，我会参考 bash 中定制的 arg 解析器[的文档。](https://medium.com/@Drew_Stokes/bash-argument-parsing-54f3b81a6a8f#:~:text=positional%20arguments%20together.-,A%20Better%20Way,-As%20it%20turns)

同样值得一提的是，在大多数 Bash 内置命令和许多其他命令中，使用一个单双破折号`--`来表示命令选项的结束，之后只接受位置参数，比如`**$** sh run.sh -a one -b two -- three four`。

# Python 的内置`argparse`

现在进入更熟悉的领域:Python 脚本及其内置的`argparse`。使用这个内置的参数解析器增加了许多现成的功能。`argparse`使用所有数据类型，支持就地转换，默认值，并定义单`—`和双`—-`破折号 CLA。我们的 Python 脚本`run.py`如下所示。

参数通过命令`parser.parse_args()`存储在一个对象中，我们通过每个参数的名称来检索这个对象，比如`args.age`或`args.debug`。如果我们在不提供任何参数的情况下打印`args`,我们将得到以下具有默认值的对象:

```
Namespace(name='', age=1, date='20210709', debug=False, alive=False)
```

# `Argparse`布尔值的教训

我得到的一个教训是，布尔值可以通过两种方式提供，要么通过使用`type=bool`的参数，要么通过使用`action="store_true"`的参数。无论你选择哪种方式，它的行为都会大不相同。我们来看下面两个案例:

```
**$** python3 run.py --name Louis --age 27 --alive --debug
>>> Namespace(name='Louis', age=27, date='20210709', debug=**None**, alive=True)**$** python3 run.py --name Louis --age 27 --alive --debug **True**
>>> Namespace(name='Louis', age=27, date='20210709', debug=**True**, alive=True)
```

如果你提供了`type=bool`，你需要添加一个标志`True`或者`False`使参数不是`None`。您只能通过在没有提供值的情况下添加默认值来避免这种情况。毫无疑问，如果您只提供标志`--debug`而不提供布尔值，它将导致`None`。

另一方面，如果您应用`action=store_true`，您不需要提供任何布尔标志:提供参数`--alive`将它存储为`True`，不提供它将它存储为`False`。用`action=store_false`反之亦然。在 Python 的 argparse 文档[中有更多关于这个的信息，请点击](https://docs.python.org/3/library/argparse.html#action)。

# 把所有的放在一起

让我们测试一下我们的知识，创建一个 shell 脚本`run.sh`，它为我们的`run.py`提供命令行参数。我在我们的 shell 脚本中添加了一些有用的功能来解释它的用法和帮助功能，并避免误用参数。

如果变量是`null` ( `None`)，则`-z ${var_name}`语法返回`True`。`${var_name}`是如何在 Bash 或 shell 中调用变量，在字符串中调用`"var_name = $var_name”`。如果我们运行`run.sh`并打印`run.py`中的参数，我们将看到以下用法:

```
**$** sh run.sh -n Elvis -b 19350108 -a 42
>>> Namespace(name='Elvis', age=42, date='19350108', debug=False, alive=False)**$** sh run.sh -n Ringo -b 19400707 -a 81 -d False -l
>>> Namespace(name='Ringo', age=81, date='19400707', debug=False, alive=True)
```

我希望你喜欢这个故事和快乐的编码！

</7-tell-tale-signs-that-you-need-to-level-up-your-python-ed2e41d0d2eb>  </python-type-hints-docstrings-7ec7f6d3416b> 