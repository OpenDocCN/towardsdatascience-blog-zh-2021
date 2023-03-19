# 如何创建一个 Ubuntu 服务器来使用 Docker 构建一个 AI 产品

> 原文：<https://towardsdatascience.com/how-to-create-an-ubuntu-server-to-build-an-ai-product-using-docker-a2414aa09f59?source=collection_archive---------9----------------------->

## [业内笔记](https://pedram-ataee.medium.com/list/notes-from-industry-265207a5d024)

## 构建 Ubuntu 服务器的代码片段集合，该服务器可用于开发简单的人工智能产品。

![](img/e5b39ce18d2075859bb3c2374981d6a8.png)

凯文·霍尔瓦特在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

要在云上部署一个人工智能引擎，你很可能需要运行一个虚拟的基于 Linux 的服务器，上面有基本的软件和库。启动服务器的一个标准方法是创建一个满足所有需求的 docker 映像。您可以在作为服务器的任何机器上使用相应的 docker 映像运行 docker 容器。一般来说，构建 AI 产品的标准服务器必须有几个基本的软件和库，包括 Python、Java、Git、Docker 和 GCloud。例如，每当 AI 模型更新时，您需要构建一个新的 docker 映像；所以`docker`必须安装。然后，你需要在云上部署带有新 AI 模型的新 docker 镜像；所以，例如，`gcloud` 和`kubectl` 必须安装。

在这篇文章中，我想分享如何在 Ubuntu 机器上安装 essentials 来构建一个 AI 产品。在这里，您可以找到**一组代码片段，这些代码片段将在 order 文件中使用，以便构建一个 Ubuntu 服务器。**我强烈建议创建一个专门的`Dockerfile` 来为你的项目量身定制，并确保它安全可靠。这有助于您在需要时启动服务器。希望这能大大加快你的发展。

## 一台 Ubuntu 机器

先说 Ubuntu 20.04。首先，你必须更新为 Ubuntu(以及 Linux 的其他主要发行版)设计的高级包管理器`apt-get`。然后，`git`必须安装软件，让你在需要的时候`checkout`使用你的代码库。下面的代码将是 docker 文件中的第一行。

```
FROM ubuntu:20.04
RUN apt-get update && apt-get -y install sudo
RUN sudo apt-get -y install software-properties-common### INSTALL GIT
RUN sudo apt-get -y install git
```

## —如何在 Ubuntu 上安装 PYTHON 3.7

您可以使用下面的代码在 docker 机器上安装一个干净的 Python 3.7。请注意，无论如何您都需要安装项目所需的库。在这里，我试图保持简单和干净。

```
### INSTALL PYTHON
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
RUN alias python=python3
```

## —如何在 Ubuntu 上安装 DATABRICKS-CONNECT

如果您处理真实世界的数据，您可能需要处理大量的数据，这些数据在小型 VPS 或强大的专用裸机服务器上无法轻松管理。Databricks 推出了一项非常有用的服务，可以让您连接到高度优化的 Databricks 集群进行大数据处理。他们强大的 Spark 机器可以让你在几秒钟内构建大型人工智能模型。为此，您必须通过名为`databricks-connect`的库连接到 Databricks 集群。您可以使用下面的代码在 Ubuntu 上安装`databricks-connect`。要了解更多关于 Databricks 集群逐步配置的信息，请查看本文:[如何将本地或远程机器连接到 Databricks 集群](/how-to-connect-a-local-or-remote-machine-to-a-databricks-cluster-18e03afb53c6)。

```
### INSTALL JAVA
RUN sudo add-apt-repository ppa:openjdk-r/ppa
RUN sudo apt-get install -y openjdk-8-jre### INSTALL DATABRICKS-CONNECT
RUN pip3 install --upgrade pip
RUN pip3 uninstall pyspark
RUN pip3 install -U databricks-connect==7.3.*
```

</how-to-connect-a-local-or-remote-machine-to-a-databricks-cluster-18e03afb53c6>  

## —如何在 Ubuntu 上安装 DOCKER 和 DOCKER-COMPOSE

假设你正在开发一个运行人工智能引擎的 API 服务。每次人工智能模型更新时，你必须建立一个新的解决方案。在这种情况下，您需要在您的机器上安装 docker 服务，让您运行诸如`docker build`或`docker push`之类的命令。你可以运行下面的代码让你在 Ubuntu 机器上安装 docker。

```
### INSTALL DOCKER
RUN apt-get update && apt-get install -y curl
RUN curl -fsSL https://get.docker.com -o get-docker.sh
RUN sudo sh get-docker.sh## INSTALL DOCKER-COMPOSE
RUN sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
RUN sudo chmod +x /usr/local/bin/docker-compose
```

## —如何在 Ubuntu 上安装 GCLOUD 和 KUBECTL

如果你想为你的产品使用谷歌云服务，你必须安装`gcloud`来与谷歌基础设施通信。例如，如果您想使用他们的容器注册服务来存储项目期间构建的 docker 映像，您必须安装`gcloud`库。另外，如果您想使用强大的 Kubernetes 引擎在这个基础设施上部署您的解决方案，您必须安装`kubectl`。请注意，您肯定可以使用其他服务，如 AWS 或 Azure，它们需要自己的配置。

```
### INSTALL GCLOUD
RUN curl https://dl.google.com/dl/cloudsdk/release/google-cloud-sdk.tar.gz > /tmp/google-cloud-sdk.tar.gz
RUN mkdir -p /usr/local/gcloud \
    && tar -C /usr/local/gcloud -xvf /tmp/google-cloud-sdk.tar.gz \   
    && /usr/local/gcloud/google-cloud-sdk/install.sh
ENV PATH $PATH:/usr/local/gcloud/google-cloud-sdk/bin### INSTALL KUBECTL
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s [https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl](https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl)
RUN chmod +x ./kubectl
RUN mv ./kubectl /usr/local/bin
```

## —如何安装节点。Ubuntu 上的 JS

如果你想为你的 AI 引擎创建一个简单的用户界面，你很可能需要安装 Node。我强烈建议保持它的干净和明亮，以确保以后不会有记忆问题。

```
### INSTALL NODE.JSENV NODE_VERSION=12.6.0
RUN apt install -y curl
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"
RUN node --version
RUN npm --version
```

## —遗言

您可能会在上面的代码中发现一些冗余。例如，我可能会多次更新`apt-get`包管理器。我这样做是为了确保每个部分都可以独立执行。最后但同样重要的是，我花了一些时间才找到如何在 Ubuntu 机器上安装必要的软件和库。希望这能加快你的发展。

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我* [*推特*](https://twitter.com/pedram_ataee) *！*

<https://pedram-ataee.medium.com/membership> 