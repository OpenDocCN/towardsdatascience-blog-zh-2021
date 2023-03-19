# 建立 conda 环境的完整指南

> 原文：<https://towardsdatascience.com/from-soup-to-nuts-guide-for-setting-up-a-conda-environment-58afc7c4801?source=collection_archive---------12----------------------->

## conda 的全面指南，从选择安装程序到设置环境、通道和安装包

![](img/60053a226e91a2a03f8a9efa18872785.png)

在 [Unsplash](https://unsplash.com/wallpapers/nature/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [veeterzy](https://unsplash.com/@veeterzy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 动机:

你好！Conda 是数据科学社区中最受欢迎的工具之一，然而，理解实施该步骤的步骤和成本可能会令人困惑，因为几乎没有一个地方解释过，所以我决定写一篇。

我将关注三个主题，第一个是关于 conda 安装程序选项，Anaconda，miniconda 和 miniforge，如果不使用它们，你会错过什么。第二个主题将是关于建立一个环境，你可以可靠地用于多个项目，以及当你需要更多的配置时如何修改。最后一部分是关于渠道与环境和包装的关系，这也是一个被忽视的话题，但如果你想以最小的麻烦生产你的作品，这对于展示良好的工程技能是非常重要的。

*PS:我用的是 macOS Catalina 10.15.7，用的是 Conda 4 . 9 . 0 版本。如果您对具体版本有疑问，请留下评论。*

# TL；博士？

所以，我向您承诺，到本文结束时，您可能会了解如何通过机会成本在以下两者之间进行选择来设置您的 conda 环境:

*   迷你康达和迷你锻造 ***安装工***
*   将 ***环境*** 命名为唯一和标准
*   针对您的环境全局添加 ***通道***
*   从不同渠道安装 ***包***

希望你喜欢，我会在最后见到你！

# 康达

![](img/e54c8f759a415acc762f2a47f872934b.png)

超能力者康达

conda 是一个[开源的](https://github.com/conda/conda)、跨平台、 ***包*** 、 ***依赖*** 和 ***环境*** 管理工具——理论上——适用于任何语言(但大部分支持在 ***数据科学和机器学习特定语言*** 上，如 Python、R、Ruby、C/C++、FORTRAN、…)。Anaconda 是首先开发它的公司，然后在 BSD 许可下开源。它拥有比 pip 和 virtual env 加起来还多的功能。Pip 是 Python 之上的一个包管理器，用于有限数量的库，也就是说，它不能安装 Python。Virtualenv 是一个简单的环境管理器，根本不能安装包…我猜你在这一点上确信使用 conda，所以我们可以继续！

安装康达的前三名安装工分别是 [Anaconda](https://docs.anaconda.com/anaconda/install/) 、 [miniconda](https://docs.conda.io/projects/continuumio-conda/en/latest/user-guide/install/macos.html) 和 [miniforge](https://github.com/conda-forge/miniforge) 。前两个由 Anaconda 开发，并在其网站上提供，而 [miniforge](https://github.com/conda-forge/miniforge) 是由社区最近创建的，因为 miniconda [不支持`aarch64`架构的](https://github.com/conda/conda/issues/8297)。

如果您需要特定的需求，比如在 aarch64(arm64)或 ppc64le (POWER8/9)架构上运行您的模型，您应该使用 miniforge 安装。它还支持 PyPy，这是 Python 的一个轻量级版本。在安装过程中，它将`conda-forge`设置为默认的——也是唯一的——通道，并且没有`defaults`通道。我们将在下一部分更详细地讨论通道。

使用 miniforge 的另一个原因是 Anaconda/miniconda 上的商业使用限制(*)是由于最近在`[Term Of Service of Anaconda](https://www.anaconda.com/terms-of-service),`上的一个变化，在那里将存储库用于商业活动被宣布为违规，这包括使用从`defaults`通道安装的包。

此外，有几个开源项目已经从 mini-conda 转移到 miniforge，[这个](https://github.com/jupyterhub/repo2docker/pull/859#discussion_r387827601)和[这个](https://github.com/jupyter/docker-stacks/pull/1189)，这表明支持这个回购的社区将会增加。

如果你喜欢 ToS 并且不需要架构需求，我们有两个选择，Anaconda 或者 miniconda。如果您想要提供一次性安装的完整版本，并且您有 5GB 的磁盘空间，Anaconda 将为您安装 Python + 250 包。对于新手来说，它可能是一个不错的选择，因为它有现成的常用包，以及 Anaconda Navigator 等应用程序，您可以在这些应用程序中为您的环境启动 JupyterLab、pySpider IDE。Anaconda 也有多个版本，即个人版是免费版本，企业版是为您的团队，如果你想扩展和管理定制的软件包和渠道。

我们的最后一个选择是最小安装程序`miniconda`将是一个很好的选择，因为它将建立`defaults`通道。*它比 Anaconda 安装要好，因为您了解了更多关于下载哪些包的信息，并且您不必释放 5 GB 的空间。

PS:如果你觉得你错过了 ***Anaconda Navigator，Spyder*** 你可以用 conda 安装，第一个在默认通道有，Spyder 两个都有。

```
$ conda install anaconda-navigator
$ conda install spyder
```

🛑也有不同的方法来运行我们的安装程序:

*   ****仅 miniconda， [miniforge](https://github.com/conda-forge/miniforge/releases) 尚不支持。**
*   ***Cloud VM*** :如果你想隔离你的环境，在云上运行，只有`miniconda`有 AWS 的 [AMI images](https://aws.amazon.com/marketplace/seller-profile?id=29f81979-a535-4f44-9e9f-6800807ad996) 的报价。
*   ***容器*** : Docker 安装程序可用于: [miniconda](https://hub.docker.com/u/continuumio) 和 [miniforge](https://hub.docker.com/u/condaforge)

**![](img/ed642f004f99bb298bb6ffa8708dcab8.png)**

**[帕克森·沃尔伯](https://unsplash.com/@paxsonwoelber?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/tents?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片**

# **环境创造**

**Conda `environment`是一种组织多个包及其依赖关系的抽象方式。我们创建的任何新环境都有一个目录，所有的包都将下载到这个目录中，并且与这个环境相关的任何配置和历史都将存储在这个目录中。如果您已经安装了 Anaconda 的安装程序，它会在`anaconda`的安装目录下创建一个名为`base`的环境。您可以通过运行`conda env list`命令来检查这一点:(*)指的是默认环境(当我们没有主动使用任何环境时)。**

```
$ conda env list
# conda environments:
#
base        *  /Users/ebrucucen/opt/anaconda3
```

**这很好，但是我们想要我们自己的环境。关于如何命名环境，有两个约定。**

**首先，你可以给你的环境起一个独特的名字*。这个实现为 Python 的每个版本创建了一个环境(因为它是我们都感兴趣的主要语言，对吗？)，比如分别针对 Python 3.7 和 Python 3.8 版本的`conda-py37`或`env-py3.8`。这很好，如果你是环境新手，可能你不会有很多版本的 Pythons，有多个复杂依赖树的包。您可以从任何项目文件夹访问您的环境，并在执行涉及环境的命令时遵循 conda 文档，不会出现任何问题，因为环境将设置在标准位置(/user/.../envs/)***

***为了创建一个环境，我们使用`conda create`命令，后跟环境名，以及一个 package=version 对列表，其中版本是可选的，代价是安装最新的版本。***

```
*$ conda create --name env-py3.8 python=3.8 numpy=1.19.5*
```

***第二个选项是使用 ***通用名称*** 到您的所有环境，并为每个项目文件夹创建一个新的，例如`conda-env.`这意味着，对于您正在处理的任何项目文件夹，您可以以相同的方式引用环境名称，并以一致的方式在您的任何自动化脚本中使用。请注意，子命令`--prefix`只与`--name`相互作用，小心选择你的毒药！***

```
*$ conda create  --prefix /<possibly a long path>/conda-env python=3.7# or if you are already on the same directory:$ conda create  --prefix ./conda-env python=3.7*
```

***![](img/6a9aff5b0495ce072dc2da048d7c407d.png)***

***马克斯·库库鲁兹亚克在 [Unsplash](https://unsplash.com/s/photos/fire?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片***

# ***环境激活***

***无论您选择了哪种命名约定，现在您都有了一个环境，(env-py3.8 或 conda-env 或两者都有(！))，接下来我们需要做的是激活，这样我们就可以开始将软件包安装到这些环境中:***

```
*$ conda activate env-py3.8*
```

***它(默认情况下)在命令行上显示环境名，准备接受下一组指令…***

```
*(env-py3.8) $*
```

***或者***

```
*$ conda activate ./conda-env*
```

***结果不是很漂亮的展示，而且肯定没有很好的利用你的空间***

```
*(/<possibly a long path>/conda-env) $*
```

***要改变这种行为，只显示环境名，您可以修改`.condarc`文件(默认情况下在您的主目录下，~/)。condarc，如果你不确定能在`conda config — show-sources`前找到答案:***

```
*conda config --set env_prompt '({name})'*
```

***现在，如果你已经设法跟随我到这一点，我们应该有一个环境，激活等待我们安装软件包。***

***如果你想放松一下，深入一下*，看看 env 文件夹的子目录，其中`conda-meta`文件夹包含`history`文件来跟踪这个环境中的每个动作，每个包的 JSON 文件列出了它的编译号、依赖项和文件位置…如果你发现/知道任何有趣的事情，请留下评论，这将有助于我们更好地理解环境之谜。我们需要软件包，而`channels`帮助我们获得正确的版本和依赖关系，让我们开始下一步吧！****

****![](img/8045dfdddcecc5379628d0ad24166900.png)****

****科迪·韦弗在 [Unsplash](https://unsplash.com/s/photos/rivers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片****

# ****频道****

****通道是我们的包的仓库。单独维护的每个通道可能有不同版本的包，每个版本有不同的内部版本，并且相同版本的包在每个通道中可能有不同的依赖关系。查看 [Stackoverflow 问题](https://stackoverflow.com/questions/39857289)以获得更多相关讨论。****

****一个很好的例子是最常见的两个包，NumPy 和 Tensorflow，其中 Anaconda 的 defaults channel 和 conda-forge 有不同的版本。****

```
**numpy                         1.19.5  py39he588a01_1  conda-forge
numpy                         1.19.2  py39he57783f_0  pkgs/maintensorflow                    2.0.0 mkl_py37hda344b4_0  pkgs/main
tensorflow                    1.14.0      hcba10bf_0  conda-forge**
```

****为避免混淆，这并不意味着我们要安装 tensorflow 2.0.0 版本时引用 conda-forge 通道，而是意味着 conda 将尝试使用 tensorflow 2.0 模块的默认通道，并为每个依赖项确定 conda-forge 的优先级，作为回报，您将获得:****

```
**_tflow_select      pkgs/main/osx-64::_tflow_select-2.3.0-mkl
  absl-py            conda-forge/osx-64::absl-py-0.11.0-py37hf985489_0
 ...
  tensorboard        pkgs/main/noarch::tensorboard-2.0.0-pyhb38c66f_1
  tensorflow         **pkgs/main/osx-64::tensorflow-2.0.0**-mkl_py37hda344b4_0
  tensorflow-base    pkgs/main/osx-64::tensorflow-base-2.0.0-mkl_py37h66b1bf0_0
  tensorflow-estima~ pkgs/main/noarch::tensorflow-estimator-2.0.0-pyh2649769_0
  termcolor          conda-forge/noarch::termcolor-1.1.0-py_2
  werkzeug           conda-forge/noarch::werkzeug-1.0.1-pyh9f0ad1d_0**
```

> ****添加频道****

****如上所述，miniconda 和 Anaconda 安装的默认通道是`defaults`通道，而对于 miniforge，默认通道是`conda-forge`通道。我们可以通过查看配置文件来显示我们的通道(不管您是否处于激活的环境中):****

```
**$ conda config --show-sources**
```

****其结果与此类似(您的会略有不同):****

```
**==> /Users/ebrucucen/.condarc <==
auto_update_conda: False
ssl_verify: True
channels:
  - conda-forge
  - defaults**
```

****当您在激活的环境中时，您可以全局地或本地地向您的环境添加通道。对于全球安装，您可以调用`conda config add` [conda-canary](https://www.anaconda.com/blog/what-conda-canary) 来测试要在 24 小时内发布的包****

```
**$ conda config --add channels conda-canary**
```

****我们还可以创建特定于环境的通道。假设我们想在 env-py3.8 环境中安装基因组相关的包，然后我们可以激活一个环境，向它传递`--env`参数，并向它添加 bioconda 通道:****

```
**$ conda activate env-py3.8
(env-py3.8) $conda config --env --add channels bioconda**
```

****结果将与此类似(您可能有/可能没有默认通道)****

```
**(env-py3.8) $conda config --show-sources==> /Users/ebrucucen/.condarc <==
auto_update_conda: False
ssl_verify: True
channels:
  - conda-canary
  - conda-forge
  - defaults==> /Users/ebrucucen/opt/anaconda3/envs/env-py3.8/.condarc <==
channels:
  - bioconda
  - defaults**
```

> ****附加频道****

****您可能已经注意到,`add`命令修改了配置文件，将最新的附加通道放在列表的顶部。conda 对频道的顺序很固执，因为它本质上是一个优先列表。如果我们的新通道应该到底部，而不是顶部，我们可以使用`append`参数(或者修改配置文件，我听到了)****

```
**(env-py3.8) $conda config --env --append channels test-channel**
```

****如果我们检查配置文件，我们将看到我们的通道是最后一项(是的，你是对的，在执行 add/append channel 命令期间没有通道验证发生，但是当你想要搜索/安装包时它将出错)****

```
**(env-py3.8) $conda config --show-sources==> /Users/ebrucucen/.condarc <==
auto_update_conda: False
ssl_verify: True
channels:
  - conda-canary
  - conda-forge
  - defaults==> /Users/ebrucucen/opt/anaconda3/envs/env-py3.8/.condarc <==
channels:
  - bioconda
  - defaults
  - test-channel**
```

> ****移除频道****

****如果你想删除一个频道(要么是你犯了一个错误，要么是你不再需要它)，就像运行`--remove`参数一样简单，同样的原理，你需要指定`--env`标签来从激活的环境中删除频道，否则，conda 会给出一个错误，告诉你它找不到这个频道。****

```
**(env-py3.8) $conda config --env --remove channels test-channel**
```

****![](img/439d112e7af0be2c50420e552c61ee20.png)****

****照片由[沃洛季米尔·托卡](https://unsplash.com/@astrovol?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mushrooms?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄****

# ****包装****

****最后，我们现在可以安装我们的软件包了。为了找到你需要的，我推荐 [Anaconda search](https://anaconda.org/search?) ，它给出了每个包的版本和下载，你可以更好地了解你的选择，另外你可以为你的包找到好的***jupyternotebooks***。****

****由于每个环境都有一个按优先顺序排列的通道列表，任何安装都将逐个检查版本(如果指定)是否可用。****

```
**$ conda search spyder**
```

****我安装软件包的 6 大方法是:在特定的环境下，特定的版本，特定的版本(可能是天书)，从特定的渠道，有依赖或者没有依赖。对于前 3 种，我们可以使用这种格式:****

```
**$ conda install -n <env-name> -c <channel> <package_name>=<version>=<build_string>$ conda install -n env-py3.9 -c conda-forge numpy=1.19.5=py39he588a01_1**
```

****默认情况下，conda install 会安装相关的软件包。我们应该明确地告诉他们我们不想要依赖(danger Williams，我假设我们知道我们在这一点上在做什么)。****

```
**$ conda install -c conda-forge numpy --no-deps**
```

## ****战斗号令****

****感谢您的耐心，并通过我的帖子阅读。现在，你已经有了一个 conda 设置的环境和通道，可以安装你需要的任何包(以及 Pip 和其他工具),正如我在每个选择背后所承诺的那样。希望你喜欢。这是一个简单的过程，你可以定制你想要的，比如创建你自己的频道，软件包，将所有可用的支持。祝您愉快！****