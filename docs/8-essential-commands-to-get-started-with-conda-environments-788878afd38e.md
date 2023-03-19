# 开始使用 Conda 环境的 8 个基本命令

> 原文：<https://towardsdatascience.com/8-essential-commands-to-get-started-with-conda-environments-788878afd38e?source=collection_archive---------22----------------------->

## 开始为所有数据科学项目使用环境

![](img/5d7d53d5973d035c5830b5270c11469d.png)

本杰明·格兰特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 介绍

对于 Python 初学者来说，虚拟环境不是最直接的概念。当我们安装软件时，比如微软 Office 和 Evernote，我们大多数人都习惯于应用默认配置，这包括为你电脑上的所有用户安装软件。换句话说，软件被安装在系统级，使得不同的用户之间只共享软件的一个副本。

这是我们大多数人多年来养成的习惯。我们在开始学习 Python 的时候就养成了这个习惯。我们在电脑上安装了 Python，学会了直接给系统安装各种 Python 包。随着 Python 在我们工作中的使用越来越多，当事情开始变得混乱时，我们有机会为不同的工作任务使用不同的包。

软件包 A 依赖于版本为 2.0 的软件包 B，所以当我们在系统级安装软件包 A 时，软件包 B 2.0 也会在系统级安装。然而，我们的另一个项目需要包 C，它依赖于版本为 2.5 的包 B。如果我们安装软件包 C，我们的系统会将软件包 B 更新到版本 2.5，以满足软件包 C 的要求。不幸的是，软件包 A 尚未更新到与软件包 B 2.5 兼容，因此我们在使用软件包 A 时会遇到问题。

这只是许多初学者在学习 Python 的过程中迟早会遇到的一个常见问题。除了这些包依赖冲突之外，还有一些项目需要不同版本的 Python。例如，可能一些遗留项目仍然使用 Python 2.x，而大多数项目使用 Python 3.x。即使对于那些使用 Python 3.x 的项目，一些项目可能工作到 Python 3.4，而另一些项目需要 Python 3.5 以上。因此，如果 Python 只安装在所有项目的系统级，Python 版本冲突可能是另一个问题。

康达环境来拯救。Conda 是一个全面的包和环境管理工具，尤其适用于数据科学家。最重要的是，conda 还优化了数据科学相关的库，如 NumPy、SciPy 和 TensorFlow，这些库最大限度地提高了可用硬件的性能，以获得最佳的计算能力(更多介绍可在 conda 的[网站](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/data-science.html)上找到)。使用 conda 工具，我们为我们的项目创建隔离的环境，这些项目可以有不同的包，甚至不同的 Python 版本。

如果你的电脑上还没有安装康达，可以参考[官网](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html)了解详细的安装说明。conda 有两个版本:Miniconda 和 Anaconda，前者提供 conda 及其依赖项，因此更加紧凑，而后者提供更多科学研究所需的包。一旦安装了 conda，您应该能够在终端(或者您选择的其他命令工具)中运行下面的命令。

```
conda --version
```

提示符将显示您计算机上安装的 conda 版本。这样的话，你可以走了。让我们探索一下在大多数情况下管理环境时应该知道的最基本的命令。

## 1.检查可用环境

让我们先做一个清单检查，看看有哪些可用的环境。运行以下命令。

```
conda env list
```

假设您刚刚安装了 conda，您可能会看到以下内容。

```
# conda environments: 
# 
base * /opt/anaconda3
```

*   `base`是 conda 在安装过程中为我们创建的默认环境。
*   星号表示该特定环境是活动环境。
*   该路径显示了环境及其相关包的物理目录。

当我们想要对我们的计算机上的 conda 环境有一个总的了解时，这个清单检查命令是有用的。

## 2.创建新环境

要创建新环境，请运行以下命令。

```
conda create --name firstenv
```

*   标志`--name`表示我们将要指定新环境的名称。在这种情况下，新创建的环境将被命名为`firstenv`。
*   作为一种快捷方式，你可以用`-n`代替`--name`。

当 conda 为您创建环境时，可以扩展这个基本命令来安装额外的包。例如，以下命令将为您安装 numpy 和请求。

```
conda create -name firstenv numpy requests
```

此外，我们甚至可以指定这些包的特定版本，如下所示。

```
conda create -n firstenv numpy=1.19.2 requests=2.23.0
```

安装完成后，您可以通过运行`conda env list`来验证环境是否已经成功创建，这将向您显示`firstenv`在列表中。

如果您知道环境需要哪些包，那么您可能希望在创建环境时安装所有的包，因为单独的安装可能会导致依赖冲突。当它们全部安装在一起时，任何潜在的冲突都会得到解决。

## 3.激活环境

一旦创建了环境，就该“进入”环境了。用一个更标准的术语来说，这叫激活环境。运行以下代码。

```
conda activate firstenv
```

一旦您运行了这个，您应该会注意到，如果您选择了不同的名称，提示前面会加上`(firstenv)`或`(your-own-env-name)`，如下所示。

```
(firstenv) your-computer-name:directory-path your-username$
```

由于`firstenv`被激活，出于好奇，你可以再次运行`conda env list`。你会发现星号会出现在`firstenv`环境的行中。

```
# conda environments: 
# 
base /opt/anaconda3 
firstenv * /opt/anaconda3/envs/firstenv
```

## 4.安装、更新和卸载软件包

由于环境已经安装，您意识到您想要安装额外的软件包，而您在创建环境时忘记了指定这些软件包。根据软件包的可用性，有几种安装软件包的方法。

您可以首先尝试运行以下命令，该命令将尝试从默认通道 anaconda 安装软件包。顺便说一下，如果需要，可以在包名后面指定版本。

```
conda install pandas
```

但是，默认的 anaconda 通道可能没有特定的包。在这种情况下，您可以尝试另一个名为`conda-forge`的公共通道，如下所示。`--channel`或简称为`-c`表示获取包的通道。

```
conda install -c conda-forge opencv
```

Python 包的另一个重要渠道是众所周知的 Python 包索引，通过 Python 包管理工具`pip`。它由 conda 原生支持。下面是一个简单的例子。

```
pip install pandas
```

尽管有多个渠道可以安装相同的包，但还是建议您尽可能多地从 anaconda 获得它们，因为它们可能已经过优化以获得更好的性能。另外，要了解更多的渠道，可以参考[网站](https://docs.anaconda.com/anaconda/navigator/tutorials/manage-channels/)。

要更新软件包，根据安装通道，您可以简单地用`update`命令替换`install`。

要卸载这个包，我们将使用`uninstall`命令，如下所示。需要注意的是，该命令是`remove`的别名(如`conda remove pandas`)。

```
conda uninstall pandas
```

## 5.检查已安装的软件包

当您在您的环境中工作时，您安装了越来越多的包，在某些时候，您可能想要检查已经安装了哪些包。为此，请运行以下命令。

```
conda list
```

您可能会看到如下所示的内容。

已安装的软件包

值得注意的一点是，列表会显示软件包是从哪个通道安装的。在这种情况下，我们有来自 pypi 和 conda-forge 的包。当没有通道信息时，这意味着软件包是从默认通道(anaconda)安装的。

如果您认为列表太长而无法识别某个特定的包，您可以通过运行下面的命令来检查单个包的信息。

```
conda list opencv
```

## 6.停用环境

如果你完成了工作，你就准备离开当前的环境。在虚拟环境术语中，它被称为停用环境。为此，请运行以下命令。

```
conda deactivate
```

运行这个命令后，您会注意到前缀`(firstenv)`从命令行的提示中删除了。如果感兴趣，您可以运行`conda env list`来查看活动星号是否不再与`firstenv`环境相关联。

## 7.共享环境

可再现性是 conda 和任何其他环境管理工具想要实现的一个关键目标，这样在您的计算机上运行的代码也可以在您的队友的计算机上运行，而不会出现任何问题。因此，我们可以在不同的计算机上重建相同的环境，这一点很重要。有几个命令对共享环境很有用。

进入虚拟环境后，运行下面的命令。

```
conda env export > environment.yml
```

该命令将在您的当前目录中创建一个需求文件，该文件列出了当前环境的信息，例如名称、依赖项和安装通道。你可以打开文件看一看，这只是一个文本文件。

你与那些想要重现这种环境的人分享这个文件。要使用该文件重建环境，只需运行下面的命令。如果需要，可以编辑`yml`文件使其工作，比如更改环境的名称。

```
conda env create -f environment.yml
```

`-f`标志表示我们想要从指定的文件中创建一个环境。如果您已经导航到`environment.yml`所在的目录，您可以简单地运行`conda env create`。

## 8.移除环境

当我们需要删除不再使用的环境时，运行以下命令。

```
conda remove -n firstenv --all
```

*   `-n`标志指定环境的名称。
*   `-all`标志指定这个环境中的所有包都将被删除。这意味着环境也将被移除。

运行该命令后，出于好奇，您可以运行`conda env list`来检查环境是否已被成功移除。

## 结论

在本文中，我们回顾了 8 个基本的 conda 命令(从技术上讲，提到了 8 个以上)。我希望它们能对您开始使用 conda 来管理数据科学项目的环境有所帮助。这里有一个快速回顾，突出了关键点。

*   为每个项目创建一个新环境。不要担心耗尽您的磁盘空间，因为环境不会占用太多空间。
*   在创建环境时安装尽可能多的包，因为任何潜在的依赖冲突都将被处理。
*   如果 anaconda 没有您需要的包，请使用其他渠道。但是，请始终将 anaconda 视为优先通道。
*   考虑共享环境，以使用相同的包再现环境。