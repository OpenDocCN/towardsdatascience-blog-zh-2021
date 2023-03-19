# 使用 Docker、AWS Lambda 和 API 网关的无服务器模型托管

> 原文：<https://towardsdatascience.com/serverless-model-hosting-with-docker-aws-lambda-and-api-gateway-2d79382ecb45?source=collection_archive---------6----------------------->

## 充分利用 Lambda 中的 Docker 支持来托管您的模型，而无需专用服务器。

![](img/499680fb2999b5075a8ce5d752cc6b82.png)

由 Markus Spiske 在 Unsplash 上拍摄的图片

以前，AWS Lambda 部署包被限制在 250MB(包括需求)的最大解压缩大小。当试图使用该服务托管机器学习模型时，这被证明是一个障碍，因为常见的 ML 库和复杂的模型导致部署包远远超过 250MB 的限制。

然而在 2020 年 12 月，AWS [宣布](https://aws.amazon.com/blogs/aws/new-for-aws-lambda-container-image-support/)支持将 Lambda 函数打包和部署为 Docker 镜像。在机器学习的背景下，这些图像的大小最大可达 10GB。这意味着图像中可以包含大型依赖关系(例如张量流)和中等大小的模型，因此可以使用 Lambda 进行模型预测。

在本文中，我们将通过一个示例来构建和部署 Lambda 上的模型托管。所有使用的相关代码都可以在[这里](https://github.com/jonathanreadshaw/ml-deploy-sam)找到。

**建筑概述**

该解决方案可以分为三个部分:

1.  Docker 映像:Docker 映像包含我们的依赖项、训练好的模型管道和功能代码。AWS 为各种运行时提供了一个基本映像，可以在其上构建以确保与服务的兼容性。
2.  **Lambda Function** :基于输入事件/请求运行 Docker 映像中的功能代码的无服务器资源。
3.  **API 网关端点**:用作 Lambda 函数的触发器和客户端请求的入口点。当在端点接收到预测请求时，Lambda 函数被触发，请求体包含在发送给该函数的事件中。然后，该函数返回的值将作为响应返回给客户端。

**型号**

在这个例子中，我们的模型将是在虹膜分类数据集上训练的简单 KNN 实现。本文不涉及培训，但结果是一个 scikit-learn Pipeline 对象，由以下对象组成:

1.StandardScaler:根据训练样本的平均值和标准偏差标准化输入。

2.KNeighborsClassifier:实际的预训练模型。用 K = 5 训练。

使用 scikit-learn 的 Joblib 实现将管道保存到' *model_pipeline.joblib* '中。

**功能代码**

让我们首先考虑 Lambda 将用来处理预测请求事件的函数代码(predict\app.py)。

*Lambda_handler* 拥有 Lambda 使用的函数所需的参数。管道对象在处理程序之外加载，以避免在每次调用时加载。Lambda 会让容器在一段时间内保持活动状态，直到没有事件发生，所以在创建时加载一次模型意味着它可以在容器被 Lambda 保持活动状态时被重用。

处理函数本身非常简单；从事件主体中提取所需的输入，并用于生成预测。预测作为 JSON 响应的一部分返回。API 网关会将函数的响应返回给客户端。进行一些检查以确保输入符合预期，并捕捉和记录任何预测错误。

**码头工人图像**

Dockerfile 文件的结构如下:

1.拉 AWS 的基本 Python 3.6 图像

2.将所需文件从本地目录复制到映像的根目录

3.安装要求

4.运行处理函数

**部署**

为了管理部署和 AWS 资源，我们将使用 AWS 无服务器应用程序管理器(SAM) CLI *。*安装 SAM 及其依赖项的说明可以在[这里](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)找到。

要使用 SAM 进行构建和部署，必须配置模板文件。这用于指定所需的 AWS 资源和相关配置。

对于此项目，SAM 模板包含以下内容:

*   杂项信息，如堆栈名称和 Lambda 超时的全局配置。
*   *MLPredictionFunction* 资源:这是我们要部署的 Lambda 函数。本节包含所需配置的大部分内容:
*   属性:这里我们指定将使用 Docker 映像( *PackageType: Image* )来定义该函数，并且该函数将通过 API 网关( *Type: API* )来触发。API 路径路由名称和类型也在这里定义。
*   元数据包含将用于构建图像的标签和用于构建图像的 docker 文件的位置/名称。
*   Outputs 列出了将由 SAM 创建的所有必需资源。在这种情况下，SAM 需要的是 API 网关端点、Lambda 函数和关联的 IAM 角色。

运行以下命令，使用定义的 SAM 模板在本地构建应用程序映像:

```
!sam build
```

如果成功，可以使用以下命令通过示例事件在本地调用该函数(参见 repo 中的示例事件):

```
!sam local invoke -e events/event.json
```

一旦在本地测试了该功能，就需要将图像推送到 AWS ECR。首先创建一个新的存储库:

```
!aws ecr create-repository --repository-name ml-deploy-sam
```

在推送映像之前，您需要登录 ECR 的托管 Docker 服务:

```
!aws ecr get-login-password --region <region> | docker login --username AWS \ --password-stdin <account id>.dkr.ecr.<region>.amazonaws.com
```

现在，您可以使用以下工具部署应用程序:

```
!sam deploy -g
```

这将在“引导”模式下运行部署，您需要确认应用程序的名称、AWS 区域和之前创建的图像存储库。在大多数情况下，接受其余设置的默认选项应该没问题。

然后将开始部署过程，并提供 AWS 资源。完成后，每个资源都将显示在控制台中。

## 考虑

*   **映像更新**:要部署更新的模型或功能代码，您可以简单地在本地重建映像并重新运行 deploy 命令。SAM 将检测应用程序的哪些方面发生了变化，并相应地更新相关资源。
*   **冷启动:**每次 Lambda 使用我们的功能代码旋转一个容器时，模型将在处理开始前被加载。这导致冷启动场景，其中第一个请求将比后面的请求慢得多。解决这个问题的一个方法是使用 CloudWatch 定期触发函数，这样容器就可以随时加载模型。
*   **多个功能:**可以部署多个 Lambda 功能，由一个 API 提供服务。如果您有多个模型要服务，或者如果您想要有一个单独的预处理/验证端点，这可能是有用的。要对此进行配置，您只需在 SAM 模板资源中包含附加功能。