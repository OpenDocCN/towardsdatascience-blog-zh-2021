# 在 Python 中使用相对路径的简单技巧

> 原文：<https://towardsdatascience.com/simple-trick-to-work-with-relative-paths-in-python-c072cdc9acb9?source=collection_archive---------0----------------------->

## 轻松计算运行时的文件路径

![](img/c40e76a964160c0d687200383820dc53.png)

让我们计算目标文件的路径(图片由 [Tobias Rademacher](https://unsplash.com/@tobbes_rd) 在 [Unsplash](https://unsplash.com/photos/JKnrqrhIOH8) 上提供)

这篇文章的目标是计算一个文件在你的项目的文件夹中的路径。我们计算这个路径的原因是，无论代码安装在哪里，您都可以引用正确的位置。当你与同事分享你的代码或者在网络服务器上部署你的代码时，就是这种情况

您不能硬编码文件的(预期)位置，因为在您的机器或我们要部署到的服务器上不存在`C:/users/mike/myproject/config/.env`。理想情况下，我们希望指定相对于项目根文件夹的路径*。*

在本文中，我们将检验一个简单的 Python 技巧，它允许我们以一种非常简单的方式引用与项目文件夹相关的文件。我们来编码吧！

*TL/DR；查看下面的“诀窍”!*

# 用例

两个简单例子可以说明这个技巧的用处:

*   从`/config/.env`加载环境文件
*   我们有一个接收文件的 API 例如，文件应该总是保存在`data/received_audio_files`。

# 准备

在本文中，我们将像这样设置我们的项目结构:

```
relative_path 
- data 
-- mydata.json 
- processes
-- load_data.py
```

我们所有的代码都包含在一个名为`relative_path`的文件夹中。这是我们项目的根，意味着它保存了我们所有的代码。如你所见，我们有两个文件夹:包含目标 json 文件的 data 文件夹，以及包含`load_data.py`的 processes 文件夹；代码将加载`mydata.json`。

# 加载数据

在这一部分中，我们将从加载 mydata.json 的最明显的方式开始，看看它的缺陷。然后我们会慢慢改进，以一个相对加载它的方法结束。然后我们将把这些知识汇编成一个简单的技巧，这对我们未来的所有项目都有帮助。

## 1.绝对路径

最直接的方法是在代码中使用绝对路径。

```
import json
f = open(r’C:\projects\relative_path\data\mydata.json’)
data = json.load(f)
```

虽然这在我的笔记本电脑上运行得很好，但是如果我们将代码部署在服务器上，如果我们将代码放在另一个文件夹中，这可能会失败。Linux 甚至没有 c 盘，所以服务器上肯定没有`C:\project\relative_path folder`。

## 2.使用 __file__

我们可以计算运行时文件的绝对路径:

```
print(__file__)# Results in
# C:\projects\relative_path\processes\load_data.py
```

上面的代码将打印出我们当前正在执行的文件
的位置，在我们的例子中是`C:\projects\relative_path\processes\load_data.py`。我们可以在那里工作。

## 3.正在计算 __file__ 的目录名

获取我们正在执行的文件的文件夹路径使我们更接近了:

```
import os
print(os.path.dirname(__file__))# Results in
# C:\projects\relative_path\processes
```

## 4.导航文件夹

我们必须转到一个文件夹，转到我们的根路径，然后转到`data`文件夹，以便获得我们的 json 数据的位置:

这里我们取我们的目录路径，并使用 os.path.join 来导航:

1.  首先，我们将使用“..”上移一个文件夹(这与在[端子](https://mikehuls.medium.com/terminals-consoles-command-line-for-absolute-beginners-de7853c7f5e8)中相同)。这将把我们导航到父文件夹，在本例中是根文件夹。
2.  然后我们加入“数据”来导航到数据目录。
3.  最后我们加入文件名

此外，我们调用 os.path.realpath 来“计算”`‘..’`-命令，因此它将产生我们的绝对路径。

## 导航故障

不错！我们已经在运行时计算了正确的路径。然而，问题是我们必须导航很多次。假设我们正在这个位置的文件中工作:`\some_folder\some_process\datacollection\thing_one.py`。如果我们想从这个文件加载' mydata.json '文件，我们必须运行:

从文件中我们必须向上移动三次。这有点复杂。我们也不能移动文件，因为它计算了我们的数据*相对于我们正在执行的文件*的路径。让我们在下一部分清理一下。

![](img/6222c3b9a485b73330331ba4a5d431d5.png)

一条非常清晰和笔直的前进路线(图像由[弹出&斑马](https://unsplash.com/@popnzebra)上的[无刷](https://unsplash.com/photos/xDxiO7sldis)产生)

# 诀窍是

诀窍是定义一个变量来计算文件中根目录的绝对路径，然后从项目中的任何地方导入这个变量。它是这样工作的:

在我们的项目根目录下创建一个名为`config`的文件夹，其中包含一个名为`definitions.py`的文件。我们将把下面的代码放在里面:

这一小段代码将始终计算出我们项目根目录的正确路径。我们知道文件在项目中的位置，只需向上移动一个位置。然后，在其他文件中，我们可以导入 ROOT_DIR 变量。

我们可以做以下事情来代替前一部分中冗长而复杂的代码:

这种方法不仅可读性更好，而且不会将处理文件的位置绑定到数据文件的位置:我们计算数据文件*相对于项目*根的位置。您可以将带有 definition.py 文件的 config 目录添加到任何项目中，这样您就有了一个非常好的处理相对路径的通用解决方案。

![](img/55d5e749c2034e12245aca6ec8f5a8d8.png)

如此美丽的小路(图片由[莉莉·波普](https://unsplash.com/@lili_popper)在 [Unsplash](https://unsplash.com/photos/lu15z1m_KfM) 上拍摄)

# 结论

使用包含单个变量的单个文件，我们可以大大简化路径计算！此外，这个技巧非常简单，可以很容易地复制到每个项目中。使用定义文件，我们可以轻松地将环境变量加载到 python 程序中。更多关于这些在 [**这篇**](https://mikehuls.medium.com/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af) 中。

如果你有建议/澄清，请评论，以便我可以改进这篇文章。同时，看看我的其他关于各种编程相关主题的文章，比如:

*   [用 FastAPI 用 5 行代码创建一个快速自动归档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)
*   [Python 到 SQL —安全、轻松、快速地升级](https://mikehuls.medium.com/python-to-sql-upsert-safely-easily-and-fast-17a854d4ec5a)
*   [创建并发布自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)
*   [创建您的定制私有 Python 包，您可以从您的 Git 库 PIP 安装该包](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893)
*   [绝对初学者的虚拟环境——什么是虚拟环境，如何创建虚拟环境(+示例)](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)
*   [通过简单的升级大大提高你的数据库插入速度](https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424)

编码快乐！

—迈克

又及:喜欢我正在做的事吗？[跟着我](https://mikehuls.medium.com/membership)！

[](https://mikehuls.medium.com/membership) [## 通过我的推荐链接加入 Medium—Mike Huls

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

mikehuls.medium.com](https://mikehuls.medium.com/membership)