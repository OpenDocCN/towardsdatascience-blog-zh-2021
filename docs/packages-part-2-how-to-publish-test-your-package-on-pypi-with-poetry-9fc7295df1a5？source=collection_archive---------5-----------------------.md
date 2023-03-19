# 制作 Python 包第 2 部分:如何用诗歌在 PyPI 上发布和测试您的包

> 原文：<https://towardsdatascience.com/packages-part-2-how-to-publish-test-your-package-on-pypi-with-poetry-9fc7295df1a5?source=collection_archive---------5----------------------->

## Python 诗歌项目指南

在本指南的上一版中，我们使用传统的 setup.py 发布了一个 python 包给 PyPI。

![](img/e8c8d72ab8060a10dd9bf5c7ab5c1e6d.png)

照片由[克里斯汀·休姆](https://unsplash.com/@christinhumephoto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/computer-digital?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

它需要一个奇怪的 Manifest.in 文件，没有附带任何测试，我们甚至没有用 requirements.txt 讨论**依赖管理**，为什么？因为 2021 年不是这样，我的朋友们。

**依赖管理**是关于你的包使用的 Python 库。如果您导入 numpy 并使用它的类/函数，您的包将依赖于 numpy 的**版本**。

如果一个新版本的 numpy 使用了不同的语法或者去掉了一个函数，那么你的包就不能在那个版本的 numpy 上工作。我们需要跟踪您使用的版本。

或者，如果您使用 numpy 和 pandas，并且它们都依赖于其他某个库的不同版本，该怎么办？谁来记录这些事情？！

诗歌会。

poem 是一个 Python 库，我们可以使用它来创建一个包，更容易地将其发布到 PyPI，它将为我们处理**依赖管理**。耶！

要开始，我们只需安装诗歌

```
pip install poetry
```

和往常一样，如果您没有 pip，请尝试 pip 3(python 3 的版本)。

**新诗**

我们的第一个命令是创建目录。当我们有诗的时候，在我们项目的开始没有 mkdir。

真的，在你这样做之前不要为你的项目创建一个目录，因为 poems 会用你的项目名创建一个目录，把所有的东西都放在里面**包括一个也是以你的项目命名的子目录，源代码就放在那里。**

所以，如果你为你的项目做一个目录，并在那里这样做，你会得到一个俄罗斯娃娃的情况:我的包/我的包/我的包/yikes。

相反，只需转到所有包所在的目录(这样当您运行 ls 时，就可以看到您的其他项目)，然后运行:

```
poetry new
```

现在你可以看到你的诗歌目录及其所有部分，包括最重要的 **pyproject.toml.**

现在我们可以开始建立一个诗歌包了！

虚拟环境

但首先，快速绕道一下诗歌作为虚拟环境是如何工作的。

虚拟环境是一个孤立的数字环境，其中的库、代码和解释程序与计算机上的其他程序完全分离。

如果你进入一个数字环境并试图导入 numpy，它将不会存在，因为它没有安装在那个数字环境中。

相反，如果你在数字环境中安装了一个库，它就不会安装在你电脑的其他部分。

本质上，环境中可用的库都存储在该环境中的某个目录中。虚拟环境使用自己的可用库目录，因此它与计算机的可用库目录没有关系。

当我们运行新诗时，我们创建了一个虚拟环境！当我们在诗歌目录中从命令行运行代码时，我们在虚拟环境中是**而不是**。但是，如果我们用

```
poetry run the-rest-of-the-command
```

然后我们可以在诗歌环境中运行这个命令。

或者，如果我们跑了

```
poetry shell
```

然后，我们将进入我们的诗歌文件夹的虚拟环境，在那里我们可以使用我们将通过诗歌安装的库。

用 ctrl+d 退出虚拟外壳。

**诗歌添加**

事实上，让我们看看如何安装带有诗歌的库，这将很好地管理我们的依赖性。

让我们进入我们创建的 pyproject.toml。它将包含两个主要内容:

1.  我们项目的文档(嗯，包括引用自述文件和许可证)。我们将在完成图书馆事务后回到文档上。
2.  我们的包需要的库版本的范围

当我们制作包时，我们需要告诉 poems 我们使用的是什么版本的库。这个命令很简单:

```
poetry add library-name
```

其中 library-name 是我们正在使用的任何库的名称，比如 numpy。

该命令会将库版本添加到我们的 pyproject.toml 中。它实际上是一系列遵循语义版本化的版本。你可以在这里看到语法:【https://classic.yarnpkg.com/en/docs/dependency-versions/[。](https://classic.yarnpkg.com/en/docs/dependency-versions/)

它还会在我们的诗歌虚拟环境中安装库。

**poem . lock**

如果您**poems 添加**一个库，这也将**安装**那个库，您不仅会看到您的 pyproject.toml 自动更新——您还会获得一个新文件:poetry.lock。

pyproject.toml 记录了您的包可接受的库版本的范围，并且只列出了您用 poeties add 直接添加的库。

另一方面，poetry.lock 记录了安装在您的包中的**确切的库版本**。它包括你的依赖项需要的每一个库。

所以如果你把**诗加上 seaborn，**

当你的 pyproject.toml 得到 seaborn 的时候，

你的诗。锁将获得:

*atomicwrites、attrs、colorama、cycler、fonttools、kiwisolver、matplotlib、more-itertools、numpy、packaing、pandas、pillow、pluggy、py、pyparsing、pytest、pytz、scipy、setuptools-scm、six、tomli、wcwidth、* ***和 seaborn***

因为这是 seaborn 所依赖的每一个库，加上每一个库所依赖的每一个库，递归地直到 seaborn 所需要的每一个库都有它所需要的库。

**关于他人使用你的源代码的旁注**

现在，如果有人从 github 克隆了你的包，他们会有你的 pyproject.toml，但是还没有安装你的库。他们需要逃跑

```
poetry install
```

如果他们也有你的 poetry.lock，他们将安装你的 poetry.lock 中声明的精确版本。

如果他们没有你的 poetry.lock，那么 poem 会为你做它做过的事情——找出要安装的编写版本，并将其写入 poetry.lock。

总的来说，poetry.lock 列出了一个包运行时安装的所有精确的库版本，这样你的包的用户就可以使用所有东西的相同版本。

**用 pyproject.toml 进行项目描述**

好的，那么我将尽快跳到 PyPI 发布，我们只需要在这里对我们的包文档稍微好一点。

pyproject.toml 顶部的那个部分将包含 setup.py 在传统打包方法中包含的所有内容，没有诗意。

pyproject.toml 的顶部应该是这样的:

pyproject.toml

```
[tool.poetry]name = “” #your package nameversion = “0.0.1”description = “” #a short description of your package that will be used in PyPI searchauthors = [“your-name <your-email>”]license = “MIT”readme = “README.md”homepage = “” #can be the reporepository = “”keywords = [“test”, “dependencies”, “documentation”]classifiers = [“Development Status :: 5 — Production/Stable”,“Intended Audience :: Education”,“Operating System :: MacOS”,“License :: OSI Approved :: MIT License”,“Programming Language :: Python :: 3”,]include = [“LICENSE”,]
```

继续把它复制到你的 toml 中并更新字段:填写你的名字和电子邮件，项目名称和版本，简短描述，回购/主页，并使用这里的选项填写这些分类器:[https://pypi.org/pypi?%3Aaction=list_classifiers](https://pypi.org/pypi?%3Aaction=list_classifiers)

如您所见，我们引用的是 README.md，所以如果您愿意，可以将 README.rst 更改为 md (markdown)文件。您将把包含变更日志的长项目描述放在那个 **README.md** 中，因此它看起来像这样:

README.md

```
My long description blah blah blah blah.Change Log**================**0.0.1 (Dec 1, 2021) **— — — — — — — — — — — — — — — -**- First Release0.0.2 (Dec 2, 2021) **— — — — — — — — — — — — — — — -**- Did some more stuff
```

**发布到 PyPI**

好了，我们开始吧。我将跳到有趣的部分，然后我将回头提供一些提示。

可以一行发布到 PyPI。在您使用以下工具设置一次您的凭证后:

```
poetry config http-basic.pypi username password
```

这些是你在 pypi.org 的证件，你可以在他们的网页上注册账户。

在您设置了这些凭证之后，下面是一行 publish:

```
poetry publish --build my_package
```

你就这样在 PyPI 上！

有一个单独的诗歌构建命令，但是您可以在发布的同时使用构建标志(— build)进行构建。

**对测试 PyPI 的测试**

但是也许在我们把半成品包扔给 PyPI 之前，我们可以在 PyPI 的测试版本上测试它。

这个测试 PyPI 将允许我们模拟对 PyPI 的更新，然后 pip 安装我们自己的包，看看它是否像预期的那样工作。

这样做的第一步是在 test.pypi.org 上创建一个新帐户(是的，你需要一个与你的 pypi 帐户不同的帐户，但是它可以有相同的用户名)。

然后，您可以将存储库添加到诗歌中:

```
poetry config repositories.testpypi https://test.pypi.org/legacy/
```

发布到存储库:

```
poetry publish –-build -r testpypi
```

pip 从存储库中安装您包:

```
pip install --index-url [https://test.pypi.org/simple/](https://test.pypi.org/simple/) my_package
```

**侧栏:pip 安装故障排除**

如果您遇到问题，这个更长的命令行功能可以解决一些常见问题:

```
python3 -m pip install -i [https://test.pypi.org/simple/](https://test.pypi.org/simple/) — extra-index-url [https://pypi.org/simple](https://pypi.org/simple) my_package==0.0.1
```

pip 之前的 python -m 是为了确保您使用的是正确的 pip 版本。您也可以尝试查看您的 pip 版本和 python 版本

```
which pip
which python
pip --version
python --version 
```

或者更新画中画

```
 python -m pip install --upgrade pip
```

和往常一样，你可能需要用 python3 切换 python，或者用 pip3 切换 pip。

额外的索引 url 指向普通的 PyPI repo(而不是测试)，您的依赖项需要从这里下载。

最后，我还添加了版本，您应该用您的包版本号替换它，以便它得到正确的版本号。

**在新的虚拟环境中安装和导入**

当您尝试 pip 安装时，我将进入一个新的虚拟环境，这样您的软件包模块名称就不会与我们所在的目录名称混淆，该目录也称为您的软件包名称。

我喜欢直接进入我的顶级项目目录，用 poems new 创建一个新目录，用

```
poetry shell
```

pip 从测试 PyPI 安装它。

然后我在虚拟环境中运行 python 解释器

```
poetry run python
```

现在，用 Python 导入您的包和函数！你可以先试试:

```
import my_package
```

接下来，尝试导入您的函数。如果它们在 __init__ 中。py 您可以直接导入它们，如

```
from my_package import my_function
```

或者它们是否在与 __init__ 相同级别的文件中。py，您将指定不带。py 来获取它的模块名。比如 utils.py 就叫 utils。

```
from my_package.my_module import my_function
```

如果在那个级别有一个文件夹。py 文件，通过添加 __init__，将该文件夹变成一个包。也复制到那个文件夹。初始化文件可以是空的。

所以如果

1.  我的 _ 包是整个库，
2.  my_package 文件夹中是 my_subpackage 文件夹，它包含。py 文件和 init 文件，
3.  my_subpackage 文件夹中是 my_module.py
4.  在 my_module.py 中存放着我的 _function，

应该是:

```
from my_package.my_subpackage.my_module import my_function
```

您还可以使用以下命令检查导入的模块中有哪些函数:

```
dir(my_package.my_module)
```

**奖励:用诗歌更新更新所有依赖关系**

为什么总有关于更新东西的加分部分？

总之，为了更新所有的库(依赖项)并将它们写入您的 poetry.lock，您运行:

```
poetry update
```

不要把它和**更新诗歌、**混淆，后者可以通过

```
pip install --upgrade poetry
```

或者

```
poetry self update
```

感谢阅读！

请关注我，了解更多关于 Python 和数据科学的知识！

查看本指南的第 1 部分，学习如何用传统的方式向 PyPI 发布 setup.py，一些其他的技巧，再加上一些幽默。那天我运气很好:

[](https://medium.com/@skylar.kerzner/publish-your-first-python-package-to-pypi-8d08d6399c2f) [## 包第 1 部分:如何将 Python 包发布到 PyPI

### 嘿！欢迎阅读这份关于如何将 python 包发布到 PyPI 的快速、有趣的指南。

medium.com](https://medium.com/@skylar.kerzner/publish-your-first-python-package-to-pypi-8d08d6399c2f)