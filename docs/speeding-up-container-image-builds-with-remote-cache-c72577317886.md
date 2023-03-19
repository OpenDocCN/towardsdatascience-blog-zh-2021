# 使用远程缓存加速容器映像构建

> 原文：<https://towardsdatascience.com/speeding-up-container-image-builds-with-remote-cache-c72577317886?source=collection_archive---------13----------------------->

## 使用这些缓存技术可以轻松地优化 CI/CD 管道中的容器映像构建

![](img/bbc2dcab4c7bd087fce101806cf4a2f5.png)

照片由[罗宾·皮尔](https://unsplash.com/@robinpierre?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在 CI/CD 管道中构建映像可能与在本地机器上构建有很大不同。一个主要的区别是缓存的可用性。在本地环境中，您很可能缓存了以前构建的所有资源、依赖项和图像层，因此您的构建可能只需要几秒钟。另一方面，在 CI 管道中，没有本地缓存，这会导致构建需要几分钟时间。这是有解决办法的，在这篇文章中，我们将看看如何在使用和不使用 Docker 的情况下，为您可能使用的任何 CI/CD 平台解决这个问题。

# 通用解决方案

适用于任何环境的通用解决方案的想法非常简单——我们需要以某种方式创建缓存或将缓存引入管道。这里我们有两种选择——要么我们将构建器工具(例如 Docker)指向我们的映像存储库，它可以从该存储库中检索映像层并将它们用作缓存，要么我们将这些层存储在文件系统上，我们使其可供管道使用，并从那里获取这些层。无论哪种方式，我们都需要通过将映像推送到存储库或文件系统来创建缓存，然后，在后续构建中，我们会尝试使用它，如果因为缓存未命中而无法使用，我们会用新的层来更新它。

现在，让我们看看如何使用各种工具在实践中做到这一点…

# 码头工人

这个问题最简单的解决方案是使用 Docker 和 *BuildKit* 。BuildKit 是对`docker build`的一组增强，它改进了性能、存储管理并增加了一些额外的特性，包括更好的缓存功能。要用 BuildKit 构建容器映像，我们需要做的就是在每个命令前面加上`DOCKER_BUILDKIT=1`:

对于曾经使用 Docker 构建过图像的人来说，这个例子应该是不言自明的。这与基本 Docker 用法之间唯一真正的区别是添加了`BUILDKIT_INLINE_CACHE=1`，它告诉 BuildKit 启用内联缓存导出器。这确保 Docker 将缓存所需的元数据写入映像。这些元数据将在后续构建中使用，以找出可以缓存的图层。上述代码片段中唯一的不同之处是命令输出——在第一次构建期间，我们可以看到 Docker 将缓存导出到存储库，而在第二次构建期间，它导入缓存清单并使用一个缓存层。

使用 BuildKit 作为 Docker 的一部分很方便，但是它隐藏了一些特性和选项。因此，如果您想要对构建和缓存进行更多的控制，那么您可以直接使用上游 BuildKit 项目。为此，您需要从 [GitHub 发布页面](https://github.com/moby/buildkit/releases)下载二进制文件，将其解压缩并移动到您的路径中(例如`/usr/local/bin/`)。最后，您需要启动 BuildKit 守护进程，然后就可以开始构建了:

如果我们想用 upstream BuildKit 执行与 Docker 集成相同的缓存构建，我们需要编写一个稍微复杂一点的命令:

正如您在这里看到的，我们必须指定许多标志和参数，这可能很烦人，但允许很好的可定制性。这种方法的一个优点是我们不需要运行`docker push`，取而代之的是我们将`push=true`包含在一个参数中，而`buildctl`负责推送图像。

以这种方式使用 BuildKit 的另一个优点是能够将图像和缓存层放入单独的存储库或标记中。在本例中，我们将图像本身存储在`docker-cached:latest`中，而缓存将位于`docker-cached:buildcache`中:

为了完整起见，我还将提到，不单独安装 BuildKit 也可以利用上面提到的高级特性。为此，你将需要`buildx`，它是一个扩展构建功能的 Docker CLI 插件。然而,`buildx`与`buildctl`有不同的参数，所以您需要根据这里的[文档来调整您的构建命令。](https://github.com/docker/buildx/blob/master/docs/reference/buildx_build.md#-use-an-external-cache-source-for-a-build---cache-from)

也就是说，我们正在做所有这些恶作剧来提高 CI/CD 构建性能，所以在本地运行这些命令对于测试来说是很好的，但是我们需要以某种方式在一些 CI/CD 平台的环境中执行，我选择的环境是 Kubernetes。

为了在 Kubernetes 中实现这一点，我们需要带一些额外的东西，即作为工作空间的图像和卷的凭证:

上面是一个单独的*作业*，它首先使用 init 容器在 *PersistentVolumeClaim* 提供的工作空间内创建一个`Dockerfile`。然后，实际的作业执行构建，如前面所示。它还从名为`buildkit-docker-config`的 *Secret* 中挂载存储库凭证，这是 BuildKit 将缓存层和图像本身推送到存储库所必需的。

为了清楚起见，我省略了上面使用的 PersistentVolumeClaim 和 Secret 的清单，但是如果您想自己测试一下，那么您可以在这里找到那些。

# 无码头的

然而，Docker 并不是构建映像的唯一工具，它可以帮助我们在 CI/CD 构建期间利用缓存。Docker 的替代品之一是谷歌的 Kaniko。它的优点是它应该作为容器映像运行，这使得它适合 Kubernetes 这样的环境。

考虑到这个工具是为 CI/CD 管道设计的，我们需要在本地模拟相同的条件来测试它。为此，我们需要几个目录和文件用作卷:

上面我们创建了 3 样东西——一个由单层组成的样品`Dockerfile`,我们将用它来测试。接下来，我们创建了一个`cache`目录，它将被挂载到容器中，用于存储缓存的图像层。最后，我们创建了包含注册表凭证的`config`目录，该目录将以只读方式挂载。

在上一节中，我们只查看了使用图像注册/存储库的缓存图像层，但是使用 Kaniko，我们还可以使用本地目录/卷作为缓存源。为此，我们首先需要*“预热”*用图像层填充缓存:

*注意:这一节是关于在没有 docker 的情况下构建图像和缓存图像，然而在 Kubernetes 之外的测试期间，我们仍然需要以某种方式运行 Kaniko 图像，这就是使用* `*docker*` *。*

Kaniko 项目提供了两个图像— `warmer`和`executor`，上面我们使用了前者，它获取可变数量的图像，并使用它们来填充指定的缓存目录。

缓存就绪后，我们可以开始构建映像了。这次我们使用`executor`映像，传入 2 个卷——一个用于注册表凭证(以只读方式挂载),另一个用于 workspace，我们用示例`Dockerfile`预先填充了 workspace。此外，我们指定标志来启用缓存以及最终图像将被推送到的目的地:

这些例子向我们展示了它在理论上是如何工作的，但是在实践中，我们希望在 Kubernetes 上运行它。为此，我们将需要与 BuildKit 示例中相似的一组对象，即——[缓存目录的卷声明](https://gist.github.com/MartinHeinz/1c9700d197f0e565d314555b26e66890)、[工作区(Dockerfile)的卷声明](https://gist.github.com/MartinHeinz/a1270557722478b65a4ec33f632a36cb)、带有注册表凭证的秘密以及将执行`kaniko`的作业或 Pod:

这里，假设我们已经使用`warmer`图像填充了缓存，我们运行`kaniko` executor，它从`/workspace`目录中检索`Dockerfile`，从`/cache`中检索缓存层，从`/kaniko/.docker/config.json`中检索凭证。如果一切顺利，我们应该在日志中看到 Kaniko `executor`找到了缓存层。

从本地卷缓存层可能是有用的，但大多数时候你可能会想使用远程注册表。Kaniko 也可以做到这一点，我们需要做的只是改变几个论点:

我们在这里做的重要改变是我们用`--cache-repo`替换了`--cache-dir`标志。此外，我们还能够省略用于缓存目录的卷声明。

除了 Kaniko，还有很多其他工具可以构建容器映像。最值得注意的是`podman`，它利用`buildah`来构建图像。然而，使用这两个用于缓存，现在不是一个选项。`--cache-from`选项在`buildah`中可用，但是它是 NOOP，所以即使你指定了它，也不会发生什么。因此，如果您想将您的配置项从 Docker 迁移到 Buildah，并且需要缓存，那么您需要等待[这个问题](https://github.com/containers/buildah/issues/620)得到实现/解决。

# 结束语

本文描述了我们如何利用层缓存来提高构建性能。如果您在映像构建中遇到糟糕的性能，问题可能不在于丢失缓存，而在于您的`Dockerfile`中的命令。因此，在你开始实现层缓存之前，我建议你先优化一下`Dockerfiles`的结构。此外，只有当您拥有结构良好的`Dockerfiles`时，缓存才会起作用，因为在第一次缓存未命中之后，就不能使用更多的缓存层了。

除了缓存层之外，您可能还想缓存依赖项，这样可以节省从 NPM、PyPI、Maven 或其他工件库下载库所需的时间。一种方法是使用 BuildKit 和它的`--mount=type=cache`标志，这里的[描述为](https://github.com/moby/buildkit/blob/master/frontend/dockerfile/docs/syntax.md#example-cache-go-packages)。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/61?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_61)

[](/making-kubernetes-operations-easy-with-kubectl-plugins-206493c1f41f) [## 使用 kubectl 插件简化 Kubernetes 操作

### 使用这些 kubectl 插件来提高您的生产力，使所有的 Kubernetes 任务和操作更容易，更快和…

towardsdatascience.com](/making-kubernetes-operations-easy-with-kubectl-plugins-206493c1f41f) [](/yq-mastering-yaml-processing-in-command-line-e1ff5ebc0823) [## yq:在命令行中掌握 YAML 处理

### 学习使用 yq 命令行实用程序和这个简单的备忘单更有效地解析和操作 YAML 文件

towardsdatascience.com](/yq-mastering-yaml-processing-in-command-line-e1ff5ebc0823) [](/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75) [## 调试 Kubernetes 工作负载的最简单方法

### 对 Kubernetes 上运行的任何应用程序进行调试和故障排除的最快最简单的方法…

towardsdatascience.com](/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75)