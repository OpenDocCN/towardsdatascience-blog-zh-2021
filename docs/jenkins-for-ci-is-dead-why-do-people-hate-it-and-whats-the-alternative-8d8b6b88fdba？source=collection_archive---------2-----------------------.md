# 詹金斯为 CI 死了:为什么人们讨厌它，有什么替代方案？

> 原文：<https://towardsdatascience.com/jenkins-for-ci-is-dead-why-do-people-hate-it-and-whats-the-alternative-8d8b6b88fdba?source=collection_archive---------2----------------------->

## 如何自动建立你的 Docker 图像；案例研究。

![](img/d3942d2f96c7a0fdc88129a47e2877b5.png)

艾米莉·法里斯在 [Unsplash](https://unsplash.com/s/photos/chef?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

Jenkins 是一个独立的、开源的自动化服务器，提供了用于 Windows、Mac OS X 和其他类似 Unix 的操作系统的软件包。

如果你访问该项目的[登陆页面](https://www.jenkins.io/)，它会告诉你 Jenkins 是领先的开源自动化服务器，拥有数百个插件来支持任何项目的构建、部署和自动化。

这种说法可能是对的，但这并不意味着 Jenkins 提供了一种简单直接的方法来实现这一切。相反，它提供了数百个插件，如果你想完成任何事情，你都需要设置这些插件，这使得这个项目既通用又复杂。

在这个故事中，我们看到了为什么如果你想在 2021 年建立一个持续集成(CI)管道，Jenkins 不是合适的选择，以及有什么替代方案。如果你有任何疑问，可以在评论区开始讨论！

> [Learning Rate](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=jenkins) 是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。订阅[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=jenkins)！

# 自动构建您的 Docker 图像

我相信很多阅读这篇文章的人都喜欢詹金斯。我相信你们中的很多人现在都很愤怒。也许你已经花了几个月的时间来学习 Jenkins，并真正深入到了它的内部。如果你处在类似的位置，我可以理解你为什么还不准备放弃。

为了试图说服你，我将用一个简单的例子。今天我将介绍最流行的 CI 用例:如何自动化构建和推送 Docker 映像的过程。

然而，这不是一个关于如何用 Jenkins 构建和推送 Docker 映像的教程。相反，我将只提供一个高层次的观点。因此，假设您已经有了一个将代码推送到 Github 的存储库，下面是实现最终目标所需的步骤:

1.  安装 Jenkins: Jenkins 通常作为一个独立的应用程序在它自己的进程中运行，带有内置的 Java servlet 容器/应用服务器(Jetty)。但是，您也可以使用官方 Docker 映像在容器中运行 Jenkins。
2.  与 GitHub 集成:你可以通过浏览器(通常在`localhost:8080`上)访问 Jenkins 并创建一个新项目。您应该指定 GitHub 存储库 URL 地址，何时触发构建，以及目标命令。
3.  安装 Docker 插件:要构建图像并将其推送到存储库(例如 Dockerhub)，您需要安装一个新的插件。例如，您可以安装 Docker 插件、Docker 构建步骤和 CloudBees Docker 构建和推送插件。
4.  配置 Docker 插件:您应该在 Jenkins UI 中指定一个新的构建步骤，利用新安装的插件。此外，您应该指定您想要将图像推入的注册表、图像名称和标记以及您的凭证。

# 为什么这样不好？

现在我们已经看到了这个过程，你可能会说这还不算太糟。只需四步。然而，有些步骤并不容易跨越:

*   Jenkins 实例通常太复杂。这意味着组织需要一名 Jenkins 专家来安装、维护和保护实例。
*   专家成为瓶颈。作为开发人员，您依赖 Jenkins 专家或管理员来创建新项目、构建、发布等。这很快成为一个漫长的过程。
*   自助服务寻求者将创建新的、不安全的 Jenkins 实例来解决专家瓶颈问题。此外，他们还会不经测试就安装新插件，引入漏洞。
*   通常，在开始使用 Jenkins 之前，你需要安装大量的插件。不幸的是，这使得实例过于复杂，难以导航，而且 Jenkins 控制器的速度也慢了下来。
*   您可能不希望在自动化工具上做这么多手工工作。Jenkins 很难有效地扩展。并不是所有的插件都受`Jenkinsfile`支持，所以一个 Jenkins 实例很难在没有手工操作的情况下备份。此外，数据库只是磁盘上的 XML 文件，这使得纵向扩展或横向扩展或执行高可用性升级成为一场噩梦。
*   Jenkins 需要一台专用服务器(或几台服务器)来运行。这导致了额外的费用。通常情况下，一个组织需要有一个 DevOps 团队专门负责 Jenkins。

# 有什么选择？

对于 CI，也许不仅仅是 CI，我更喜欢使用 GitHub 动作。那么，如何用 GitHub 动作构建和推送 Docker 图片呢？让我们看看步骤:

1.  设置:在项目的根文件夹中创建一个新文件夹。把它命名为`.github/workflows`。
2.  配置:在您创建的`workflows`文件夹中添加一个新的 YAML 配置文件。这个文件看起来有点像[这个](https://raw.githubusercontent.com/dpoulopoulos/techtrends/master/.github/workflows/techtrends-dockerhub.yaml)。一系列步骤，每一步都做一件简单的事情。
3.  认证:将你的注册中心(例如 Dockerhub)的证书作为一个[加密的秘密](https://docs.github.com/en/actions/reference/encrypted-secrets)添加到你的项目中。
4.  Run:开始这个过程所需要的只是将新的代码提交到 GitHub。

最重要的事？无需安装、配置、维护、升级或修补。一切都运行在 GitHub 服务器上，你不必在意。此外，有了 [GitHub marketplace](https://github.com/marketplace) (把它当成 Jenkins 插件的替代品)，你几乎可以做任何事情。

# 结论

Jenkins 是领先的开源自动化服务器，拥有数百个插件来支持任何项目的构建、部署和自动化。

然而，Jenkins 的方法并不是建立 CI 渠道的简单直接的方法。安装、维护、插件、安全，等等，使得过程变得缓慢，制造瓶颈，并引入漏洞。

这个故事展示了使用 Jenkins 和 GitHub 操作自动构建和推送 Docker 图像所需的步骤。在我看来，使用像 GitHub Actions 这样的工具就像用 Jenkins 的方式一样。你有什么看法？

# 关于作者

我叫 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=jenkins) ，是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。