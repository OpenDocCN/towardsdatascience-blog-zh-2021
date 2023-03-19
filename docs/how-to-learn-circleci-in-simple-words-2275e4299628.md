# 如何用简单的话学习 CircleCI

> 原文：<https://towardsdatascience.com/how-to-learn-circleci-in-simple-words-2275e4299628?source=collection_archive---------22----------------------->

## [行业笔记](https://towardsdatascience.com/tagged/notes-from-industry)

## 作为数据科学家创建 CI/CD 管道的行动手册

![](img/2c79ad02f95214f7078fb2518330ceec.png)

照片由 [Alexas_Fotos](https://unsplash.com/@alexas_fotos?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在过去的几年里，我开发了几个软件产品。其中常见的一个关键步骤是构建一个管道来**测试**、**构建**，以及**部署**。无论你是建立一个人工智能产品还是 Web 应用程序，你都需要一个持续集成和部署(CI/CD)管道。拥有这条管道有助于您构建高质量的软件，并加快开发过程。有许多 CI/CD 工具，如 CircleCI、TeamCity 和 Jenkins。在本文中，我用简单的语言描述了**如何用 CircleCI** 建立一个简单的 CI/CD 管道，并为您提供了您必须使用它的**场景**。首先，让我从我如何熟悉软件开发中的这个重要概念开始。

> **当时的 MLOps 方案并不多。**因此，我们开始使用现有的通用工具创建自己的 MLOps 解决方案。

我在 2013 年开发了一个基于 ML 的手势识别引擎。我们需要一个自动化的管道来构建、测试和归档 ML 模型，以确保质量并加快开发过程。**当时的 MLOps 解决方案并不多。因此，我们开始使用现有的通用工具创建自己的 MLOps 解决方案。我们开始与团队合作。那时候， [Jenkins](https://www.jenkins.io/) 是在 DevOps 团队中建立 CI/CD 渠道最流行的选择。然而，我们是一个小型的机器学习团队，没有太多关于 DevOps 和 Jenkins 的经验，这不是最简单的解决方案！我们发现，与 Jenkins 相比，TeamCity 提供了一个用户友好且易于设置的环境。因此，我们使用 TeamCity 创建了一个自动化的管道来构建、测试和归档 ML 模型。**

几年后，我发现了 CircleCI 并爱上了它。你可能会问为什么？因为它非常强大，而且简单直观。**在这里，我想帮助您建立 CircleCI 的 CI/CD 渠道。**您可能希望使用 CircleCI 构建自己的 MLOps 解决方案，或者构建一个自动化管道来构建和部署 Web 应用。希望这能帮助你更好地适应这项神奇的技术。

![](img/9606af31fcd533a9386ff01ed53d8c2f.png)

照片由[德米特里·拉图什尼](https://unsplash.com/@ratushny?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# CircleCI 的“工作”是什么？

“作业”是在单个单元中执行的步骤的集合。您可以出于各种目的创建工作。例如，您可以为*集成测试*创建一个作业，并为*性能测试*创建另一个作业。您还可以创建一个用于构建解决方案的作业和另一个用于部署该解决方案的作业。一个作业有三个主要部分:(a) **执行者** , (b) **环境变量** , (c) **步骤。**这里有更多关于创建工作的细节，你可以在这里找到[。](https://circleci.com/docs/2.0/configuration-reference/#jobs)

## 执行者

必须确定作业执行者(即运行作业的机器)。您可以使用 Docker 容器或虚拟机(Linux 或 Windows)。我通常为作业执行者使用 Docker 容器，因为我对它们有更多的控制权。使用 Docker 技术有助于我减少对服务的依赖，并能够在需要时迁移到其他服务。

## 环境变量

您必须确定该作业所需的所有环境变量。这里定义的环境变量可用于所有步骤。您还可以在某个步骤中定义环境变量，该步骤专用于该步骤。如果你想成为软件开发专家，你应该彻底学习如何定义和使用环境变量。

## 步伐

最后，在前面的步骤中设置了环境变量之后，您必须确定需要运行的步骤。例如，如果 CircleCI 和 Github 连接正确，可以使用`checkout`命令将代码库复制到执行器中。您可能还需要运行 CircleCI 团队预先构建的特殊步骤。例如，如果没有特殊的配置，就不能在 docker 容器中构建 docker 映像。CircleCI 让您的生活变得轻松！您只需要在`checkout`之后添加一个名为`setup_remote_docker`的新步骤。要了解更多关于如何在 docker 容器中运行 docker 命令的信息，你可以在这里阅读。

```
version: 2.1 jobs:
  build:
    **docker**:
      - image: circleci/<language>:<version TAG>
        auth:
          username: $USERNAME
          password: $PASSWORD
    **environment**:      
      - USERNAME: my-dockerhub-user
      - PASSWORD: my-dockerhub-password
    **steps**:
      - checkout
      - run: echo "this is the build job"
```

# CircleCI 中的“工作流”是什么？

“工作流”是一组规则，用于根据需要按特定顺序运行一组作业。例如，您可以定义一个简单的工作流来为开发过程中的每个提交运行一个作业(通常是单元测试)。您还可以定义一个更复杂的工作流来构建一个在“构建”作业之前需要“测试”作业的解决方案。工作流中使用了几个重要的操作来设置所需的规则，例如(a) **过滤器**、( b) **要求**和(c) **触发器。**这里有关于创建工作流的更多细节，你可以在这里找到[。](https://circleci.com/docs/2.0/workflows/)

下面，您可以找到一些需要设计特殊工作流来满足需求的场景。

*   **场景 1 (** 当您需要**一个特定分支的作业)** —您希望只在名称以“feature”开头的分支上运行构建作业。您应该过滤分支，只找到那些名称以目标名称开头的分支。在这种情况下，可以使用`filters`命令。
*   **场景 2** **(** 当你需要**一个有序的作业系列)** —你想按顺序设置一系列作业。例如，当一个新的拉请求被合并到主分支中时，您想要在 web 上自动部署一个解决方案。在部署作业开始之前，必须彻底运行构建作业。在这种情况下，可以使用`requires`命令。

```
Workflow:
  jobs:
    - build:
        **filters**:
          branches:
            only:
              - /feature\/.*/
    - deploy:
        **requires**:
          - build
```

*   **场景 3** (当您需要一个事件在午夜**被触发**)——您想要使用最新的开发在每天午夜创建一个 ML 模型。您还想在那个时候自动创建一个测试报告，并在第二天的清晨提供给团队。因此，您希望“train_test”作业在夜间运行。在这种情况下，可以使用`triggers`命令。[阅读更多](https://circleci.com/docs/2.0/workflows/#nightly-example)。

```
workflow:
  version: 2
  nightly:
    **triggers**:
      - schedule:
          cron: "0 0 * * *"
          filters:
            branches:
              only:
                - master
                - beta
  jobs:
    - train_test
```

*   **场景 4** (当您需要一个事件**由一个拉请求**触发时)——您希望确保没有错误注入到主分支中。因此，您不仅希望在每次提交时运行测试作业，还希望在拉请求时运行构建作业。很抱歉。**在代码**上做不到。您应该在 CircleCI 高级设置面板上更改配置。[阅读更多。](https://circleci.com/docs/2.0/oss/#only-build-pull-requests)

# —遗言

我强烈建议从项目一开始就使用 CircleCI，尤其是在开发基于云的服务时。他们提供令人敬畏的免费服务，可以长期满足个人项目的要求。如今，您可以将 MLflow 等专门的 MLOps 解决方案用于数据科学项目。但这并不意味着他们可以取代 CircleCI。2013 年，我们使用类似于 CircleCI 的服务创建了自己的 MLOps 解决方案。但这并不意味着你应该这样做！

# 感谢阅读！

如果你喜欢这个帖子，想支持我…

*   *跟我上* [*中*](https://medium.com/@pedram-ataee) *！*
*   *在* [*亚马逊*](https://www.amazon.com/Pedram-Ataee/e/B08D6J3WNW) *上查看我的书！*
*   *成为* [*中的一员*](https://pedram-ataee.medium.com/membership) *！*
*   *连接上*[*Linkedin*](https://www.linkedin.com/in/pedrama/)*！*
*   *关注我的* [*推特*](https://twitter.com/pedram_ataee) *！*

[](https://pedram-ataee.medium.com/membership) [## 通过我的推荐链接加入 Medium-Pedram Ataee 博士

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

pedram-ataee.medium.com](https://pedram-ataee.medium.com/membership)