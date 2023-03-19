# 使用 git 子模块在 docker 映像中安装一个私有的定制 python 包

> 原文：<https://towardsdatascience.com/use-git-submodules-to-install-a-private-custom-python-package-in-a-docker-image-dd6b89b1ee7a?source=collection_archive---------8----------------------->

## 这是一个复杂的题目，但我发誓并不难

![](img/ad146858fcfc62cfe2c978b8b1e8ba04.png)

这条蟒蛇已经包装好了，可以装船了！(图片由[世界光谱](https://www.pexels.com/@worldspectrum)在[像素](https://www.pexels.com/photo/beige-python-on-brown-branch-of-tree-1108192/)上拍摄)

哇，这个标题包含了很多术语！简单地说，这篇文章为您提供了最好的方式，让您可以私下轻松地分享您的 Python 代码，甚至可以在 docker 容器中运行它！最终，您将能够分发随处运行、易于维护和更新的私有包。我们来编码吧！

# 准备

您已经创建了一个 Python 脚本，其中包含了您想要与其他人共享的各种方便的函数。对于如何实现这一点，您有两种选择:

1.  将您的代码打包并在 PyPi [**上公开发布，详见本文**](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc) 。这意味着任何人都可以通过调用`pip install *yourpackage*` 来安装这个包，就像熊猫一样
2.  将你的代码打包并私下分发 [**如本文**](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893) 所述。这意味着只有某些人能够安装这个软件包，比如你公司的同事。

我们选择第二个选项，确保我们的代码保持私有。我们将基于第二个选项中的文章，因此请务必先阅读它。阅读完之后，您就有了一个 Python 包，您可以从您的 g it 存储库中 pip 安装它。

![](img/4f779f8615a2c9c141622488b362275c.png)

一个专为你准备的私人 Python 包！(图片由[像素](https://www.pexels.com/@pixabay)在[像素](https://www.pexels.com/photo/brown-jest-for-you-box-264771/)上拍摄)

# 问题是

如果你遵循了本文 中的说明，你的 git repo 中有一个包，你可以像`pip install git+https://github.com/mike-huls/toolbox.git`一样安装它。在这个例子中，我们将想象我们正在创建一个 Python 项目，它使用了本文中的*工具箱*包。

我们希望在 docker 容器中运行我们的项目。为了构建 Docker 映像，我们必须首先将源代码复制到映像中，然后安装我们使用的所有软件包。这是通过提供我们使用的所有包的列表来完成的。您可以使用`pip freeze > requirements.txt`生成这个列表。生成的 requirements.txt 将包含我们所有包的名称和版本。

问题是即使是我们私装的包也是按名称和版本列出的。这导致 pip 在 PyPi 上搜索*工具箱*，但是它找不到这个包，因为我们已经从我们的 Git repo 中私下安装了它。我们也不能提供我们的`git+[GIT_REPO_URL]`,因为我们的 Docker 映像没有登录 git 的凭证。有一种方法可以使用 ssh 密钥来解决这个问题，但在我看来这很难管理；有一个简单得多的选项，可以让您的同事更容易地获得代码，而不必麻烦地生成或分发 ssh 密钥。

![](img/cf5fca77069c43065fe463b3ce984cce.png)

pip 试图在公共 PyPi 上找到我们的私有包(图片由 Andrea Piacquadio 在 [Pexels](https://www.pexels.com/photo/thoughtful-man-using-smartphone-on-street-3800149/) 上提供)

# 解决方案

我们将使用 git 子模块将包拉到我们的项目目录中，安装它，然后修改我们的 Docker 文件，将包安装到我们的 Docker 映像中。方法如下:

## 1 添加子模块

转到您的项目根目录，然后:

```
git submodule add [https://github.com/mike-huls/toolbox](https://github.com/mike-huls/toolbox) _submodules/toolbox
```

*(为了便于说明，我在这里使用了一个公共存储库)* 这将在您的根目录下创建一个名为 *_submodules* 的文件夹，其中包含另一个包含您的 Python 包的文件夹。

## 2 安装软件包

只需执行
`pip install _submodules/toolbox`来安装软件包

## 3 创建 Dockerfile 文件

一旦我们想要创建 Docker 图像，只需重复前面的两个步骤；将子模块文件夹复制到映像中，然后再次运行安装。

```
**FROM** python:3.8
WORKDIR **/**app

**#** Install regular packages
**COPY** requirements.txt .
RUN pip install **-**r requirements.txt

**#** Install submodule packages
**COPY** _submodules**/**toolbox _submodules**/**toolbox
RUN pip install _submodules**/**toolbox*--upgrade*

**#** **copy** **source** code
**COPY** .**/** .

**#** command **to** run **on** container **start**
CMD [ "python", "./app.py"]
```

就是这样！现在你有了一个拥有“真相之源”的中央储存库。您可以在这里提交问题、添加功能请求、协作和推送更新，非常简单。其他优势包括:

可用性:
用户可以继续使用`pip install git+REPO_URL`；这不足以建立形象。你的程序员可以用一种非常简单的方式继续安装你的包。然后，当代码准备好构建到映像中时，只需拉出子模块并将其包含在 docker 文件中，就可以非常容易地包含该包。

一个干净的 repo
除此之外，git 将为您跟踪您的子模块。调用`pip freeze > requirements.txt``并注意到*工具箱*没有列出。这是因为 git 知道它是一个子模块。它也被忽略了，这样你就不会“污染”你的项目报告。

轻松更新
更新我们项目中使用的软件包非常简单；只需执行:

```
git submodule update --remote --merge
pip install _submodules/toolbox --upgrade
```

这将首先把所有新代码拉到包文件夹中，然后升级包。别忘了用一个[虚拟环境](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)！

![](img/d9eca53cdb670df94677eeafcbcc0b89.png)

用定制的、私人的 Python 包运送那些容器(图片由 [Cameron Venti](https://unsplash.com/@ventiviews) 在 [Unsplash](https://unsplash.com/photos/1cqIcrWFQBI) 上提供)

# 结论

定制的私有 Python 包易于安装、维护、更新和分发。使用这个简单的方法将它们构建成 Docker 图像会使它们更加令人惊叹。

我希望我在这篇文章中的解释是清楚的。如果你有建议/澄清，请评论，以便我可以改进这篇文章。同时，查看我的其他关于各种编程相关主题的文章。编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟我来](https://github.com/mike-huls)！