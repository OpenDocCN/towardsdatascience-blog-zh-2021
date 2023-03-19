# 使用 kubectl 插件简化 Kubernetes 操作

> 原文：<https://towardsdatascience.com/making-kubernetes-operations-easy-with-kubectl-plugins-206493c1f41f?source=collection_archive---------23----------------------->

## 使用这些`kubectl`插件来提高你的生产力，使所有的 Kubernetes 任务和操作更容易，更快，更有效

![](img/6f02931c12639379d19ba5fc0158f030.png)

Gabriel Crismariu 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

`kubectl`是一个功能强大的工具，允许您执行几乎所有与 Kubernetes 相关的任务。不管你是需要列出 pod，调试节点，管理 RBAC 还是其他什么，`kubectl`都可以做到。然而，这些常见任务中的一些可能相当笨重，或者可能包括许多可能需要相当长时间来执行的步骤。在其他情况下,`kubectl`的输出可能不完全可读，或者可能包含大量噪声，这可能非常烦人，尤其是当你试图调试某些东西时，在这种情况下时间是至关重要的。所以，当我们可以避免的时候，为什么要浪费时间在重复的，普通的，耗时的任务上呢？问如何？好吧，让我给你介绍一下`kubectl`插件！

# 什么插件？

`kubectl`附带有限的核心功能，不包括 Kubernetes 管理员或用户可能需要执行的所有任务。因此，为了解决这个限制，我们可以用插件来扩展`kubectl`，插件的功能相当于`kubectl`本身的子命令。所有这些插件都是独立的可执行文件，可以用任何语言编写，但是考虑到我们正在谈论 Kubernetes 工具和生态系统，它们中的大多数显然是用 Go 编写的。

现在你可能会想— *“我在哪里可以找到所有这些插件？为什么不直接使用不带* `*kubectl*` *的独立二进制文件呢？”* -这两个问题的答案是`krew` -一个针对`kubectl`插件和 Kubernetes SIG 的包管理器，旨在解决`kubectl`的包管理问题。

作为一个软件包管理员，有助于发现、安装和更新我们所有的插件，但是要使用它，我们首先需要安装它，因为...`krew`本身也是插件。您可以在此处导航至安装指南/脚本[，使用您喜欢的方法安装。](https://krew.sigs.k8s.io/docs/user-guide/setup/install/)

现在我们有了`krew`，让我们找到并安装一些插件吧！

上面的代码展示了几种搜索和获取特定插件信息的方法。除了使用`kubectl krew`搜索，你还可以使用`krew`网站[的插件索引](https://krew.sigs.k8s.io/plugins/)。除了`kubectl krew`显示的信息之外，这也给了你源代码库的链接和每个插件的 GitHub 星级数。因此，当您找到您需要的东西时，您只需运行`kubectl krew install`并开始使用它:

请注意上面输出中的警告——尽管这些插件被列在官方插件索引中，但这并不保证它们使用起来是安全的，也不保证它们实际上做了它们声称正在做的事情。你应该把所有这些看作是从互联网上下载的任何随机的，未经验证的脚本。

尽管`krew`包含了很多插件，但这并不意味着它是所有可用插件的详尽列表。所以，万一你找不到解决你的任务/问题的插件，你也可以去别的地方看看。一个这样的地方是`[awesome-kubectl-plugins](https://github.com/ishantanu/awesome-kubectl-plugins)` [仓库](https://github.com/ishantanu/awesome-kubectl-plugins)，那里有几个额外的插件，或者你也可以试着谷歌一下。

考虑到这些不是`krew`的一部分，要安装它们，我们需要采用手动方式，如下所示:

如前所述，这些插件只是脚本或二进制文件，因此你可以手动下载并使用它们。如果您想让`kubectl`将它们识别为插件，您还需要以格式`kubectl-plugin-name`给它们命名，并将它们放在 path 中的某个位置。在上面的例子中，我们安装了`dig`插件，方法是下载它的源代码，编译二进制文件，并把它移动到路径中的`krew`目录。要检查`kubectl`是否找到了新安装的插件，你可以运行`kubectl plugin list`。

# 必须有

索引中有相当多的插件(在撰写本文时有 149 个),在`krew`索引之外还有更多，所以为了节省您浏览所有插件的时间，我列出了我认为特别有用的插件。因此，让我们从最容易被忽视的领域开始，按类别对其进行细分，即安全性:

*   `[rakkess](https://github.com/corneliusweig/rakkess)`—`krew`中的`access-matrix`是一个插件，用于显示和查看对 kubernetes 资源的访问。这在设计 RBAC 角色时非常有用——例如，您可以运行`kubectl access-matrix --as other-user --namespace some-ns`来验证用户或服务帐户在指定的名称空间中具有所需的访问权限。
*   `[kubesec](https://github.com/controlplaneio/kubectl-kubesec)` -在`krew`中称为`kubesec-scan`，是一个用[https://kubesec.io/](https://kubesec.io/)扫描仪扫描资源的插件。当您针对清单运行这个插件时，它会告诉您建议的更改，以提高工作负载的安全性。要查看扫描器使用的所有规则，请访问上述网站。
*   `[rbac-lookup](https://github.com/FairwindsOps/rbac-lookup)` -类似于我们提到的第一个插件，这个插件也有助于你集群中的 RBAC。这可用于执行角色的反向查找，为您提供用户、服务帐户或组已分配的角色列表。例如，要查找绑定到名为`my-sa`的服务帐户的角色，可以使用下面的- `kubectl rbac-lookup my-sa --kind serviceaccount --output wide`。

当调试一些关键问题时，真的没有时间浪费，有一些调试插件可以帮助加快这个过程:

*   `[ksniff](https://github.com/eldadru/ksniff)` -即`sniff`是一个调试和捕获网络数据的工具。它可以连接到一个 pod，并使用`tcpdump`将网络数据转发到您本地的 *Wireshark* 。这个工具在使用 Wireshark 的命令行版本`tshark`时也能很好地工作。
*   `[dig](https://github.com/sysdiglabs/kubectl-dig)` —这个由 *SysDig* 构建的插件提供了非常好的终端接口，用于探索各种节点级数据——例如——端口、跟踪、运行窗格、页面错误等。要观看正确的演示，请在`dig`资源库[中查看视频，点击这里](https://github.com/sysdiglabs/kubectl-dig)。然而这个插件不在`krew`中，可能还需要在你的集群节点上做一些额外的设置(见这个[问题](https://github.com/sysdiglabs/kubectl-dig/issues/1))。

还有一些有用的插件可以帮助集群及其资源的日常管理:

*   `[neat](https://github.com/itaysk/kubectl-neat)` -可能是所有插件中我最喜欢的是`neat`，它从 Kubernetes 资源的 YAML 输出中移除了所有生成的冗余字段。如果你厌倦了滚动浏览所有的`managedFields`和其他垃圾，那么一定要试试这个。
*   `[kube-capacity](https://github.com/robscott/kube-capacity)` -在`krew`中称为`resource-capacity`，试图更好地了解集群资源的使用和利用率。它本质上是一种类固醇。它可以显示每个命名空间或单元的资源利用率和消耗，允许节点或单元标签过滤，以及输出排序。
*   `[kube-pug](https://github.com/rikatz/kubepug)` -是一个在`krew`中被称为`deprecations`的插件。每个集群迟早都需要升级，在某个时候，您会遇到 API 被弃用和/或被移除的情况。发现什么被否决可能是一个漫长且容易出错的过程，这个插件试图简化这个过程。您需要做的就是运行`kubectl deprecations --k8s-version=v1.XX.X`，您将获得集群中所有 API 对象实例的列表，这些实例将在指定版本中被弃用或删除。

最后也是最大的一类是电动工具——用普通的`kubectl`可以完成很多复杂、繁琐或需要多个重复步骤的任务，所以让我们用这些插件简化一些:

*   `[tree](https://github.com/ahmetb/kubectl-tree)` -在 Kubernetes 中创建单个对象可以触发更多依赖资源的创建，无论是仅仅*部署*创建*复制集*还是一个操作员实例创建 20 个不同的对象。这种层次结构可能很难导航，而`kubectl tree`可以通过创建依赖资源的类似文件系统的树形可视化来帮助导航。
*   `[kubelogin](https://github.com/int128/kubelogin)` -如果你正在使用 Google、Keycloak 或 Dex 等 OIDC 提供商对 Kubernetes 集群进行身份验证，那么这个插件也就是`krew`中的`oidc-login`可以帮助你避免一次又一次地手动登录集群。当您设置此插件时，每当您在没有有效认证令牌的情况下尝试运行任何`kubectl`命令时，`oidc-login`将自动打开您的提供商的登录页面，并在成功认证后获取令牌并让您登录到集群。要查看工作流视频，请点击此处查看存储库[。](https://github.com/int128/kubelogin)
*   `[kubectx](https://github.com/ahmetb/kubectx)` -在`krew`中被称为`ctx`，可能是所有插件中最受欢迎的。它允许您轻松地在`kubectl`上下文和集群名称空间之间切换，而不必处理`kubectl config`。
*   `[ketall](https://github.com/corneliusweig/ketall)` -我们都知道`kubectl get all`并没有真正给你*所有的*资源。要真正列出所有资源，您可以使用`ketall`，在`krew`中也称为`get-all`。这个插件可以将所有的资源转储到你的终端中，并根据时间、排除、标签选择器或范围(集群或名称空间)进行过滤。

# 结束语

这只是一个我觉得有用的东西的列表，所以对我有用的可能对你不一定有用，同时，可能有很多我省略了的插件，但是它们对你可能非常有用。所以，去查看一下`krew`索引或者`[awesome-kubectl-plugins](https://github.com/ishantanu/awesome-kubectl-plugins)` [库](https://github.com/ishantanu/awesome-kubectl-plugins)了解更多。如果你碰巧发现了一些很酷的东西，请分享出来，这样其他人也可以从中受益。

也就是说，并不是每个用例都有一个插件，所以如果你找不到插件来解决你的问题，也许你可以构建一个来填补这个空白(更多信息请见[文档](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/#writing-kubectl-plugins))。😉

除了`kubectl`插件，还有其他工具可以提高你的生产力，简化 Kubernetes 的操作。最突出的一个是，所以如果插件不够，你想抓住一个更大的锤子，那么这可能是一个正确的工具给你。

*本文原帖*[*martinheinz . dev*](https://martinheinz.dev/blog/58?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_58)

[](/could-kubernetes-pods-ever-become-deprecated-e8ee6b4b8066) [## Kubernetes 豆荚会被废弃吗？

### Pods、服务或部署等资源会被弃用并从 Kubernetes 中删除吗？如何…

towardsdatascience.com](/could-kubernetes-pods-ever-become-deprecated-e8ee6b4b8066) [](/yq-mastering-yaml-processing-in-command-line-e1ff5ebc0823) [## yq:在命令行中掌握 YAML 处理

### 学习使用 yq 命令行实用程序和这个简单的备忘单更有效地解析和操作 YAML 文件

towardsdatascience.com](/yq-mastering-yaml-processing-in-command-line-e1ff5ebc0823) [](/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75) [## 调试 Kubernetes 工作负载的最简单方法

### 对 Kubernetes 上运行的任何应用程序进行调试和故障排除的最快最简单的方法…

towardsdatascience.com](/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75)