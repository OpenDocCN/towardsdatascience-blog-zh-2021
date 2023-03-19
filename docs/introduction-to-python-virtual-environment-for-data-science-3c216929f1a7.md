# 面向数据科学的 Python 虚拟环境简介

> 原文：<https://towardsdatascience.com/introduction-to-python-virtual-environment-for-data-science-3c216929f1a7?source=collection_archive---------37----------------------->

## 数据科学最佳实践

## 了解虚拟环境的基础知识

如果您使用 Python 已经有一段时间了，那么您可能会遇到这样的情况:即使您已经安装了所需的包，在其他人的计算机上运行完全相同的脚本却不能在您的计算机上运行。这可能是因为两台计算机上使用的 Python 环境不同(例如，可能使用两个不同版本的包)。能够重现准确的输出不仅依赖于准确的脚本，还依赖于正确的环境设置。我们可以通过创建虚拟环境来确保为我们的项目建立合适的 Python 环境。

> 虚拟环境可以帮助我们提高项目的可重复性，更容易与其他人协作，并使部署更加顺利。

在本帖中，我们将熟悉一个虚拟环境管理器: *venv* 并了解虚拟环境的基础知识。

![](img/ff60d2ca27bded162d0b8f0882da634b.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 📍基础

## 0.设置

*这篇文章假设你的电脑上已经安装了 Python 3.3+。*

Python 标准库包括 *venv* ，因此当你安装 Python 时默认安装它。因此，我们不需要安装任何额外的包来开始。

## 1.转到项目目录

我们将使用命令行来键入命令。这里有一种打开命令行界面的方法，取决于你的操作系统:
◼️如果你在 Mac 上，打开一个终端窗口，步骤如下:
➡️按 cmd +空格键➡️类型终端➡️按 enter
◼️如果你在 Windows 上，打开一个命令提示窗口，步骤如下:➡️按 windows 键➡️键入 cmd ➡️按 enter

现在将您的工作目录更改为您的项目目录。例如，如果您的项目目录在桌面上，下面的脚本将帮助您实现这一点。如果您的项目文件夹在其他地方，您可以相应地调整脚本。

```
$ cd Desktop/project # Mac
$ cd Desktop\project # Windows
```

## 2.创建虚拟环境

既然我们现在在项目文件夹中，让我们创建一个虚拟环境。在本例中，我们选择将虚拟环境命名为'*。*。这将创建一个包含虚拟环境文件的隐藏(因为名称以点开头)目录。你可以替换掉'*。用你喜欢的任何名字。但是，如果您对虚拟环境的名称没有强烈的偏好，我建议遵循我们在这里使用的相同命名约定:'*。venv'* 。这个命名约定得到了[布雷特·卡农](https://www.linkedin.com/in/drbrettcannon/?originalSubdomain=ca)的认可。如果你好奇，这里是他关于这个话题的讨论。*

要创建一个虚拟环境，我们只需要运行下面的一个命令行程序:

```
$ python -m venv .venv
```

这将使用基础 Python 的相同 Python 版本(或[最新 Python 版本，如果您有多个版本](https://docs.python.org/3/tutorial/venv.html))创建一个虚拟环境。一旦执行了这个命令，您会注意到您有了一个名为'*'的新文件夹。venv'* 。在创建虚拟环境之前和之后，如果您在 Mac 上，可以使用`ls -A`检查项目目录中的内容；如果您在 Windows 上，可以使用`dir`检查项目目录中的内容。

## 3.激活虚拟环境

让我们检查一下你已经安装在你的基础 Python 上的包的版本(即 Python 的系统安装)。在本例中，我们将选择 *numpy* ,但它可以替换为您选择的任何第三方软件包:

```
$ python -c "import numpy as np; print(np.__version__)"
```

它应该返回您的基本 Python 环境中的包版本。现在，让我们激活虚拟环境并再次检查软件包版本:

```
# Activate virtual environment
$ source .venv/bin/activate # Mac
$ .venv\Scripts\activate # Windows# Check again
$ python -c "import numpy as np; print(np.__version__)"
```

当我们运行与之前完全相同的脚本时，它会返回一个错误。这是因为我们刚刚创建的新虚拟环境被激活，我们不再使用基本的 Python 环境。判断虚拟环境已激活的最简单方法是在命令行的开头查找环境名称。

## 4.设置/准备虚拟环境

当我们创建一个虚拟环境时，它就像一张白纸。我们需要将所有必需的包安装到环境中来设置它。将软件包安装到虚拟环境非常简单。你可以像平常安装软件包一样使用`pip`。在这样做的时候，您需要确保您的虚拟环境是激活的。

让我们安装一个与基础环境中的版本不同的特定版本的包。我们将继续我们的 *numpy* 示例，并选择版本 1.19.4(如果这恰好与您的基本版本 *numpy* 相同，请选择另一个版本)进行本练习:

```
# Install numpy v1.19.4
$ pip install numpy==1.19.4# Check again
$ python -c "import numpy as np; print(np.__version__)"
```

这应该会返回 1.19.4。因此，我们只是在虚拟环境中安装了不同版本的 *numpy* 。在上面的例子中，如果我们只想安装软件包的最新版本，我们可以键入软件包名称而不指定任何版本:`pip install numpy`

更常见的是，我们需要根据概述必要包版本的 *requirements.txt* 文件来准备我们的虚拟环境。下面是如何根据 *requirements.txt* 中概述的规范安装软件包:

```
$ pip install -r requirements.txt
```

一旦使用必要的包设置并激活了您的环境，您就可以开始使用该环境为项目开发和测试 Python 代码了。

## 5.在虚拟环境中导出包版本

有时，您可能需要导出您创建的虚拟环境的需求。我们只用一行代码就可以做到:

```
$ pip freeze > requirements.txt
```

如果您想检查您刚刚保存的 *requirements.txt* 的内容，您可以像打开其他文件一样打开它。txt 文件。或者，您可以使用以下脚本在命令行解释器中查看其内容:

```
$ cat requirements.txt # Mac
$ type requirements.txt # Windows
```

## 6.停用虚拟环境

在环境中完成工作后，如果您想切换回基本 Python，您需要做的就是像这样停用环境:

```
$ deactivate
```

运行此命令后，命令行的前缀应该不再显示虚拟环境的名称。这将向您确认虚拟环境已被停用。

## 7.删除虚拟环境

如果您想要删除虚拟环境，只需删除文件夹(*)。venv'* 文件夹)就像你通常删除任何文件夹一样。如果您希望从命令行删除，可以使用以下脚本:

```
$ rm -r .venv # Mac
$ rd /s .venv # Windows, when prompted press Y and enter
```

# 摘要🍀

总结一下，下面是我们在这篇文章中访问过的主要命令的整理摘要:

```
# Create a virtual environment
$ python -m venv <environment_name># Activate a virtual environment
$ source <environment_name>/bin/activate # Mac
$ <environment_name>\Scripts\activate # Windows# Install required packages from requirements.txt
$ pip install -r requirements.txt# Install a package with a specified version
$ pip install <package_name>==<version># Export installed libraries to requirements.txt
$ pip freeze > requirements.txt# Deactivate virtual environment
$ deactivate
```

通常你会在项目开始的时候建立一个虚拟环境(可能在这里有一个小的调整，稍后需要)。设置完成后，典型的项目工作流程主要涉及激活和停用虚拟环境。

![](img/c6908ddad52ce00c1e6532852b130e7e.png)

阿诺德·弗朗西斯卡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*您想访问更多这样的内容吗？媒体会员可以无限制地访问媒体上的任何文章。如果您使用* [*我的推荐链接*](https://zluvsand.medium.com/membership)*成为会员，您的一部分会费将直接用于支持我。*

谢谢你看我的帖子。希望你在使用 *venv* 时感觉更舒服。对于那些狂热的学习者，这里有一些其他流行的创建虚拟环境的方法:
◼️ [康达虚拟环境](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)t13】◼️[pyenv](https://github.com/pyenv/pyenv)用于管理多个版本的 Python

如果你喜欢这篇文章，这里有我的一些其他文章的链接:
◼️[git 数据科学简介](/introduction-to-git-for-data-science-ca5ffd1cebbe?source=your_stories_page-------------------------------------)
◼️ [用这些提示整理你的 Jupyter 笔记本](/organise-your-jupyter-notebook-with-these-tips-d164d5dcd51f)
◼️[python 中的简单数据可视化，你会发现有用的](/simple-data-visualisations-in-python-that-you-will-find-useful-5e42c92df51e)
◼️ [在 Seaborn (Python)中更漂亮和定制情节的 6 个简单提示](/6-simple-tips-for-prettier-and-customised-plots-in-seaborn-python-22f02ecc2393)
◼️️ [给熊猫用户的 5 个提示](/5-tips-for-pandas-users-e73681d16d17)
◼️️

再见🏃💨