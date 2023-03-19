# Kubernetes 豆荚会被废弃吗？

> 原文：<https://towardsdatascience.com/could-kubernetes-pods-ever-become-deprecated-e8ee6b4b8066?source=collection_archive---------21----------------------->

## Pods、服务或部署等资源会被弃用并从 Kubernetes 中删除吗？这将如何发生？

![](img/3f9d3acea7ed21396c173f13973b3a80.png)

照片由 [Gary Chan](https://unsplash.com/@gary_at_unsplash?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

在任何软件项目中，随着时间的推移，会添加新的特性和 API，有时它们中的一些也会被弃用并最终被删除。即使是像 Kubernetes 这样的大型项目也不例外，但是当考虑弃用和最终移除时，并没有真正想到其 API 的核心部分。因此，问题是 Kubernetes 中的核心对象或 API，如 Pod、部署或服务，是否可以删除？如果可以，如何删除？

# 长话短说

如果这个问题的答案是*“否”*，那么这篇文章就没有存在的理由了，长话短说— *“是”*—GA 中的任何核心 API 对象，例如来自`v1` API 组的某些东西都绝对会被弃用。

这个简单的一般答案并没有告诉我们太多。当谈到弃用时，Kubernetes 区分了几种类型的对象，例如 REST APIs、CLI 或特性门。它们中的每一个都有自己的一组不同成熟度的对象，如 alpha、beta 或 GA。所有这些都决定了某个对象——甚至像 Pod 这样的东西——在多长时间内以及在什么条件下会被弃用。所以，让我们更仔细地看看每一个，以及一些过去的例子和一些假设的未来可能发生的例子。

# 说来话长

不同的规则适用于不同的对象/功能，因此在我们讨论弃用规则和时间表之前，让我们先看一下所有不同的对象组:

*   *REST 对象*——我们最感兴趣的部分——REST 对象或 REST APIs 涵盖了我们最经常交互的所有东西，即——顶层对象，如 Pods 或 Deployment，它们的模式字段，如`containers`、`volumes`或`env`，以及用于`imagePullPolicy`的常量，如`Always`、`IfNotPresent`和`Never`。
*   *标志或 CLI*—第二个最相关的组涵盖所有 CLI。这里最明显的是`kubectl`，但它也包括像`kubelet`、`kube-apiserver`或`kube-scheduler`以及它们所有的子命令和标志。
*   *特性/行为* —并不是所有的东西都可以用 API 准确标记或者成为 CLI 的一部分。还有整个系统的行为以及不同成熟度的实验特征。这些也需要(并且有)自己的弃用流程和时间表。
*   *指标* —最后，Kubernetes 还公开了许多关于各种服务的`/metrics`端点的指标。考虑到它们中的许多被用于例如监控，它们也不能随时被改变或删除，所以它们有自己的一套规则。

# 剩余对象

对于 REST APIs 或对象，一般规则是在宣布弃用后，API 版本必须至少支持:

*   GA: 12 个月或 3 个版本(以较长者为准)
*   测试版:9 个月或 3 个版本(以较长者为准)
*   Alpha: 0 版本

这听起来很简单，但是还有许多其他的(不太容易理解的)规则适用于这里，所以让我们直接看一个例子，这样应该就清楚了。让我们假设一个名为 *Task* 的 API 对象(有趣的事实——这实际上是 *Pods* 的原名——参见 Kubernetes 的 [first commit)。这个任务在 API 版本`v1`中是 GA，并且决定它应该被弃用，那么真正会发生什么呢？](https://github.com/boddumanohar/kubernetes-first-commit/blob/2c4b3a562ce34cddc3f8218a2c4d11c7310e6d56/pkg/client/client.go#L19)

从上表中可以看出，如果在 API 版本`v2alpha1`中*任务*对象被弃用，那么它还需要 9 个版本才能从 Kubernetes 中真正消失。让我也提醒你，以[目前每年 3 个版本的发布节奏](https://github.com/kubernetes/enhancements/tree/master/keps/sig-release/2572-release-cadence)，整个弃用过程将需要 3 年以上！

然而，你应该考虑所有不是 GA 的对象，然而我们都在使用它们，就好像它们是 GA 一样。一个这样的例子是 Ingress，它[在 1.19](https://opensource.googleblog.com/2020/09/kubernetes-ingress-goes-ga.html) 才成为 GA，或者最近在 [1.21](https://kubernetes.io/blog/2021/04/09/kubernetes-release-1.21-cronjob-ga/) 成为 CronJob。在这种 beta 甚至 alpha 特性的情况下，折旧时间表就不会这么慷慨了。如果您想检查某些资源属于哪个类别，您可以运行例如`kubectl api-resources | grep beta`获取集群中所有测试 API 的列表。

几乎相同的规则适用于整个 REST API 对象及其字段、常量值或对象结构。这意味着我们都用来表示`imagePullPolicy`的`Always`、`IfNotPresent`和`Never`等常数不会凭空消失或随机变化，同样，字段也不会从一个部分移动到另一个部分。

至于一些现实世界的例子— *PodSecurityPolicy* 可能是最近历史上最大的一个。该 API 对象将从 v1beta1 升级到 EOL，从 1.21 版开始已被弃用，并将在 1.25 版中被移除。有关详细信息，请查看 [KEP-2579](https://github.com/kubernetes/enhancements/blob/master/keps/sig-auth/2579-psp-replacement/README.md) 。

另一个重要的最近/正在进行的弃用是移除`selfLink`字段。这是 [KEP-1164](https://github.com/kubernetes/enhancements/tree/master/keps/sig-api-machinery/1164-remove-selflink) 的一部分，在这个 [GitHub 问题](https://github.com/kubernetes/enhancements/issues/1164)中也对这一变化进行了跟踪。

如果你想知道还有哪些反对意见，他们的理由是什么，或者他们的整个删除过程，那么你可以搜索[kubernetes/enhancements repository](https://github.com/kubernetes/enhancements)中提到的*“反对”*，你会找到所有相关的 KEPs。

# 标志或 CLI

类似于 REST 对象，`kubectl`或`kubelet`子命令或它们的标志也可以被弃用，因此有自己的策略。

这比前一种情况简单得多。这里，对于面向用户的组件，例如`kubectl`，策略是:

*   GA: 12 个月或 2 个版本(以较长者为准)
*   测试版:3 个月或 1 个版本(以较长者为准)
*   Alpha: 0 版本

对于面向管理的组件，如`kubelet`、`kube-apiserver`或`kube-scheduler`，它是:

*   GA: 6 个月或 1 次发布(以较长者为准)
*   测试版:3 个月或 1 个版本(以较长者为准)
*   Alpha: 0 版本

这方面最近一个大的例子是`dockershim`，它是`kubelet`的一部分。以下[章节](https://github.com/kubernetes/enhancements/tree/master/keps/sig-node/2221-remove-dockershim)概述了其弃用和移除，其中包括*移除计划*的整个章节，该计划将版本 1.20 列为弃用目标，版本 1.24 列为移除目标。

这方面的另一个显著变化是`seccomp`配置文件将正式发布，在本 [KEP](https://github.com/kubernetes/enhancements/blob/master/keps/sig-node/135-seccomp/README.md) 中概述。`seccomp`配置文件实际上不是对标志或任何 CLI 的直接更改，但是将它们正式发布需要弃用`kubelet`标志`--seccomp-profile-root`，此处标注为

因此，本节的底线是 CLIs 的弃用时间表也相当宽松，但是如果您使用一些`kubectl alpha ...`命令来实现自动化，那么您最好在升级您的集群甚至 CLI 二进制文件/工具之前检查弃用情况。

# 特征门

在任何时间点上，Kubernetes 都包含了许多实验性的特性。这些功能由所谓的*功能门*控制，这些功能门是我们可以用来打开或关闭它们的键/值对。

考虑到特性门用于实验特性，它们的弃用策略不同于其他 Kubernetes 对象。此外，随着特性经历成熟阶段，其门的行为也会发生变化。对于 alpha 特征，默认情况下门是禁用的；对于测试版功能，默认情况下是启用的；并且当特征达到 GA 状态时，门不再需要，并且变得废弃和不可操作。

至于弃用和移除这些功能所需的时间——alpha 功能可以随时消失，beta 功能在 1 次发布后(如果它们被移除)或 2 次发布后(如果它们进入正式版状态)就会消失。

如需具体示例，您可以点击查看功能门[的完整列表。例如，您可以看到`AffinityInAnnotations`功能从 alpha 版本升级到了弃用版本，对于一直升级到 GA 版本的功能，我们可以列出`BlockVolume`、`DryRun`或`EndpointSlice`。至于功能在测试阶段后被弃用的情况，我找不到任何证据。](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/)

如果您决定打开其中的任何一个，请确保在升级集群之前检查它们的状态变化，尤其是在集群升级之后可能会消失的 alpha。

# 韵律学

列表中的最后一种对象是指标，在弃用时也需要保留相当长的时间，因为它们中的许多都是由监控工具消耗和聚集的。与前几节不同，指标只分为两类。在这里，我们只有稳定和 alpha 指标，其中稳定指标可能会在宣布弃用 3 个版本后被删除，而 alpha 指标可以随时被删除。

关于已弃用和已删除指标的示例，您可以看看这个删除了`rest_client_request_latency_seconds`指标的[提交](https://github.com/kubernetes/kubernetes/pull/83836)。您还可以在 1.17 版本的发行说明中找到它，以及其他一些[变更/废弃的指标](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#deprecated-changed-metrics)。

如果您想了解关于指标生命周期及其组成部分的更多信息，您可以查看此[文档页面](https://kubernetes.io/docs/concepts/cluster-administration/system-metrics/)。

# 结论

如今，似乎许多项目更多地采用了一种*“快速移动和打破常规”*的方法来弃用，同时频繁地进行大量更改，因此很高兴看到像 Kubernetes 这样的大项目具有经过深思熟虑的弃用过程，这为用户从计划要删除的 API 和功能中迁移出来留出了大量时间。

那么，这篇文章的要点是什么呢？—有什么东西会被废弃吗？— *是*。你应该担心吗？——*明明没有*。由于一些折旧时间表很长，没有真正的理由担心东西突然被拿走。也就是说，你可能应该查看发行说明，留意所有你可能会用到的 alpha 特性，就好像它们是正式版一样。你也可以查看[废弃的 API 迁移指南](https://kubernetes.io/docs/reference/using-api/deprecation-guide/)，它列出了所有将在未来某个时候被移除的 API。最后要注意的是——这些都不一定适用于 CRD——对于外部供应商开发的 CRD，您必须检查他们自己的策略，因为他们可以对其应用程序/集成/解决方案为所欲为。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/53?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_53)

[](/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75) [## 调试 Kubernetes 工作负载的最简单方法

### 对 Kubernetes 上运行的任何应用程序进行调试和故障排除的最快最简单的方法…

towardsdatascience.com](/the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75) [](https://itnext.io/hardening-docker-and-kubernetes-with-seccomp-a88b1b4e2111) [## 用 seccomp 强化 Docker 和 Kubernetes

### 您的容器可能不像您想象的那样安全，但是 seccomp 配置文件可以帮助您解决这个问题…

itnext.io](https://itnext.io/hardening-docker-and-kubernetes-with-seccomp-a88b1b4e2111) [](/deploy-any-python-project-to-kubernetes-2c6ad4d41f14) [## 将任何 Python 项目部署到 Kubernetes

### 是时候深入 Kubernetes，使用这个成熟的项目模板将您的 Python 项目带到云中了！

towardsdatascience.com](/deploy-any-python-project-to-kubernetes-2c6ad4d41f14)