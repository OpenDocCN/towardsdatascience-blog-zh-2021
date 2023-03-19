# Pipenv 与 Conda(针对数据科学家)

> 原文：<https://towardsdatascience.com/pipenv-vs-conda-for-data-scientists-b9a372faf9d9?source=collection_archive---------6----------------------->

## 截至 2021 年 1 月，pipenv 和 conda 基于各种“数据科学”标准的比较

# 介绍

Python 有许多工具可用于向开发人员分发代码，并且不遵循[“应该有一种——最好只有一种——显而易见的方法来做这件事”](https://www.python.org/dev/peps/pep-0020/)。例如，管理无处不在的 scipy 堆栈的 scipy.org 的[推荐 Conda+Anaconda，而 python 打包权威的](https://www.scipy.org/install.html#installation) [PyPA](https://packaging.python.org/guides/tool-recommendations/) 推荐 pipenv+PyPI。这可能会让数据科学家有点左右为难。本文使用以下一组标准对截至 2021 年 1 月的 pipenv 和 conda 进行了比较，其中一些标准与数据科学家更相关:

[**包可用性**](#3de7)**[**依赖解析**](#49aa)[**Python 版本**](#1ffa)[**依赖规范**](#b8b8)[**磁盘空间**](#2dc3)**

**本文并不推荐一种工具而不推荐另一种，而是应该帮助读者根据他们的需求做出决定。本文假设读者已经熟悉 python 打包生态系统、pipenv 和 conda。对于那些不太熟悉的人，我还在文章末尾列出了一个 [**有用资源的列表**](#e411) 。**

# **包装可用性**

***包装是否有合适的格式？***

**正如 Anaconda 所说，“Anaconda 资源库中有超过 1500 个包，包括最流行的数据科学、机器学习和人工智能框架。这些，以及 Anaconda cloud 上来自 channeling 的数千个额外的软件包，包括 [conda-forge](https://conda-forge.org/) 和 [bioconda](https://bioconda.github.io/) ，都可以使用 conda 安装。尽管有这么多的软件包，但与 PyPI 上的 150，000 个软件包相比还是很少。另一方面，并不是 PyPI 中的所有包都可以作为 wheels 使用，这对于通常需要 C/C++/Fortran 代码的数据科学库来说尤其成问题。虽然在 conda 环境中使用 pip 安装 PyPI 包是可能的，但这要求所有子依赖项本身也是 pip 包，这可能会引起麻烦，所以不推荐使用。与 PyPI 相比，Anaconda 主通道中可用的包之间通常会有延迟。例如，熊猫的延迟似乎是几个星期。**

**我想检查 pipenv+PyPI 和 conda+Anaconda 是否可以提供数据科学家的基本工具集:pandas、scikit-learn、sqlalchemy、jupyter、matplotlib 和 networkx。我用的是 python3.8，因为 3.9 是最近才出的。**

```
$ pipenv install pandas scikit-learn sqlalchemy jupyter matplotlib networkx --python 3.8$ conda create --name env_ds scikit-learn sqlalchemy jupyter matplotlib networkx python=3.8
```

**两个环境都在大约 3 分钟内成功创建。请注意，我使用的是 Ubuntu WSL1，不同的平台在创建环境方面可能不太成功。**

# **依赖性解析**

***解析直接和间接依赖关系***

****康达****

**为了测试这个标准，我使用了依赖 numpy 的 pandas。我首先尝试使用 conda 安装 numpy1.15.3 和 pandas，这样环境就直接依赖 pandas 和 numpy，间接依赖 numpy:**

```
$ conda create --name env_a numpy==1.15.3 pandas python=3.7
```

**Conda 成功地创建了一个环境，并安装了 pandas1.0.5，这是支持 numpy1.15.3 的最后一个 pandas 版本。**

**如果现有环境的软件包版本需要升级或降级:**

```
$ conda create --name env_b pandas python=3.7
$ conda activate env_b
$ conda install numpy==1.15.3
```

**Conda 会在更新环境前询问您:**

> **以下软件包将被降级:**
> 
> **numpy 1 . 19 . 2-py 37h 54 aff 64 _ 0→1 . 15 . 3-py 37h 99 e 49 EC _ 0
> numpy-base 1 . 19 . 2-py 37 HFA 32 c 7d _ 0→1 . 15 . 3-py 37 H2 f 8d 375 _ 0
> 熊猫 1 . 2 . 0-py 37 ha 9443 f 7 _ 0→1 . 0 . 5-py 37h 0573 a6f _ 0**
> 
> **继续吗？**

**注意，建议同时指定所有包，以帮助 Conda 解决依赖性。**

****Pipenv****

**然后，我尝试用 pipenv 安装相同的包:**

```
$ pipenv install numpy==1.15.3 pandas --python 3.7
```

**Pipenv 使用 numpy1.19.1 创建了一个环境，它不符合我的规范。Pipenv 确定存在冲突，无法创建 Pipfile.lock 并打印以下有用的消息:**

> **✘锁定失败！
> 解析后的依赖关系中存在不兼容的版本:
> numpy = = 1 . 15 . 3(from-r/tmp/pipenvzq 7 o 52 yj requirements/pipenv-5 BF 3v 15 e-constraints . txt(第 3 行))
> numpy>= 1 . 16 . 5(from pandas = = 1 . 2 . 0->-r/tmp/pipenvzq 7 o 52 yj requirements/pipenv-5 BF 3v 15 e-1**

**Pipenv 还有 graph 和 graph-reverse 命令，可以打印依赖关系图，并允许用户跟踪包如何相互依赖，帮助解决冲突。**

```
$ pipenv graph
```

> **pandas = = 1 . 2 . 0
> -numpy[必选:> =1.16.5，已安装:1 . 19 . 5】
> -python-dateutil[必选:> =2.7.3，已安装:2 . 8 . 1】
> —six[必选:> =1.5，已安装:1 . 15 . 0】
> -pytz[必选:> =2017.3，已安装:2020.5**

**注意，pip 依赖解析器[正在经历变化](https://pip.pypa.io/en/stable/user_guide/#resolver-changes-2020)。我使用了最新版本(20.3.1 ),但结果可能会因 pip 版本而异。**

# **Python 版本**

***管理不同的 python 版本***

****康达****

**Conda 将 python 发行版视为一个包，并自动安装您直接指定的任何 python 版本。此外，在创建新环境时，conda 将确定最佳 python 版本(如果没有指定)。例如:**

```
$ conda create —-name env_a pandas
```

**用 python3.8.5 和 pandas1.1.5 创建一个环境，但是**

```
$ conda create —-name env_c pandas==0.25.0
```

**使用 python3.7.9 创建环境，python 3 . 7 . 9 是支持 pandas0.25.0 的最新 python 版本。**

**如果需要升级/降级现有环境的 python 版本，安装将会失败:**

```
$ conda create —-name env_d python==3.8
$ conda activate env_d
$ conda install pandas==0.25.0
```

**但是错误消息非常有用:**

> **UnsatisfiableError:发现以下规范
> 与您环境中现有的 python 安装不兼容:
> 规范:
> -pandas = = 0 . 25 . 0->python[version = '>= 3.6，< 3.7.0a0| > =3.7，< 3.8.0a0']**

****Pipenv****

**Pipenv 本身并不安装不同的 python 版本。它将使用系统 python(通常存储在/usr/lib 中)或基础 python(如果安装了 miniconda，通常存储在~/miniconda3/bin 中)来创建新环境。但是，如果安装了 pyenv，pipenv 可以使用 pyenv 安装其他 python 版本。您可以使用 pyenv 预安装 python 版本，或者如果本地没有 python 版本，pipenv 会要求您安装该版本:
[https://towardsdatascience . com/python-environment-101-1 d68 BDA 3094d](/python-environment-101-1d68bda3094d)**

**不幸的是，pipenv+pyenv 不能解析最好的 python 版本，即使从头开始创建环境也是如此。例如:**

```
$ pipenv install pandas
```

**使用 python3.8.5 和 pandas1.2.0 创建环境。尝试安装 pandas0.25.0，其中默认 pyenv python 版本为 3.8 暂停:**

```
$ pipenv install pandas==0.25.0
```

**请注意，暂停可能是由于 pandas0.25.0 的要求是如何配置的。pip 依靠 python_requires 属性来确定 python 版本是否合适，这是最近添加的。在不满足 python_requires 属性的情况下，尝试安装更新的包通常会失败，并显示“找不到发行版”错误。请注意，如果没有指定，pipenv 还会尝试安装软件包的最新版本，而不管 python 版本如何。例如，在 python3.5 环境中尝试熊猫:**

```
$ pipenv install pandas --python 3.5
```

**将失败，并显示以下错误消息:**

> **[pipenv . exceptions . install ERROR]:错误:找不到满足要求的版本 pandas==1.1.5
> [pipenv . exceptions . install ERROR]:错误:找不到 pandas = = 1 . 1 . 5 的匹配分发**

**该信息并不十分有用，已作为一个问题向 pip 提出[。](https://github.com/pypa/pip/issues/8831)**

# **依赖性说明**

***确保可升级的可重复构建***

**Pipenv 使用两个文件来指定依赖关系:Pipfile 用于直接依赖关系，Pipfile.lock 用于直接和间接依赖关系。使用 Pipfile.lock 创建一个环境可以确保安装完全相同的包，包括包的散列。如果需要，使用 Pipfile 创建环境可以让 it 灵活地升级间接依赖关系。Pipenv 希望 Pipfiles 在未来能够取代 requirements.txt(参见[https://github.com/pypa/pipfile](https://github.com/pypa/pipfile))。**

**Conda 使用一个 environment.yaml 文件来指定直接和间接依赖关系。用户在更新他们的环境时必须使用试错法。有一个 [conda-lock](https://pypi.org/project/conda-lock/) 库复制了 Pipfile.lock 功能，但是 Anaconda 目前不支持它。**

# **磁盘空间**

***环境会占用多少空间？分享有帮助吗？***

**数据科学家使用的 Python 环境往往很大，尤其是 conda 环境。例如，包含 jupyter 和 pandas 的 conda 环境占用 1.7GB，而同等的 pipenv 环境占用 208MB。虽然与大多数开发环境无关，但这在生产中可能变得更加重要，例如当使用容器时:
[https://towards data science . com/how-to-shrink-numpy-scipy-pandas-and-matplotlib-for-your-data-product-4 EC 8d 7 e 86 ee 4](/how-to-shrink-numpy-scipy-pandas-and-matplotlib-for-your-data-product-4ec8d7e86ee4)**

**由于规模庞大，数据科学家经常在多个探索项目中使用 conda 环境，甚至在同一个解决方案的多个生产项目中使用:
[https://stack overflow . com/questions/55892572/keeping-The-same-shared-virtualenvs-when-switching-from-pyenv-virtualenv-to-pip](https://stackoverflow.com/questions/55892572/keeping-the-same-shared-virtualenvs-when-switching-from-pyenv-virtualenv-to-pip)
conda 环境可以从任何位置创建、激活和使用。**

**pipenv 环境被绑定到一个项目存储库。创建之后，Pipenv 将 pip 文件保存到存储库的根目录。已安装的软件包会保存到~/。本地/共享/。virtualenvs /默认情况下，其中 pipenv 通过创建一个新目录并将路径的散列附加到名称(即`my_project-a3de50`)来确保为每个 repo 创建一个环境。用户必须 cd 到项目存储库的根目录才能激活环境，但是即使您离开该目录，shell 也将保持激活状态。通过将 pip 文件存储在一个单独的目录中，可以跨多个项目共享一个环境。然后，用户必须记住 cd 到存储库来激活和更新环境。**

# **安全性**

***安装软件包有多安全？***

**Anaconda 主频道[https://anaconda.org/anaconda/](https://anaconda.org/anaconda/)由 Anaconda 员工维护，包裹在上传前要经过[严格的安全检查](https://docs.anaconda.com/anaconda/reference/security/)。在使用 PyPI 的 pipenv 的情况下，任何人都可以上传任何包，并且在过去已经发现了恶意的包(参见[https://www . zdnet . com/article/twelve-malicious-python-libraries-found-and-removed-from-PyPI/](https://www.zdnet.com/article/twelve-malicious-python-libraries-found-and-removed-from-pypi/))。conda-forge 也是如此，尽管他们正在开发一个流程，在工件上传到存储库之前对其进行验证。**

**变通办法包括:**

*   **使用 x 射线[https://jfrog.com/xray/](https://jfrog.com/xray/)等工具进行安全检查**
*   **仅安装至少一个月前的软件包，以便有足够的时间发现和解决问题**

# **寿命**

**conda/pipenv 会留下来吗？有多成熟？谁支持？**

**Pipenv 最初是由流行的[请求](https://pypi.org/project/requests/)库的创建者在 2017 年推出的。Pipenv 在 2018 年 11 月至 2020 年 5 月期间没有发布任何新代码，这引起了人们对其未来的一些担忧:
[https://medium . com/telnyx-engineering/rip-Pipenv-tryed-too-did-hard-do-what-you-need-with-pip-tools-d 500 EDC 161d 4](https://medium.com/telnyx-engineering/rip-pipenv-tried-too-hard-do-what-you-need-with-pip-tools-d500edc161d4)
[https://Chris warrick . com/blog/2018/07/17/17/Pipenv-promises-a-a](https://chriswarrick.com/blog/2018/07/17/pipenv-promises-a-lot-delivers-very-little/)**

**Conda/Anaconda 是由 scipy 背后管理 scipy 堆栈的同一个团队在 2012 年创建的。Conda 是一个开源工具，但是 anaconda 存储库是由一个盈利组织 Anaconda Inc .托管的。虽然这意味着 conda/anaconda 不太可能很快消失，但这引起了人们对 Anaconda Inc .可能开始向用户收费的担忧。他们最近[改变了他们的条件条款](https://www.anaconda.com/blog/sustaining-our-stewardship-of-the-open-source-data-science-community)向重度或商业用户收费，包括镜像 anaconda 库。请注意，新的条件条款[不适用于](https://conda-forge.org/blog/posts/2020-11-20-anaconda-tos/)康达-福吉渠道。**

# **定制**

***定制包管理器带来哪些优势？***

**Conda/Anaconda 是由 python 科学社区创建的，用于解决特定于他们社区的问题，例如非 python 依赖关系:
[http://technical discovery . blogspot . com/2013/12/why-I-promote-conda . html](http://technicaldiscovery.blogspot.com/2013/12/why-i-promote-conda.html)
这赋予了它灵活性和动力来创建面向数据科学家的产品。**

**Conda 可以分发非 Python 构建需求，比如`gcc`，这极大地简化了在其分发的预编译二进制文件之上构建其他包*的过程。康达也可以安装 R 包。Anaconda 开发了一些最流行的数字/科学 Python 库的 MKL 驱动的二进制版本。这些[已经被证明](https://www.anaconda.com/blog/tensorflow-in-anaconda)能够显著提高性能。虽然 MKL 优化[不再生产](https://docs.anaconda.com/archive/)，Anaconda 仍然可以开发只与 conda 环境兼容的工具。***

# **包装**

***代码是如何打包的？***

**conda 和 pipenv 都依赖额外的工具来打包代码。根据代码是否包含非 python 代码和目标平台，两者都依赖于下面的“配方”。**

**Conda-build 用于创建 Conda 包:【https://docs.conda.io/projects/conda-build/en/latest/[T21](https://docs.conda.io/projects/conda-build/en/latest/)**

**PyPA 建议使用 setuptools 构建可以使用 pipenv 安装的轮子。下面是一个很棒的概述:
[https://realpython.com/python-wheels/](https://realpython.com/python-wheels/)**

**注意，随着 pyproject.toml 文件和 pep 518:
[https://grassfedcode . medium . com/pep-517-and-518-in-plain-English-47208 ca 8 b 7 a 6](https://grassfedcode.medium.com/pep-517-and-518-in-plain-english-47208ca8b7a6)的引入，python 打包预计未来会有很大变化**

# **多方面的**

**还有其他需要考虑的因素吗？**

*   **在安装软件包之前，Conda 解析并打印将要安装的软件包，让用户有机会在经历漫长的安装过程之前继续或重新考虑**
*   **更改项目目录的名称/路径会破坏 pipenv 环境，并自动创建一个新环境(参见
    [https://github.com/pypa/pipenv/issues/796](https://github.com/pypa/pipenv/issues/796))**
*   **Conda 不会自动创建/更新 environment.yaml 文件，不像 pipenv 会更新 Pipfile。因此，如果您忘记更新您的 environment.yaml 文件，您的环境和 environment.yaml 文件可能会变得不同步**

# **有用的资源**

****python 打包生态系统回顾** [https://packaging.python.org/overview/](https://packaging.python.org/overview/)
[https://towardsdatascience . com/packaging-in-python-tools-and-formats-743 EAD 5 f 39 ee](/packaging-in-python-tools-and-formats-743ead5f39ee)**

****https://realpython.com/pipenv-guide/**
T22 导游**

****数据科学家 conda/Anaconda 指南** *(Whist 面向 Windows 该理论适用于任何操作系统)*
[https://real python . com/python-Windows-machine-learning-setup/](https://realpython.com/python-windows-machine-learning-setup/)**

****康达与 pip 的比较**
[https://jakevdp . github . io/blog/2016/08/25/康达-神话-误解/](https://jakevdp.github.io/blog/2016/08/25/conda-myths-and-misconceptions/)
[https://www.anaconda.com/blog/understanding-conda-and-pip](https://www.anaconda.com/blog/understanding-conda-and-pip)**

****确保可重现的构建，并且仍然能够快速更改您的依赖关系**
[https://python speed . com/articles/conda-dependency-management/](https://pythonspeed.com/articles/conda-dependency-management/)**

****打包你的 Python 代码的选项**
[https://pythonspeed.com/articles/distributing-software/](https://pythonspeed.com/articles/distributing-software/)**