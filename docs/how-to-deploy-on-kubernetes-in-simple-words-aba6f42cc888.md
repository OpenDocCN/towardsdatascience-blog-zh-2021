# 如何在 Kubernetes 上简单部署

> 原文：<https://towardsdatascience.com/how-to-deploy-on-kubernetes-in-simple-words-aba6f42cc888?source=collection_archive---------42----------------------->

## [行业笔记](https://pedram-ataee.medium.com/list/notes-from-industry-265207a5d024)

## 在 Kubernetes 集群上部署 web 应用程序的行动手册

![](img/5349dafcc70f71bfd56391a4a054124c.png)

由[乔尔·里维拉-卡马乔](https://unsplash.com/@actuallyjoel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我最近在 RapidAPI 上开发了一个名为 [Owl](https://rapidapi.com/pedram.ataee/api/word-similarity/) 的*上下文化的最相似单词* API 服务。这对我来说是一个伟大的旅程，所以我决定与你分享它发展的各个阶段。在本文中，我想分享**一个关于**如何使用 Kubernetes** 部署容器化 web 应用程序的分步指南(创建、连接、部署和公开)**。您可以在您的数据科学项目以及您正在开发的任何其他 web 应用程序中使用该指南。如果你想了解更多关于 OWL API 的内容，你可以阅读这篇文章:[如何计算单词相似度——比较分析](/how-to-compute-word-similarity-a-comparative-analysis-e9d9d3cb3080)。

> 在本文中，我想分享关于如何使用 Kubernetes 部署容器化 web 应用程序的分步指南(创建、连接、部署和公开)。

<https://www.amazon.com/gp/product/B08D2M2KV1/ref=dbs_a_def_rwt_hsch_vapi_tkin_p1_i0>  

# 第 0 步—为什么选择 Kubernetes？->可扩展性🔑

在我构建 OWL API 的过程中，为了构建一个比当前解决方案更好的“最相似单词”服务，我面临了许多 NLP 挑战。除了这些挑战，我还需要部署 [OWL API](https://rapidapi.com/pedram.ataee/api/word-similarity/) ，这样它就可以**管理** **大量的 API 请求。我本可以使用简单的 VPS(虚拟专用服务器)进行部署，但我确信我很快就会面临可扩展性问题。这就是我决定使用 Kubernetes 技术的原因。**

> Kubernetes 是一个开源容器编排器，用于容器化应用程序的可伸缩部署。

所以，我假设您已经准备好部署一个容器化的应用程序。您面临的问题是**如何部署它，以便它可以在需要时以最少的参与进行扩展。**

# 步骤 1—如何“创建”Kubernetes 集群？

## —我应该使用哪种云基础架构？☁

您必须使用您所使用的云基础设施的控制面板来创建 Kubernetes 集群。我最近和 [OVH 云](https://www.ovhcloud.com/en-ca/)和谷歌云合作过。在这两个集群上创建 Kubernetes 集群的过程很简单，但是有所不同。你应该找到他们自己的指南，在那里他们描述得很透彻。例如，您可以在下面的链接中找到创建集群的 OVH 指南。

<https://docs.ovh.com/ca/en/kubernetes/creating-a-cluster/>  

OVH 是一家法国云计算公司，提供 VPS、专用服务器和其他网络服务。他们是世界上最大的云提供商之一，但不如 GCP 或 AWS 有名。我也不知道他们直到几个月前，但我强烈推荐他们，因为 OVH 的界面是用户友好的，其成本更合理的竞争对手，如 AWS 或 GCP。如果你没有充分的理由使用 AWS 或 GCP，我强烈推荐 OVH 云。

## —在哪里🍄魔法🍄会发生什么？->负载平衡器

为了确保您的 Kubernetes 集群可以在需要时扩展，请确保在此步骤中启用自动扩展选项。另外，选择您希望在集群中运行的节点的正确数量。随着您向群集中添加的节点数量的增加，每月成本也会增加。

你可以在 OVH 获得的 Kubernetes 服务是一种**托管的 Kubernetes 服务**，这主要意味着在需要时自动管理扩展。神奇的是由一个名为**负载平衡器**的节点完成的，这个节点将所有到达的任务分配到一组资源上，以使整个过程更有效地工作。负载平衡有不同的策略，这不是本文的重点。所以，这里不多描述了。

# 步骤 2—如何“连接”到 Kubernetes 集群？

## —“kube config”就是你所需要的！(几乎所有😃)

创建 Kubernetes 集群后，您必须能够从控制面板下载一个**【key】**来从远程机器连接到集群。密钥主要是一个`YAML`文件，其中包含连接到 Kubernetes 集群所需的一切(例如，集群地址和所需的凭证)。您不能更改或修改该文件中的任何值。所以，你只需要从控制面板中找出必须下载的地方。

为了在远程机器和 Kubernetes 集群之间建立一个经过验证的连接，您需要这个文件。此密钥的格式在 GCP 或 OVH 等云基础架构中有所不同，这在现阶段并不重要。**唯一重要的事情**是您必须将该文件保存在一个安全的地方，并将`KUBECONFIG`环境变量的值设置为该文件的地址。如果使用 Linux 终端，可以使用下面的代码设置`KUBECONFIG`的值。

```
export KUBECONFIG=\\path\\to\\config\\kubeconfig.yml
```

## —还需要一样东西:“KUBECTL”👍

一旦将`KUBECONFIG`环境变量的值设置为相应文件的地址，就可以开始使用 Kubernetes 集群了。Kubernetes 命令行工具或`kubectl`是您开始使用集群所需的全部工具。`kubectl`允许您对 Kubernetes 集群运行命令，例如，部署应用程序或检查集群资源。**不过如何安装** `**kubectl**` **？**你可以阅读[这篇文章的作者写的](https://kubernetes.io/docs/tasks/tools/)。

> `kubectl`(即 Kubernetes 命令行工具)允许您对 Kubernetes 集群运行命令，例如部署应用程序或检查集群资源。

# 步骤 3—如何在 Kubernetes 集群上“部署”？

在前面的步骤中，您创建了一个 Kubernetes 集群，并构建了一个经过身份验证的连接来使用它。现在，您必须在集群上部署容器化的应用程序。

## —创建部署清单😮

*请不要混淆！*“manifest”只是一个`YAML`文件，其中的部署细节如“**如何映射外部端口？**或**如何设置环境变量**？”决心已定。您可以将下面的代码用作部署清单的模板。

```
apiVersion: v1
kind: Service
metadata:  
  name: MY-SERVICE  
  labels:    
    app: MY-APP
spec:  
  type: LoadBalancer
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetport: 443
    protocol: TCP
    name: https
  selector:
    app: MY-APPapiVersion: apps/v1
kind: Deployment
metadata:  
  name: MY-DEPLOYMENT
  labels:
    app: MY-APP
spec:  
  replicas: 1
  selector:
    matchLabels:
      app: MY-APP
  template:
    metadata:
      labels:
        app: MY-APP
    spec:
      containers:
      - name: MY-APP
        image: IMAGE-NAME
        ports:
        - containerPort: 80
```

关于上述部署清单的几点说明如下。

*   端口 80 和 443 在`LoadBalanacer`节点上打开。因此，如果您想在`https`上运行 web 应用程序，相应的端口(即 443)是打开的。
*   `MY-SERVICE`、`MY-APP`和`MY-DEPLOYMENT`必须替换为您想要在项目中使用的名称。您在此处分配的内容将在步骤 4 中使用，在该步骤中，您要将群集公开到 internet。
*   必须用您想要部署的容器化应用程序的完整名称/地址来替换`IMAGE-NAME`。如果你需要动态地改变这个变量的值，把它声明为一个环境变量(即`$IMAGE-NAME`)并使用`[envsubst](https://www.gnu.org/software/gettext/manual/html_node/envsubst-Invocation.html)`库。

## —提交部署 Manifest🧨

要进行部署，您必须使用`kubectl`向 Kubernetes 集群提交部署清单文件，如上所述。对于第一轮部署，您必须使用`kubectl apply`命令，而在下一轮部署中，您必须使用`kubectl replace`命令。为什么？因为您基本上是用集群上已经存在的版本替换一个新版本！您可以使用下面的代码提交一个`replace`命令。

```
kubectl replace -f path/to/manifest/manifest.yml
```

完成这一步后，运行`kubectl get services`。如果服务部署正确，您将会看到如下结果。

```
NAME         TYPE         CLUSTER-IP   EXTERNAL-IP  PORT(S)     AGE kubernetes   ClusterIP     X.X.X.X     <none>       443/TCP      X
MY-SERVICE   LoadBalancer  X.X.X.X     <none>          X         X
```

# 步骤 4—如何向互联网“公开”Kubernetes 集群？🔥

**祝贺你！你差不多完成了。**在步骤 3 中，您部署了应用程序。唯一剩下的事情就是将负载平衡器节点暴露给互联网。换句话说，您应该为 LoadBalancer 节点获取一个公共 IP，让流量通过它。尝试使用下面的代码将 internet 流量打开到 LoadBalancer 节点。

```
kubectl expose deployment MY-DEPLOYMENT --type=LoadBalancer --name=MY-SERVICE
```

获取公共 IP 需要几秒钟。然后，尝试`kubectl get services`找出服务可用的外部 IP！现在，您已经完全完成了部署过程！🎉

# 遗言

在各种情况下，您可能需要删除 Kubernetes 集群。例如，您想要部署最新的版本，并且您按照书上说的做每件事。然而，部署没有通过。在这种情况下，集群很有可能被锁定在不健康的状态，无法正确响应您的命令。您必须删除集群。要删除 Kubernetes 集群，您必须删除`Service`和`Deployment`对象。您可以使用命令`kubectl delete deployments MY-DEPLOYMENT`或`kubectk delete services MY-SERVICE`。

> 请注意，删除过程总是会产生一些不必要的后果，例如负载平衡器节点的外部 IP 发生变化。因此，在决定删除集群时，一定要小心谨慎。

最后一件事。在本文中，我旨在帮助您尽快在 Kubernetes 集群上部署应用程序。然而，你可以在 Kubernetes 的官方[文档](https://kubernetes.io/docs/tutorials/kubernetes-basics/)中找到更多关于 Kubernetes 的信息。希望你喜欢这篇文章！

# 感谢阅读！❤️

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我的* [*推特*](https://twitter.com/pedram_ataee) *！*

<https://pedram-ataee.medium.com/membership> 