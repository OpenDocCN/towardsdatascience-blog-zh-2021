# Python:用 pyenv、pyenv-virtualenv & pipX 创建干净的学习环境

> 原文：<https://towardsdatascience.com/python-how-to-create-a-clean-learning-environment-with-pyenv-pyenv-virtualenv-pipx-ed17fbd9b790?source=collection_archive---------0----------------------->

## 设置您的 Python 学习环境，而不污染您未来的开发环境

![](img/17173542bc93c72811a04b78dfa9b7ed.png)

萨法尔·萨法罗夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我最近写了一篇文章，详细介绍了我学习 Python 编程语言的早期经历。这篇文章的主要焦点是在学习之旅的一开始就建立一个合适的开发环境的重要性。这是许多针对初学者的教程和在线课程经常忽略的内容，可能会导致许多学习者在准备开始自己的一些项目时，陷入一个复杂而不可行的开发环境。这是我从惨痛的教训中学到的！

<https://medium.com/pythoneers/want-to-learn-python-what-nobody-tells-you-until-it-is-too-late-16bb1f473095>  

设置一个合适的 Python 环境，使您能够有效地管理不同 Python 版本的多个安装，并将所有包/库及其相关的依赖项与您的全局系统和用户环境隔离开来，这一点的重要性肯定是不可低估的。

在本文中，我打算向您介绍我当前的 Python 开发环境，并列出允许您复制它的步骤。以下步骤与我在运行 macOS Big Sur 的 MacBook Pro 上使用默认 zsh shell 的设置相关。我将向您展示如何:

*   使用 pyenv 安装和管理 Python 版本
*   使用 pyenv-virtualenv 创建虚拟环境，并为学习和项目目录/存储库提供隔离
*   使用 pipX 安装基于 CLI 的工具和软件包，以便在所有目录和虚拟环境中使用

这不是一个完整的教程，解释这些工具必须提供的所有功能，而是简单地解释如何安装每个工具，以及如何使用它们来隔离您的学习和后续项目环境。

## 系统 Python 版本 2.7 —请勿触摸！

**但首先发出警告！**您的 Mac 将已经安装了 Python 2.7 版本。Mac 操作系统使用此 Python 安装(位于 usr/bin/python 中)来执行您的计算机平稳运行所需的关键任务。请务必不要从您的计算机上删除这个版本的 Python，并且强烈建议您不要将它用于您自己的编程活动，因为您可能会使您的计算机变得无用！

# 终端(命令行界面)

如果您希望发展开发人员的技能，或者只是想提高您对计算机的掌握程度，您需要学习如何使用一个关键工具，那就是命令行界面(CLI)。这是通过在 Mac 上打开“终端”应用程序来启动的(应用程序→实用程序)。这是一个应用程序，它在您的计算机上打开一个文本窗口(称为 Shell ),并允许您键入基于文本的命令或一系列指令，然后您的计算机将直接执行这些命令或指令。

有许多免费的课程和教程可以帮助您学习如何有效地使用 CLI，但是对于本文的目的和设置您的 Python 环境，我将简单地提供一些命令，您可以将它们复制并粘贴到终端窗口的提示符中。

# 预设置和目录结构

在安装一些特定于 Python 的工具之前，您首先需要为您的 Mac 提供帮助创建和管理 Python 环境所需的工具。

1.安装命令行工具

虽然您的 Mac 已经有了大量有用的命令和工具，可以通过命令行界面来指导您的计算机的活动，但为了完成我们的 Python 设置，这些需要用一些开发人员特定的工具来增强。这些可以通过直接安装苹果 Xcode 开发者应用中包含的工具子集来获得。

只需打开终端，输入以下命令，然后按照屏幕上出现的一系列弹出窗口中的说明进行操作:

```
xcode-select --install
```

2.安装自制软件包管理器

家酿是一个软件包管理器的 Mac，让您轻松地安装和管理免费的开源软件应用程序和工具，直接从终端窗口。我们将使用 Homebrew 来安装 pyenv、pyenv-virtualenv 和 pipX，但是很可能 Homebrew 将成为您将来需要的更多工具的软件包管理器的选择。

要安装 Homebrew，只需将下面一行复制到您的终端窗口，然后按照屏幕上的任何指示操作:

```
/bin/bash -c “$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
```

3.创建目录结构

在您安装 Python 并开始创建虚拟环境之前，创建一个简单的目录结构非常重要，您可以在其中组织您的项目和相关文件。下图所示的结构对我很有用，因为它让我能够圈住我最初的学习活动，同时创造一个空间，一旦我发展了足够的技能，就可以开始从事我自己的一些具体项目。

```
--> Python
   --> Learning
      --> Python_Playground
      --> Training_Course1
      --> Training_Course2
   --> Projects
      --> Project1
      --> Project2
```

在随后的章节中，您将了解如何为每个文件夹安装/分配特定的 Python 版本，以及如何为每个文件夹创建单独的虚拟环境，从而完全控制每个文件夹中安装的库和模块。

您会注意到，在学习目录中，除了特定的培训课程文件夹之外，我还创建了一个名为 Python Playground 的文件夹。我使用 Python Playground 文件夹作为一个安全的地方来试验和尝试我从培训教程中学到的东西的变体。我在这个环境中安装了最新版本的 Python，并自由地安装了我想要试验的所有库和模块，安全地知道它们将被完全隔离，不会影响我的整体开发环境。

在特定的培训课程文件夹中，我可以安装符合特定培训课程的 Python 版本，以及完成课程或教程所需的任何特定库或模块。

这个阶段的项目文件夹是为将来的使用而创建的——指定的、隔离的环境，在那里我可以开始构建我的第一个项目。

# 使用 pyenv 安装 Python

我们将使用名为 pyenv 的工具安装最新版本的 Python，而不是简单地通过 Homebrew、APT 或从 Python.org 直接安装到我们的全球系统环境中。Pyenv 允许您安装多个版本的 Python，并让您完全控制希望使用哪个版本，何时何地使用特定版本，并让您能够快速简单地在这些版本之间切换。

要使用 Homebrew 安装 pyenv，您可能首先需要安装一些关键的依赖项。将以下内容复制到终端中。

```
brew install openssl readline sqlite3 xz zlib
```

现在，您可以通过在终端中输入以下内容来安装 pyenv。

```
brew install pyenv
```

安装完成后，您还有一步要完成，以确保 pyenv 可以添加到您的路径中，允许它在命令行上正确工作，并启用填充和自动完成。

在终端中依次输入以下各行，这将更新。包含所需信息的 zshrc 文件。

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

现在只需关闭并重新加载终端窗口，这将确保 Shell 拾取这些更改。

祝贺您，您现在已经准备好使用 pyenv 安装您的第一个 Python 版本了。

要查看可以通过 pyenv 安装的 Python 的所有可用版本的列表，只需在终端中键入以下内容。

```
pyenv install --list
```

一旦您确定了想要的版本，请输入以下命令进行安装(用您选择的版本替换 3.9.1)。您可以根据需要多次重复此步骤，以安装所有需要的版本。

```
pyenv install 3.9.1
```

现在您已经安装了多个版本的 Python，您可以开始选择在哪些实例中使用哪个版本。

输入命令`pyenv versions`将显示您当前安装在计算机上的所有 Python 版本。要将特定版本设置为默认的全局版本，只需输入以下命令(用您选择的版本替换 3.9.1)。

```
pyenv global 3.9.1
```

您还可以设置要在特定目录中使用的特定 Python 版本，方法是导航到该目录，然后运行以下命令(用您选择的版本替换 3.8.1)

```
pyenv local 3.8.1
```

或者，您可以通过输入以下命令，简单地设置要在当前 Shell 中使用的 Python 版本(用您选择的版本替换 3.8.1)。

```
pyenv shell 3.8.1
```

您可以通过重新输入相关命令并输入不同的版本号来更改分配给上述任何场景的 Python 版本。

# pyenv-virtualenv

![](img/892364a26eb96fdae6af8ade64778951.png)

[JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

既然您已经习惯了使用 pyenv 安装和管理多个版本的 Python，我们就继续创建一个虚拟环境。这将允许您为特定的目录/文件夹分配特定版本的 Python，并确保安装在该文件夹中的所有库和依赖项都是完全隔离的。除非您明确地将它们安装在其他特定的虚拟环境中或者全局安装它们，否则它们对其他目录不可用(不推荐，我将在下一节向您展示如何使用 pipX 来实现这一点)。

首先，您需要安装 pyenv-virtualenv。您可以通过在终端中输入以下命令来做到这一点。

```
brew install pyenv-virtualenv
```

您现在可以创建您的第一个虚拟环境。在本例中，您将为 Python Playground 目录创建一个虚拟环境。同样的步骤也适用于为您希望的任何目录创建虚拟环境。

使用终端，导航到将要创建虚拟环境的目录，在本例中是 Python Playground。

```
cd Python/Learning/Python_Playgrounds
```

接下来，您需要设置您希望在环境中使用的 Python 版本(在这个例子中我们将使用 3.9.1，但是您可以用您需要的版本替换)。

```
pyenv local 3.9.1
```

要创建虚拟环境，请输入下面的命令。`pyenv virtualenv`是创建环境的实际命令。版本号与您刚刚设置为环境的本地版本的 Python 版本一致，最后一部分是虚拟环境的名称。我个人的偏好是在名称前加上“venv ”,然后将虚拟环境的名称与它相关的目录的名称对齐。

```
pyenv virtualenv 3.9.1 venv_python_playground
```

最后一步是将目录的本地 python 版本设置为虚拟环境。

```
pyenv local venv_python_playground
```

当您想要在虚拟环境中工作时，您需要激活它。一旦激活，将使用指定的 Python 版本，并且您安装的任何库都将包含在环境中。在虚拟环境中完成工作后，请务必记住停用虚拟环境，以便返回到您的全球环境。使用以下命令可以激活和停用环境。

```
pyenv activate venv_python_playground
pyenv deactivate
```

要在进出相关目录时启用虚拟环境的自动激活和停用，只需在终端中输入以下命令来更新。包含所需信息的 zshrc 文件。

```
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

现在您知道了如何为您的每个培训课程和未来项目创建一个隔离的 Python 环境。

# pipX

随着您 Python 学习经验的进步，并开始从事自己的一些项目，您可能会开始确定您希望在每个项目中使用的某些包或工具。您可以使用一个名为 pipX 的工具，而不是在每个虚拟环境中复制安装。

pipX 将创建一个特定的全局虚拟环境，在该环境中可以安装这些工具和软件包，这些工具和软件包可以通过命令行、所有全局目录和所有虚拟环境进行访问。这对于 Python formatters (Autopep8 或 Black)、Pylint 等 linters 或 Jupyter Notebooks 等您可能需要在所有项目中使用的工具尤其有用。

要安装 pipX，请在您的终端中运行以下命令。

```
brew install pipx
pipx ensurepath
```

然后用 pipX 安装、升级或卸载软件包和工具，您只需使用`pipx install`、`pipx upgrade`和`pipx uninstall`，就像使用 pip 管理您的库一样。您还可以使用`pipx list`来查看您已经用 pipX 安装的包的列表。

# 结论

干得好！您现在有了一个很好的 Python 开发设置，允许您自由地进行实验和学习，使您能够尝试尽可能多的包和库，安全地知道一切都隔离在一个组织良好和受控的环境中。

此外，您已经学习了一些进入 Python 开发旅程下一阶段所需的工具和技能，并且拥有一个干净整洁的项目基础设施，随时等待您充分利用。

# 后续步骤

既然已经建立了基本的开发环境，为什么不花些时间探索一下开发设置中更有趣和可定制的元素呢？下面列出了一些可供您进一步探索的领域…

1.  选择并安装您选择的 IDE，并定制主题和扩展
2.  用 iTerm2 &哦，我的 ZSH 升级你的基本 mac 终端
3.  安装并学习如何使用 GIT 和 GitHub

# 参考

如果您希望探索我在本文中提到的一些工具，并增长您对它们全部功能的了解，那么下面的链接可能会很有用…

1.  [https://github.com/pyenv/pyenv](https://github.com/pyenv/pyenv)
2.  https://realpython.com/intro-to-pyenv/
3.  【https://github.com/pyenv/pyenv-virtualenv 
4.  [https://github.com/pipxproject/pipx](https://github.com/pipxproject/pipx)