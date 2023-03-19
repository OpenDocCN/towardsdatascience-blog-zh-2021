# 调试 Kubernetes 工作负载的最简单方法

> 原文：<https://towardsdatascience.com/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75?source=collection_archive---------6----------------------->

## 对 Kubernetes 上运行的任何应用程序进行调试和故障排除的最快最简单的方法

![](img/445b70fd9a4ea9a3861bb66efebe80e0.png)

照片由[杰米街](https://unsplash.com/@jamie452?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

调试容器化的工作负载和 *Pods* 是每个使用 Kubernetes 的开发人员和 DevOps 工程师的日常任务。通常简单的`kubectl logs`或`kubectl describe pod`就足以找到某个问题的罪魁祸首，但是有些问题更难找到。在这种情况下，你可能会尝试使用`kubectl exec`,但即使这样也可能不够，因为有些容器如*发行版*甚至不包含你可以 SSH 的 shell。那么，如果以上所有方法都失败了，我们还剩下什么？...

# 可能会有更好的方法…

有时候，你需要拿一把更大的锤子或者使用更合适的工具来完成手头的任务。在 Kubernetes 上调试工作负载的情况下，合适的工具是`kubectl debug`，这是不久前(1.18 版)添加的一个新命令，允许您调试正在运行的 pods。它将一种叫做*蜉蝣容器*的特殊类型的容器注入到有问题的容器中，允许你四处查看并排除故障。这对于简介中描述的情况或任何其他交互式调试更可取或更有效的情况非常有用。所以，`kubectl debug`看起来是个不错的选择，但是要使用它，我们需要*短暂的容器*，那么这些容器到底是什么呢？

短暂容器是 Pod 中的一个子资源，类似于普通`containers`。然而，与常规容器不同，临时容器不是用来构建应用程序的，而是用来检查应用程序的。我们不在创建 Pod 时定义它们，而是使用特殊的 API 将它们注入到正在运行的 Pod 中，以运行故障排除命令并检查 Pod 的环境。除了这些差异，临时容器还缺少一些基本容器的字段，比如`ports`或`resources`。

但是，我们为什么需要它们呢？我们不能只用基本的容器吗？嗯，您不能将容器添加到 Pod，因为它们应该是一次性的(或者换句话说，可以随时删除和重新创建)，这使得很难排除需要检查 Pod 的重现错误。这就是为什么 API 中添加了临时容器——它们允许您将容器添加到现有的 pod 中，从而更容易检查正在运行的 pod。

考虑到短暂容器是 Pod 规范的一部分，而 Pod 规范是 Kubernetes 的核心，你(可能)还没有听说过它吗？这些大多是未知特性的原因是因为临时容器处于早期阶段，这意味着它们在默认情况下是不可用的。这个阶段的资源和特性可能会发生很大的变化，或者在 Kubernetes 的未来版本中被完全删除。因此，要使用它们，您必须使用`kubelet`中的*特性门*明确启用它们。

# 配置特征门

我们已经确定我们想要尝试一下`kubectl debug`,那么我们如何启用短暂容器特性 gate 呢？这取决于您的集群设置。例如，如果您正在使用`kubeadm`加速创建集群，那么您可以使用下面的*集群配置*来启用临时容器:

但是在下面的例子中，为了简单和测试的目的，我们将使用*KinD(Docker 中的 Kubernetes)*cluster，它也允许我们指定我们想要启用的特性门。因此，要创建我们的操场集群:

随着集群的运行，我们应该验证它确实在工作。查看这种配置是否得到应用的最简单的方法是检查 Pod API，它现在应该在通常的容器旁边包括 ephemeralContainers 部分:

这确认了我们拥有它，因此我们可以开始使用`kubectl debug`。所以，让我们从一个简单的例子开始:

我们首先启动一个名为`some-app`的 Pod，这样我们就有东西要*“调试”*。然后，我们对这个 Pod 运行`kubectl debug`，将`busybox`指定为临时容器的图像，以及原始容器的目标。此外，我们还包含了`-it`参数，这样我们可以立即连接到容器并获得一个 shell 会话。

在上面的代码片段中，您还可以看到，如果我们在 Pod 上运行`kubectl debug`后对其进行描述，那么它的描述将包括*临时容器*部分，其值是我们之前指定的命令选项。

# 进程命名空间共享

`kubectl debug`是一个非常强大的工具，但是有时向 Pod 添加另一个容器可能不足以获得关于在 Pod 的另一个容器中运行的应用程序的相关信息。当被诊断的容器不包括必要的调试工具甚至 shell 时，可能会出现这种情况。在这种情况下，我们可以使用*进程共享*来允许我们使用注入的临时容器检查 Pod 的原始容器。

但是进程共享的一个问题是它不能应用于现有的 pod，因此我们必须创建一个新的 pod，将`spec.shareProcessNamespace`设置为`true`，并在其中注入一个临时容器。这样做会很麻烦，尤其是如果我们必须调试多个 pod/container 或者只是重复执行这个任务。幸运的是，`kubectl debug`可以使用`--share-processes`选项为我们做到这一点:

上面的代码片段显示，通过进程共享，我们可以在一个 Pod 中看到另一个容器中的所有内容，包括它的进程和文件，这对于调试来说肯定非常方便。

您可能已经注意到，除了`--share-processes`之外，我们还包含了`--copy-to=new-pod-name`，因为——正如前面提到的——我们需要创建一个新的 pod，其名称由这个标志指定。如果我们随后从另一个终端列出正在运行的 pod，我们将看到以下内容:

这是我们在原有应用程序窗格中新增的调试窗格。与原始容器相比，它有两个容器，因为它还包括临时容器。

另外，如果您想在任何时候验证某个 Pod 中是否允许进程共享，那么您可以运行:

# 好好利用它

现在我们已经启用了特性门，并且知道了命令是如何工作的，让我们试着好好利用它并调试一些应用程序。让我们想象一下下面的场景——我们有一个行为不当的应用程序，我们需要对其容器中的网络相关问题进行故障诊断。该应用程序没有我们可以使用的必要的网络 CLI 工具。为了解决这个问题，我们可以以如下方式使用`kubectl debug`:

启动 pod 后，我们首先尝试将 shell 会话放入它的容器中，这看起来似乎可行，但是当我们尝试运行一些基本命令时，我们可以看到实际上什么也没有。因此，相反，我们使用包含类似`curl`、`ping`、`telnet`等工具的`praqma/network-multitool`映像将临时容器注入到 pod 中。现在，我们可以执行所有必要的故障排除。

在上面的例子中，对于我们来说，Pod 中有另一个容器就足够了。但是有时，您可能需要直接查看这个令人困扰的容器，却没有办法进入它的外壳。在这种情况下，我们可以像这样利用进程共享:

这里我们再次运行了使用 Distroless 映像的容器。知道我们不能在它的外壳中做任何事情，我们用`--share-processes --copy-to=...`运行`kubectl debug`，这创建了一个新的 Pod，它有一个额外的临时容器，可以访问所有的进程。当我们列出正在运行的进程时，我们可以看到我们的应用程序容器的进程具有 PID 8，我们可以用它来探索它的文件和环境。为此，我们必须遍历`/proc/<PID>/...`目录——在本例中是——`/proc/8/root/app/...`。

另一种常见的情况是，应用程序在容器启动时不断崩溃，这使得调试变得困难，因为没有足够的时间将 shell 会话放入容器并运行一些故障排除命令。在这种情况下，解决方案是创建具有不同入口点/命令的容器，这将阻止应用程序立即崩溃，并允许我们执行调试:

# 好处:调试集群节点

本文主要关注 Pods 及其容器的调试——但是任何集群管理员都知道——通常需要调试的是节点而不是 Pods。幸运的是，`kubectl debug`还允许通过创建 Pod 来调试节点，该 Pod 将在指定的节点上运行，节点的根文件系统安装在`/root`目录中。考虑到我们甚至可以使用`chroot`来访问主机二进制文件，这实际上充当了进入 node 的 SSH 连接:

在上面的代码片段中，我们首先确定了要调试的节点，然后使用`node/...`作为参数显式运行`kubectl debug`,以访问集群的节点。之后，当我们连接到 Pod 时，我们使用`chroot /host`来逃出`chroot`监狱，并获得对主机的完全访问权。最后，为了验证我们真的可以看到主机上的一切，我们查看了部分`kubeadm.conf`，其中我们可以看到我们在文章开始时配置的`feature-gates: EphemeralContainers=true`。

# 结论

能够快速有效地调试应用程序和服务可以为您节省大量时间，但更重要的是，它可以极大地帮助您解决一些问题，如果不立即解决这些问题，最终可能会花费您很多钱。这就是为什么让像`kubectl debug`这样的工具为您所用并启用非常重要，即使它们还没有正式发布或默认启用。

如果——不管出于什么原因——启用临时容器不是一个选项，那么尝试实践替代的调试方法可能是一个好主意，例如使用应用程序映像的调试版本，它将包括故障排除工具；或者临时改变 Pod 的容器的命令指令来阻止它崩溃。

也就是说，`kubectl debug`和短命容器只是许多有用但鲜为人知的 Kubernetes 特性门中的一个，所以请关注后续文章，深入了解 Kubernetes 的其他隐藏特性。

*本文原帖*[*martinheinz . dev*](https://martinheinz.dev/blog/49?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_49)

[](https://itnext.io/cloud-native-ci-cd-with-tekton-laying-the-foundation-a377a1b59ac0) [## 使用 Tekton 的云原生 CI/CD—奠定基础

### 是时候通过 Tekton Pipelines 在 Kubernetes 上开始您的云原生 CI/CD 之旅了…

itnext.io](https://itnext.io/cloud-native-ci-cd-with-tekton-laying-the-foundation-a377a1b59ac0) [](https://itnext.io/hardening-docker-and-kubernetes-with-seccomp-a88b1b4e2111) [## 用 seccomp 强化 Docker 和 Kubernetes

### 您的容器可能不像您想象的那样安全，但是 seccomp 配置文件可以帮助您解决这个问题…

itnext.io](https://itnext.io/hardening-docker-and-kubernetes-with-seccomp-a88b1b4e2111) [](/networking-tools-every-developer-needs-to-know-e17c9159b180) [## 每个开发人员都需要知道的网络工具

### 让我们学习被忽视的网络技能，如检查 DNS 记录，扫描端口，排除连接故障…

towardsdatascience.com](/networking-tools-every-developer-needs-to-know-e17c9159b180)