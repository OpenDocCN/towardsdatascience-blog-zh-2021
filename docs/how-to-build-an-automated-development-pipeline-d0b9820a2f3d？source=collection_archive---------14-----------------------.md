# 如何构建自动化开发管道

> 原文：<https://towardsdatascience.com/how-to-build-an-automated-development-pipeline-d0b9820a2f3d?source=collection_archive---------14----------------------->

## [行业笔记](https://pedram-ataee.medium.com/list/notes-from-industry-265207a5d024)

## 用最小的挫折开发软件的剧本

![](img/978bd9c0d00b19c528d5d2bc0ebd3124.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Manouchehr Hejazi](https://unsplash.com/@patrol?utm_source=medium&utm_medium=referral) 拍摄的照片

在过去的几年里，我领导过许多开发团队。我发现其中的一个共同点是**需要建立一个自动化开发管道。你不一定需要复杂的开发管道。我发现即使是一个基本的管道也可以防止你日常开发中的许多挫折。如果你想开发一个软件产品，即使是一个小玩意，开发管道对你来说也是必不可少的。自动化开发管道帮助您持续评估产品的健全性。**对于开发团队来说，没有什么比破软件更伤人的了。你必须尽你所能去阻止它。我来分享两个你发自内心感受到这种需求的场景。****

*   **场景 1(当您想要保护主分支免于意外错误时)——**您在团队中工作，并且使用 git 存储库来管理开发。每天都有几个拉取请求要合并到主分支。每个人都尽最大努力编写高质量的功能代码，但不时会有错误注入主分支。结果，代码库停止运行，因此，团队变得沮丧。进度变得非常缓慢，最后期限一个接一个地错过。你希望有办法阻止这种事情发生。
*   **场景 2(当您想要加快开发速度时)——**您正在开发一个 web scraper，为一个数据科学项目收集数据。数据科学团队每天都需要大量的数据。在解析机器学习管道中的数据时，您不能承受任何延迟。尽管如此，目标网站中的 HTML 结构不时会发生变化，导致 scraper 停止运行。您希望有一种方法可以自动测试刮刀，并尽快识别错误。

在本文中，我用简单的语言描述了开发管道。我还描述了构建自动化开发管道的最重要的步骤:

*   如何构建和启动**服务器**
*   如何构建**容器化解决方案**
*   如何自动构建容器化的解决方案

我希望这有助于你更好地构建开发管道，远离不必要的挫折。

# 什么是“开发管道”？

开发管道是自动连续执行的一系列命令，用于测试、构建或部署软件产品。这些命令在不同的级别上运行，并使用不同的工具。例如，需要一个服务器来构建开发管道。一个问题是“**如何配置这个服务器？”**您可以手动配置，但最佳实践是使用 Docker 技术。

使用 Docker 技术有很多优势，比如能够在需要时以最少的麻烦从一个服务迁移到另一个*。当您开始构建真实世界的产品时，您会发现能够从一个服务迁移到另一个服务是多么重要。几个月前我也遇到过这种情况。我们被要求从 [GCloud](https://cloud.google.com/) 迁移到 [OVH](https://www.ovhcloud.com/) ，因为该公司在 OVH 基础设施上有一些其他服务。我们只需不到几天的时间就可以将所有内容从 GCloud 迁移到 OVH。想象一下，如果我们手动配置，会发生什么！*

> 开发管道是自动连续执行的一系列命令，用于测试、构建或部署软件产品。

# 如何构建自动化开发管道

构建自动化开发管道需要三个步骤:(a)构建并启动服务器，(b)构建容器化的解决方案，(c)连续设置一系列命令。

![](img/7d124453197eae09a6a10b62841678bd.png)

照片由[泰勒·维克](https://unsplash.com/@tvick?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## —如何构建和启动服务器

启动服务器需要三个步骤:(1) **构建** docker 映像，(2) **存储** docker 映像，(3) **运行** docker 映像。如果你不想有一个自动化的开发管道，你不需要采取这一步。基本上可以在自己的机器上运行。但是，如果您想要设置 CI/CD 工具，您必须为此启动服务器。

**1。构建 Docker 映像—** 您需要启动服务器来构建自动化开发管道。有很多方法可以做到这一点。我的建议是*构建*一个基于 Linux 的 *Docker 镜像*并安装所有需要的库。你可以通过编写一个 *Dockerfile* **来构建一个 Docker 镜像。**docker file 是一个文本文档，包含了所有用于安装所有必需的库和软件包的命令。您可以使用下面的代码基于 Docker 文件构建 Docker 映像。

```
docker **build** —f Dockerfile -t REPOSITORY_NAME/IMAGE_NAME:TAG .
```

要了解如何为服务器编写 Dockerfile，可以阅读本文:[如何创建 Ubuntu 服务器使用 Docker 构建 AI 产品](/how-to-create-an-ubuntu-server-to-build-an-ai-product-using-docker-a2414aa09f59)。

**2。存储一个 Docker 映像—** 然后，你必须*在 *Docker 注册表*中存储*Docker 映像*T21。Docker 注册表基本上是一个存储 Docker 图像的地方，仅此而已。可以选择包括 DockerHub 或者[亚马逊 ECR](https://aws.amazon.com/ecr/) 在内的任何服务。如果你不知道，我推荐使用 DockerHub，因为它是 Docker 命令行界面的原生版本，与其他服务相比，它让你的生活更轻松。在选择注册中心的过程中，会有一些关于安全性和效率的问题，这些问题在现阶段并不重要。例如，如果您有一个复杂的管道，最好使用位于正在使用的其他云服务旁边的注册服务。这使得命令如`docker pull`或`docker push`执行得更快。您可以使用下面的代码将 Docker 映像推送到远程 Docker 存储库。*

```
docker **push** REPOSITORY_NAME/IMAGE_NAME:TAG
```

**3。运行 Docker 镜像—** 你可以在许多服务上运行 Docker 镜像来启动服务器。最后两步是关于如何构建和存储一个 Docker 映像作为服务器使用。这一步是关于如何启动服务器。如果你不想有一个“自动化”的开发管道，你就不需要服务器。

让我分享一个在 CircleCI 上用于启动服务器的代码片段，circle CI 是目前使用的一个著名的 CI/CD 工具。下面的代码是定义要在服务器上运行的测试作业的一部分，该服务器使用 Docker 映像启动。这是 CircleCI 使用的语法，因此其他 CI/CD 工具可能不同，但概念是相同的。启动后，可以运行其他步骤，如`checkout`。`checkout`是 CircleCI 用来在服务器上克隆一个代码库的特殊代码，该代码库是在之前的一个步骤中启动的。如果你想了解更多关于 CircleCI 的内容，我推荐你阅读这篇文章:[如何用简单的话学习 circle ci](/how-to-learn-circleci-in-simple-words-2275e4299628)。

```
jobs:  
  test:
    docker:     
      - image: REPOSITORY_NAME/DOCKERIMAGE_NAME:TAG
        auth: 
          username: $USERNAME
          password: $PASSWORD
    steps:      
      - checkout      
      - run:
          ...
```

![](img/f761bff9cdf99f18980d923c24c9f9bb.png)

照片由[奥拉夫·阿伦斯·罗特内](https://unsplash.com/@olav_ahrens?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## —如何构建容器化的解决方案

构建软件最重要的一步是确保它能工作。你可以把它看作是一个“集成测试”,将单个的模块组合在一起，作为一个组进行测试。因此，如果您正在开发一个数据科学项目，您可能希望构建一个集成测试，如果您正在开发一个 web 应用程序，您可能希望使用 Docker 构建一个容器化的解决方案。

使用 Docker 技术，您可以为服务器构建一个 Docker 映像，并为您的解决方案构建一个 Docker 映像。您在为服务器和解决方案构建 Docker 映像时面临的挑战是相似的；然而，他们有不同的目的。正如你在上面读到的，你必须有一个 docker 文件作为“如何做”的处方来构建你的解决方案。当你有了一个功能 Dockerfile 文件，你就可以进入下一步了。请注意，这些是下一节将解释的开发管道的构建块。

![](img/f66ac51ee1102ba160d482ed27009e54.png)

照片由 [JJ 英](https://unsplash.com/@jjying?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## —如何自动构建容器化解决方案

*开发管道是一系列连续执行的命令。*您可以通过两种方式自动执行一系列命令*:Bash 脚本或 CI/CD 工具。*后者给你更多的选项去配置；但是，前一种可以自己解决很多问题。让我们深入了解这些工具是什么。

**1。Bash 脚本(当你想要构建一个管道时)——**Bash 脚本是一个文本文件，包含一系列命令，当你运行脚本时，这些命令会连续运行。Bash 脚本中的每一行都是您先前在终端中执行的一个命令，以完成特定的任务。Bash 脚本中编写的命令的顺序与您在终端中运行它们的顺序完全相同。下面的代码提示了什么是 bash 脚本。请注意，Bash 脚本在 Windows 和 macOS 上略有不同，这不是本文的重点。您可以使用下面用 Bash 脚本编写的代码来`build` 和`push`Docker 图像。

```
#!/bin/bash 
export IMAGE_NAME=XXX
export USERNAME=XXX
export PASSWORD=XXXdocker login -u $USERNAME -p $PASSWORD
docker build -f Dockerfile -t $IMAGE_NAME .
docker push $IMAGE_NAMEE
```

**2。CI/CD(当你想要建立一个“自动化”的管道时)——**CI/CD(即持续集成/持续部署)工具是一个专门的软件，主要运行在云上，当它们被触发时，以特定的顺序自动执行一系列命令。它们通常使用 YAML 文件进行配置，您可以在其中定义许多“作业”和“工作流”。“作业”是在单个单元中执行的步骤的集合，类似于上面解释的 Bash 脚本。“工作流”是一组规则，用于根据需要按特定顺序运行一组作业。

我强烈建议首先编写一个 Bash 脚本，然后开始配置您选择的 CI/CD 工具。你可能会问为什么？**首先**，您很可能需要 Bash 脚本中编写的所有代码来配置 CI/CD 工具。**其次**，如果你不是专家，设置 CI/CD 工具可能会有点困难。下面的代码是要在 CircleCI 上运行的作业的一部分。正如你所看到的，这些命令与上面的 Bash 脚本完全相同*，但是采用了 CircleCI 可以解析的 YAML 形式*。

```
...
      - run:
          name: Authentication
          command: docker login -u $USERNAME -p $PASSWORD
      - run:
          name: Build 
          command: docker build -f Dockerfile -t $IMAGE_NAME .
      - run:
          name: Store
          command: docker push $IMAGE_NAME
...
```

[](/how-to-learn-circleci-in-simple-words-2275e4299628) [## 如何用简单的话学习 CircleCI

### 作为数据科学家创建 CI/CD 管道的行动手册

towardsdatascience.com](/how-to-learn-circleci-in-simple-words-2275e4299628) 

# —遗言

自动化开发管道是一个复杂的概念，可以以多种形式实现。在这篇文章中，我试图与你分享主要的概念。你肯定需要学习更多来为你的软件产品建立一个有用的管道。**然而，你需要从这篇文章中学到的是，即使是一个小的自动化管道也能帮你很多**。所以，我建议你今天就建立一个自动化的管道。你永远不会后悔！

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *跟我上* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium-Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)