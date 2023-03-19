# 数据科学家和工程师必备的 14 个 pip 命令

> 原文：<https://towardsdatascience.com/14-must-know-pip-commands-for-data-scientists-and-engineers-a59ebbe0a439?source=collection_archive---------2----------------------->

## 探索一些对日常编程最有用的 pip 命令

![](img/401058cdc2c4fcdca486fed94ec12810.png)

照片由[凯利·麦克林托克](https://unsplash.com/@kelli_mcclintock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/package?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 介绍

即使 Python 标准库提供了广泛的工具和功能，开发人员仍然需要使用广泛可用的外部(标准库)包。大多数编程语言都有标准的包管理器，允许用户管理他们项目的依赖关系。

Python 中最常用的包管理器肯定是`pip`。在今天的文章中，我们将探索一些有用的`pip`命令，您可以使用它们来正确地管理 Python 项目的依赖性。

# 皮普是什么

`pip`是一个 Python 包管理器，支持安装不包含在该语言标准库中的包。相当于 JavaScript 的`npm`或者. NET 的`NuGet`

Python 是最流行的编程语言之一，当涉及到开发服务于特定目的的包时，它的社区非常活跃。这些包通常上传到 Python 包索引(PyPI)上。默认情况下，`pip`使用 PyPI 作为解析特定应用程序所需的所有依赖项的来源。

如果您想知道如何分发您自己的 Python 项目并使它们广泛可用，请务必阅读下面的指南。

[](/how-to-upload-your-python-package-to-pypi-de1b363a1b3) [## 如何将 Python 包上传到 PyPI

towardsdatascience.com](/how-to-upload-your-python-package-to-pypi-de1b363a1b3) 

如果您使用的是 Python 2 >=2.7.9 或 Python 3 >=3.4，那么`pip`应该已经安装在您的机器上了。如果您在使用`venv`或`virtualenv`创建的虚拟环境中工作，同样适用。

# 用于日常编程的 14 个 pip 命令

在接下来的章节中，我们将讨论 14 个常用的`pip`命令，每个 Python 开发人员都必须了解这些命令。请注意，强烈建议您为每个 Python 项目创建一个单独的虚拟环境，在其中您将能够安装和管理所有必需的依赖项。

## 1.安装软件包

为了在您当前的环境中安装一个包，您需要运行`pip install`,除了包本身，它还将安装它所有的依赖项。

```
$ python3 -m pip install package_name
```

## 2.卸载软件包

如果出于任何原因你想卸载一个软件包，你必须使用`pip uninstall`命令。

```
$ python3 -m pip uninstall package_name
```

但是请注意，这个命令**不会卸载这个包的依赖项**。如果出于某种原因，您也希望卸载未使用的依赖项，那么看看您可以使用的`[pip-autoremove](https://github.com/invl/pip-autoremove)`。

## 3.从需求文件安装依赖项

现在让我们假设您有一个`requirements.txt`,它列出了一个特定包所需的所有依赖项。

```
# requirements.txt
pandas==1.0.0
pyspark
```

您可以使用下面显示的命令直接从该文件安装所有的依赖项，而不是一个一个地检查包。

```
$ python3 -m pip install -r requirements.txt
```

## 4.列出已安装的软件包

如果你想列出一个环境中安装的所有 Python 包，`pip list`命令就是你要找的。

```
$ python3 -m pip list
```

该命令将返回所有已安装的软件包，以及它们的特定版本和位置。

```
Package                Version           Location                                            
---------------------- ----------------- -------------
pandas                 1.0.0
pyspark                2.1.0
```

如果从远程主机(例如 PyPI 或 Nexus)安装软件包，该位置将为空。当从计算机上的某个位置安装软件包时，该特定位置将列在输出的相应列中。

如果需要，你甚至可以使用`pip list`命令来获取特定包的版本。您可以使用

```
$ python3 -m pip list | grep package_name
```

## 5.自动创建 requirements.txt

在开发 Python 应用程序时，您可能需要安装其他依赖项，这些依赖项将帮助您实现您需要的任何功能。不必在一个`requirements.txt`文件中一个接一个地编写包，这样其他人也可以在使用你的包时安装所需的依赖项，你可以使用`pip freeze`在你的环境中安装所有的包。

```
$ python3 -m pip freeze > requirements.txt
```

请注意，该命令还将对您已经安装在您的环境中的软件包的特定版本进行硬编码(例如`pandas==1.0.0`)，这在某些情况下可能不是您真正想要的。在这种情况下，您可能需要手动删除或修改版本。

## 6.在可编辑模式下安装软件包

现在，如果你正在本地开发一个包，你可能必须在**可编辑模式**下安装它。这意味着安装会将软件包链接到指定的本地位置，这样对软件包所做的任何更改都会直接反映到您的环境中。

```
$ cd /path/to/your/local/python/package
$ python3 -m pip install -e .
```

以上命令将以`editable`(或`develop`)模式将软件包安装在当前目录中。现在，如果您运行`pip list | grep package_name`，包的位置应该映射到您本地机器上的位置(例如`path/to/your/local/python/package`)。

## 7.升级软件包

如果您安装的某个软件包已经过时，并且您希望升级它(总是推荐，但是这可能会破坏与其他软件包的兼容性)，您可以通过运行

```
$ python3 -m pip install package_name --upgrade
```

## 8.删除 pip 安装的所有软件包

现在，如果出于某种原因你想删除 pip 安装的所有软件包，那么你可以运行下面的命令，用现有的已安装软件包创建一个中间的`requirements.txt`文件，然后用`pip uninstall`卸载所有的软件包。

```
$ python3 -m pip freeze > requirements.txt && python3 -m pip uninstall -r requirements.txt -y
```

如果您想跳过任何中间文件(即`requirements.txt`)的创建，那么您可以运行下面的命令。

```
$ python3 -m pip uninstall -y -r <(pip freeze)
```

## 9.检查已安装的软件包是否兼容

```
$ python3 -m pip check
```

如果没有发现兼容性问题，该命令的输出将是

```
No broken requirements found.
```

## 10.获取特定包的信息

如果你想检索关于一个特定包的信息，`pip show`将返回详细信息，如名称、版本、概要、许可证、依赖关系等。

```
$ python3 -m pip show package_name
```

`pandas`的输出示例如下所示:

```
**$ pip show pandas**Name: pandas
Version: 1.2.3
Summary: Powerful data structures for data analysis, time series, and statistics
Home-page: [https://pandas.pydata.org](https://pandas.pydata.org)
Author: None
Author-email: None
License: BSD
Location: /path/to/lib/python3.6/site-packages
Requires: pytz, python-dateutil, numpy
Required-by: shap, seaborn
```

## 11.搜索 PyPI 包

正如我们已经提到的，`pip`使用 PyPI 作为它检索包的默认来源。`pip search`命令用于搜索索引并识别与搜索词匹配的包。

```
$ python3 -m pip search <search_term>
```

例如，`python3 -m pip search pandas`将返回满足搜索词`pandas`的所有包。

## 12.为您的需求和依赖关系构建 Wheel 档案。

[Wheels](https://wheel.readthedocs.io/en/latest/) 是 Python 生态系统的一部分，它是内置的包格式，提供了在每次安装时不重新编译软件的优势。`pip wheel`命令用于为您的需求和依赖项构建 Wheel 档案。

```
$ python3 -m pip wheel
```

注意 pip 使用来自`wheel`包的`bdist_wheel` setuptools 扩展来构建单独的轮子。

## 13.管理本地和全局配置

`pip`自带配置，显然可以根据您的使用需求进行修改。`pip`带有`config`选项，允许使用`edit`、`get`、`list`、`set`或`unset`配置。

```
$ python3 -m pip config <action name>
```

例如，要列出启用的配置选项，可以运行

```
$ python3 -m pip config list
```

为了设置特定的配置选项，那么可以使用`set`方法。例如，如果您需要更改`global.index`配置参数，您必须运行

```
$ python3 -m pip config set global.index-url https://your.global.index
```

## 14.显示调试信息

现在，如果您想检索特定于 pip 的调试信息，您可以使用`pip debug`。该命令将返回与 pip 安装和本地系统环境相关的信息。

```
$ python3 -m pip debug
```

请注意，此命令仅用于调试，因此您不得在生产工作流程中使用它来解析或推断细节，因为该命令的输出可能会在没有通知的情况下发生变化。

# 最后的想法

在今天的文章中，我们快速了解了 Python 中最常用的包管理器`pip`。此外，我们引入了 Python 包索引(或 PyPI ),它托管了 Python 开发人员可用的所有第三方包。最后，我们探讨了 14 个不同的`pip`命令，为了正确管理依赖关系，您一定要知道这些命令。

对于`pip`选项的完整列表，只需在您的终端中运行`pip --help`，该命令将返回使用信息。

```
Usage:pip <command> [options]Commands:
install      Install packages.
download     Download packages.
uninstall    Uninstall packages.
freeze       Output installed packages in requirements format.
list         List installed packages.
show         Show information about installed packages.
check        Verify installed packages have compatible dependencies.
config       Manage local and global configuration.
search       Search PyPI for packages.
cache        Inspect and manage pip's wheel cache.
wheel        Build wheels from your requirements.
hash         Compute hashes of package archives.
completion   A helper command used for command completion.
debug        Show information useful for debugging.
help         Show help for commands.
```