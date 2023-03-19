# 使用 Docker 在 EC2 + CI/CD 上运行气流，使用 GitLab

> 原文：<https://towardsdatascience.com/running-airflow-with-docker-on-ec2-ci-cd-with-gitlab-72326d4baeb4?source=collection_archive---------2----------------------->

## 使用基本命令创建简单的数据管道，并运行您自己的基于云的管道！

![](img/7db376d7231093d0347cfdda51cb8944.png)

约书亚·阿拉贡在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我已经学习数据科学一年了，但直到几周前，我仍然发现自己对工程方面完全一无所知。数据分析和人工智能是金字塔顶端的东西，需要强大的数据管道基础。在这篇文章中，我将分享我在尝试使用开源编排工具 Airflow 构建简单数据管道时学到的经验。这个 orchestrator 部署在 Amazon EC2 之上，利用 GitLab 的 CI/CD 来简化开发过程。好的，一个接一个，我们来谈谈每个术语的定义。

## 气流

基于其网站，Airflow 是一个由社区创建的“*平台，用于以编程方式创作、安排和监控工作流*数据工程师通常使用气流来协调工作流程和管道。假设一名数据工程师每天需要将数据从一个源移动到另一个目的地四次(例如，为了保持数据的新鲜)。他们可以使用 Airflow 自动运行脚本，按照特定的时间表运行，而不是手动完成该任务。Airflow 还可以用于管理依赖关系、监控工作流执行和可视化工作流。

气流需要安装在机器上。与其使用本地电脑，不如安装在基于云的主机上，提高可靠性。

## **亚马逊 EC2**

Amazon EC2(代表弹性计算云)是由 Amazon Web Services 提供的基于云的租用计算机。EC2 将充当主机，气流将在其上部署。EC2 只是一台计算机，就像我们本地的笔记本电脑一样，但是这台计算机 99%的时间都是运行的，由 AWS 团队管理。我们可以通过使用 SSH 在 EC2 机器的命令行界面中“挖隧道”。稍后，我们将学习如何使用 SSH 和 git 将代码从远程存储库部署到 EC2。

## 码头工人

Airflow 不会直接安装在 EC2 本地机器上，而是安装在 Docker 容器内部。Docker 是一组创建虚拟操作系统(称为容器)的平台。我们使用 Docker 来减少设置，因为所有设置都将由 Docker 映像的维护者(在本例中是气流贡献者)来管理。Docker 映像包含**应用程序代码、库、工具、依赖项和其他运行应用程序所需的文件**。我们将从 Docker image registry 中提取一个图像，构建它，启动容器，并在里面运行 Airflow。

## GitLab

GitLab 将被用作我们的存储库和 DevOps 生命周期工具。实际上，在 GitLab 中有无数种方法可以利用 CI/CD，但为了简单起见，我们将只使用“CD”部分。每次用户将任何分支合并到主分支时，都会触发部署脚本。部署管道由进入我们的 EC2 的 SSH 隧道组成，将主分支从 GitLab 存储库拉入 EC2，将几个变量写入一个文件，并重建+重启 Docker 容器。

在深入细节之前，请查看[这个库](https://github.com/ammarchalifah/airflow-pipeline)，它包含了完整的代码库和我们将成功创建一个持续的气流部署管道的步骤摘要。好了，我们开始吧！

# 安装

## EC2

访问 AWS 控制台，然后准备一个 EC2 实例。我推荐在 ubuntu OS 上使用 instance，性能中等偏下。下载 EC2 实例的 SSH 密钥(通常以`.pem`后缀结尾)，并记下实例的 IP 地址。然后，将您的实例配置为打开所有端口(至少打开端口 8080 以访问 Airflow 服务器)。最后，通过使用 SSH 键从命令行界面安装 Docker 和 docker-compose 来访问 EC2。

```
ssh -i ssh-key.pem ubuntu@EC2_ADDRESS
```

【https://www.youtube.com/watch?v=YB_qanudIzA】EC2 条款指南:

## GitLab

在 git lab([https://gitlab.com](/gitlab.com))上注册并创建一个新项目。将内容从 [github 仓库](https://github.com/ammarchalifah/airflow-pipeline)克隆到我们新的 GitLab 仓库。在用代码填充存储库之后，我们需要配置 GitLab runner 并设置存储库变量。GitLab runner 用于执行我们在`gitlab-ci.yml`中定义的 CI/CD 脚本。存储库变量用于存储敏感信息(如用户名、密码、EC2 的 IP 地址和 EC2 的 SSH 密钥)。存储库变量作为环境变量加载到 CI/CD 管道中，在那里它可以作为变量/文件部署到服务器。

![](img/1ee2b031a8036307edffa8863ac5e451.png)

GitLab 知识库的接口。图片作者。

按照 [GitLab 转轮配置指南](https://docs.gitlab.com/runner/install/)中的说明配置我们的转轮。对于我来说，我使用 EC2 实例和 ubuntu OS 来安装我的 runner。如果您通过提供您的信用卡信息激活 GitLab 的免费试用，您就不必配置自己的跑步者，因为 GitLab 会为您提供一个共享的跑步者。

**GitLab 储存库变量**

![](img/09060a015fbeca89b231a79d92faa878.png)

存储库变量—按作者分类的图像。

我们需要设置六个存储库变量。进入**设置> CI/CD >变量**，点击**展开**，开始添加变量。

前两个是 _AIRFLOW_WWW_USER_PASSWORD 和 _AIRFLOW_WWW_USER_USERNAME，它们将用于在部署 AIRFLOW 服务器后登录到该服务器。为这两个变量选择任意值。

第二对是 EC2_ADDRESS(您的 EC2 实例的 IP 地址)和 SSH_KEY_EC2(您的 EC2 实例的 SSH key，通常以`.pem`后缀结尾)。这些值将用于访问您的 EC2 机器。将 SSH_KEY_EC2 设置为文件而不是变量。

最后一对是 GITLAB_PASSWORD 和 GITLAB_USERNAME，用于从 GITLAB 存储库中提取提交的凭证。

# 代码解释

从下面的目录结构开始。

![](img/e6e4f5b319ee7a20af25b20ba73f8cae.png)

这个项目的目录结构-作者图片。

三个目录(Dag、日志和插件)是基本的气流目录。我们让日志和插件(几乎)为空，因为这些文件夹将在部署后被填满。我们希望自动化的脚本写在 Dag 内部，我们将在后面介绍。

在对 GitLab 的回购进行任何推送的过程中，都会触发`.gitlab-ci.yml`(定义 CI/CD 管道)中的脚本。该脚本执行一系列命令来将存储库部署到 EC2。在 EC2 内部，我们将通过在`docker-compose.yaml`上使用`docker-compose up`来构建包含气流的 Docker 容器。

## CI/CD 管道

在变得更复杂之前，请先参考[本指南](https://docs.gitlab.com/ee/ci/quick_start/)了解什么是 GitLab 的 CI/CD。

CI/CD 管道在`.gitlab-ci.yml`中定义。

因为我使用的 GitLab runner 使用 Docker 执行管道，所以默认图像在顶部指定。我使用`ubuntu:latest`作为我的默认 Docker 图像。

如前所述，我们不会利用 CI/CD 的持续集成方面。我们只使用仅在主分支中触发的部署。这意味着所有推送到 main 之外的其他分支不会导致部署。

```
deploy-prod:
  only:
   - main
  stage: deploy
  before_script: ...
  script: ...
```

`deploy-prod`是我们管道的名称，它将只在主分支中运行。这个管道的阶段是部署阶段(与测试或构建等其他阶段不同)。命令在`before_script`和`script`中指定。

```
before_script:
- ls -la
- pwd
- 'which ssh-agent || ( apt-get update -y && apt-get install openssh-client -y )'
- eval $(ssh-agent -s)
- mkdir -p ~/.ssh
- chmod 700 ~/.ssh
- cat $SSH_KEY_EC2
- echo "$(cat $SSH_KEY_EC2)" >> ~/.ssh/ssh-key.pem
- chmod 400 ~/.ssh/ssh-key.pem
- cat ~/.ssh/ssh-key.pem
- echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config
- apt-get update -y
- apt-get -y install rsync
```

`before_script`由几个命令组成，这些命令让我们的跑步者为部署做好准备。这些命令用于安装 open-ssh、评估 ssh-agent、将 ssh 密钥从存储库变量转储到文件、更改 ssh 密钥的权限、设置 SSH 配置以及安装 rsync。

```
ssh -i ~/.ssh/ssh-key.pem ubuntu@$EC2_ADDRESS \
'git config --global --replace-all user.name "Full Name"
&& git config --global --replace-all user.email "email@email.com"
'
```

`script`部分分为三个部分。每个部分都是从进入 EC2 的 ssh 隧道开始的。第一部分用于设置 git config，使用您的全名和电子邮件作为全局配置。

```
ssh -i ~/.ssh/ssh-key.pem ubuntu@$EC2_ADDRESS \
'
if [ -d airflow-pipeline ];
then cd airflow-pipeline
&& git status
&& git restore .env
&& git pull --rebase
&& git status
&& sed -i s:%AIRFLOW_UID%:'"$(id -u)"':g .env
&& sed -i s:%AIRFLOW_GID%:0:g .env
&& sed -i s:%_AIRFLOW_WWW_USER_USERNAME%:'"$_AIRFLOW_WWW_USER_USERNAME"':g .env
&& sed -i s:%_AIRFLOW_WWW_USER_PASSWORD%:'"$_AIRFLOW_WWW_USER_PASSWORD"':g .env
else git clone https://'"$GITLAB_USERNAME"':'"$GITLAB_PASSWORD"'@gitlab.com/gitlab-group/gitlab-project.git
&& cd airflow-pipeline
&& ls -la
&& chmod 777 logs
&& sed -i s:%AIRFLOW_UID%:'"$(id -u)"':g .env
&& sed -i s:%AIRFLOW_GID%:0:g .env
&& sed -i s:%_AIRFLOW_WWW_USER_USERNAME%:'"$_AIRFLOW_WWW_USER_USERNAME"':g .env
&& sed -i s:%_AIRFLOW_WWW_USER_PASSWORD%:'"$_AIRFLOW_WWW_USER_PASSWORD"':g .env
&& docker-compose up airflow-init;
fi'
```

第二部分用于从 GitLab 存储库中的主分支提取变更，并将环境变量写入`.env`文件。这里我们使用基于 EC2 主目录上目录存在的条件。如果我们之前已经克隆了 GitLab 存储库，我们只执行了:从`.env`文件中恢复所有更改，从远程 GitLab 存储库中提取更改，并将环境变量写入`.env`。如果还没有，我们首先克隆存储库，然后将环境变量写入`.env`文件，并启动 Docker 容器。

```
ssh -i ~/.ssh/ssh-key.pem ubuntu@$EC2_ADDRESS \
'cd airflow-pipeline &&
if [ docker ps | grep -q keyword ];
then docker-compose down && docker-compose up -d --build;
else docker-compose up -d --build;
fi;'
```

最新的部分用于重新启动 Docker 以合并新的更改。

可以在 GitLab 的 **CI/CD >管道**上监控 CI/CD 管道。

![](img/2af43360bd2f4306a976e43cc78e89b8.png)

GitLab CI/CD 管道—图片由作者提供。

如果一切顺利，你的气流网络服务器将准备就绪。在浏览器上点击`https://{EC2_ADDRESS}:8080`进入用户界面。使用之前在存储库变量上设置的 Airflow 用户名和密码登录。

![](img/1c59d3efdafbbcf44b15f1a4597fefca.png)

Airflow webserver 登录页面—作者图片。

![](img/bc8dfa7fe40d23c3d38f5b15101a69c4.png)

Airflow webserver 的界面—图片由作者提供。

稍后，您希望自动化的任何脚本都可以写在 DAG(或*有向无环图，*没有有向循环的有向图)中。DAG 内的任务被框定为图的节点，其中可以定义依赖性。**存储库中的任何更改都可以通过将更改推送到 GitLab 存储库、合并到主分支(或直接推送到主分支)并等待 CI/CD 管道成功来部署。**

您可以在 DAG 中定义许多东西，例如数据接收任务、数据查询任务、发送自动电子邮件、抓取数据、进行 API 调用等。

![](img/200cd78a9fe47a27f85b3ca162cec8b3.png)

DAG 中的依赖关系可视化—按作者排序的图像

![](img/a939e6bd296ed74d1b8e2c09a73cff17.png)

DAG 中的任务监控—按作者排列的图像

## 为什么指定。gitignore？

不建议在本地记录气流活动。然而，由于时间和成本的限制，我在本地记录所有的活动，尤其是在`/logs`目录下。除了日志，DAG 执行还会在`/dags/__pycache__/`中创建缓存。因此，我通过使用`.gitignore`忽略了那些文件，以安全地提取更改，而不会导致冲突。

## EC2 检验

如果出现任何问题，您总是可以通过使用 SSH 键来检查 EC2 内部发生了什么。

```
ssh -i ssh-key.pem ubuntu@EC2_ADDRESS
```

删除未使用的文件，未使用的 Docker 图像，或从内部进行任何调整！

## **参考文献**

[1][https://airflow.apache.org/](https://airflow.apache.org/)
【2】[https://aws.amazon.com/ec2/](https://aws.amazon.com/ec2/)
【3】[https://www.docker.com/](https://www.docker.com/)
【4】[https://about.gitlab.com/](https://about.gitlab.com/)
【5】[https://air flow . Apache . org/docs/Apache-air flow/stable/start/docker . html](https://airflow.apache.org/docs/apache-airflow/stable/start/docker.html)
【6】Bunga Amalia K 关于设置 GitLab runner 的代码参考