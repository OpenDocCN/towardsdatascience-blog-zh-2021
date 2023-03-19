# Python，系统路径以及 conda 和 pyenv 如何操纵它

> 原文：<https://towardsdatascience.com/python-the-system-path-and-how-conda-and-pyenv-manipulate-it-234f8e8bbc3e?source=collection_archive---------5----------------------->

## 深入探究当您键入“python”时会发生什么，以及流行的工具是如何操作的

![](img/6cfdd24dcba8702490bacd369858f8bd.png)

[都铎·巴休](https://unsplash.com/@baciutudor?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

系统路径(在 Mac/Linux 上的`echo $PATH`或`echo -e ${PATH//:/\\n}`版本稍微漂亮一点)不仅仅是 python 的事情，它对 python 的功能也非常重要。如果你打开一个命令行，输入一个“命令”，计算机(或“操作系统”)需要知道在哪里寻找这个命令，找到它的底层代码，然后“执行”它。这些“命令文件”通常被称为可执行文件，当你键入与它们的文件名匹配的单词时，它们就会运行。

让我们以`echo`为例。当我们键入`echo $PATH`时，我们的计算机需要知道在哪里找到命令`echo`的代码，并通过传入一个参数`$PATH`来执行它。我们可以通过下面的代码看到代码在哪里:

```
> whereis echo
/bin/echo
```

其中`whereis`是我们的计算机知道如何定位的另一个可执行文件:

```
> whereis whereis
/usr/bin/whereis
```

有意思。他们坐在两个不同的位置。如果我们在这两个地方有相同的可执行文件名称会怎么样？我们会执行哪一个？

## 系统路径排序

`$PATH`变量决定了这一点，幸运的是，操作系统做了简单的事情，从左到右/从上到下地完成了它。作为一个例子，让我们看看我的系统变量`$PATH`的如下输出:

```
> echo -e ${PATH//:/\\n}
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

假设我们去寻找一个名为`myspecialexec`的可执行文件。我们的系统将从顶层开始。在这种情况下，它将在目录`/usr/local/bin`中查找名为`myspecialexec`的可执行文件。如果找到了，就执行它。如果没有找到，它就移动到路径中指定的下一个目录——在这种情况下是`/usr/bin`,如果没有找到，就移动到`/bin`,依此类推。

## 这和 Python 有什么关系？

Python 是可执行的。当你运行`brew install python`或前往[这里](https://www.python.org/downloads/)并运行 python 安装程序时，这就是你正在下载的东西(连同标准库和一些其他东西)。您正在执行以下操作之一:

*   获取 python 可执行文件，并将其粘贴到`$PATH`中的上述目录之一
*   获取 python 可执行文件并粘贴到其他地方，然后确保*目录名现在在`$PATH`中*

因此，当您运行命令`python`时，您的操作系统开始向下滑动`$PATH`列表，寻找它能找到的第一个匹配的名字。如果您有多个版本，那么它将使用最先找到的版本。**如果你有一个你*想让*它找到的版本，那么你应该把那个 python 版本的位置放在路径的顶部。**最后一点是理解(和简化)所有这些工具“管理环境”和“管理 python 版本”的关键。一旦我们理解了我们的计算机如何决定使用哪个版本的 python，像包管理这样的其他事情就非常简单了。

## 例 1:conda 是如何做到这一点的，所以我们使用基于 conda 环境的正确版本的 python？

[Conda](https://docs.conda.io/en/latest/) 正如它自己描述的那样*“一个运行在 Windows、macOS 和 Linux 上的开源包管理系统和环境管理系统”*。这是我学习的第一个试图创建和管理稳定的 python 环境的系统，它因其与 python 中机器学习和统计编程的兴起(matplotlib、numpy、scipy、pandas、sklearn 等)的原始联系而广受欢迎。

它的“较轻”版本， [miniconda](https://docs.conda.io/en/latest/miniconda.html) ，是大多数人在谈论“康达”时谈论的内容。如果您要前往[这里](https://docs.conda.io/en/latest/miniconda.html#installing)并安装它，您现在就会这样做，就像使用 python 一样:

*   在你电脑的某个地方有一个`conda`可执行文件
*   通过位于`$PATH`变量中的某个位置，该可执行文件将对您的 shell/终端可见

这意味着当你在终端中输入`conda`时，它会知道如何执行 conda 附带的功能。

## 那么 conda 如何确保你使用的是想要的 python 版本呢？

**通过操作$PATH 变量，特别是利用它自顶向下的特性**。为了建立一个例子，让我们做以下事情。假设安装了 conda，并且您打开了一个新的终端/外壳，您将看到:

```
(base) >
```

即，您将被定向到为您自动创建的默认基础 conda 环境。这里的目的不是谈论环境创建，而是说 conda 已经在某个地方创建了一个“基础”目录。在该目录中，它放置了:

*   该环境附加到的 python 版本(因此，每次在基本环境中键入`python`时，它都会运行相同版本的 python)
*   您可以为该环境下载的所有其他软件包依赖项

这个目录到底在哪里？我们可以通过使用命令`which python`看到:

```
(base) > which python
/Users/jamisonm/opt/miniconda3/bin/python
```

好极了。所以 conda 在某个地方创建了一个目录，并在其中放了一个我们可以调用的 python 版本。但是当我们在*这个*环境中的时候，我们的计算机怎么知道调用*这个*版本的 python 呢？因为它将该目录放在了$PATH 的顶部。我们可以证实这一点:

```
(base) > echo -e ${PATH//:/\\n}
/Users/jamisonm/opt/miniconda3/bin
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

现在，当我停用环境，即我不想再使用这个环境和相应的 python 版本时，会发生什么？

```
(base) > conda deactivate

> echo -e ${PATH//:/\\n} # environment disabled so lose the (base)
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin

> which python
/usr/bin/python
```

所以`conda deactivate`已经从系统路径的顶部移除了`/Users/jamisonm/opt/miniconda3/bin`，所以我们默认使用之前的自顶向下搜索，并最终在`/usr/bin/python`中找到预装的 python。

创造一个新的环境怎么样？让我们创建一个名为`conda-demo`的新环境。

```
> conda create --name conda-demo
> conda activate conda-demo
(conda-demo) > echo -e ${PATH//:/\\n}
/Users/jamisonm/opt/miniconda3/envs/conda-demo/bin
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

因此，康达又一次做到了以下几点:

*   在某个地方创建了一个文件夹(`/Users/jamisonm/opt/miniconda3/envs/conda-demo/bin`)来存放附加到这个环境中的 python 版本(以及其他潜在的下载包)
*   将该目录放在$PATH 的顶部，这样当我们键入`python`时，它会执行存在于该目录中的 python，而不是位于`$PATH`变量下方的目录

## 示例 pyenv 是如何做到这一点的，以便我们根据特定的环境使用正确的 python 版本？

与 conda 不同， [pyenv](https://github.com/pyenv/pyenv) (最初)并不是一个完全成熟的环境管理器，但更倾向于解决以下问题:

*“我如何在同一台计算机上处理使用不同版本 python 的多个项目，并确保当我键入* `*python*` *时，我使用的是我想要使用的 python 版本？”*

这是一个“python 版本管理”工具，关键是不涉及任何 Python。为了安装和管理不同的 python 版本，重要的是它本身不依赖于 python，而是主要是 bash 脚本的负载。

正如您可能已经猜到的，它通过操纵`$PATH`变量来完成 python 版本管理。事情比康达复杂一点，我们来看一个例子。假设您已经通过类似`brew install pyenv`的东西安装了，当您启动一个新的终端/shell 时，您应该准备好了(因为语句(或类似的东西)`eval "$(pyenv init --path)"`将被添加到您的`.zshrc` / `.zprofile` / `.bash_profile` / `.bashrc`的末尾，pyenv 命令将被加载到您的 shell 中。

## 树立榜样

在开始之前，让我们使用 pyenv 来建立一个示例，这样我们就可以准确地演示它是如何工作的。安装 pyenv 后，让:

**安装 2 个不同版本的 python，并将 3.9.7 设为全局版本**

在新的终端中，键入以下内容以安装 python 3.8.12 和 python 3.9.7，并将 python 3.9.7 设置为要使用的首选“全局”python 版本:

```
> pyenv install 3.8.12
> pyenv install 3.9.7
> pyenv global 3.9.7
```

然后，我们可以通过检查以下内容来检查这是否成功:

```
> pyenv versions
   system
   3.8.12
 * 3.9.7 (set by /Users/jamisonm/.pyenv/version)
```

**新建一个项目，将 python 的本地版本设置为 3.8.12**

为了理解 pyenv 如何同时管理多个 python 版本，我们需要创建一个“新项目”,并告诉它我们不想使用我们已经设置的“全局”python 版本——3 . 9 . 7。为此，您可以创建任何新目录并运行以下命令:

```
> mkdir ~/pyenv-demo    # make a new directory called pyenv-demo in my home directory
> cd ~/pyenv-demo       # cd into it
> pyenv local 3.8.12    # set 3.8.12 as the python version to be used in this directory
```

这做了什么？这就创建了一个只包含以下文本的`.python-version`文件:`3.8.12`。如果一切按计划进行，那么:

*   当我们在这个目录(现在是一个简单类型的“环境”)中时，我们将使用 python 3.8.12
*   当我们在它之外时，我们将使用默认的 3.9.7

现在我们已经准备好了，我们可以检查 pyenv 如何确保我们使用期望的 python 版本。

## 当我键入`python`时，pyenv 如何知道运行 python 的“本地”版本？

首先，让我们通过使用`cd ~/pyenv-demo`将自己转移到新目录。让我们看看当我们键入`python`时我们指向哪里，并且根据我们的路径，这是否有意义:

```
> which python
/Users/jamisonm/.pyenv/shims/python
> echo -e ${PATH//:/\\n}
/Users/jamisonm/.pyenv/shims
/usr/local/bin
/usr/bin
/bin
/usr/sbin
/sbin
```

因此，我们似乎在`/Users/jamisonm/.pyenv/shims`中使用了 python 可执行文件，正如 conda 的情况一样，这是因为 pyenv 在我们的路径顶部放置了一个名为`/Users/jamisonm/.pyenv/shims`的目录(因此首先搜索这个目录)。如果我们检查指定的 python 可执行文件，我们会发现它实际上不是真正的 python 可执行文件，而是一个名为“python”的 bash 脚本。这就是“填补程序”的含义——我们欺骗我们的操作系统运行*这个*可执行文件(它*被称为* python，但实际上并不是*python)，而不是进一步搜索`$PATH`并找到一个真正的 python 可执行文件。*

## 那么这个“填充”python 脚本是做什么的呢？

让我们来看看:

```
> less /Users/jamisonm/.pyenv/shims/python

#!/usr/bin/env bash
set -e
[ -n "$PYENV_DEBUG" ] && set -x

program="${0##*/}"

export PYENV_ROOT="/Users/jamisonm/.pyenv"
exec "/usr/local/opt/pyenv/bin/pyenv" exec "$program" "$@"
```

简单地说，它检查一些调试标准，设置一个变量`PYENV_ROOT`，然后调用*另一个*可执行文件:`/usr/local/opt/pyenv/bin/pyenv`。因此，到目前为止，它所做的不是调用 python，而是拦截对 python 的调用(通过操纵`$PATH`)，然后调用自己。

我们*可以*检查这个可执行文件(`/usr/local/opt/pyenv/bin/pyenv`)，但是它只有大约 150 行 bash 脚本。相反，假设一切顺利，我们可以把注意力集中在底部，它有:

```
command="$1"
case "$command" in
"" )
  { pyenv---version
    pyenv-help
  } | abort
  ;;
-v | --version )
  exec pyenv---version
  ;;
-h | --help )
  exec pyenv-help
  ;;
* )
  **command_path="$(command -v "pyenv-$command" || true)"**
  if [ -z "$command_path" ]; then
    if [ "$command" == "shell" ]; then
      abort "shell integration not enabled. Run \`pyenv init' for instructions."
    else
      abort "no such command \`$command'"
    fi
  fi

  shift 1
  if [ "$1" = --help ]; then
    if [[ "$command" == "sh-"* ]]; then
      echo "pyenv help \"$command\""
    else
      exec pyenv-help "$command"
    fi
  else
    **exec "$command_path" "$@"**
  fi
  ;;
esac
```

您可以自己检查脚本，或者相信我的话，它会为您指出:

*   `command_path` = `/usr/local/Cellar/pyenv/2.2.0/libexec/pyenv-exec`(这是因为我用 brew 安装了 pyenv，brew 把这个可执行文件放在了‘Cellar’目录中)
*   `"$@"` = `python`

这意味着现在我们已经被引导到另一个可执行文件*上，即带有参数`python`的`/usr/local/Cellar/pyenv/2.2.0/libexec/pyenv-exec`。*

## 那么接下来会发生什么？

现在这个脚本有点短了，所以我们可以把它全部打印出来，然后用文字浏览一遍:

```
set -e
[ -n "$PYENV_DEBUG" ] && set -x

# Provide pyenv completions
if [ "$1" = "--complete" ]; then
  exec pyenv-shims --short
fi

**PYENV_VERSION="$(pyenv-version-name)"**
PYENV_COMMAND="$1"

if [ -z "$PYENV_COMMAND" ]; then
  pyenv-help --usage exec >&2
  exit 1
fi

export PYENV_VERSION
PYENV_COMMAND_PATH="$(pyenv-which "$PYENV_COMMAND")"
PYENV_BIN_PATH="${PYENV_COMMAND_PATH%/*}"

OLDIFS="$IFS"
IFS=$'\n' scripts=(`pyenv-hooks exec`)
IFS="$OLDIFS"
for script in "${scripts[@]}"; do
  source "$script"
done

shift 1
if [ "${PYENV_BIN_PATH#${PYENV_ROOT}}" != "${PYENV_BIN_PATH}" ]; then
  # Only add to $PATH for non-system version.
  export PATH="${PYENV_BIN_PATH}:${PATH}"
fi
exec "$PYENV_COMMAND_PATH" "$@"
```

重要的一点是我强调的那一点。这个位在当前目录中检查一个`.python-version`文件，如果它存在，它就使用它。否则，它会遍历到该目录的所有路径，寻找其他的`.python-version`文件，直到找到一个。否则，它将默认使用我们设置的全局 python 版本。在这种情况下，它:

*   在我们的 pyenv-demo 目录中找到`.python-version`文件
*   为`3.8.12`读取
*   转到`/Users/jamisonm/.pyenv/versions`，pyenv python 可执行文件*实际上*存储在那里
*   找到我们想要的，并将我们的可执行文件设置为`/Users/jamisonm/.pyenv/versions/3.8.12/bin/python`

上述过程在[他们的 git](https://github.com/pyenv/pyenv#choosing-the-python-version) 上有描述。

## 对于它正在实现的目标来说，这似乎相当复杂

这相当复杂——尤其是来自编程 python 和尝试通读一堆 bash 脚本。然而，最终所有这些都隐藏在对`pyenv`的简单调用之下。有时候，确切地了解正在发生的事情是很好的，但是日常生活中没有必要知道这些。然而，要点仍然存在——理解不同的环境管理程序和 python 版本化有时看起来很复杂，但归根结底只有一件事:以一致的方式操作`$PATH`变量，以便您想要的 python 版本就是您得到的 python 版本。

## 结论

希望通过这次经历，事情开始变得简单。“环境管理”和“python 版本控制”实际上可以归结为以下几点:

*   环境管理:在某个地方创建一个目录，并将我们希望用于该环境的 python 版本放在那里
*   Python 版本控制:确保当我们在特定的目录/环境中时，在浏览`$PATH`时，首先找到附加的 python 可执行文件的路径
*   包管理:将所需的包下载到我们想要的 python 可执行文件所在的“相同位置”,这样就可以用它们来代替其他环境中的包

最后一点我们还没有真正经历过，但是一旦我们理解了当我们键入`python`时，我们的计算机如何知道使用哪个版本的 python，它真的一点也不复杂。然而，要理解它，我们需要理解**模块搜索路径和**，正如我们将在下一篇文章中看到的，这实际上只不过是在 python 启动时对`$PATH`进行一些小的标准操作。

[](https://markjamison03.medium.com/python-and-the-module-search-path-e71ae7a7e65f) [## Python 和模块搜索路径

markjamison03.medium.com](https://markjamison03.medium.com/python-and-the-module-search-path-e71ae7a7e65f)