# 创建并发布您自己的 Python 包

> 原文：<https://towardsdatascience.com/create-and-publish-your-own-python-package-ea45bee41cdc?source=collection_archive---------12----------------------->

## 关于如何 pip 安装您定制的软件包的简短指南

![](img/4dd8a6477a6c600e4c41cf89bcee03fe.png)

这些鸟是从源头安装的 pip(图片由[詹姆斯·温斯科特](https://unsplash.com/@tumbao1949)在 [Unsplash](https://unsplash.com/photos/PWR9m_ebonQ) 上提供)

您可能对 requests、Pandas、Numpy 和许多其他可以用 pip 安装的包很熟悉。现在是时候创建自己的包了！在这篇文章中，我们将通过所需的步骤来打包和发布您的 Python 代码，供全世界进行 pip 安装。首先，我们将看看如何打包您的代码，然后我们如何发布它，使其易于访问。

*(你更愿意和像同事这样的少数人分享你的代码吗？也可以允许人们从私有库* *中* [***pip 安装包。甚至当***](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893) **[***将你的包放入 Docker 容器时也是如此！***](https://mikehuls.medium.com/use-git-submodules-to-install-a-private-custom-python-package-in-a-docker-image-dd6b89b1ee7a)T22)**

# 目标和准备

在本文中，我创建了几个真正重要的函数，希望与大家分享。让我们创建一个名为“ *mikes_toolbox* 的包，人们只需通过`pip install mikes_toolbox`就可以使用它。

首先，我们将概述一下安装过程是如何工作的:如果有人调用`pip install mikes_toolbox`，代码必须来自某个地方。Pip 在 PyPi 上搜索具有该名称的包；Python 包索引。你可以把这个想象成 Python 包的 YouTube。首先我们将代码打包，然后上传到 PyPi，这样其他人就可以找到我们的包，下载并安装它。

# 包装

首先，我们将封装我们的代码。我们将创建一个名为“toolbox_project”的项目目录，其中包含以下目录和文件:

```
toolbox_project
    mikes_toolbox
        __init__.py
        functions.py
        decorators.py
    LICENSE.txt
    README.md
    setup.cfg
    setup.py
```

项目文件夹的内容由两部分组成:mikes_toolbox 文件夹和 4 个附加文件。该文件夹是我们包含源代码的实际包。这 4 个附加文件包含如何安装软件包的信息和一些附加信息。

![](img/b77fefce75fab238528eab299bf38f14.png)

一个包含我们的代码并附有安装说明的包(图片由 [Christopher Bill](https://unsplash.com/@umbra_media) 在 [Unsplash](https://unsplash.com/photos/3l19r5EOZaw) 上拍摄)

## 包文件夹

mikes_toolbox 是我们实际的包，包含了我们所有的源代码。请确保将该文件夹命名为您想要的包名。在我的案例中，它包含以下内容:

*   **mikes_toolbox/function.py 和 decorators.py** 这是我的源代码。Function.py 包含了一个函数比如叫做 weirdCase()；一个可以让字符串完全不可读的函数。
*   **mikes_toolbox/__init__。py** 这个文件是必需的。它告诉 python mikes _ toolbox 是一个 Python 包文件夹。你可以让它空着。您可以选择在这里包含 import 语句，以便更容易地从您的包中导入代码。一个例子:
    包括`from functions import weirdCase`。这样一来，人们就不必在软件包安装完毕后进行`from mikes_toolbox.functions import weirdCase`，而是直接`from mikes_toolbox import weirdCase`。

## LICENSE.txt

描述人们如何使用您的许可证。从[这个](https://choosealicense.com/)站点中选择一个，然后将内容粘贴到这个文件中。

## README.md

这个文件包含关于这个包的信息:它是做什么的？它有什么特点？如何安装使用？在此查看示例[或在此](https://pypi.org/project/mikes-toolbox/)查看示例[。
这个文件是用标记写的。在这个](https://github.com/mike-huls/toolbox_public/blob/main/README.md)站点上查看一个已经包含一些示例标记的编辑器；只需根据您的需求进行编辑即可。

## setup.cfg

这是一个简单的文件，只包含下面的代码。参考 README.md。

```
[metadata]
description-file = README.md
```

## setup.py

该文件确保软件包正确安装。复制下面的代码，并在需要的地方进行修改。大多数条目都是符合逻辑的，但 download_url 需要一点解释(见下文):

## 下载 _ 网址

当你`pip install pandas` pip 在 PyPi 上搜索熊猫时。一旦找到，PyPi 就告诉 pip 在哪里可以下载这个包。大多数情况下，这是 git 存储库中的一个文件。download_url 关键字指的是这个文件的位置。为了获得一个 URL，我们首先把源代码放在 pip 可以到达的地方。最好的方法是将您的代码上传到 GitHub repo 并创建一个版本。这个过程超级简单，步骤如下:

1.  创建一个存储库(名称不必与包名相匹配，但更有序)。
2.  在你的回购中，在右边；单击“创建新版本”
3.  填写“标签版本”与 setup.py 中的相同(*版本*关键字)
4.  给发布一个标题和描述。这并没有反映在包装中，这纯粹是为了在您的回购中保持一个概览
5.  点击“发布发布”
6.  复制“源代码(tar.gz)”的链接地址
7.  将复制的链接地址作为*下载 url* 的值粘贴在 setup.py 中

就是这样！您的软件包现在可以下载和安装了。下一步是处理分销。

# 分配

到目前为止，我们所做的唯一一件事就是打包我们的代码并上传到 GitHub，pip 仍然不知道我们的包的存在。因此，让我们确保 pip 可以找到我们的软件包，以便人们可以安装它。

![](img/d5870178d480bf908e7b1490c7adabe1.png)

(图片由[凯皮尔格](https://unsplash.com/@kaip)在 [Unsplash](https://unsplash.com/photos/tL92LY152Sk) 上拍摄)

## PyPi 帐户

如果你想在 YouTube 上发布视频，你需要先创建一个帐户。如果你想上传软件包到 PyPi，你也需要先创建一个帐户。去 [PyPi](https://pypi.org/account/register/) 注册一个账号。继续之前，请确认您的电子邮件地址。

## 创建源分布

这将创建一个 tar.gz 文件，其中包含运行包所需的所有内容。打开一个终端，cd 到您的项目目录并执行下面的命令:

```
python setup.py sdist
```

*(对终端不熟悉？查看* [***本***](https://mikehuls.medium.com/terminals-consoles-command-line-for-absolute-beginners-de7853c7f5e8) *文章为绝对基础知识)*

# 上传

我们准备好上传我们的包了！首先，我们需要 pip 安装 Twine，这将有助于我们上传。简单来说就是`pip install twine`。

最后一步是实际上传包。在终端中，如果您还不在项目目录中，请转到该目录并执行

```
python -m twine upload dist/*
```

Twine 会要求您提供 PyPi 凭证，但在此之后，您的包应该会被上传！

# 测试

创建一个新的 python 项目，并(可选地)启动一个新的[虚拟环境](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)。然后`pip install mikes_toolbox`。通过调用来测试代码

```
from mikes_toolbox import weirdCase

print(weirdCase("This function is essential and very important"))
```

# 更新您的包

如果你更新你的包，你需要确保更新 setup.py 中的版本，并在 GitHub 上创建一个带有相同标签的新版本。此外，更新 setup.py 中的 download _ URL。
一旦完成，用户就可以使用

```
pip install mikes_toolbox --upgrade
```

# 结论

在这些简单的步骤中，你已经学会了如何使用 PyPi 和 pip 打包你的代码并发布给全世界。此外，您还创建了一个存放代码的中心位置，可用于跟踪 bug、发布问题或请求新功能。不再通过邮件共享代码

我希望我已经阐明了很多创建和分发 Python 包的过程。如果你有建议/澄清，请评论，以便我可以改进这篇文章。同时，查看我的其他关于各种编程相关主题的文章。编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟我来](https://github.com/mike-huls)！

<https://mikehuls.medium.com/membership> 