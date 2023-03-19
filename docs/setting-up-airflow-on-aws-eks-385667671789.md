# 在 AWS 上设置气流

> 原文：<https://towardsdatascience.com/setting-up-airflow-on-aws-eks-385667671789?source=collection_archive---------6----------------------->

![](img/6c9a72356d0c9ab23a84be6097dcaa7c.png)

亚历克斯·丘马克在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 介绍

Airflow 是我最喜欢的工具之一，我经常使用它来设置和管理数据科学管道。气流用户界面为我们提供了 DAGS 及其当前状态的清晰画面。我可能是错的，但是从我的经验来看，我认为单个机器上的气流是不可扩展的。因此，为了扩大气流，我们可以使用 Kubernetes。

当我试图在 AWS EKS 上部署气流时，我不得不通过多个来源，从社区获得澄清。因此，我写这篇文章是为了让在 AWS EKS 上部署气流变得尽可能容易。

本文的先决条件是安装 aws-cli、kubectl 和 helm，在 aws 中设置一个 EKS 集群。我们将使用 helm 在 EKS 自动气象站部署气流舵图表。

我发现 Helm 在设置和管理 Kubernetes 应用程序时非常有用。Artifacthub.io 中提供了许多舵图。

气流也有一个舵图表，有很好的社区支持。我得感谢气流舵图表社区，他们帮了我大忙！舵轮图可在[https://github . com/air flow-helm/charts/tree/main/charts/air flow](https://github.com/airflow-helm/charts/tree/main/charts/airflow)获得。

# 先决条件

首先，我们需要设置我们的 kubectl 配置以指向 AWS EKS 集群。它的“如何”部分可在以下链接中找到:

[https://docs . AWS . Amazon . com/eks/latest/user guide/create-kube config . html](https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html)

一旦 kubectl kubeconfig(本地配置)设置为与 AWS EKS 集群通信，我们就可以继续在 AWS EKS 上部署气流了！！！！

# 舵图和自定义默认安装

在设置了前面指定的所有先决条件之后，我们现在可以开始我们的旅程了！

首先，我们需要添加回购并更新回购。这可以使用以下代码来完成:

```
helm repo add airflow-stable [https://airflow-helm.github.io/charts](https://airflow-helm.github.io/charts)
helm repo update
```

更新 helm repo 后，我们可以在 Kubernetes 集群上安装气流。为此，您可以先在本地的 minikube 上试用，然后再在 AWS EKS 集群上使用，或者直接安装在 AWS EKS 集群上。

以下命令将在 Kubernetes 集群上安装 Airflow:

```
helm install RELEASE_NAME airflow-stable/airflow 
--namespace NAMESPACE \
--version CHART_VERSION
```

RELEASE_NAME 可以取用户给定的任何值，名称空间是我们要安装 Airflow 的 Kubernetes 名称空间。默认情况下，如果没有指定名称空间，它将安装在 kubeconfig 的默认名称空间上。图表版本是需要安装的特定图表的版本，可以从[https://github.com/airflow-helm/charts/releases](https://github.com/airflow-helm/charts/releases)获得

这将在 Kubernetes 集群上安装气流，并将创建 pod。这可能需要一些时间，所以请耐心等待。成功安装后，我们可以使用 kubectl 端口转发访问 airflow UI，如下所示:

```
export POD_NAME=$(kubectl get pods --namespace NAMESPACE -l "component=web,app=airflow" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward --namespace NAMESPACE $POD_NAME 8080:8080
```

这样，您可以通过 localhost:8080 上的浏览器访问 UI。它可能会问你用户名和密码，默认的用户名和密码分别是 admin 和 admin。

# 自动气象站 EKS 上的气流

Kubernetes 上的 Airflow 使用 PostgreSQL 数据库、持久性卷来存储 dags 代码和日志、Redis 数据库和其他组件。更多信息请参考[https://github . com/air flow-helm/charts/tree/main/charts/air flow](https://github.com/airflow-helm/charts/tree/main/charts/airflow)

上述组件在 Kubernetes 集群中创建为 pod，并由气流使用。

从 AWS 的角度来看，其中一些可以用 AWS 提供的服务来替代。就像我们可以使用 AWS EFS 作为永久卷的基础来保存我们的气流日志并保留我们的气流 DAG 代码，AWS RDS PostgreSQL 而不是使用 PostgreSQL 数据库作为 pod，AWS 弹性缓存而不是 Redis 数据库(虽然我没有尝试过)。气流推荐用 PostgreSQL 代替 MySQL。

AWS 的所有这些定制都可以在 helm 安装过程中使用的 values.yml 文件中完成。利用 values.yml 覆盖默认值的方法如下:

```
helm install RELEASE_NAME airflow-stable/airflow 
--namespace NAMESPACE \
--version CHART_VERSION \
--values values.yml
```

可以在以下链接中找到示例 values.yml 文件:

[https://github . com/manojbalaji 1/AWS-air flow-helm/blob/main/values . yml](https://github.com/manojbalaji1/aws-airflow-helm/blob/main/values.yml)

假设我们有一些 AWS RDS PostgreSQL。我们可以通过编辑上面提到的 values.yml 文件的“# Database-External Database”部分，用它来替代默认的 PostgreSQL。

在此之前，我们需要将我们的 AWS RDS PostgreSQL 密码作为 kubernetes-secret。这可以通过创建一个类似于[https://github . com/manojbalaji 1/AWS-air flow-helm/blob/main/airflow-db-secret.yml](https://github.com/manojbalaji1/aws-airflow-helm/blob/main/airflow-db-secret.yml)的 air flow-d b-secret . yml 文件来完成

该文件包含 AWS RDS PostgreSQL base64 编码的密码。在基于 Linux 的系统上，密码可以编码如下:

```
>$echo “password” | base64
cGFzc3dvcmQK
```

其中 cGFzc3dvcmQK 是字符串“password”的 base64 编码值。类似地，您可以获得各自密码的 base64 编码值。

密码的编码值存储在 airflow-db-secret.yml 中，与密钥“密码”相对应，如下所示:

```
apiVersion: v1
kind: Secret
metadata: 
    name: airflowdb
type: Opaque
data: 
    password: cGFzc3dvcmQK
```

创建秘密文件后，我们可以使用以下命令使其在 Kubernetes 集群上可用，helm 将在安装期间访问该集群，如下所示:

```
kubectl apply -f airflow-db-secret.yml
```

现在继续，如果我们希望 Airflow 使用 AWS RDS PostgreSQL，首先我们需要在 values.yml 文件的“# Database-PostgreSQL Chart”部分将 enable 设置为 false，并编辑“# Database-External Database”部分，如下所示:

```
###################################
# Database - External Database
# - these configs are only used when `postgresql.enabled` is false
###################################
externalDatabase:  
    ## the type of external database:{mysql,postgres}  ##  
    type: postgres   

    ## the host of the external database  ##  
    host: <aws-postgresql-url>   

    ## the port of the external database  ##  
    port: 5432       ## the database/scheme to use within the the external database##         
    database: airflow       ## the user of the external database  ##  
    user: postgres   

    ## the name of a pre-created secret containing the external                       database password  ##  
    passwordSecret: "airflowdb"   

    ## the key within `externalDatabase.passwordSecret` containing the password string  ##  
    passwordSecretKey: "password"   

    ## the connection properties for external database, e.g. "?sslmode=require"  
    properties: ""
```

这里，我们需要在主机部分添加 AWS RDS PostgreSQL，将用户部分编辑为创建数据库时使用的适当用户名，添加我们在上一步中创建的密码的名称，在本例中，它是 passwordSecret 部分中的“airflowdb”和包含 base64 编码密码的密钥。在 passwordSecretKey 部分。

假设有一个在 s3 中持久化日志的需求，那么我们可以做同样的事情，首先在气流配置部分中添加一个具有适当值的连接，如下所示:

```
connections:     
    - id: s3_conn      
      type: aws      
      description: my AWS connection      
      extra: |-        
      { 
        "aws_access_key_id": "<aws_key_id>",             
        "aws_secret_access_key": "<aws_key>",           
        "region_name":"<aws-region>"         
      }
```

除此之外，我们还需要在 config 部分下设置一些环境变量，如下所示:

```
 AIRFLOW__SCHEDULER__DAG_DIR_LIST_INTERVAL: "30"            
AIRFLOW__LOGGING__REMOTE_LOGGING : "True"        
AIRFLOW__LOGGING__REMOTE_LOG_CONN_ID : "s3_conn"        
AIRFLOW__LOGGING__REMOTE_BASE_LOG_FOLDER : "s3://<s3-path>/"        
AIRFLOW__CORE__ENCRYPT_S3_LOGS: "False"AIRFLOW__KUBERNETES_ENVIRONMENT_VARIABLES__AIRFLOW__CORE__REMOTE_LOGGING: "True"        AIRFLOW__KUBERNETES_ENVIRONMENT_VARIABLES__AIRFLOW__CORE__REMOTE_BASE_LOG_FOLDER: "s3://<s3-path>/"        AIRFLOW__KUBERNETES_ENVIRONMENT_VARIABLES__AIRFLOW__CORE__REMOTE_LOG_CONN_ID: "s3_conn"        AIRFLOW__KUBERNETES_ENVIRONMENT_VARIABLES__AIRFLOW__CORE__ENCRYPT_S3_LOGS: "False"
```

除此之外，还有其他需要注意的事情，比如我们如何获取代码库中的代码库。有一个 git-sync 选项，可以将 git 存储库中的代码与持久性卷中的代码同步。对于企业来说，这可能不是一个理想的选择，因为代码库是不可公开访问的。相反，我们可以有一个 Jenkins 作业，将代码部署到 AWS EFS 的一个可以通过气流访问的位置。

我们首先可以通过如下定义 efs-pvc.yml 来设置持久卷和持久卷声明，以便使用 AWS:

```
kind: StorageClassapi
Version: storage.k8s.io/v1
metadata:  
    name: efs-sc
provisioner: efs.csi.aws.com 
---apiVersion: v1
kind: PersistentVolume
metadata:  
    name: efs-pvc
spec:  
    capacity:    
        storage: 10Gi  
    volumeMode: Filesystem  
    accessModes:    
        - ReadWriteMany  
    persistentVolumeReclaimPolicy: Retain  
    storageClassName: efs-sc  
    nfs:    
        server: <sub-region>.<efs-key>.efs.<region>.amazonaws.com     
        path: "/" 
---apiVersion: v1
kind: PersistentVolumeClaim
metadata:  
    name: efs-storage-claim
spec:  
    accessModes:    
        - ReadWriteMany  
    storageClassName: efs-sc  
    resources:    
        requests:      
            storage: 10Gi
```

在代码中，我们需要用适当的 AWS EFS 值替换服务器属性的值，如上所述。

创建这个文件后，我们可以使用下面的代码创建一个 PV 和 PVC:

```
kubectl apply -f efs-pvc.yml
```

这将在 Kubernetes 集群中创建一个 PV 和 PVC，并且可以在 values.yml 文件中使用，如下所示:

```
airflow:
    extraVolumeMounts:
        - name: airflow-efs-dag
          mountPath: /opt/efs/

    extraVolumes:
        - name: airflow-efs-dag
          persistentVolumeClaim:
              claimName: efs-storage-claim
```

在上面的代码中，我们要求 helm 使用前面定义的 PVC 名称创建一个额外的卷。然后，使用它在 extraVolumeMounts 中指定的路径上创建一个卷装载。这样，我们现在可以通过路径/opt/efs/访问 helm install 创建的 Airflow pods 上的 AWS EFS。

要将该路径用作 dags 文件夹的路径，我们需要对 values.yml 文件中的“# air flow-DAGs Configs”部分进行如下小改动:

```
dags:
    path: /opt/efs/dags # /opt/efs is EFS mount path specified above
```

使用 values.yml 文件中的上述代码，Airflow 集群将从上述路径访问 dags 文件。现在，我们可以创建一个 Jenkins 作业，将代码从 git 存储库复制到前面提到的 EFS 的 dags 文件夹路径中。

同样，如果想使用 AWS ElasticCache 而不是 Redis 数据库，他们可以编辑 values.yml 文件中的# External Redis 数据库部分，并能够使用 ElasticCache。仅供参考，如果 ElasticCache 不需要密码，您可以忽略密码部分，在前面提到的部分填写其他详细信息。

# 气流 AWS 连接器

最后但同样重要的是，airflow 默认不提供连接器和其他库来与 AWS 一起工作，所以我们需要安装 Airflow AWS 提供程序。这可以通过在 values.yml 文件中使用以下代码进行编辑来完成:

```
airflow:
    extraPipPackages: 
        - "apache-airflow-providers-amazon"
        - "awscli"
```

使用上面的代码，helm 将确保这些包安装在 web、worker 和 scheduler pods 上。如果有任何其他外部 pip 包需求，它们可以被添加到上面的代码中，它也将被安装。

**注意:**不要忘记更改 fermatkey 默认值…！！！！

# 结论

通过遵循上述所有步骤，我们可以在 AWS EKS 上设置一个气流集群，利用它我们可以轻松地进行纵向扩展和横向扩展。

设置气流集群的代码可在 https://github.com/manojbalaji1/aws-airflow-helm[的](https://github.com/manojbalaji1/aws-airflow-helm)处获得

希望这是有帮助的，请让我知道你的想法！干杯……！！！