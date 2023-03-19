# 通过 Pod 就绪关口提高应用程序可用性

> 原文：<https://towardsdatascience.com/improving-application-availability-with-pod-readiness-gates-4ebebc3fb28a?source=collection_archive---------39----------------------->

## 当 Pod 就绪和活性探测不够好时，您能做什么？

![](img/a77ff13caef0352e05c50edbd1040bb3.png)

由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/traffic-arrow?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

使用 *Pod* 活跃度和就绪度探测器，确保您在 Kubernetes 上运行的应用程序可用并准备好服务流量是非常容易的。但是，并不是所有应用程序都能够使用探测器，或者在某些情况下需要更复杂的就绪检查，而这些探测器根本无法执行这些检查。然而，如果 Pod 探针不够好，还有其他解决方案吗？

# 准备关卡

答案很明显，是。借助*就绪门*，可以为 Kubernetes Pods 执行复杂的定制就绪检查。

就绪门允许我们创建类似于`PodScheduled`或`Initialized`的自定义状态条件类型。然后，这些条件可用于评估 Pod 准备情况。

通常情况下，箱准备状态仅由箱中所有容器的准备状态决定，这意味着如果所有容器都是`Ready`，那么整体也是`Ready`。如果准备状态门被添加到一个箱，那么一个箱的准备状态由所有集装箱的准备状态*和所有准备状态门条件的*状态决定。

让我们看一个例子，以便更好地理解这是如何工作的:

上面的清单显示了一个名为`www.example.com/some-gate-1`的有单一准备门的分离舱。查看 status 节中的条件，我们可以看到`ContainersReady`条件是`True`，这意味着所有容器都准备好了，但是定制就绪门条件是`False`，因此 Pod 的`Ready`条件也必须是`False`。

如果您在这样的 pod 上使用`kubectl describe pod ...`，您还会在*条件*部分看到以下内容:

# 基本原理

我们现在知道实现这些额外的就绪条件是可能的，但是它们真的有必要吗？难道仅仅利用探针进行健康检查还不够吗？

在大多数情况下，探测应该足够了，但是也有需要进行更复杂的准备状态检查的情况。准备就绪关口最常见的用例可能是与外部系统同步，例如云提供商的负载平衡器。例如 GKE 的 [AWS 负载平衡器](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.1/deploy/pod_readiness_gate/)或[容器本地负载平衡](https://cloud.google.com/kubernetes-engine/docs/concepts/container-native-load-balancing)。在这些情况下，就绪门允许我们让工作负载网络感知。

使用准备状态门的另一个原因是，如果您有外部系统，可以使用应用程序指标对您的工作负载执行更彻底的健康检查。这有助于将您的系统集成到 Kubernetes 的工作负载生命周期中，而不需要对 kubelet 进行更改。它还允许外部系统订阅 Pod 条件变化并对其采取行动，可能应用变化来补救任何可用性问题。

最后，如果您有部署到 Kubernetes 的遗留应用程序，而该应用程序与活性或就绪性探测不兼容，那么就绪性检查可以是一个救命稻草，但是它的就绪性可以通过不同的方式来检查。

要了解该特性的完整原理，请查看 GitHub 中的原始 [KEP。](https://github.com/kubernetes/enhancements/tree/master/keps/sig-network/580-pod-readiness-gates#motivation)

# 创建第一个门

说够了，让我们创建我们的第一个就绪门。我们所需要做的就是在 Pod `spec`中添加`readinessGates`节，其中包含我们想要的条件的名称:

添加门很容易，但是更新稍微复杂一点。`kubectl`子命令不支持对象状态的修补，因此我们无法使用`kubectl patch`将条件设置为`True` / `False`。相反，我们必须使用直接发送到 API 服务器的`PATCH` HTTP 请求。

访问集群 API 服务器最简单的方法是使用`kubectl proxy`，它允许我们在`localhost`上访问服务器:

除了在后台启动代理之外，我们还使用`curl`来检查服务器是否可达，并向服务器查询我们将要更新的 pod 的清单/状态。

既然我们有了到达 API 服务器的方法，让我们尝试更新 Pod 状态。每个就绪门状态条件默认为`False`，但让我们从显式设置它开始:

在这个代码片段中，我们首先使用针对 API 代理服务器的`PATCH`请求，将 JSON 补丁应用到 Pod 的`status.condition`字段。在这种情况下，我们使用了`add`操作，因为状态尚未设置。此外，您还可以看到，当我们列出带有`-o wide`的 pod 时，`READINESS GATES`列显示`0/1`，表明闸门设置为`False`。在`kubectl describe`的输出中也可以看到同样的情况。

接下来，让我们看看如何将值切换到`True`:

与前面的代码片段类似，我们再次使用`PATCH`请求来更新条件，但是这一次我们使用了`replace`操作，特别是在由`/status/conditions/0`指定的列表中的第一个条件上。但是请注意，自定义条件不一定要在列表中排在第一位，所以如果您将使用一些脚本来更新条件，那么您应该首先检查您应该更新哪个条件。

# 使用客户端库

像我们上面看到的用`curl`更新条件适用于简单的脚本或快速手动更新，但是通常你可能需要更健壮的解决方案。考虑到`kubectl`在这里不是一个选项，您的最佳选择将是 Kubernetes 客户端库之一。出于演示目的，让我们看看如何在 Python 中实现:

我们需要做的第一件事是向群集进行身份认证并创建 Pod。在这种情况下，使用从`~/.kube/config`加载您的凭证的`config.load_kube_config()`来完成身份验证部分，一般来说，虽然使用服务帐户和令牌来对集群进行身份验证更好，但是可以在[文档](https://github.com/kubernetes-client/python/blob/master/kubernetes/README.md#getting-started)中找到相关示例。

至于第二部分——Pod 创建——这相当简单，我们只需应用 Pod 清单，然后等待，直到它的状态阶段从`Pending`开始发生变化。

在 Pod 运行的情况下，我们可以通过将其状态设置为初始值`False`来继续:

除了设置状态，我们还在更新后查询集群的当前 Pod 状态。我们查找对应于`Ready`条件的部分并打印其状态。

最后，我们可以用下面的代码将值翻转到`True`:

这里的代码与前面的例子非常相似，但是这一次我们寻找类型为`www.example.com/gate-1`的条件，我们用`print`验证它的当前状态，然后使用`replace`操作将更改应用到在`index`列出的条件。

# 结束语

上面的 shell 脚本和 Python 代码都演示了如何实现就绪门及其更新。但是在实际应用中，您可能需要一个更健壮的解决方案。

这种情况的理想解决方案是定制控制器，它可以监视 pod，将`readinessGate`节设置为相关的`conditionType`节。然后，控制器将能够根据观察到的 pod 状态更新条件，无论是基于定制 pod 指标、外部网络状态还是其他。

如果您正在考虑实现这样的控制器，那么您可以从现有的解决方案中获得一些灵感，比如前面提到的 AWS 和 GKE 负载平衡器或者(现在已存档的)`[kube-conditioner](https://github.com/itaysk/kube-conditioner)`。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/63?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_63)

</the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75>  <https://itnext.io/hardening-docker-and-kubernetes-with-seccomp-a88b1b4e2111>  </making-kubernetes-operations-easy-with-kubectl-plugins-206493c1f41f> 