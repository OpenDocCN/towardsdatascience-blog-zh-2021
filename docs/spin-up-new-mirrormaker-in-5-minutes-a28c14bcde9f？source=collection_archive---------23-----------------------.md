# 5 分钟后启动新的 MirrorMaker

> 原文：<https://towardsdatascience.com/spin-up-new-mirrormaker-in-5-minutes-a28c14bcde9f?source=collection_archive---------23----------------------->

## 使用 Kubernetes 部署脚本的逐步演练

![](img/6ca7a7353b51270d0a73cf4468e779b1.png)

[图片](https://pixabay.com/photos/board-chalk-business-job-work-3695073/)由[Pixabay.com](http://pixabay.com/)的[树 23](https://pixabay.com/users/athree23-6195572/) 提供

正如最近包含在 [Apache Kafka](https://kafka.apache.org/) 中以及在[我的前一篇博客](/a-fault-tolerant-kafka-replication-tool-across-multiple-datacenters-that-scales-1a36a96764b1)中介绍的那样， [new MirrorMaker](https://cwiki.apache.org/confluence/display/KAFKA/KIP-382%3A+MirrorMaker+2.0) 成为了官方认证的开源工具，可以在数据中心的两个 Kafka 实例之间复制数据。

为了获得新 MirrorMaker 的第一手经验，在本文中，我们将在本地 Kubernetes 上进行端到端部署。

作为先决条件，在执行以下步骤之前，需要在本地安装 [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) 和一个虚拟机监视器实例(例如 [VirtualBox](https://www.virtualbox.org/) 、[VMWare Fusion](https://www.vmware.com/products/fusion.html)……)。

> *注意:以下使用的脚本可以在 Kubernetes 集群中使用，但不保证生产质量部署*

## 第一步:开始本地 Kubernetes

`minikube start --driver=<driver_name> --kubernetes-version=v1.15.12 --cpus 4 --memory 8192`

如果使用 [VirtualBox](https://www.virtualbox.org/) ，< driver_name >将会是“VirtualBox”

## 步骤 2:克隆 Kubernetes 部署脚本并启动 Kafka

克隆 repo([https://github.com/ning2008wisc/minikube-mm2-demo](https://github.com/ning2008wisc/minikube-mm-demo))并运行以下命令来创建名称空间，2 个 kafka 实例

```
kubectl apply -f 00-namespace
kubectl apply -f 01-zookeeper
kubectl apply -f 02-kafka
kubectl apply -f 03-zookeeper
kubectl apply -f 04-kafka
```

然后验证 2 个 kafka 集群正在运行，每个集群有 3 个节点

```
kubectl config set-context --current --namespace=kafka
kubectl get podsNAME                               READY   STATUS    RESTARTS   AGE
kafka-0                            1/1     Running   0          2m5s
kafka-1                            1/1     Running   0          86s
kafka-2                            1/1     Running   0          84s
kafka2-0                           1/1     Running   0          119s
kafka2-1                           1/1     Running   0          84s
kafka2-2                           1/1     Running   0          82s
zookeeper-<hash>                   1/1     Running   0          2m8s
zookeeper-backup-<hash>            1/1     Running   0          2m2s
```

## 步骤 3:部署新的 MirrorMaker

镜子制造者将由[头盔](https://helm.sh/)部署。在本地安装 Helm，然后如下初始化:

```
helm init --tiller-namespace kafkakubectl create serviceaccount --namespace kafka tillerkubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kafka:tillerkubectl patch deploy --namespace kafka tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
```

为了最大限度地减少占用空间，MirrorMaker 被部署为分布式和独立的 kubernetes 服务，而不是设置 Kafka Connect 集群并通过 Kafka Connect REST 接口部署 MirrorMaker。

```
cd kafka-mm
helm --tiller-namespace kafka install ./ --name kafka-mm
```

检查 MM 2 的日志，确保它运行正常

```
kubectl logs -f kafka-mm-<hash> -c kafka-mm-server
```

## 步骤 4:用 Kafka 实例测试 MirrorMaker

现在，让我们在源 kafka 集群(`kafka-{0,1,2}`)上生产一些东西，并从目标集群(`kafka2-{0,1,2}`)上消费，以验证数据同时被镜像。

打开一个新的终端，切换到`kafka`名称空间，登录到`source` Kafka 集群的代理节点，然后启动控制台生成器

```
kubectl exec -i -t kafka-0 -- /bin/bash
bash-4.4# unset JMX_PORT
bash-4.4# /opt/kafka/bin/kafka-console-producer.sh  --broker-list localhost:9092 --topic test
```

打开另一个新终端，切换到`kafka`名称空间，登录到`target` Kafka 集群的代理节点，然后启动控制台消费者

```
kubectl exec -i -t kafka2-0 -- /bin/bash
bash-4.4# unset JMX_PORT
bash-4.4# /opt/kafka/bin/kafka-console-consumer.sh  --bootstrap-server localhost:9092 --topic primary.test
```

现在在控制台生成器中键入一些随机字符。预计在控制台消费者处会同时看到相同的字符。

## 步骤 5:监控 MirrorMaker

为了跟踪性能和健康状况，MirrorMaker 通过 JMX bean 公开了许多指标。下面是如何通过端口转发快速验证它们的原始格式。

```
kubectl port-forward kafka-mm-<hash> 8081:8081
```

打开本地 web 浏览器，输入 [http://localhost:8081/](http://localhost:8081/) 以原始和纯文本格式查看相关指标。

## 结论

在接下来的几篇博客中，我计划围绕新的 MirrorMaker 介绍更多有趣的话题，包括:

*   跨数据中心的一次性消息传递保证
*   从现有镜像解决方案迁移到 MM2 的工具

更多文章敬请关注！