# 轻松导入和执行 Python 模块的 3 种技术

> 原文：<https://towardsdatascience.com/3-advance-techniques-to-effortlessly-import-and-execute-your-python-modules-ccdcba017b0c?source=collection_archive---------9----------------------->

## 如何让你的 Python 模块更加用户友好

![](img/a4c7991fffd31b1122251234536a8ab1.png)

由[弗洛里安·克劳尔](https://unsplash.com/@florianklauer?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 动机

作为一名数据科学家，您很可能希望与您的队友或其他用户共享您创建的有用模块。尽管你的模块可能是有用的，但是如果其他人要花很大力气才能访问你的模块中有用的函数，他们就不会使用它。

因此，你想让用户更容易使用你的模块。导入和运行模块的代码应该很短。在本文中，我将向您展示 3 种简化导入和执行 Python 模块的方法。

# 导入所有内容

## 方案

假设我们有一个名为`utils.py`的文件，其中包含所有重要的函数和类

在另一个脚本中，我们想要使用`from utils import *`从`utils.py` **导入除了**变量`a`之外的所有内容。我们如何做到这一点？

## 解决办法

这很容易通过添加`__all__ = [“add_two”, “multiply_by_two”]`来实现。使用`import *`时，将导入`__all__`中指定的函数、类和包。

现在尝试在另一个脚本中使用`import *`

```
5
6
Traceback (most recent call last):
  File "main.py", line 6, in <module>
    print(a)
NameError: name 'a' is not defined
```

酷！该错误告诉我们只有来自`utils.py`的`add_two`和`multiply_by_two`被导入，而`a`没有被导入。

# 合并多个文件

## 方案

想象一下我们目录中的文件结构如下所示:

```
.
├── data_modules
│   ├── load_data.py
│   └── process_data.py
└── main.py
```

`load_data.py`看起来是这样的:

而`process_data.py`看起来是这样的:

为了使用两个不同文件中的类，我们需要从每个文件中导入每个类。

这个方法很好，但是使用 2 个 import 语句是多余的。有没有一种方法可以把两个 import 语句变成一个 import 语句，如下所示？

## 解决办法

这可以用`__init__.py`文件轻松解决。在`data_modules`目录下插入`__init__.py`文件:

```
touch data_modules/__init__.py
```

然后将前面提到的导入语句插入到`__init__.py`文件中:

我们在这里使用一个`.`，因为`load_data.py`和`__init__.py`在同一个目录中。

现在让我们试着从`data_modules`导入`DataLoader`和`DataProcessor`

```
Loading data from data/
Processing data1
```

不错！有用！

# 像运行主脚本一样运行一个目录

## 方案

我们的目录如下所示:

```
.
└── data_modules
    ├── __init__.py
    ├── load_data.py
    ├── main.py
    └── process_data.py
```

而不是逃跑

```
$ python data_modules/main.py
```

我们可能想让我们的用户或队友更简单地运行代码`main.py`文件，只需运行父目录:

```
$ python data_modules
```

这比运行`python data_modules/main.py`要好，因为它更短，用户不需要知道`data_modules`里有什么文件。

我们如何做到这一点？

## 解决办法

这就是`__main__.py`派上用场的时候。只需将运行顶层目录时要运行的脚本改为`__main__.py`。在我们的例子中，`main.py`将变成`__main__.py`。

```
# Rename main.py to __main__.py
$ mv data_modules/main.py data_modules/__main__.py
```

我们的新目录将如下所示

```
.
└── data_modules
    ├── __init__.py
    ├── load_data.py
    ├── __main__.py
    └── process_data.py
```

现在快跑

```
$ python data_modulesLoading data from data/
Processing data1
```

它的效果非常好！

# 结论

恭喜你！您刚刚学习了如何通过以下方式使您和您的软件包的使用者更加轻松:

*   使用`import *`时控制导入的内容
*   缩短您的进口陈述
*   将顶级目录作为主脚本运行

我希望这些技巧能让你的 Python 包更加人性化。如果你的团队成员和用户很容易访问你的有用模块，他们会很乐意使用你的包。

本文的源代码可以在这里找到:

<https://github.com/khuyentran1401/Data-science/tree/master/python/module_example>  

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上联系我。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

</python-clean-code-6-best-practices-to-make-your-python-functions-more-readable-7ea4c6171d60>  </how-to-effortlessly-publish-your-python-package-to-pypi-using-poetry-44b305362f9f>  </3-python-tricks-to-read-create-and-run-multiple-files-automatically-5221ebaad2ba>  </rich-generate-rich-and-beautiful-text-in-the-terminal-with-python-541f39abf32e>  

# 参考

d . m . beazley 和 b . k . Jones(2014)。 *Python 食谱*。北京；剑桥；法纳姆；科隆；塞瓦斯托波尔；东京:奥赖利。