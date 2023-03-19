# 如何将本地或远程机器连接到 Databricks 集群

> 原文：<https://towardsdatascience.com/how-to-connect-a-local-or-remote-machine-to-a-databricks-cluster-18e03afb53c6?source=collection_archive---------11----------------------->

## [业内笔记](https://pedram-ataee.medium.com/list/notes-from-industry-265207a5d024)

## Databricks、Python 和 Docker 的交集

![](img/17f2d98e588e4ec89377a2dda8d178c5.png)

[粘土银行](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

当您开始使用 Databricks 时，您将会决定在 Databricks 之外进行编码，并远程连接到它的计算能力，也就是 Databricks 集群。为什么？主要是因为 Databricks 的主要特性之一是它的 Spark 作业管理,这可以让您的生活变得轻松。使用该服务，您可以向大规模数据集提交一系列 Spark 作业，并在几秒钟内返回您的结果；多亏了它的火花引擎。在本文中，我想描述如何配置您的本地或远程机器连接到 Databricks 集群，作为这个过程的第一步。

[](https://www.amazon.com/gp/product/B08D2M2KV1/ref=dbs_a_def_rwt_hsch_vapi_tkin_p1_i0) [## 人工智能:非正统的教训:如何获得洞察力和建立创新的解决方案

### 亚马逊网站:人工智能:非正统课程:如何获得洞察力和建立创新的解决方案电子书…

www.amazon.com](https://www.amazon.com/gp/product/B08D2M2KV1/ref=dbs_a_def_rwt_hsch_vapi_tkin_p1_i0) 

# —什么是数据块？

Databricks 是一个抽象层，位于 AWS 和 Azure 等冷云基础设施上，使您可以轻松管理计算能力、数据存储、作业调度和模型管理。它为您提供了一个开发环境，以获得您的数据科学或人工智能项目的初步结果。Databricks 由 Spark 提供支持，Spark 是一个开源数据处理引擎，专门为大规模数据设计。

Databricks 提供的简单性有一些限制。例如，Databricks 使您作为数据科学新手能够快速生成结果，因为您不需要大量处理云配置或可视化工具。然而，如果您想要控制开发的每个方面，Databricks 肯定会限制您。

# —要求是什么？

## 1-使用受支持的数据块运行时创建数据块集群

每个 Databricks 集群都必须运行一个称为 Databricks 运行时的专用操作系统。在配置集群时，您会发现许多版本的 Databricks 运行时。如果您想要远程连接到 Databricks 集群，**您必须谨慎选择 Databricks 运行时版本**。为什么？因为`databricks-connect`客户端支持有限数量的数据块运行时版本，所以 Spark 客户端库支持这种远程连接。

> 数据块运行时版本必须与数据块连接库兼容。

换句话说，您不能远程连接到其数据块运行时不受`databricks-connect`支持的集群。您可以在 Databricks Connect 的原始文档中找到 Databricks 运行时版本的更新列表。

[](https://docs.databricks.com/dev-tools/databricks-connect.html#requirements) [## 数据块连接

### Databricks Connect 允许您连接您喜爱的 IDE (IntelliJ、Eclipse、PyCharm、RStudio、Visual Studio)…

docs.databricks.com](https://docs.databricks.com/dev-tools/databricks-connect.html#requirements) 

## 2-安装所需的 Python 和 Java 版本

选择与`databricks-connect`兼容的 Databricks 运行时版本后，您必须确保在您的机器上使用兼容的 Python 版本。如原始文档中所述:**“客户机 Python 安装的次要版本必须与 Databricks 集群的次要 Python 版本相同。”**

> 开发环境的 Python 版本必须与在 Databricks 集群上工作的 Databricks 运行时版本兼容。

假设您为 Databricks 运行时版本选择了 **7.3 LTS** 。在这种情况下，您必须在本地或远程机器上安装 **Python 3.7** 才能与之兼容。您还必须在您的机器上安装 Java 运行时环境(JRE) 8。这些都是由 Databricks 网站指导的。

 [## Java 8

### 未找到结果您的搜索没有匹配任何结果。我们建议您尝试以下方法来帮助找到您想要的…

www.oracle.com](https://www.oracle.com/java/technologies/java8.html) 

## 3-安装所需的 Databricks Connect 版本

正如 Python 开发所建议的，最好创建一个隔离的虚拟环境，以确保没有冲突。你可以用你选择的库，如`venv`、`pipenv`或`conda`，轻松构建一个虚拟环境。我在这里选了最新的。

如果不想创建新的虚拟环境，确保不要在现有环境上安装(或卸载)`pyspark`。您应该卸载现有版本的`pyspark`，因为它与`databricks-connect`客户端冲突。`databricks-connect`有自己的方法，相当于`pyspark`的方法，这使得它可以独立运行。通过下面的代码，您用 Python 3.7 和一个版本的`databricks-connect`创建了一个虚拟环境。

```
conda create --name ENVNAME python=3.7
conda activate ENVNAME
pip3 uninstall pyspark
pip3 install -U databricks-connect==7.3.*
```

你可能不需要上面的`pip3 uninstall pyspark`，因为虚拟环境是干净的。我把它放在那里只是为了完整。

# —如何根据需求创建 Docker 映像

如果您想用 Python 3.7 和 Java 8 以及一个版本的`databricks-connect`构建 docker 映像，您可以使用下面的 docker 文件。

```
FROM ubuntu:20.04RUN apt-get update && apt-get -y install sudo
RUN sudo apt-get -y install software-properties-common### INSTALL PYTHON
RUN sudo apt-get -y install libssl-dev openssl
RUN sudo apt-get -y install libreadline-gplv2-dev libffi-dev
RUN sudo apt-get -y install libncursesw5-dev libsqlite3-dev
RUN sudo apt-get -y install tk-dev libgdbm-dev libc6-dev libbz2-dev 
RUN sudo apt-get -y install  wget
RUN apt-get update && apt-get install make
RUN sudo apt-get -y install zlib1g-dev
RUN apt-get -y install gcc mono-mcs && \
    rm -rf /var/lib/apt/lists/*
RUN wget [https://www.python.org/ftp/python/3.7.10/Python-3.7.10.tgz](https://www.python.org/ftp/python/3.7.10/Python-3.7.10.tgz)
RUN tar xzvf Python-3.7.10.tgz
RUN ./Python-3.7.10/configure
RUN sudo make install
RUN alias python=python3### INSTALL JAVA
RUN sudo add-apt-repository ppa:openjdk-r/ppa
RUN sudo apt-get install -y openjdk-8-jre### INSTALL DATABRICKS-CONNECT
RUN pip3 install --upgrade pip
RUN pip3 uninstall pyspark
RUN pip3 install -U databricks-connect==7.3.*
```

# —如何配置连接属性？

使用`databricks-connect configure`，很容易配置`databricks-connect`库以连接到 Databricks 集群。运行此命令后，它会以交互方式询问您有关主机、令牌、组织 ID、端口和集群 Id 的问题。要了解更多信息，您可以查看下面的官方文档。

[](https://docs.databricks.com/dev-tools/databricks-connect.html#step-2-configure-connection-properties) [## 数据块连接

### Databricks Connect 允许您连接您喜爱的 IDE (IntelliJ、Eclipse、PyCharm、RStudio、Visual Studio)…

docs.databricks.com](https://docs.databricks.com/dev-tools/databricks-connect.html#step-2-configure-connection-properties) 

然而，**如果您想自动配置 Docker 映像**中的连接属性，您可以将以下代码添加到上述 Docker 文件的末尾。

```
RUN export DATABRICKS_HOST=XXXXX && \
    export DATABRICKS_API_TOKEN=XXXXX && \
    export DATABRICKS_ORG_ID=XXXXX && \
    export DATABRICKS_PORT=XXXXX && \
    export DATABRICKS_CLUSTER_ID=XXXXX && \
    echo "{\"host\": \"${*DATABRICKS_HOST*}\",\"token\": \"${*DATABRICKS_API_TOKEN*}\",\"cluster_id\":\"${*DATABRICKS_CLUSTER_ID*}\",\"org_id\": \"${*DATABRICKS_ORG_ID*}\", \"port\": \"${*DATABRICKS_PORT*}\" }" >> /root/.databricks-connectENV *SPARK_HOME*=/usr/local/lib/python3.7/site-packages/pyspark
```

上面的代码人工创建了配置文件，并将其保存在适当的地址。此外，它将`SPARK_HOME`地址设置为正确的值。当您运行`databricks-connect configure`时，这些步骤在没有您参与的情况下被执行。当您想要自动配置连接属性时，您应该这样做。

最后，但同样重要的是，您可以通过运行`databricks-connect test`来检查连接的健康状况。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium—Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)