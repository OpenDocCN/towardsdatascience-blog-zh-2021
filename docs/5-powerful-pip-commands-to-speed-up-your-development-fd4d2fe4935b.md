# 5 个强大的 Pip 命令来加速您的开发⏰

> 原文：<https://towardsdatascience.com/5-powerful-pip-commands-to-speed-up-your-development-fd4d2fe4935b?source=collection_archive---------21----------------------->

## 专家提示

## 你以为安装 Python 包就是 pip 能做的一切吗？

![](img/e993da9715ea005df5ea8334c6d5e3a9.png)

Marc-Olivier Jodoin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 您的旅程概述

*   [设置舞台](#07ce)
*   [快速:什么是 PIP？](#0e13)
*   [1 —安装软件包](#8949)
*   [2 —更新软件包](#afc7)
*   [3 —卸载软件包](#0dea)
*   [4 —查看您已安装的软件包](#ac68)
*   [5 —检出特定的包](#841b)
*   [额外提示—检查缺失的依赖关系](#99fb)
*   [收尾](#885d)

# 搭建舞台

学习 Python 的时候，很快就给你介绍了 pip — [Python 的包安装器](https://pip.pypa.io/en/stable/)。这是将外部包安装到您的 Python 生态系统的主要工具。

Python 的主要卖点之一是第三方包的丰富生态系统。例如，软件包 **NumPy** 、 **Matplotlib** 、 **Pandas** 、 **Seaborn** 和 **Scikit-Learn** 都在数据科学中使用。事实上，在不使用上述任何库的情况下构建一个数据科学项目将是一个挑战(不是以一种好的方式！).

当我开始使用 Python 时，我只知道 pip 命令:

```
pip install <package name>
```

不错，但是 pip 有更多的命令。如果我知道这些命令，我会节省几个小时的时间。不要犯我的错误！

> 在这篇博文中，我将向您展示如何使用 5 个最强大的 pip 命令。

先决条件:除了安装 pip，你不需要任何 pip 的知识😃

# 快问:皮普是什么？

Pip 是 Python 开发者用来管理包的主要工具。这包括(您将很快看到):

*   下载包
*   更新包
*   删除包
*   查看已安装的软件包
*   签出特定的包

> **你应该知道:** Pip 并不是用 Python 管理包的唯一方式。在数据科学中，使用 [conda 包管理器](https://docs.conda.io/en/latest/)处理 Python 包也很常见。

知道安装的 pip 版本是很好的:要检查这一点，您可以运行命令

```
python -m pip --version**Output:** pip 21.2.3 from <path to pip>
```

一般来说，保持 pip 更新是一种良好的做法。要获得 pip 的最新版本，请使用以下命令:

```
python -m pip install --upgrade pip
```

# 1 —安装软件包

## 基本安装

pip 的第一个也是最重要的用途是安装第三方软件包。假设您想要安装 [Seaborn](https://seaborn.pydata.org/) 来创建可爱的数据可视化。为此，只需运行以下命令:

```
python -m pip install seaborn**Output:** Successfully installed seaborn-0.11.1 (and dependencies)
```

命令`pip install`是您最常使用的 pip 命令。默认情况下，它会安装最新版本的 Seaborn。此外，它会自动安装任何依赖项(Seaborn 依赖的其他库)。

如果您出于某种原因不想要依赖项，那么您可以使用标志`--no-deps`:

```
python -m pip install --no-deps seaborn**Output:** Successfully installed seaborn-0.11.1 (and nothing else)
```

## 安装特定版本

但是有时候，你并不真的想要下载一个包的最新版本。这通常是为了避免破坏与其他包的兼容性。要下载软件包的特定版本，请用两个等号指定:

```
python -m pip install numpy==1.20.0**Output:** Successfully installed numpy-1.20.0
```

## 从需求文件安装

假设您用 Python 开发了一个非常棒的数据科学应用程序。您的应用程序只使用三个包:

*   NumPy(版本 1.21.1)
*   熊猫(版本 1.3.1)
*   Seaborn(版本 0.11.1)

这个应用程序太酷了，你不得不与你的朋友分享。你把 Python 文件发给他们。但是，他们不能运行 Python 文件，除非他们也安装了 NumPy、Pandas 和 Seaborn。让你的朋友一个接一个地安装它们(用正确的版本)会有多累？

幸运的是，有一个更好的方法:一个 **requirements.txt** 文件。在包含这三行代码的项目文件夹中创建一个名为 **requirements.txt** 的文件:

```
numpy==1.21.1
pandas==1.3.1
seaborn==0.11.1
```

当您的朋友收到您的项目时，她可以简单地运行命令:

```
python -m pip install -r requirements.txt**Output:** Successfully installed cycler-0.10.0 kiwisolver-1.3.1 matplotlib-3.4.2 pandas-1.3.1 pillow-8.3.1 seaborn-0.11.1
```

> **你应该知道:**`-r`旗是`--requirements`的简写，所以你可能会看到两者都被使用。另外，文件名 **requirements.txt** 是一个约定。然而，通常没有一个好的理由来打破惯例。

# 2-更新包

这是一个短的。新版本的软件包经常出现。假设您想升级到 NumPy 的最新版本。然后，您可以简单地运行命令:

```
python -m pip install -U numpy**Output:** Successfully installed numpy-1.21.1
```

旗帜`-U`是`--upgrade`的简写。两者都可以接受，你可以自由选择使用哪一个👍

# 3 —卸载软件包

有时候，你想卸载一个软件包。这可能是因为该包与其他包产生了冲突。

不管是什么原因，使用命令`pip uninstall`都很简单。尝试运行以下命令:

```
python -m pip uninstall numpy**Output:** Successfully uninstalled numpy-1.21.1
```

执行上述命令时，通常需要输入`Y`，然后按回车键进行验证。这是 pip 在问:“你真的确定要卸载 NumPy 吗？”

我觉得皮普的直升机家长方式很烦人。如果你有同样的感觉，那么你可以使用标志`-y`(T2 的简写)来避免被问到:

```
python -m pip uninstall -y numpy**Output:** Successfully uninstalled numpy-1.21.1
```

# 4 —查看已安装的软件包

如果您想知道您的系统上安装的 Python 包，那么使用命令`pip freeze`:

```
python -m pip freeze**Output:** 
appdirs==1.4.4
atomicwrites==1.4.0
attrs==21.2.0
beautifulsoup4==4.9.3
black==21.7b0
.
.
.
uvicorn==0.13.4
Werkzeug==2.0.1
```

`pip freeze`命令显示了每个已安装的 Python 包和相应的版本号。

嗯……输出的格式看起来很熟悉。这正是我之前谈到的 **requirements.txt** 文件的格式！

如果您想将您安装的软件包保存在一个需求文件中，那么您可以编写:

```
python -m pip freeze > requirements.txt
```

该命令创建一个名为 **requirements.txt** 的新文件，并用`pip freeze`的输出填充它。这为您提供了一种生成 **requirements.txt** 文件的简单而巧妙的方法🔥

> **你要知道:**我们称`output > file`为[标准输出流](https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/)。这与 pip 无关，是(有趣的)终极废话。简而言之，该语法将左边的输出发送到右边的文件。

# 5 —检出特定的包

这是我个人最喜欢的 pip 命令。假设您安装了 NumPy。

问题 1:NumPy 有哪些依赖关系？

一个更难的问题:

**问题 2:** 你的其他哪些包依赖 NumPy？

获取这些信息(以及更多！)你可以使用命令`pip show`:

```
python -m pip show numpy**Output:** 
Name: numpy
Version: 1.21.1
Summary: NumPy is the fundamental package for array computing with Python.
Home-page: [https://www.numpy.org](https://www.numpy.org)
Author: Travis E. Oliphant et al.
Author-email:
License: BSD
Location: <path to numpy on my system>
Requires:
Required-by: seaborn, scipy, pandas, matplotlib
```

仔细阅读上面的信息。最重要的是:

*   包的名称。
*   安装的版本。
*   包的摘要。
*   包的主页。
*   软件包在系统上的位置。
*   依赖关系(根据需要编写)。
*   需要这个包的其他包(按 Required-by 编写)。

注意`pip show`解决了我们的两个问题:

**答案 1:** NumPy 不需要任何其他外部 Python 包。

**回答 2:**Seaborn、SciPy、Pandas、Matplotlib 这些包都需要 NumPy。

如果你需要一些关于软件包的快速信息，那么我强烈推荐使用`pip show`😎

# 额外提示—检查缺失的依赖项

![](img/f395ad3665ee32b66a57df70b5abdf05.png)

[Bermix 工作室](https://unsplash.com/@bermixstudio?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

说我装了 NumPy，Matplotlib，Pandas，Seaborn。然而，出于某种原因，我已经设法卸载 NumPy 错误。现在 Matplotlib、Pandas 和 Seaborn 将无法工作，因为它们都依赖于 NumPy😧

你如何发现什么是错的？只需运行命令`pip check`:

```
python -m pip check**Output:** 
seaborn 0.11.1 requires numpy, which is not installed.
scipy 1.7.1 requires numpy, which is not installed.
pandas 1.3.1 requires numpy, which is not installed.
matplotlib 3.4.2 requires numpy, which is not installed.
```

命令`pip check`将打印出所有 Python 包中所有缺失的依赖项。现在你意识到 NumPy 不见了，你可以简单地用`pip install`把它装回去。

# 包扎

现在，您应该可以放心地使用 pip 来满足您的所有软件包需求。如果您需要了解更多关于 pip 的信息，请查阅 [pip 文档](https://pip.pypa.io/en/stable/')。

**喜欢我写的？**查看我的博客文章[类型提示](/modernize-your-sinful-python-code-with-beautiful-type-hints-4e72e98f6bf1)、[黑色格式](/tired-of-pointless-discussions-on-code-formatting-a-simple-solution-exists-af11ea442bdc)、Python 中的[下划线](https://medium.com/geekculture/master-the-5-ways-to-use-underscores-in-python-cfcc7fa53734)和 [5 个字典提示](/5-expert-tips-to-skyrocket-your-dictionary-skills-in-python-1cf54b7d920d)了解更多 Python 内容。如果你对数据科学、编程或任何介于两者之间的东西感兴趣，那么请随意在 LinkedIn 上加我，并向✋问好