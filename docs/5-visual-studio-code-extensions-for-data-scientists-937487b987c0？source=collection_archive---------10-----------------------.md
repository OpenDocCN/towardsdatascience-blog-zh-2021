# 面向数据科学家的 5 个 Visual Studio 代码扩展

> 原文：<https://towardsdatascience.com/5-visual-studio-code-extensions-for-data-scientists-937487b987c0?source=collection_archive---------10----------------------->

## 借助这些强大的附加组件，提高 VS 代码的生产率

![](img/09d83d464fa1862d5c5e08cdd771880e.png)

图片由[皮克斯拜](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=996004)的[金钟培](https://pixabay.com/users/ruins-1404384/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=996004)拍摄

[Visual Studio Code](https://code.visualstudio.com/) 是一个免费的、轻量级的、跨平台的代码编辑器。它可能不像 IntelliJ Idea 或 PyCharm 那样是一个成熟的 IDE，但它是一个强大的、不受干扰的工具，拥有专门的粉丝群和蓬勃发展的生态系统。

VS 代码内置了对 JavaScript、TypeScript 和 Node.js 的支持，但是您可以扩展它以支持许多其他语言(Python、C++和 Go)。它提供的功能包括[智能感知](https://code.visualstudio.com/docs/editor/intellisense)代码完成、简化的[调试](https://code.visualstudio.com/docs/editor/debugging)体验、林挺、类型检查、代码导航，以及与 Git 和 GitHub 的深度集成。更重要的是，一系列强大的插件可以让 VS 代码成为最终的开发环境。

这个故事从数据科学家的角度看 VS 代码，重点关注 Python 生态系统和工具，它们使机器学习模型的部署成为一种乐趣。

> [学习率](https://www.dimpo.me/newsletter?utm_source=article&utm_medium=medium&utm_campaign=vs_code_extensions&utm_term=vs_code)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# Python 和数据科学

Visual Studio 代码自带对 JavaScript 生态系统和 web 开发的原生支持，但远不止于此。本节展示了如何将 VS 代码转换成一个功能丰富的 Python 编辑器，以满足数据科学的需求。

*   **VS 代码的 Python 扩展:**如果你是 Python 开发者，安装 VS 代码后的下一步应该是下载并设置 VS 代码的 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)。这很简单，导航到左边的`Extensions`面板，按名称搜索，然后点击 install。VS 代码的 Python 扩展提供了对 Python 语言的丰富支持，只要你安装了 Python 3.6。它支持智能感知代码完成、调试、代码导航、格式化、重构、代码片段等等！
*   **Pylance:** 您想要启用的第二个附加组件是 [Pylance](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance) 。Pylance 可以说是 VS 代码的最佳 Python 扩展，它是一个新的、改进的、更快的、功能丰富的 Python 语言服务器。Pylance 是对 Monty Python 的[勇敢的兰斯洛特爵士](https://en.wikipedia.org/wiki/Lancelot)的引用，它依赖于 Python 的核心扩展，并建立在这种体验之上。此外，随着 VS 代码 12 月更新，Pylance 可以执行期待已久的操作，将 Python 开发体验提升到一个新的水平，如自动导入、代码提取和类型检查。在下面的故事中阅读更多关于 Pylance 的内容。

[](/pylance-the-best-python-extension-for-vs-code-ae299f35548c) [## pylance:VS 代码的最佳 Python 扩展

### Microsoft Python 语言服务器的未来以及为什么应该使用它。

towardsdatascience.com](/pylance-the-best-python-extension-for-vs-code-ae299f35548c) 

*   **Jupyter:** 微软[针对 VS 代码的 Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) 扩展为著名编辑带来了对 Jupyter 笔记本的全面支持。[笔记本](https://jupyter.org/)一直是软件思想增量开发的工具。数据科学家使用笔记本记录他们的工作，探索和试验新算法，快速勾画新方法，并立即观察结果。正如 Donald Knuth 曾经说过的，“把一个程序当作一篇文学作品，写给人类而不是电脑。”因此，数据科学家的武器库中不应该缺少这样的扩展。

[](/jupyter-is-taking-a-big-overhaul-in-visual-studio-code-d9dc621e5f11) [## Jupyter 正在对 Visual Studio 代码进行一次大检查

### 把一个程序当作一篇文学作品，写给人类而不是电脑

towardsdatascience.com](/jupyter-is-taking-a-big-overhaul-in-visual-studio-code-d9dc621e5f11) 

# 部署

有了想法并用代码写下来只是开发周期的第一步。因此，在煞费苦心地测试和评估结果之后，部署是您和公众之间的最后一道障碍。

使用 Docker 封装应用程序并使用 Kubernetes 编排其组件已经成为部署的事实标准。为此，以下扩展促进了这一过程，使数据科学家能够控制整个项目生命周期。

**VS 代码的 Docker 扩展:**[Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)扩展简化了构建 Docker 映像、运行容器、附加容器、流式传输日志以及管理其整个生命周期的整个过程。它还提供了 Node.js、Python 和。容器内的 NET Core。如果你想知道更多，请阅读下面的故事。

[](/docker-you-are-doing-it-wrong-e703075dd67b) [## 码头工人:你做错了

### 使用 VS 代码和 Docker 扩展成为 Docker 超级用户。

towardsdatascience.com](/docker-you-are-doing-it-wrong-e703075dd67b) 

**云代码:** [云代码](https://marketplace.visualstudio.com/items?itemName=GoogleCloudTools.cloudcode)可以说是 VS 代码的终极 DevOps 扩展。它包含了 Google 命令行[容器工具](https://github.com/GoogleContainerTools)如`[skaffold](https://skaffold.dev/)`和`[kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)`的强大功能，并使在 Kubernetes 上开发成为无缝体验。有了这个工具，你就可以在云中运行你的应用程序，或者在像 minikube 这样的本地 Kubernetes 上运行，就像你在笔记本电脑上运行一样。阅读下面故事中关于云代码和`skaffold`的模式。

[](/kubernetes-development-beyond-configuration-files-f78d7ab9a43) [## Kubernetes 开发:超越配置文件

### 关注你的代码，而不是基础设施！

towardsdatascience.com](/kubernetes-development-beyond-configuration-files-f78d7ab9a43) [](/kubernetes-local-development-the-correct-way-1bc4b11570d8) [## Kubernetes 地方发展:正确的道路

### 在 Kubernetes 上开发时，痛苦和苦难的无限循环已经成为过去。

towardsdatascience.com](/kubernetes-local-development-the-correct-way-1bc4b11570d8) 

*   **远程开发:**最后但并非最不重要的一点，[远程开发](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)让你可以随时随地，从几乎任何媒介进行编码；只要安装了 VS 代码，就不需要花里胡哨的机器来做深度学习。只需连接到具有 GPU 加速、大量 RAM 和多核 CPU 的虚拟机。除此之外，远程开发还可以帮助您在不同的、隔离的环境之间切换，与其他团队成员协作，当您在 Windows 机器上时在 Linux 上开发(使用 WSL)，或者使用容器来开发您的应用程序，保持您的系统整洁。

[](/the-only-vs-code-extension-you-will-ever-need-e095a6d09f24) [## 你唯一需要的 VS 代码扩展

### 如果您必须安装一个 Visual Code Studio 扩展，这就是它！

towardsdatascience.com](/the-only-vs-code-extension-you-will-ever-need-e095a6d09f24) 

# 序

Visual Studio 代码是一个免费的、轻量级的、跨平台的代码编辑器。然而，它的真正力量在于它的扩展。在这个故事中，我展示了我在开发机器学习应用程序时几乎每天都会用到的 5 个扩展。

Python 和 Jupyter 扩展帮助我完成工作，而 Kubernetes 生态系统工具让我了解部署过程，因为容器化是新标准。我希望它们对你的日常活动也有帮助。

# 关于作者

我的名字是[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=article&utm_medium=medium&utm_campaign=vs_code_extensions&utm_term=vs_code)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=article&utm_medium=medium&utm_campaign=vs_code_extensions&utm_term=vs_code)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！