# 数据科学家和工程师必须知道的 8 个 venv 命令

> 原文：<https://towardsdatascience.com/8-must-know-venv-commands-for-data-scientists-and-engineers-dd81fbac0b38?source=collection_archive---------12----------------------->

## 探索一些对 Python 日常编程最有用的虚拟环境命令

![](img/cbc261dec1305f66a3a9310caa071876.png)

由[凯利·西克玛](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/inside-a-box?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 介绍

Python 应用程序通常使用标准包库中不包含的第三方模块和包。此外，正在开发的软件包可能需要另一个库的特定版本才能正常工作。

这意味着一些应用程序可能需要一个特定的包版本(比如版本`1.0`)，而其他应用程序可能需要一个不同的包版本(比如`2.0.1`)。因此，我们将以冲突结束，因为安装两个版本中的任何一个都会导致需要不同版本的包中的问题。

在今天的文章中，我们将讨论如何使用虚拟环境来处理这个问题。此外，我们将探索一些最常用的命令，帮助您创建、使用和管理 Python 虚拟环境。

# 什么是虚拟环境

虚拟环境是一个自包含的目录，带有特定的 Python 版本和用户指定的附加包。此外，一旦虚拟环境被激活，用户就可以安装或卸载任何 Python 库(使用`pip`)。

Python 版本(默认情况下将包括安装在您的操作系统上的版本，除非您明确指定不同的版本)，安装在一个虚拟环境中的附加库和脚本，与安装在您的机器或任何其他虚拟环境中的那些完全隔离**。**

Python 附带了一个虚拟环境管理器，对于 Python 3 叫做`venv`，对于 Python 2 叫做`virtualenv`。在本文中，我们将展示如何使用`venv`，它是在环境中管理 Python 包的最底层工具。`[venv](https://docs.python.org/3/library/venv.html)`模块包含可用于支持创建和管理轻量级虚拟环境的功能。

# 用于日常编程的 8 个 venv 命令

在接下来的章节中，我们将讨论每个 Python 开发者都必须知道的 8 个常用的`venv`命令。请注意，强烈建议您为每个 Python 项目创建一个单独的虚拟环境，在其中您将能够安装和管理所有必需的依赖项。

## 1.创建虚拟环境

在开发自己的 Python 应用程序或库时，您需要做的第一件事是创建一个虚拟环境。

```
$ python3 -m venv my_venv
```

上面的命令将创建一个名为`my_venv`的虚拟环境，它位于当前目录下。现在，如果您检查创建的目录的内容，结果应该类似于下面所示

```
$ ls my_venv
bin/        doc/        etc/        images/     include/    lib/        pyvenv.cfg  share/
```

如果您想在一个特定的目录下创建一个虚拟环境，那么只需将它和 venv 名称包含在一起。举个例子，

```
python3 -m venv path/to/your/venv/my_venv
```

## 2.激活虚拟环境

现在您已经创建了一个虚拟环境，您需要通过首先激活来指示您的操作系统使用它。为此，您需要调用`activate`脚本，该脚本位于您的虚拟环境的树结构中的`bin/`子目录下。

```
$ source my_venv/bin/activate
```

现在你应该注意到，在终端中，每一行都以`(my_venv)`开始，这表示当前名为`my_venv`的虚拟环境被激活。

一旦虚拟环境被激活，您安装或卸载的所有内容将只在该特定环境中生效，在其他地方不起作用。

## 3.停用虚拟环境

现在，如果您需要停用一个虚拟环境(假设您已经完成了您的包的开发，并且您可能需要在另一个上工作)，您可以通过在命令行中运行`deactivate`来这样做(这是一个内部实现细节)。

```
$ deactivate 
```

现在您应该注意到名为的虚拟环境(用括号括起来，如`(my_venv)`)已经消失了，这意味着当前没有活动的虚拟环境。

## 4.移除/删除虚拟环境

虚拟环境实际上只不过是一个自动创建的包含特定内容的树形目录。因此，为了去掉一个`venv`，你只需要从磁盘上删除这个目录。

```
$ rm -rf /path/to/my_venv
```

## 5.清除现有的虚拟环境

有时，您可能不想完全删除虚拟环境，而是希望清除以前安装的所有软件包。如果是这样的话，那么你只需要`clear`这个`venv`就可以了。为此，只需运行

```
$ python3 -m venv --clear path/to/my_venv
```

## 6.更新 Python 版本

现在，如果您想指示环境使用现有的 Python 版本，假设它已经就地升级，那么只需提供`--upgrade`标志:

```
$ python3 -m venv /path/to/my_venv --upgrade
```

## 7.允许虚拟环境访问系统站点包

正如已经强调的，安装在虚拟环境中的依赖项与安装在实际系统或任何其他环境中的相应包是隔离的。

现在，如果出于任何原因，你想让你的虚拟环境访问系统站点包目录，那么只需指定`--system-site-packages`标志来指示`venv`这样做。请注意，这不是一个推荐的做法，您应该只在真正需要时才使用它。

```
$ python3 -m venv /path/to/my_venv --system-site-packages
```

## 8.跳过 pip 安装

如果出于某种原因你想跳过`pip`(默认的软件包管理器)的安装和/或升级，那么你可以通过传递`--without-pip`标志来实现。

```
$ python3 -m vevn /path/to/my_venv --without-pip
```

# 最后的想法

在今天的文章中，我们讨论了在开发 Python 包时创建和使用虚拟环境的重要性。此外，我们还探讨了一些最常用的`venv`命令，为了正确创建、使用和管理您的虚拟环境，您应该了解这些命令。

通过运行 help 命令，您可以找到使用`venv`时所有选项的更全面列表:

```
$ python3 -m venv -h
```

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。**

## **你可能也会喜欢**

</14-must-know-pip-commands-for-data-scientists-and-engineers-a59ebbe0a439>  </scikit-learn-vs-sklearn-6944b9dc1736>  </16-must-know-bash-commands-for-data-scientists-d8263e990e0e> 