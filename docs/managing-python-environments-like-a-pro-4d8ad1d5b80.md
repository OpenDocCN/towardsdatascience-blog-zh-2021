# 像专家一样管理 Python 环境

> 原文：<https://towardsdatascience.com/managing-python-environments-like-a-pro-4d8ad1d5b80?source=collection_archive---------7----------------------->

## 还在用 virtualenv？试试这个新工具。

![](img/214a9322ca4c390825a590997d00c63c.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 拍摄

Python 虚拟环境帮助我们轻松地管理依赖性。最常见的环境创建工具是 [virtualenv](https://virtualenv.pypa.io/en/latest/) 和 [conda](https://docs.conda.io/en/latest/) ，后者用于多种语言的环境管理，而前者是专门为 python 开发的。

为什么不使用全局 python 包，那么我们就不需要陷入这种环境混乱，对吗？是的，这将节省我们管理环境的时间，但代价是，为项目做好准备的痛苦将呈指数增长。我通过艰难的方式了解到这个事实，对所有事情都使用全局包，而不是对每个项目都有一个专用的环境。

在这篇博客中，我将写关于 [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) 的内容，这是一个 python 库，用于管理和定制 python 中的环境，它运行在优秀的旧 virtualenv 之上。随着我们的进展，我们将看到 VEW CLI 命令如何类似于 Linux 命令，如 mkdir、rmdir 和 cp。

*注意:在本文中，我将把 virtualenvwrapper 称为 VEW*

## 开始前

值得注意的是，pyenv 与 virtualenv 或 VEW 没有关系。pyenv 用于在多个 python 版本之间切换，并且不管理已安装的包。另外，pip 是 python 中的一个包管理器，pip 也不能帮助我们管理环境，因为它不是用来做这个的。有关更多信息，请阅读此 stackoverflow 线程。

<https://stackoverflow.com/questions/38217545/what-is-the-difference-between-pyenv-virtualenv-anaconda>  

## 装置

安装过程与任何其他库相同。

```
pip install virtualenvwrapper
```

在 Linux 系统中，安装之后，我们需要编辑。bashrc 文件，这将允许用户在任何终端和位置访问 VEW。

```
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3export WORKON_HOME=~/my_env_folderexport VIRTUALENVWRAPPER_VIRTUALENV=/home/my_user/.local/bin/virtualenvsource ~/.local/bin/virtualenvwrapper.sh
```

1.  在第一行中，我们设置了 VIRTUALENVWRAPPER_PYTHON 变量，该变量指向 VEW 必须引用的 PYTHON 二进制安装
2.  接下来，WORKON_HOME 是 VEW 将存储所有环境和实用程序脚本的文件夹
3.  VIRTUALENVWRAPPER_VIRTUALENV 是原始 VIRTUALENV 二进制文件的路径

## 创建新的虚拟环境

如前所述，这里的命令类似于 Linux 命令。要创建一个新环境，请执行下面一行。

```
mkvirtualenv my-env
```

该环境将存储在 WORKON_HOME 变量中指定的路径中。除 virtualenv 选项外，还支持三个选项，它们是:

1.  -一个 *my_path* :环境的文件夹，表示每当环境被激活时，无论用户当前在什么路径，都会被重定向到 *my_path* 。这并不意味着环境将创建在 *my_path 中。*
2.  -i *package1 package2 …* :创建环境后，尝试安装提到的包。
3.  -r *requirements.txt* :创建一个环境，从 requirements.txt 文件安装。

## 删除虚拟环境

```
rmvirtualenv my_env
```

删除环境文件夹。请记住在删除环境之前将其停用。

## 显示环境的详细信息

```
showvirtualenv my-env
```

## 列出所有虚拟环境

```
lsvirtualenv
```

列出通过该工具创建的所有虚拟环境。使用-b 选项仅列出环境并忽略细节。

## 激活环境

virtualenv 使用以下命令激活环境。

```
source my-env/bin/activate
```

`source`是一个常用的 Linux 命令，主要用于改变当前 shell 的环境变量。点击阅读更多信息[。VEW 抽象了这个源命令，并提供了一个易于记忆的替代命令`workon`。](https://linuxhandbook.com/source-command/)

```
workon my-env
```

在引擎盖下，VEW 执行`source`命令。

## 停用环境

停用 VEW 的环境与 virtualenv 相同。在活动环境 shell 中，执行以下命令。

```
deactivate
```

## 卸载环境中的第三方软件包

```
wipeenv
```

该命令将在活动环境中运行。当这被执行时，VEW 将识别所有第三方库并卸载它们。

## 结论

虽然 virtualenv 可以很好地管理我们所有的环境，但 virtualenvwrapper 是一个推荐的附加组件。它与 Linux 命令的相似性使得操作更容易记忆。关注更多这样的文章。感谢阅读到最后:)