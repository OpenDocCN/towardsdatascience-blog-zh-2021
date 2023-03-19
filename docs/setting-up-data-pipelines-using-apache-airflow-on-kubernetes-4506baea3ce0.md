# 在 Kubernetes 上使用 Apache Airflow 设置数据管道

> 原文：<https://towardsdatascience.com/setting-up-data-pipelines-using-apache-airflow-on-kubernetes-4506baea3ce0?source=collection_archive---------7----------------------->

## 在 Kubernetes 上部署可扩展的生产就绪型气流

![](img/a0bae671d416e80c2f5c70ec0e7f703a.png)

安德拉兹·拉济奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

Apache Airflow 是一个用于调度和监控数据管道的开源工作流编排工具。它还可以用于机器学习工作流和其他目的。Kubernetes 是一个事实上的编排工具，用于调度容器化的应用程序。气流可以利用 Kubernetes 提供的能力，根据工作负载来放大/缩小工作人员。

去年，我在 Kubernetes 上部署了 Apache Airflow。Bitnami 提供了一个 Apache 气流舵图，使我易于部署和扩展。它主要由三部分组成:webserver、scheduler 和 worker。

1.  Web 服务器:您可以浏览您的 Dag，使用 GUI 启动/停止进程
2.  调度程序:在后台管理和调度任务
3.  工人:执行任务的实际实例

除了这三个重要部分，Airflow 还有 Redis 和 PostgreSQL 来存储您的连接设置、变量和日志。

你可以在下面的 GitHub 库查看 Bitnami 图表的细节。

<https://github.com/bitnami/charts/tree/master/bitnami/airflow>  

# 加载气流 DAG 文件

在 Airflow 中，用户创建有向无环图(DAG)文件来定义必须执行的流程和任务、它们的执行频率、流程中任务的执行顺序以及它们之间的关系和依赖性。

当 Airflow 部署在标准服务器上时，我们只需要将 DAG 文件加载到服务器上的特定目录中。但是当您的 Airflow 部署在 Kubernetes 上时，您将需要其他方式让 Airflow 加载您的 DAG 文件。

我过去加载 DAG 文件的方式是从 Git 存储库。在我的本地 Kubernetes 集群上，有一个私有的 Git 存储库 GitLab。我必须启用舵图中的 Git 部分，并指定 Airflow 将使用的存储库。

可以从他们的资源库下载 Bitnami 气流图的 values . YAML:[https://github . com/Bitnami/charts/tree/master/Bitnami/air flow](https://github.com/bitnami/charts/tree/master/bitnami/airflow)

在 values.yaml 的 Git 部分，有一个字段定义了 Airflow 将同步和加载所有 DAG 文件的存储库。

```
git:
  dags:
  ## Enable in order to download DAG files from git repositories.
    enabled: true
    repositories:
      - repository: http://<User>:<Personal-Access-Tokens>@gitlab-webservice.gitlab.svc.cluster.local:8181/airflow/airflow.git
        branch: master
        name: gitlab
        path:
```

之后，您可以将所有 DAG 文件放在 Git 存储库中名为 dags 的文件夹中。因为我使用的是私有存储库，所以我使用如上所示的个人访问令牌来克隆 DAG 文件。

# 配置 Web 用户界面访问

为了让您的用户和您自己访问 Airflow 的 web UI，您需要公开 web 服务器的服务。如果您的 Kubernetes 集群中有 ingress 控制器，您可以配置 Ingress 资源，并允许自己通过 Ingress 访问 Airflow 服务器。您只需要启用入口，并将主机修改为您在 Helm 图表的 values.yaml 中的主机名。

```
ingress:
## Set to true to enable ingress record generation
  enabled: true
  hosts:
    name: airflow.yourdomain.com
    path: /
```

在我们部署气流之前，我们需要考虑舵图的更多值。因为我们将来需要升级我们的版本，所以必须提供数据库 PostgreSQL 和 Redis 的密码。您可以在 PostgreSQL 和 Redis 图表配置中设置它们。

此外，我们需要提供用户的密码和 fernetKey 进行身份验证。访问 web UI 的`username`和`password`可以在 values.yaml 中的`auth`部分进行修改。记住还要将`forcePassword`设置为 true，这是将来升级正常工作所必需的。

要生成新的 fernet 密钥，可以使用 Airflow 官方文档提供的以下[代码片段](https://airflow.apache.org/docs/apache-airflow/1.10.12/security.html#generating-fernet-key)。一些随机的文本(例如`2r3EPAQ-Fzc-rAexn5ltRsnaUP9QjoVi7Z36I3AcyZ4=`)会被打印出来。只需将其复制到`auth`部分的`fernetKey`字段即可。

```
from cryptography.fernet import Fernet
fernet_key= Fernet.generate_key()
print(fernet_key.decode()) # your fernet_key, keep it in secured place!
```

# 使用头盔展开

一旦我们完成了舵图的修改，你就可以使用三个命令在 Kubernetes 上部署气流了！

首先，创建一个名为 airflow 的名称空间:

```
kubectl create ns airflow
```

其次，将 Bitnami 图表存储库添加到 Helm:

```
helm repo add bitnami [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami)
```

最后，你用头盔安装气流。轻松点。当然，您可以将“my-release”更改为任何您喜欢的名称。

```
helm install –n airflow my-release bitnami/airflow -f values.yaml
```

气流展开！您应该会看到类似这样的内容:

```
NAME: my-release
LAST DEPLOYED: Thu Mar 11 01:27:30 2021
NAMESPACE: airflow
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
1\. Get the Airflow URL by running:echo URL : [http://127.0.0.1:8080](http://127.0.0.1:8080)2\. Get your Airflow login credentials by running:
 export AIRFLOW_PASSWORD=$(kubectl get secret — namespace airflow my-release-airflow -o jsonpath=”{.data.airflow-password}” | base64 — decode)
 echo User: user
 echo Password: $AIRFLOW_PASSWORD
```

您可以按照上面的说明获取您的 Airflow 登录凭据。您还可以通过 values.yaml 中指定的 URL 访问您的 web UI(在本例中是 airflow.yourdomain.com)

过一会儿，通过命令`kubectl get pod -n airflow`，您应该看到所有的 pod 都在名称空间 airflow 中运行。

```
# kubectl get pod -n airflow
NAME READY STATUS RESTARTS AGE
airflow-postgresql-0 1/1 Running 0 3m53s
airflow-redis-master-0 1/1 Running 0 3m53s
airflow-redis-slave-0 1/1 Running 0 3m53s
airflow-redis-slave-1 1/1 Running 0 3m53s
airflow-scheduler-5f8cc4fd5f-jf4s9 2/2 Running 0 3m53s
airflow-web-859dd77944-sfgw4 2/2 Running 0 3m53s
airflow-worker-0 2/2 Running 0 3m53s
airflow-worker-1 2/2 Running 0 3m53s
airflow-worker-2 2/2 Running 0 3m53s
```

# 安装额外的 Python 包

有时，您可能需要原始图像不包含的额外 Python 包。Bitnami 的图表允许您挂载自己的 requirements.txt 来安装所需的包。您需要将您的卷安装到`/bitnami/python/requirements.txt`。当容器启动时，它将执行`pip install -r /bitnami/python/requirements.txt`。

在 worker 组件中，需要通过提供卷的`name`和容器实例的`mountPath`来修改`extraVolumeMounts`。还需要加上你的`extraVolumes`。我使用 ConfigMap 将 requirements.txt 添加到容器中，因为我可以更容易地添加更多额外的 python 包。

```
extraVolumeMounts:
  - name: requirements
    mountPath: /bitnami/python/## Add extra volumes
extraVolumes:
  - name: requirements
    configMap:
# Provide the name of the ConfigMap containing the files you want
# to add to the container
      name: requirements
```

为了创建我们需要的配置图，首先，我们准备 requirements.txt 文件。然后，我们通过以下命令创建配置映射:

```
kubectl create -n airflow configmap requirements --from-file=requirements.txt
```

在我们创建了 ConfigMap 并修改了 values.yaml 之后，我们需要升级我们的气流。只需一个简单的命令:

```
helm upgrade –n airflow my-release bitnami/airflow -f values.yaml
```

# 结论

在本文中，我向您展示了如何在 Kubernetes 上部署气流的基础知识。Bitnami 提供了一个组织良好的舵图，让我们可以轻松地在 Kubernetes 上部署气流。基于 Bitnami 的掌舵图，仍然有一些小参数，如数据库用户名和密码，auth fernet 密钥，额外的 python 包等，我们需要调整。虽然 Bitnami 已经帮我们省去了很多辛苦，但是在建立“正确”的设置之前，我还是经历了很多试错的过程。希望这个指南能帮你节省一些时间！如果你对 Kubernetes 上的气流架构有什么想法，请告诉我。我想听听你对 Kubernetes 更好、更可扩展的气流的看法。

如果你想了解更多关于气流的用例，可以看看我之前的文章。在那篇文章中，我展示了如何使用 Airflow 构建一个用于移动网络性能监控的自动报告系统。

</designing-data-pipeline-system-for-telco-performance-measurement-3fa807dbd009>  

如果您想了解如何使用 Kubespray 构建本地 Kubernetes，请访问本文:

</deploy-a-production-ready-on-premise-kubernetes-cluster-36a5d62a2109>  

如果您想了解更多关于 Apache Airflow 的知识，Udemy 上有一个很好的课程，可以教授 Airflow 的基础知识，还可以动手构建数据管道。如有兴趣，请参考以下附属链接:

<https://click.linksynergy.com/deeplink?id=0wsuN4lypZM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fthe-complete-hands-on-course-to-master-apache-airflow%2F>  <https://click.linksynergy.com/link?id=0wsuN4lypZM&offerid=916798.19111388448&type=2&murl=https%3A%2F%2Fwww.coursera.org%2Flearn%2Fetl-and-data-pipelines-shell-airflow-kafka> 