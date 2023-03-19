# 如何使用 Bash 来自动化数据科学的枯燥工作

> 原文：<https://towardsdatascience.com/how-to-use-bash-to-automate-the-boring-stuff-for-data-science-d447cd23fffe?source=collection_archive---------7----------------------->

## 使用命令行为您的数据科学项目编写一些可重用代码的指南

![](img/700e28b1c0b87592af6cc15717261fe0.png)

伊莲娜·科伊切娃在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

作为一名数据科学家，您倾向于通过终端反复使用一些命令。这些可能是命令*为一个项目创建一个新目录*，*启动一个新的虚拟环境*和*激活*它，安装一些*标准库*等等。

当你开始一个项目的工作时，你自己的标准工作流程可能每次都不一样。

**例如，**我总是为一个新项目创建一个新文件夹，并通过以下方式将其移入:

```
mkdir newproject
cd newproject
```

然后我通过以下方式创建了一个新的虚拟环境:

```
pipenv shell # orpython -m venv newenv
```

最后，我还做了 **numpy** 和 **pandas** 作为项目的样板安装。为了形象化的目的，我也经常使用 **matplotlib** 和 **plotly** 。

事实上，我也喜欢为我的机器学习应用程序快速开发 web 应用程序，所以我也倾向于安装 streamlit,因为它是最好的库。如果你不太了解它，这里有一个我写的快速介绍，让你开始使用它。

因此，到目前为止我们需要执行的命令是:

```
pip install numpy pandas matplotlib plotly streamlit
```

如果您忘记添加库，您必须返回并通过终端重新安装它们。现在来看，通过一个命令来自动完成这一切不是一个好主意吗？

> 你可以使用一个脚本，每次执行**时自动执行一些重复的命令，这样可以提前一点，节省一些宝贵的时间。**

在本文中，我将演示一个简单的命令行过程，您可以很容易地习惯于有效地自动化枯燥的东西，这是我经常使用的一种方法。

我们开始吧！👇

# 检查系统上的 bash

了解 bash 在系统中的位置的一个简单方法是使用:

```
$ which bash
```

输出将类似于:

```
**/bin/bash**
```

检查 bash 版本:

```
bash --version
```

输出应该如下所示:

![](img/65ceab103c01d19fb00bc910d3a659ff.png)

例如，Mac 上的 bash 版本

太好了，现在我们有了一点信息，让我们看看我们可以用它来构建什么。

# 制作新的 bash 脚本

我们将创建一个包含所有要执行的样板命令的文件。这将被称为我们的 **bash 脚本**，它将具有**的扩展。sh** 就一般惯例而言。

首先，创建一个新文件。

```
touch createmlapp.sh
```

接下来，让我们在文件的顶部添加一行代码，以确保系统知道使用默认的 bash shell 来运行我们的脚本。

```
#!/bin/bash
```

现在，让我们明白我们在这里想要做什么。我们的流程如下:

1.  为我们的项目创建一个新目录
2.  创建和激活虚拟环境
3.  安装我们需要的任何软件包
4.  打开项目目录中的 VSCode。

让我们现在过一遍。

# 写我们的剧本

我们所有的命令都将类似于我们在终端中正常运行的命令。

只有一个区别——为了使我们的项目成功，我们需要一个名字，我们将通过一个参数传递这个名字。

```
APP_NAME="$1"
```

执行脚本时输入的第一个参数将是我们的 **$1** 。

现在，剩下的代码将是熟悉的:

```
cd
cd Desktop/ # use whatever directory you wish here 
mkdir $APP_NAME
cd $APP_NAME
```

现在进入虚拟环境:

```
python -m venv newenv
source newenv/bin/activate
```

最后，我们安装我们需要的任何软件包:

```
pip install --upgrade pip
pip install numpy pandas matplotlib streamlit
```

最后，作为奖励，让我们打开我们最喜欢的代码编辑器 **VSCode** 开始我们的项目。

```
code .
```

我们完事了。😄。您的脚本现在应该是这样的:

```
#!/bin/bashAPP_NAME="$1"
cd
cd Desktop/
mkdir $APP_NAME
cd $APP_NAME
python -m venv newenv
source newenv/bin/activate
pip install --upgrade pip
pip install numpy pandas matplotlib streamlit
code .
```

# 运行我们的脚本

首先，我们做`chmod +x`(在我们的脚本上)使它可执行。

```
chmod +x createmlapp.sh
```

厉害！这太棒了。剩下的唯一一件事就是将这个脚本移动到我们的**主目录**中，这样我们就可以运行它，而不用每次都将 **cd — *ing*** 放入任何其他文件夹中。

首先，找出您的主目录——在终端中键入 **cd** 。无论您要去哪里，您都需要将脚本移动到那里，以便使它可以从那里执行。

现在，只需输入:

```
./createmlapp.sh yourmlappname
```

来看看实际的脚本！

## 结束…

恭喜你。在这个小指南之后，你现在应该能够自动化类似的工作流程，以便在开始一个新项目时节省一些时间！

我现在能给你的建议就是探索更多，尝试创建更多的脚本，也可能执行更复杂的任务，比如**运行新的应用**、**自动**、T21【将代码推送到 GitHub 等等。

你可以在这里找到[代码库](https://github.com/yashprakash13/data-another-day#bash-essential-tricks-to-make-you-even-more-lazy)。

如果您喜欢这篇文章，我每周都会在这里分享一些来自数据科学世界的有用工具和技术。跟随我永远不会错过他们！

最后，这里有几篇我的类似文章，你可能也会觉得有用:

[](/five-numpy-functions-you-should-understand-e0e35704e7d2) [## 你应该了解的五个数字函数

### 如何水平和垂直堆叠你的数组，找到唯一的值，分割你的数组和一些更多的技巧来使用…

towardsdatascience.com](/five-numpy-functions-you-should-understand-e0e35704e7d2) [](/26-datasets-for-your-data-science-projects-658601590a4c) [## 为您的数据科学项目提供 26 个数据集

### 众多基于任务的数据集的汇编，可用于构建您的下一个数据科学项目。

towardsdatascience.com](/26-datasets-for-your-data-science-projects-658601590a4c)