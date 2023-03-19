# 创建您的自定义私有 Python 包，您可以从您的 Git 存储库中 PIP 安装该包

> 原文：<https://towardsdatascience.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893?source=collection_archive---------1----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

## 使用您的 git repo 共享您的自建 Python 包。

![](img/8180901db2ace15e07c5e42131a50407.png)

像蜜蜂传播花粉一样传播你的代码(图片由[迪米特里·格里戈列夫](https://unsplash.com/@at8eqeq3)在 [Unsplash](https://unsplash.com/photos/yxXpjF-RrnA) 上提供)

您已经创建了一些方便的脚本，希望您的同事或其他人使用。在许多公司，像这样的代码被复制并通过电子邮件互相发送。尽管电子邮件是一种非常容易使用的共享代码的工具，但是我们已经不是生活在 90 年代了，所以让我们以一种聪明的方式来分发你的代码。

本文分两步解决上述问题:打包和分发。首先，我们将着重于将您的代码转换成 python 包，以便人们可以轻松地安装它。然后我们将把这个包放到一个存储库中(比如在 Github 或 Bitbucket 上)，这样人们就可以访问它了。在本文结束时，您将:

*   了解 Python 包的要求
*   能够构建 Python 包或将现有项目转换成包
*   能够从存储库中 pip 安装自建包
*   能够更新您的软件包

我们来编码吧！

# 目标和准备工作

我们在一家为餐馆进行分析的公司工作。餐馆的老板收集关于客人、饭菜和价格的数据。他们给我们发送了一个数据文件，里面有一个具体的问题，比如“什么样的客人会吃鸭肉橙子？”，“我们的南瓜汤是不是定价过高？”，以及“自从我们降低了甜点的价格后，我们看到顾客增加了吗？”

在我多年的工作中，我注意到我使用了很多相同的代码，只是从以前的任务中复制过来。我们的目标是创建一个“工具箱”包，其中包含一些非常通用的代码，我和我的同事可以很容易地用 pip 安装这些代码。然后，每当我们中的一个人想到另一个方便的功能时，我们可以添加它并更新我们的包。

为了实现这一点，我们将首先**打包**我们现有的代码到一个 Python 包中。然后我们将关注**分发**这个包，这样我的同事就可以得到它。

# 包装我们的代码

首先，我们将使用我们的函数并创建一个 Python 包。如果您想使用 pip 进行安装，这是必要的。您可以在 [**这个资源库**](https://github.com/mike-huls/toolbox) 中查看我的所有文件夹和文件。

[](/create-and-publish-your-own-python-package-ea45bee41cdc) [## 创建并发布您自己的 Python 包

### 关于如何 pip 安装您定制的软件包的简短指南

towardsdatascience.com](/create-and-publish-your-own-python-package-ea45bee41cdc) 

我们将在下面的步骤中创建一个。查看上面关于如何创建公共 Python 包的文章

## **1。创建一个 venv**

创建一个虚拟环境并添加一个 gitignore，否则，我们将创建一个不必要的大包。

[](/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b) [## 绝对初学者的虚拟环境——什么是虚拟环境，如何创建虚拟环境(+例子)

### 深入探究 Python 虚拟环境、pip 和避免纠缠依赖

towardsdatascience.com](/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b) 

## **2。创建包文件夹**

用您的包的名称创建一个文件夹。对我来说，这就是“工具箱”。这将是我们正在安装的软件包。我们将在包文件夹中创建一些文件:

*   toolbox/functions.py
    这个文件将保存一些我们想要共享的函数。我已经包含了三个函数:`listChunker`、`weirdCase`和`report`。
*   工具箱/__init__。py
    这将告诉 python 工具箱文件夹是一个 Python 包。这个文件也可以用来导入函数，这样我们除了`from toolbox.functions import listChunker`还可以`import listChunker from toolbox`。创建该文件是必需的，但内容是可选的

[](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e) [## Python 为什么这么慢，如何加速

### 看看 Python 的瓶颈在哪里

towardsdatascience.com](/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e) 

## **3。创建 setup.py**

这个文件是告诉 pip 你的软件包需要什么来安装它所必需的。让我们来看看我用过的 setup.py。

下面我们将浏览需要更多解释的安装文件行。

*   第 3 行:在一个名为 long_description 的变量中加载 README.md。这是可选的。
*   第 7 行:给我们的包命名。必须与您的包文件夹名称匹配
*   第八行。这是我们软件包的版本。Pip 使用这个版本来查看软件包是否需要更新，因此如果您希望用户能够更新，请确保增加这个版本
*   第 12 行和第 13 行:从第 3 行加载 README.md 第 13 行指出了自述文件的格式。
*   第 14 行:您的回购协议的 URL
*   第 15 行:可选地列出一些方便的 URL
*   第 18 行:用户如何使用你的套餐？检查[choosealicense.com](https://choosealicense.com/)
*   第 19 行:需要构建的所有包的列表:确保这与您的包文件夹名匹配
*   第 20 行:您的包所依赖的包的列表。尽管我的函数都不使用请求，但出于演示的目的，我还是决定包含它。在这里包含一个包可以确保当 pip 安装工具箱包时，首先安装 requests，以便工具箱可以使用它。

[](/thread-your-python-program-with-two-lines-of-code-3b474407dbb8) [## 用两行代码线程化您的 Python 程序

### 通过同时做多件事来加速你的程序

towardsdatascience.com](/thread-your-python-program-with-two-lines-of-code-3b474407dbb8) 

## **4。其他可选文件**

我决定包含一个 README.md 和一个许可文件。这些是简单的文本文件，并不是真正需要的，但却是一个很好的补充。

我们的仓库是完整的！大家来了解一下怎么分配吧！

# 通过 GitHub 发布我们的代码

现在我们的包已经创建好了，我们可以使用一个存储库来进行分发。首先，我们将创建存储库并使用它来 pip 安装我们的包。最后，我们将在修改源代码后更新我们的包。

[](/image-analysis-for-beginners-destroying-duck-hunt-with-opencv-e19a27fd8b6) [## 用 OpenCV 破坏猎鸭——初学者的图像分析

### 编写代码，将击败每一个鸭子狩猎高分

towardsdatascience.com](/image-analysis-for-beginners-destroying-duck-hunt-with-opencv-e19a27fd8b6) 

首先，创建一个存储库。你可以在任何使用 Git (GitHub、BitBucket 等)的平台上做到这一点。然后添加你所有的文件，确保忽略不必要的文件，并推送至 repo。

## Pip 安装

复制您的存储库的 URL。您可以使用以下 URL pip 安装您的软件包:

```
pip install git+[https://github.com/mike-huls/toolbox.git](https://github.com/Muls/toolbox.git)
```

就是这样！很容易不是吗？另请注意，您可以从公共(如工具箱)和私有存储库安装！

[](/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e) [## 用 5 行代码创建一个快速、自动记录、可维护且易于使用的 Python API

### 非常适合只需要一个完整、有效、快速和安全的 API 的(没有经验的)开发人员

towardsdatascience.com](/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e) 

## 更新您的包

假设我的同事提出了一个新功能，并决定将其提交给存储库。我可以使用 pip 来更新我的软件包。每次我调用`pip install git+[https://github.com/mike-huls/toolbox.git](https://github.com/Muls/toolbox.git)` pip 都会检查 setup.py 文件中的版本号。如果我的同事记得增加版本，那么我的包就会更新。轻松点。

## 将我们的包裹放入码头集装箱

在 Docker 中使用你的包有一个非常简单的技巧:

[](/use-git-submodules-to-install-a-private-custom-python-package-in-a-docker-image-dd6b89b1ee7a) [## 使用 git 子模块在 docker 映像中安装一个私有的定制 python 包

### 这是一个复杂的题目，但我发誓并不难

towardsdatascience.com](/use-git-submodules-to-install-a-private-custom-python-package-in-a-docker-image-dd6b89b1ee7a) 

## 其他优势

GitHub 提供了一个记录问题的地方，在“首页”有一个很好的自述文件，甚至在你的包需要更多解释时提供一个 wiki。

# 结论

正如我们在本文中看到的，结合 Python 打包和 Git 的能力提供了很多优势:

1.  从一个中心来源(一个真实的来源)轻松分发、安装和更新
2.  我们包的版本控制和协作能力
3.  能够在修改后更新软件包
4.  能够从私有存储库 pip 安装和更新软件包

我希望我已经阐明了很多创建和分发 Python 包的过程。如果你有建议/澄清，请评论，以便我可以改进这篇文章。与此同时，请查看我的[其他文章](https://mikehuls.medium.com/)关于各种与编程相关的主题，例如:

*   [Python 中的多任务处理:通过同时执行多个任务，将程序速度提高 10 倍](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e)
*   [用 FastAPI 用 5 行代码创建一个快速自动归档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)
*   [从 Python 到 SQL —安全、轻松、快速地升级](https://mikehuls.medium.com/python-to-sql-upsert-safely-easily-and-fast-17a854d4ec5a)
*   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)
*   [创建您的定制私有 Python 包，您可以从您的 Git 库 PIP 安装该包](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893)
*   [面向绝对初学者的虚拟环境——什么是虚拟环境以及如何创建虚拟环境(+示例)](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)
*   [通过简单的升级大大提高您的数据库插入速度](https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424)

编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟着我！](https://mikehuls.medium.com/membership)

[](https://mikehuls.medium.com/membership) [## 通过我的推荐链接加入 Medium—Mike Huls

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

mikehuls.medium.com](https://mikehuls.medium.com/membership)