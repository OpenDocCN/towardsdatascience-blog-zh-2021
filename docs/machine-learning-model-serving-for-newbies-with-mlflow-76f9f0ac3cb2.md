# 基于 MLflow 的面向新手的机器学习模型

> 原文：<https://towardsdatascience.com/machine-learning-model-serving-for-newbies-with-mlflow-76f9f0ac3cb2?source=collection_archive---------10----------------------->

## 为您的 sklearn 模型构建一个 API 的完全可复制的、分阶段的、循序渐进的教程

![](img/608c600154a9b5801a8b683fe7fd665b.png)

保持那些模型流动！(作者画的漫画很差。)

一个机器学习中的常见问题是构建机器学习模型的数据科学家和试图将这些模型集成到工作软件中的工程师之间的笨拙交接。数据科学家所熟悉的计算环境并不总能很好地融入生产质量系统。

这个模型部署问题已经变得如此普遍，以至于围绕寻找解决方案出现了一个全新的术语和子领域——MLOps，或者将 DevOps 原则应用于将机器学习管道推向生产。

# 模型部署的简单方法

我最喜欢的机器学习模型部署工具是 [MLflow](https://www.mlflow.org/) ，它自称是“管理 ML 生命周期的开源平台，包括实验、可复制性、部署和中央模型注册。”MLflow 可以与您可能用于机器学习的几乎所有编程语言一起工作，可以在您的笔记本电脑上或云中以相同的方式轻松运行(在 Databricks 中集成了一个非常棒的托管版本)，可以帮助您对模型进行版本控制(特别适合协作)并跟踪模型性能，并允许您打包几乎任何模型并提供它，以便您或任何其他人可以使用它通过 REST API 发送自己的数据来进行预测，而无需运行任何代码。

如果你觉得这听起来很酷，那是因为它确实很酷。MLflow 太酷了，做了这么多事情，以至于我花了很长时间来筛选所有的文档和教程，以弄清楚如何实际使用它。但是我做了(我想)，我用 Docker 做所有的事情，所以我想我应该继续下去，在混乱中添加另一个教程。因为都是用 Docker 容器化的，所以只需要几个命令就可以让一切正常工作。

# 佐料

所有资料都可以在我的 GitHub[ml flow-tutorial repo](https://github.com/mtpatter/mlflow-tutorial)中获得。接下来，将 repo 克隆到您的本地环境中。您可以在系统上仅运行 Docker 和 Docker Compose 来运行该示例。

这个 GitHub repo 演示了一个用 sklearn 训练分类器模型和用 mlflow 服务模型的例子。

回购有几个不同的组成部分:

*   一个 [Jupyter 笔记本](https://github.com/mtpatter/mlflow-tutorial/blob/main/mlflow-sklearn.ipynb)浏览管道，包括使用 Python API 加载注册的模型
*   一个[Docker 文件](https://github.com/mtpatter/mlflow-tutorial/blob/main/Dockerfile)，可用于构建本教程的 Docker 映像
*   两个 Docker 使用 mlflow 注册表编写文件来运行整个训练到服务管道[和](https://github.com/mtpatter/mlflow-tutorial/blob/main/docker-compose.yml)或[而不使用](https://github.com/mtpatter/mlflow-tutorial/blob/main/docker-compose-no-registry.yml)
*   两个示例 sklearn 分类器模型训练脚本使用 mlflow 注册表构建带有的 mlflow 模型[或不带](https://github.com/mtpatter/mlflow-tutorial/blob/main/clf-train-registry.py)的 ml flow 模型
*   几个示例 shell 脚本，用于[运行注册中心的 mlflow 服务器](https://github.com/mtpatter/mlflow-tutorial/blob/main/runServer.sh)，[服务于指定的模型](https://github.com/mtpatter/mlflow-tutorial/blob/main/serveModel.sh)，以及[使用您的测试数据的 csv 进行预测](https://github.com/mtpatter/mlflow-tutorial/blob/main/predict.sh)。

# 方向(TLDR 版)

要使用 Docker Compose 跳过并运行所有组件，您可以使用注册表运行
整个教程:

```
docker compose -f docker-compose.yml up --build
```

或者没有注册表:

```
docker compose -f docker-compose-no-registry.yml up --build
```

该模型将在端口 1234 上提供，通过运行包含测试数据 csv 的以下脚本来访问预测:

```
./predict.sh test.csv
```

它运行以下 curl 命令，将 csv 作为输入:

```
curl http://localhost:1234/invocations \
-H ‘Content-Type: text/csv’ --data-binary @test.csv
```

并返回预测概率的数组。

# 方向(完整版)

第一部分将 mlflow 模型保存到本地磁盘，第二部分展示了如何使用 mlflow registry 进行模型跟踪和版本控制。

# 培训和服务 mlflow 模型(无注册)

`[clf-train.py](https://github.com/mtpatter/mlflow-tutorial/blob/main/clf-train.py)`脚本使用 sklearn 乳腺癌数据集，训练一个简单的随机森林分类器，用 mlflow 将模型保存到本地磁盘。添加用于写入输出测试数据的可选标志将首先分割训练数据以添加示例测试数据文件。

若要训练模型，请使用以下命令:

```
python clf-train.py clf-model --outputTestData test.csv
```

下面是主脚本的代码片段。最后一行将模型组件保存到本地的`clf-model`目录中。

通过运行以下命令为模型提供服务:

```
mlflow models serve -m clf-model -p 1234 -h 0.0.0.0
```

然后，您可以通过使用 csv 测试数据运行以下脚本来进行预测:

```
./predict.sh test.csv
```

它运行以下 curl 命令:

```
curl [http://localhost:1234/invocations](http://localhost:1234/invocations) \
-H ‘Content-Type: text/csv’ --data-binary @test.csv
```

# 使用 mlflow 注册表训练和服务模型

使用 mlflow 注册表可以让您做很多很酷的事情:

*   有一个中心位置来跟踪和分享不同的实验
*   跟踪并轻松查看模型和代码输入参数
*   记录和查看指标(准确性、召回率等)
*   将您的模型版本化，并比较在相同实验下记录的不同模型的性能
*   与任何其他可以访问注册表的用户协作，在同一个实验中注册他们的模型
*   轻松地将模型转换到不同的阶段(例如，`Staging`、`Production`)，这意味着，*不是引用特定编号的模型，而是将您最近喜欢的模型切换到特定的阶段，下游组件可以通过引用该阶段无缝地使用该新模型*

通过运行以下脚本，首先启动 mlflow 服务器以在本地使用注册表:

```
./runServer.sh
```

它只运行以下命令:

```
mlflow server \
 --backend-store-uri sqlite:///mlflow.db \
 --default-artifact-root ./mlflow-artifact-root \
 --host 0.0.0.0
```

这将运行在您的浏览器中可见的 mlflow UI。

`[clf-train-registry.py](https://github.com/mtpatter/mlflow-tutorial/blob/main/clf-train-registry.py)`脚本使用 sklearn 乳腺癌数据集，训练一个简单的随机森林分类器，并将模型和指标保存并注册到指定 url 的 mlflow 注册表中。添加用于写入输出测试数据的可选标志将首先分割训练数据以添加示例测试数据文件。

若要训练模型，请使用以下命令:

```
python clf-train-registry.py clf-model "[http://localhost:5000](http://localhost:5000)" \
--outputTestData test.csv
```

下面是该脚本的一个代码片段，它将最新的模型移入阶段`Staging`，并将之前的模型移回`None`。

将新转换的`Staging`型号服务到端口 1234:

```
mlflow models serve models:/clf-model/Staging -p 1234 -h 0.0.0.0
```

然后，您可以通过使用 csv 测试数据运行以下脚本来进行预测:

```
./predict.sh test.csv
```

它再次运行下面的 curl 命令:

```
curl http://localhost:1234/invocations \
-H ‘Content-Type: text/csv’ --data-binary @test.csv
```

就是这样！

有几点需要注意:

*   如果您使用 Docker Compose 运行本教程，容器将访问并写入您的本地挂载目录。
*   当通过 Python API 加载模型时，您需要确保您的环境 sklearn 版本与您用来保存它的版本相同(这就是为什么我更喜欢在 Docker 中做任何事情)。
*   你可以用`docker compose down`清理运行中的容器。

希望这是生产机器学习模型的良好开端。通过替换示例培训脚本，您应该能够将本教程用于您自己的模型。

在后续的文章中，我将介绍如何构建您自己的定制 mlflow 模型，以便您可以通过 REST api 提供几乎任何您喜欢的功能。敬请期待！