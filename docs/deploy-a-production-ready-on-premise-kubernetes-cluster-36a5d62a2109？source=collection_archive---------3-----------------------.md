# 部署生产就绪的本地 Kubernetes 集群

> 原文：<https://towardsdatascience.com/deploy-a-production-ready-on-premise-kubernetes-cluster-36a5d62a2109?source=collection_archive---------3----------------------->

## 走向现代软件架构

## 你需要考虑的几件事

去年在疫情期间，我有机会自己部署了一个本地 Kubernetes。在本文中，我想标记一些事情来提醒我，并告诉您在部署本地 Kubernetes 时应该知道的所有事情。

![](img/a1886ed266e1fd1ec55f3ad7bbde3744.png)

[兰斯·安德森](https://unsplash.com/@lanceanderson?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

# 库贝斯普雷

Kubespray 使用 Ansible 行动手册作为库存和供应工具，帮助我们部署通用的 Kubernetes 集群。它还处理我们的配置管理任务。它自动化了你部署 Kubernetes 的所有困难。你所需要的只是修改 Kubespray 提供的 YAML 文件配置。以下指南是从 Kubespray 的文档中复制的，您可以在这里查看:

[](https://github.com/kubernetes-sigs/kubespray) [## kubernetes-sigs/kubespray

### 如果您有任何问题，请查看 kubespray.io 上的文档，并加入我们的 kubernetes slack 频道#kubespray。

github.com](https://github.com/kubernetes-sigs/kubespray) 

首先，您需要 git 克隆存储库并使用 pip 安装 Python 需求。

```
git clone [https://github.com/kubernetes-sigs/kubespray.git](https://github.com/kubernetes-sigs/kubespray.git)
# Install dependencies from ``requirements.txt``
sudo pip3 install -r requirements.txt
```

如果您无法安装 Ansible，请查看以下链接:

[](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) [## 安装 Ansible — Ansible 文档

### 本页描述如何在不同的平台上安装 Ansible。Ansible 是一个无代理的自动化工具，由…

docs.ansible.com](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) 

清单文件夹中有一个样本清单。您需要复制并命名您的整个集群(例如 mycluster)。存储库已经为您提供了库存构建器来更新 Ansible 库存文件。

```
# Copy ``inventory/sample`` as ``inventory/mycluster``cp -rfp inventory/sample inventory/mycluster# Update Ansible inventory file with inventory builderdeclare -a IPS=(10.10.1.3 10.10.1.4 10.10.1.5)CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
```

接下来，你要修改`inventory/mycluster/hosts.yml`。下面是一个示例，它将三个节点设置为主节点，将另外三个节点设置为工作节点。etcd 与主设备位于相同的节点中。

```
all:
  hosts:
    node1:
      ansible_host: 192.168.0.2
      ip: 192.168.0.2
      access_ip: 192.168.0.2
    node2:
      ansible_host: 192.168.0.3
      ip: 192.168.0.3
      access_ip: 192.168.0.3
    node3:
      ansible_host: 192.168.0.4
      ip: 192.168.0.4
      access_ip: 192.168.0.4
    node4:
      ansible_host: 192.168.0.5
      ip: 192.168.0.5
      access_ip: 192.168.0.5
    node5:
      ansible_host: 192.168.0.6
      ip: 192.168.0.6
      access_ip: 192.168.0.6
    node6:
      ansible_host: 192.168.0.7
      ip: 192.168.0.7
      access_ip: 192.168.0.7
  children:
    kube-master:
      hosts:
        node1:
        node2:
        node3:
    kube-node:
      hosts:
        node4:
        node5:
        node6:
    etcd:
      hosts:
        node1:
        node2:
        node3:
    k8s-cluster:
      children:
        kube-master:
        kube-node:
    calico-rr:
      hosts: {}
```

因为 Ansible 将与您的主机建立 SSH 连接，所以必须将 Ansible 主机的 SSH 密钥复制到清单中的所有服务器或主机。您需要做的是:

1.  用`ssh-keygen`生成一个 SSH 密钥
2.  使用`ssh-copy-id -i ~/.ssh/mykey user@host`将密钥复制到服务器

现在，您需要检查并更改集群的参数。有些方面你应该考虑。

```
# Review and change parameters under ``inventory/mycluster/group_vars``cat inventory/mycluster/group_vars/all/all.ymlcat inventory/mycluster/group_vars/k8s-cluster/k8s-cluster.yml
```

# 主节点 HA API 服务器负载平衡器

第一个是为 API 服务器的高可用性设置一个负载平衡器。负载平衡器用于在我们的一个主节点不工作时防止服务失败。通常我们会设置至少三个主服务器。您可以在文档的 K8s 的 HA 端点部分找到更多详细信息。

[](https://github.com/kubernetes-sigs/kubespray/blob/master/docs/ha-mode.md) [## kubernetes-sigs/kubespray

### 以下组件需要高度可用的端点:etcd 集群、kube-apiserver 服务实例。的…

github.com](https://github.com/kubernetes-sigs/kubespray/blob/master/docs/ha-mode.md) 

> 对于生产就绪的 Kubernetes 集群，我们需要使用外部负载平衡器(LB)而不是内部 LB。外部 LB 为外部客户机提供访问，而内部 LB 只接受到本地主机的客户机连接。

我使用[ha proxy](http://www.haproxy.org/)+[keepalive](https://github.com/acassen/keepalived)来配置一个高可用的负载平衡器。设置了两个虚拟机来执行负载平衡器功能。我提到的参考资料来自这个网站:

[](https://www.digitalocean.com/community/tutorials/how-to-set-up-highly-available-haproxy-servers-with-keepalived-and-floating-ips-on-ubuntu-14-04) [## 如何在 Ubuntu 14.04 上设置具有 Keepalived 和 Floating IPs 的高可用 HAProxy 服务器|…

### 高可用性是系统设计的一个功能，它允许应用程序自动重启或重新路由工作到…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-set-up-highly-available-haproxy-servers-with-keepalived-and-floating-ips-on-ubuntu-14-04) 

文章虽然有点老，但是概念是一样的，程序也挺像的。

> 给定前端的`VIP`地址和后端的`IP1, IP2`地址，下面是作为外部 LB 的 HAProxy 服务的配置示例:

```
#/etc/haproxy/haproxy.cfglisten kubernetes-apiserver-httpsbind <VIP>:8383mode tcpoption log-health-checkstimeout client 3htimeout server 3hserver master1 <IP1>:6443 check check-ssl verify none inter 10000server master2 <IP2>:6443 check check-ssl verify none inter 10000balance roundrobin
```

然后，您需要更改外部负载平衡器配置。同样，配置是从文档中 K8s 部分的 HA 端点复制的。

```
#kubespray/inventory/mycluster/group_vars/all/all.yml##   External LB example config##   apiserver_loadbalancer_domain_name: "elb.some.domain"#   loadbalancer_apiserver:#   address: <VIP>#   port: 8383
```

# 代理人

如果您的集群不能直接访问互联网，并且在像我这样的公司代理后面，您将在部署过程中经历痛苦…确保您在`kubespray/inventory/mycluster/group_vars/all/all.yml`中添加您的代理设置

将您的代理添加到`http_proxy` ，并在`additional_no_proxy` 部分添加您不希望您的集群通过代理访问的域/IP。(重要！)

# 从外部访问 kubectl

如果您的主虚拟机有两个网络接口，并且您需要从 Kubernetes 使用的接口之外的另一个接口访问 Kubernetes，您需要在配置文件中添加补充地址:

```
## Supplementary addresses that can be added in kubernetes ssl keys.## That can be useful for example to setup a keepalived virtual IPsupplementary_addresses_in_ssl_keys: [10.0.0.1, 10.0.0.2, 10.0.0.3]
```

# 负载平衡器

为了让您的用户访问部署在 Kubernetes 中的服务，您需要公开您的服务。从集群外部访问有三种方式:入口、负载平衡器和节点端口。为了在内部实施负载平衡器类型的服务，我们选择了 MetalLB 来为我们处理网络负载平衡功能。 [MetalLB](https://metallb.universe.tf/) 是裸机 Kubernetes 集群的负载平衡器实现，使用标准路由协议。

使用 Kubespray 安装 MetalLB 非常容易。你只需要修改插件 YAML 文件和 k8s 集群 YAML 文件。请注意，您分配给 MetalLB 的 IP 范围必须由您自己预先准备。

```
# kubespray/inventory/mycluster/group_vars/k8s-cluster/addons.ymlmetallb_enabled: truemetallb_ip_range: - “10.5.0.50–10.5.0.99”
```

请记住将参数 kube_proxy_strict_arp 设置为 true。

```
# kubespray/inventory/mycluster/group_vars/k8s-cluster/k8s-cluster.yml# must be set to true for MetalLB to workkube_proxy_strict_arp: true
```

# 部署

最后，您可以使用 Ansible 行动手册部署 Kubespray！根据您的群集大小和网络吞吐量，群集大约需要 15–30 分钟才能准备好。之后，您就可以使用您的 Kubernetes 集群了！

```
# Deploy Kubespray with Ansible Playbook - run the playbook as root# The option `--become` is required, as for example writing SSL keys in /etc/,# installing packages and interacting with various systemd daemons.# Without --become the playbook will fail to run!ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml
```

![](img/b3e27012acf467da25467611cf249cc8.png)

[Guille Álvarez](https://unsplash.com/@guillealvarez?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在将应用程序部署到 Kubernetes 集群之前，我们还有很长的路要走。

# 舵

Helm 是 Kubernetes 的包装经理。在赫尔姆的官方网站上，它写道

> Helm 是查找、共享和使用为 Kubernetes 构建的软件的最佳方式。Helm 帮助您管理 Kubernetes 应用程序— Helm Charts 帮助您定义、安装和升级最复杂的 Kubernetes 应用程序。

要安装 Helm，最简单的方法是通过脚本安装:

```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3$ chmod 700 get_helm.sh$ ./get_helm.sh
```

您可以获取该脚本，然后在本地执行它。它有很好的文档记录，因此您可以通读它，并在运行它之前了解它在做什么。

[](https://helm.sh/docs/intro/install/) [## 安装舵

### 本指南介绍了如何安装 Helm CLI。Helm 可以从源代码安装，也可以从预构建的二进制文件安装…

helm.sh](https://helm.sh/docs/intro/install/) 

# 进入

为了让您的用户访问部署在 Kubernetes 的服务，您需要公开您的服务。有三种从集群外部访问的方式:入口、负载平衡器和节点端口。在生产中，不建议使用节点端口，因为它缺乏可用性。对于负载平衡器，我们已经使用 MetalLB 实现了。现在，我们将实现入口。请查看本网站入口的详情:

[](https://kubernetes.io/docs/concepts/services-networking/ingress/) [## 进入

### 功能状态:Kubernetes v 1.19[稳定]一个管理对集群中服务的外部访问的 API 对象…

kubernetes.io](https://kubernetes.io/docs/concepts/services-networking/ingress/) 

根据官方 Kubernetes 文档的描述，[入口](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#ingress-v1-networking-k8s-io)公开了从集群外部到集群内[服务](https://kubernetes.io/docs/concepts/services-networking/service/)的 HTTP 和 HTTPS 路由。流量路由由入口资源上定义的规则控制。入口控制器控制入口资源。我使用的那个叫做 NGINX 入口控制器。您可以使用 Helm 轻松安装。

[](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/) [## NGINX 文档|使用 Helm 安装

### 入口控制器 Helm 3.0+支持的 Kubernetes 版本。Git。如果您想使用 NGINX Plus:构建一个…

docs.nginx.com](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/) 

```
helm repo add nginx-stable https://helm.nginx.com/stablehelm repo updatehelm install my-release nginx-stable/nginx-ingress
```

# 储存；储备

pod 中的数据是短暂的，如果您希望数据持久化，您可以将数据存储在持久性卷中。根据 Kubernetes 官方文档的描述，*持久卷* (PV)是集群中的一块存储，由管理员提供或使用[存储类](https://kubernetes.io/docs/concepts/storage/storage-classes/)动态提供。

[](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) [## 持久卷

### 本文档描述了 Kubernetes 中持久性卷的当前状态。建议熟悉卷…

kubernetes.io](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) 

每个存储类都有一个处理卷分配的置备程序。我选的是长角牛。根据 Longhorn 官方文档的描述，它指出

[](https://longhorn.io/) [## 长角牛

### 在过去，ITOps 和 DevOps 发现很难添加…

longhorn.io](https://longhorn.io/) 

> Longhorn 是一个轻量级的、可靠的、功能强大的分布式块存储系统。Longhorn 使用容器和微服务实现分布式块存储。它为每个块设备卷创建一个专用的存储控制器，并跨存储在多个节点上的多个副本同步复制该卷。存储控制器和副本本身是使用 Kubernetes 编排的。

这使得我们的存储没有单点故障。它还有一个直观的 GUI 控制面板，您可以在其中查看卷的所有详细信息。

以下安装指南复制自 Longhorn 官方文档:

**安装长角牛**

1.添加长角牛头盔存储库:

```
helm repo add longhorn https://charts.longhorn.io
```

2.从存储库中获取最新图表:

```
helm repo update
```

3.在 longhorn-system 命名空间中安装 Longhorn。

```
kubectl create namespace longhorn-systemhelm install longhorn longhorn/longhorn --namespace longhorn-system
```

4.要确认部署成功，请运行:

```
kubectl -n longhorn-system get pod
```

如果准备就绪，您应该会看到所有 pod 都在运行。

# 登记处

您需要一个注册表来存储您开发的 docker 图像。其中一个开源选择是 Harbor。

[](https://goharbor.io/) [## 海港

### 什么是港湾？Harbor 是一个开源注册表，它通过策略和基于角色的访问控制来保护工件…

goharbor.io](https://goharbor.io/) 

同样，你可以很容易地安装舵图港。我用的那个是 bitnami 维护的:

[](https://github.com/bitnami/charts/tree/master/bitnami/harbor/#installing-the-chart) [## 比特纳米/图表

### 这种舵图是在 goharbor/harbor-helm 图的基础上开发的，但包括了一些与

github.com](https://github.com/bitnami/charts/tree/master/bitnami/harbor/#installing-the-chart) 

# 结论

最后，我将带您了解一个本地 Kubernetes 集群的整个安装过程。让我们做一个简单的清单:

1.  库贝斯普雷
2.  可变配置
3.  主节点 HA API 服务器负载平衡器(HAProxy + keepalived)
4.  代理设置
5.  从集群外部配置 kubectl 访问
6.  服务公开:负载平衡器和入口
7.  包管理:掌舵
8.  存储:长角牛
9.  内部图像存储库:注册表

# 还没有…

它仍然只涵盖了生产就绪的 Kubernetes 集群的一部分，我仍在探索中。其他重要的方面，如监控(如普罗米修斯& Grafana)，安全(RBAC/用户管理)，CICD(阿尔戈)等，在这篇文章中仍然没有

> 欢迎来到库伯内特的世界！

如果您想知道如何准备认证 Kubernetes 应用程序开发人员(CKAD)考试，请查看这篇文章:

[](/pass-ckad-certified-kubernetes-application-developer-with-a-score-of-97-af072a65f1ce) [## 以 97 分通过 CKAD(认证 Kubernetes 应用开发者)！

### 学习指南和技巧

towardsdatascience.com](/pass-ckad-certified-kubernetes-application-developer-with-a-score-of-97-af072a65f1ce) 

你可能也想检查下面的附属链接。

[](https://click.linksynergy.com/deeplink?id=0wsuN4lypZM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fcertified-kubernetes-application-developer%2F) [## Kubernetes 认证应用程序开发人员(CKAD)培训

### Kubernetes 认证可以让你的职业生涯达到一个全新的水平。在 Kubernetes 上学习、实践并获得认证…

click.linksynergy.com](https://click.linksynergy.com/deeplink?id=0wsuN4lypZM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fcertified-kubernetes-application-developer%2F)