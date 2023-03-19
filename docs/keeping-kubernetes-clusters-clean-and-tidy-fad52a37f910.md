# 保持 Kubernetes 集群干净整洁

> 原文：<https://towardsdatascience.com/keeping-kubernetes-clusters-clean-and-tidy-fad52a37f910?source=collection_archive---------14----------------------->

## 清除所有未使用的资源，这些资源会使您的 Kubernetes 集群变得杂乱并浪费其计算资源

![](img/25a55504ab0414586acee876890c915a.png)

照片由[尼尔·约翰逊](https://unsplash.com/@neal_johnson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

随着集群的增长，资源、卷或其他 API 对象的数量也会增加，迟早会达到极限。无论是`etcd`，卷，内存还是 CPU。当您可以设置简单而复杂的规则、自动化和监控来保持集群整洁时，为什么要让自己遭受不必要的痛苦和麻烦呢？这些规则、自动化和监控可以让您的集群保持整洁，而不会让流氓工作负载吃掉您的资源或到处都是陈旧的对象。

# 何必呢？

*一些被遗忘的豆荚，未使用的持久卷，* `*Completed*` *乔布斯或者也许旧的 ConfigMap/Secret 无所谓，或者呢？它就放在那里，我可能在某个时候需要它！*

嗯，它现在不会造成任何损害，但是当事情随着时间的推移而积累时，它们就会开始影响集群的性能和稳定性。因此，让我们来看看这些被遗忘的资源可能导致的一些常见/基本问题:

每个 Kubernetes 集群都有一些基本限制。首先是每个节点的 pod 数量，根据 Kubernetes 文档，应该最多为 110 个。也就是说，如果您有非常大的节点，有大量的内存/CPU，您肯定可以更高—甚至可能每个节点 500 个，正如这里用 OpenShift 测试的[，但是如果您突破了极限，当事情开始出问题时，不要感到惊讶，不仅仅是因为内存或 CPU 不足。](https://www.openshift.com/blog/500_pods_per_node)

您可能遇到的另一个问题是临时存储不足。节点上所有正在运行的 pod 都至少使用一点临时存储空间来存储日志、缓存、暂存空间或`emptyDir`卷。您可能会很快达到极限，这可能会导致 pod 被逐出或新的 pod 无法在节点上启动。在节点上运行太多 pod 也会导致这个问题，因为临时存储用于容器映像和容器的可写层。如果节点的临时存储空间不足，它就会被感染，您可能会很快发现这一点。如果您想检查节点上存储的当前状态，您可以在节点上运行`df -h /var/lib`。

与短暂存储类似，持久卷也可能成为问题的来源，尤其是如果您在一些云中运行 Kubernetes，因此需要为您提供的每个 PVC 付费。因此，很明显，清理所有未使用的 PVC 以节省资金是很重要的。此外，保持使用过的 PVC 干净也很重要，这样可以避免应用程序空间不足，尤其是在集群中运行数据库时。

过多的物品也会产生问题，因为它们都存放在`etcd`储物件中。随着`etcd`数据库的增长，它的性能会开始下降，考虑到`etcd`是 Kubernetes 集群的大脑，你应该尽力避免这种情况。也就是说，你将需要非常大的集群来达到`etcd`的极限，正如 OpenAI 的这篇[帖子所示。然而，没有一个单一的指标可以用来衡量`etcd`的性能，因为它取决于对象的数量、大小或它们变化的频率，所以你最好保持它的干净，否则你可能会得到一些令人讨厌的惊喜。](https://openai.com/blog/scaling-kubernetes-to-2500-nodes/)

最后，杂乱的集群也会导致安全问题。当不需要/不使用角色绑定或服务帐户时，就把它们留在那里是一种邀请，有人会抓住它们并滥用它们。

# 基础

你不需要做任何复杂的事情来解决上面提到的大部分问题。解决这些问题的最好方法是完全预防它们。实现这一点的一种方法是使用对象配额，您(作为集群管理员)可以在单个名称空间上强制实施该配额。

您可以通过配额解决的第一个问题是 PVC 的数量和大小:

在上面的代码片段中，我们有两个对象——第一个是 *LimitRange* ,它规定了名称空间中单个 PVC 的最小和最大大小。这有助于阻止用户请求大量数据。这里的第二个对象( *ResourceQuota* )额外对 PVC 的数量及其累积大小进行了硬性限制。

接下来，为了防止人们创建一堆对象，然后在不需要时将它们丢弃，您可以使用*对象计数*配额，该配额对给定名称空间中特定类型资源的实例数量进行硬性限制:

您可以使用几个内置字段来指定对象计数配额，例如上面显示的`configmaps`、`secrets`或`services`。对于所有其他资源，你可以使用`count/<resource>.<group>`格式，如`count/jobs.batch`所示，这有助于防止错误配置的 *CronJob* 产生大量的作业。

我们可能都知道可以设置内存和 CPU 配额，但是您可能不知道还可以为暂时存储设置配额。对本地临时存储的配额支持是作为 1.18 版中的一项 alpha 功能添加的，它允许您以与内存和 CPU 相同的方式设置临时存储限制:

然而，要小心这些，因为当它们超过极限时会被驱逐，这可能是由例如集装箱原木的尺寸引起的。

除了为资源设置配额和限制之外，还可以为部署设置修订历史限制，以减少保留在集群中的*副本集*的数量。这是使用`.spec.revisionHistoryLimit`完成的，默认为 10。

最后，还可以设置 TTL(生存时间)来清理集群中存在时间过长的对象。这使用 TTL 控制器，该控制器从 1.21 版开始处于测试阶段，目前仅适用于使用`.spec.ttlSecondsAfterFinished`字段的作业，但将来可能会扩展到其他资源(例如 pod)。

# 手动清理

如果预防还不够，并且您已经有一堆孤立的、未使用的或以其他方式废弃的资源，那么您可以进行一次性清除。这可以只用`kubectl get`和`kubectl delete`来完成。您可以做的一些基本示例如下:

第一个命令使用资源标签执行基本的删除操作，这显然要求您首先使用某个`key=value`对来标记所有相关的资源。第二个示例展示了如何基于某个字段(通常是某个状态字段)删除一种类型的资源——在本例中是所有的 completed/succeeded 窗格。这一点也可以应用于其他资源，例如已完成的作业。

除了这两个命令之外，很难找到能够批量删除内容的模式，因此您必须寻找未使用的单独资源。然而，有一个工具可能会有所帮助——它叫做`[k8spurger](https://github.com/yogeshkk/k8spurger)`。该工具查找未使用的*角色绑定*、*服务账户*、*配置映射*等。并生成适合删除的资源列表，这有助于缩小搜索范围。

# kube-看门人

前几节探索了一些简单的、特别的清理方法，但是消除任何混乱的最终解决方案是使用`[kube-janitor](https://codeberg.org/hjacobs/kube-janitor)`。`kube-janitor`是一个在集群中运行的工具，可以作为任何其他工作负载，并使用 JSON 查询来查找资源，然后可以根据指定的 TTL 或到期日期删除这些资源。

要将该工具部署到您的集群中，您可以运行以下命令:

这会将`kube-janitor`部署到`default`名称空间，并使用示例规则文件和*模拟运行*模式运行它(使用`kube-janitor` *部署*中的`--dry-run`标志)。

在关闭*空转*模式之前，我们应该设置自己的规则。这些位于`kube-janitor`配置图中，看起来像这样:

我们显然对需要填充的`rules:`部分感兴趣。因此，这里有一些有用的示例，您可以在集群清理中使用:

此示例显示了清理临时、陈旧或未使用资源的一些基本用例。除了这种规则，您还可以为特定对象设置绝对到期日期/时间。这可以使用注释来完成，例如:

当你设置好你的规则和注释后，你可能应该让这个工具在*模拟运行*模式下运行一段时间，并打开调试日志，看看正确的对象是否会被删除，并避免删除你不想删除的内容(换句话说，如果你因为错误的配置和缺乏测试而删除了生产卷，不要怪我)。

最后，使用`kube-janitor`时要考虑的一件事是，如果集群中有很多对象，它可能需要比默认的`100Mi`更多的内存。所以，为了避免它的 pod 卡在 *CrashLoopBackOff* 中，我更喜欢给它`1Gi`作为内存限制。

# 监控集群限制

并非所有问题都可以通过手动甚至自动清理来解决，在某些情况下，监控是确保您不会触及集群中任何限制的最佳方式，无论是 pod 数量、可用的临时存储还是`etcd`对象数量。

然而，监控是一个很大的话题，需要一篇(或两篇)文章来阐述，所以为了本文的目的，我将列出来自 *Prometheus* 的几个指标，当您保持集群整洁时，这些指标可能会对您有用:

*   `etcd_db_total_size_in_bytes`-`etcd`数据库的大小
*   `etcd_object_counts` - `etcd`对象计数
*   `pod:container_cpu_usage:sum` -集群中每个 pod 的 CPU 使用率
*   `pod:container_fs_usage_bytes:sum` -集群中每个 pod 的文件系统使用情况
*   `pod:container_memory_usage_bytes:sum` -集群中每个 pod 的内存使用情况
*   `node_memory_MemFree_bytes` -每个节点的空闲内存
*   `namespace:container_memory_usage_bytes:sum` -每个名称空间的内存使用量
*   `namespace:container_cpu_usage:sum` -每个名称空间的 CPU 使用率
*   `kubelet_volume_stats_used_bytes` -每个卷的已用空间
*   `kubelet_running_pods` -节点中正在运行的 pod 数量
*   `kubelet_container_log_filesystem_used_bytes` -每个集装箱/箱的原木尺寸
*   `kube_node_status_capacity_pods` -每个节点的推荐最大箱数
*   `kube_node_status_capacity` -所有指标的最大值(CPU、pod、短期存储、内存、大页面)

这些只是您可以使用的众多指标中的一部分。可用的度量也取决于您的监控工具，这意味着您可能能够获得一些由您运行的服务公开的额外的定制度量。

# 结束语

在本文中，我们探讨了清理 Kubernetes 集群的许多选项——有些非常简单，有些更复杂。不管你选择哪种解决方案，试着保持在这个*“清理任务”*的顶端，并在你进行的过程中清理东西。它可以让你省去一个大麻烦，如果没有别的，它可以清除你的集群中一些不必要的混乱，这与清理你的桌子的目的相同。还要记住，如果你把东西放在那里一段时间，你会忘记它们为什么在那里，它们是否被需要，这使得把东西恢复到干净状态变得更加困难。

除了这里介绍的方法，您可能还想使用一些 *GitOps* 解决方案，如 *ArgoCD* 或 *Flux* 来创建和管理资源，这可以大大减少孤立资源的数量，并且也使清理更容易，因为它通常只需要您删除一个自定义资源的单个实例，这将触发从属资源的级联删除。

*本文最初发布于*[*martinheinz . dev*](https://martinheinz.dev/blog/60?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_60)

</the-easiest-way-to-debug-kubernetes-workloads-ff2ff5e3cc75>  </yq-mastering-yaml-processing-in-command-line-e1ff5ebc0823>  <https://itnext.io/hardening-docker-and-kubernetes-with-seccomp-a88b1b4e2111> 