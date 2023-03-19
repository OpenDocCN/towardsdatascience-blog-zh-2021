# 双重引导已死:验尸

> 原文：<https://towardsdatascience.com/dual-boot-is-dead-the-post-mortem-cee4db42d059?source=collection_archive---------1----------------------->

## 只需一个命令，就可以将任何机器变成开发人员工作站。

![](img/5af717dbf55a5e1c6f0bff46481d6032.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1415562)

两年前，我有一个问题:一方面，我工作的公司是一个重度微软产品用户。微软办公软件对我的工作至关重要。同时，我更喜欢在 Linux 机器上进行编程工作。我的 Linux 环境中的一个 Windows 虚拟机运行太慢。

所以，我想出的解决方案很简单。我决定双启动 Ubuntu 和 Windows 10。很长一段时间里，双靴都是答案。然而，一百万次上下文切换之后，Linux 的 Windows 子系统，简称 WSL，发布了。我决定尝试一下，并开始将我的部分工作流程转移到 Windows 和 WSL。

</dual-boot-is-dead-windows-and-linux-are-now-one-27555902a128>  

尽管如此，许多东西还是不见了。但是 WSL 2 来了，它似乎是一个游戏改变者。一年后，WSL 2 仍然没有实现它的承诺或潜力。也许有一天会的，但我现在需要一些东西。此外，由于我是远程工作，我需要一些可移植的东西，一个我可以随身携带并在 10 分钟内设置好的开发环境。

这个故事介绍了解决这个问题的两个解决方案，以及如何将它们结合起来以获得最佳效果！到目前为止，这种组合解决方案对我来说非常有效，但是我很想在下面的评论区听到你的建议！

> [*学习率*](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=dual_boot_post_mortem) *是为那些对 AI 和 MLOps 的世界感到好奇的人准备的简讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅* [*此处*](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=dual_boot_post_mortem) *！*

# 便携式基站

当我坐下来完成一些工作时，我需要三样东西:

1.  我最喜欢的代码编辑器
2.  我正在做的项目
3.  项目的依赖项

我是机器学习工程师，所以大部分时间都在摆弄 Python 脚本。我最喜欢的代码编辑器是 [Visual Studio 代码](https://code.visualstudio.com/)。当我坐在一个全新的工作站前时，这是一个我可以遵循的算法:

1.  从 GitHub 克隆项目
2.  创建一个 python 虚拟环境(`python -m venv venv`)
3.  激活环境(`source venv/bin/activate`)
4.  安装依赖项(`pip install -r requirements.txt`)

看起来很简单，对吧？一点也不！首先，我是否安装了 Git？如果没有，我必须安装和配置它。我是否安装了正确版本的 Python？如果没有，我得安装`pyenv`，安装我需要的 Python 版本，重新开始。我安装了`venv`吗？这是一场噩梦。

所以，我们用 Docker 吧。让我们将项目及其依赖项打包到 docker 映像中，很快就可以成为开发容器。但是我是否安装了 Docker 并配置为与注册表对话？这开始让我心烦。

我需要一个便携式底座。一些我可以快速设置的东西。一些已经具备所有核心元素的东西:Git、Docker、Python 甚至是用于测试我的部署的单节点 Kubernetes 集群。

输入流浪者。要获得便携式基础工作站，我必须执行一次以下算法的步骤:

1.  从一个基本的 Ubuntu 流浪盒开始
2.  安装和配置我需要的一切:Git，Docker，K3s，我需要的其他 Debian 包等等。
3.  将我的环境打包到一个新的流浪盒中(`vagrant package --output workstation.box`)
4.  上传盒子到流浪云

现在，当我坐在一个全新的工作站前，我要做的就是安装好游民和 VirtualBox，从游民云上拉下我的游民盒子。这需要不到 10 分钟的时间，并且以后永远可用。

# 代码编辑器

下一步是解决代码编辑器的问题。当我在 VirtualBox 中运行 Ubuntu 时，我更喜欢使用不带 GUI 的 Ubuntu。它只是让事情变得更快更容易。终端环境绰绰有余。

但是，我之前说过，我最喜欢的代码编辑器是 VS Code。那么，我现在如何在虚拟机内部使用它呢？我应该直接使用 ViM 吗？

虽然 ViM 真的很棒，而且我每天都在使用它，尤其是当我在 Jupyter 内部工作时，我仍然更喜欢使用 VS 代码。

</jupyter-has-a-perfect-code-editor-62147cb9bf21>  

Visual Studio 代码是一个免费的、轻量级的、跨平台的代码编辑器。它可能不像 IntelliJ Idea 或 T2 py charm 那样是一个成熟的 IDE，但它是一个强大的、不受干扰的工具，拥有专门的粉丝群和蓬勃发展的生态系统。

更重要的是，VS 代码有一个扩展，它将保持您的环境整洁，并且您的项目是隔离的和可维护的。它可以让你在任何地方从事任何项目。一个可以让你与团队中的任何人毫无争议地合作的平台。欢迎来到*远程*开发的美丽世界。

Visual Studio 代码的[远程开发](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)扩展是您需要安装在本地 VS 代码设置中的唯一扩展。使用它，你将能够在你想要的开发环境中工作；需要更多资源吗？一个特定的 Linux 发行版？专门的硬件？没问题。

因此，您可以使用它的[远程 SSH](https://aka.ms/vscode-remote/download/ssh) 变种来连接到您的 VM。此外，您在 VM 内部安装的任何扩展都安装在那里，因此它不会污染核心 VS 代码安装。要了解更多关于如何设置和使用它的信息，请阅读下面的故事:

</the-only-vs-code-extension-you-will-ever-need-e095a6d09f24>  

# 摘要

两年前，我有一个问题:一方面，我工作的公司是一个重度微软产品用户。同时，我更喜欢在 Linux 机器上进行编程工作。

从长远来看，双引导对我来说并不奏效，所以我转向了 WSL 2。一年后，WSL 2 仍然没有实现它的承诺或潜力。我需要一些可移植的东西，一个我可以随身携带的开发环境，并在 10 分钟内设置好。

我的解决方案总结为以下五个步骤:

1.  安装 VirtualBox
2.  安装流浪者
3.  安装 VS 代码和远程扩展
4.  从流浪云中取出我的“工作站”盒子
5.  SSH 进入虚拟机并开始工作

这个解决方案是可移植的，你设置一次，它就永远在那里。此外，它的设置需要不到 10 分钟，它很容易维护和扩展。请随意采用它，但最重要的是，请随意建议我可以做些什么来改进它。作为一名 ML 工程师，我总是在寻找改进算法的机会。

# 关于作者

我叫[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=dual_boot_post_mortem)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据运算的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。