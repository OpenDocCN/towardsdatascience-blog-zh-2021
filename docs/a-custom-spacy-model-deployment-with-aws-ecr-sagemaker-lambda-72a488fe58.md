# 使用 AWS ECR、SageMaker 和 Lambda 进行定制空间模型部署

> 原文：<https://towardsdatascience.com/a-custom-spacy-model-deployment-with-aws-ecr-sagemaker-lambda-72a488fe58?source=collection_archive---------14----------------------->

## 关于如何使用 AWS 部署 SpaCy 的教程。

![](img/453d42ed69630944c1e7a0cb9a880836.png)

托马斯·伊万斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 背景:

SpaCy 是我最喜欢的 NLP 图书馆之一。我一直在使用 spaCy 执行许多命名实体识别(NER)任务。通常，我们首先需要加载一个特定语言的 spaCy 预训练模型，并使用我们的训练数据集对该模型进行微调。训练过程可以在本地计算机上离线完成，我们甚至可以通过 Flask / Streamlit 在本地托管它来测试微调后的模型性能。

[](/building-a-flask-api-to-automatically-extract-named-entities-using-spacy-2fd3f54ebbc6) [## 使用 SpaCy 构建 Flask API 来自动提取命名实体

### 如何使用 spaCy 中的命名实体识别模块来识别文本中的人、组织或位置，然后…

towardsdatascience.com](/building-a-flask-api-to-automatically-extract-named-entities-using-spacy-2fd3f54ebbc6) 

虽然我发现了很多关于使用 Flask / Streamlit 在本地部署 spaCy 模型的很棒的教程，但是关于如何在更大范围内部署的教程并不多，例如，如何使用 AWS 部署 spaCy 模型。

这是一个非常有趣的话题，在做了大量的工作后，我在这篇文章中总结了我的解决方案；希望能对面临同样问题的人有所帮助。

# 简介:

在本文中，我将解释我的解决方案，说明如何使用 AWS 服务部署自定义空间模型，包括:

*   弹性集装箱登记处
*   AWS SageMaker
*   自动气象站λ
*   AWS S3 铲斗(可选)

这是我的计划 🧗🏻：

*   首先，原始数据输入可以作为事件发送到 **AWS Lambda** 。
*   然后，在 AWS Lambda **处理函数**中，我们调用一个 SageMaker **端点**。端点工作是将事件作为输入，并从部署在 SageMaker 上的微调 spaCy 模型返回预测结果。
*   最后，这个端点预测将作为执行结果显示在 AWS Lambda **结果选项卡**上。如果出现错误，我们可以在 AWS CloudWatch 上调试。
*   在真实的场景中，我们可能还想连接一个 AWS API 网关，例如，如果我们想为模型托管一个接口。或者可能将预测结果与另一个 Lambda 连接起来，并集成到生产工作流中。

在上述步骤中，spaCy 模型托管在 AWS SageMaker 上，并且可以作为端点随时调用。为了创建 SageMaker 端点，我们需要首先在 AWS ECR 上创建我们的**定制空间模型容器**,并准备好 tar.gz 格式的经过训练的**模型工件**。

此外，要创建一个模型工件，我们可以离线或在线完成。每次我们训练完一个空间模型，训练好的模型会被保存到一个文件夹中。该文件夹通常包含以下文件:

**文件名可能不同*

*   meta.json
*   ner(文件夹)
*   标记器
*   vocab(文件夹)

要创建一个模型工件，我们可以简单地将这个文件夹压缩成 tar.gz 格式，并上传到 S3 存储桶。或者，我们也可以通过在 SageMaker 培训部分下创建一个培训任务，用 AWS SageMaker 在线培训模型。但是我们需要提供来自 AWS ECR 的**定制训练图像**和来自 AWS S3 bucket 的**训练数据**。

希望您现在对一般的工作流程有一点清楚了。现在，让我们开始创建我们的自定义空间模型容器，并将其上传到 AWS ECR 作为我们的训练图像。

# 在 AWS ECR 上创建自定义容器

因为 spaCy 不是 SageMaker 内置算法之一。我们必须首先在 AWS ECR 上创建一个空间模型容器，并在 SageMaker 上创建模型端点时将该容器指定为训练映像。

> SageMaker 提供了两个选项，其中第一个选项是使用 sage maker 提供的内置算法，包括 KNN、XgBoost、线性学习器等。而另一个选项是使用 ECR 中的**自定义 docker 容器**。

[](/deploy-custom-deep-learning-based-algorithm-docker-container-on-amazon-sagemaker-4c334190e278) [## 在 Amazon Sagemaker 上部署基于定制深度学习的算法 Docker 容器

### 在这篇文章中，我将介绍我们如何在 Amazon Sagemaker 上部署定制的深度学习容器算法…

towardsdatascience.com](/deploy-custom-deep-learning-based-algorithm-docker-container-on-amazon-sagemaker-4c334190e278) 

上面的文章是一个很棒的参考，它启发了我如何部署一个定制的 spaCy 容器。我强烈建议通读这篇文章，并对 docker 容器的内部工作方式有更深入的了解。一开始对我来说理解起来很复杂，但是很有帮助。🧠

简而言之，该部分包括以下步骤:

*   编辑 Github repo 下的文件(主要是 *docker 文件，train.py 和 predictor.py* )来训练自定义 spaCy 模型，并可以返回模型预测。
*   通过对 spaCy docker 容器执行**本地测试**来检查计算机终端上的预测结果，如果出现任何错误，则调试代码。
*   最后，将空间容器上传到 AWS ECR。

对于训练文件，目标是修改代码来训练自定义空间模型。将 CSV 格式的数据读取到名为 train_data 的数据帧后，我们需要将数据帧转换为 spaCy model 可以接受的输入格式。我建议阅读我以前写的一篇关于如何为 NER 任务训练空间模型的文章。

[](/an-overview-of-building-a-merchant-name-cleaning-engine-with-sequencematcher-and-spacy-9d8138b9aace) [## 使用 SequenceMatcher 和 spaCy 构建商家名称清理引擎概述

### 使用预训练的 spaCy 深度学习模型构建一个商家名称清理引擎…

towardsdatascience.com](/an-overview-of-building-a-merchant-name-cleaning-engine-with-sequencematcher-and-spacy-9d8138b9aace) 

本质上，这里我们需要将数据帧转换成以下空间格式:

```
TRAIN_DATA = 
[
('Amazon co ca', {'entities': [(0, 6, 'BRD')]}),
('AMZNMKTPLACE AMAZON CO', {'entities': [(13, 19, 'BRD')]}),
('APPLE COM BILL', {'entities': [(0, 5, 'BRD')]}),
('BOOKING COM New York City', {'entities': [(0, 7, 'BRD')]}),
('STARBUCKS Vancouver', {'entities': [(0, 9, 'BRD')]}),
('Uber BV', {'entities': [(0, 4, 'BRD')]}),
('Hotel on Booking com Toronto', {'entities': [(9, 16, 'BRD')]}),
('UBER com', {'entities': [(0, 4, 'BRD')]}),
('Netflix com', {'entities': [(0, 7, 'BRD')]})]
]
```

然后，在训练文件中，我们定义我们的 **train_spacy** 函数来训练模型。train_spaCy 函数类似于下一篇文章中的函数。请注意，本文中的代码是用 spaCy 版本 2 编写的。

[](https://manivannan-ai.medium.com/how-to-train-ner-with-custom-training-data-using-spacy-188e0e508c6) [## 如何使用 spaCy 自定义训练数据训练 NER？

### 使用我们的自定义数据集训练空间名称实体识别(NER)

manivannan-ai.medium.com](https://manivannan-ai.medium.com/how-to-train-ner-with-custom-training-data-using-spacy-188e0e508c6) 

训练文件示例

在预测器文件中，我们需要编辑**预测**函数下的代码。预测函数将 test_data 作为输入。test_data 需要是一个数据帧，我们可以将训练好的空间模型应用到包含我们想要预测的文本的列上。

然后，我们可以为模型的预测创建一个新列。如果这是一个 NER 任务，那么新列将包含从每行输入中提取的实体。此外，我们可以调整 predict 函数是只返回预测列表，还是返回包含源列和预测列的整个数据帧。

预测文件

最后，在 docker 文件中，只需根据所需的库相应地编辑代码。对我来说，我添加了*RUN pip 3 install-U spacy = = 2 . 3 . 5*来安装我使用的 spaCy 的正确版本。

如果你来了，恭喜你！🥳

要将我们的容器图像推送到 Amazon ECR，我们可以遵循里面的代码(来自上面的 [**文章的**](/deploy-custom-deep-learning-based-algorithm-docker-container-on-amazon-sagemaker-4c334190e278) Github)

*   构建 _ 和 _ 推送. sh 文件
*   自带创建算法和模型包笔记本

这些文件在那篇文章中有很好的解释。我们可以简单地运行代码，将我们自己的定制空间容器推到 AWS ECR 上。

直到这一步，我们终于将自定义空间容器上传到了 AWS ECR。这是模型部署的关键步骤。现在，我们将切换到 AWS SageMaker 并创建一个模型端点。

# 在 AWS SageMaker 上创建一个模型端点

如 AWS SageMaker 开发者指南[中所述](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html):

> Amazon SageMaker 是一项完全托管的服务，使数据科学家和开发人员能够快速轻松地构建、训练和部署任何规模的机器学习模型。Amazon SageMaker 包括可以一起使用或单独使用的模块，以构建、训练和部署您的机器学习模型。

SageMaker 的界面结构如下:

```
Sections on the AWS SageMaker...--->Training
------>Algorithms
------>**Training jobs** 
------>Hyperparameter tuning jobs--->Inference
------>Compilation jobs
------>Model packages
------>**Models**
------>**Endpoint configurations**
------>**Endpoints**
------>Batch transform jobs...
```

要创建空间模型端点，我们需要完成以下步骤:

*   在 AWS sage maker-Training 下创建一个**培训任务**。或者，简单地在本地训练你的空间模型，把它压缩成 tar.gz*格式，然后上传到 S3 桶。目标是在这一步准备一个**模型工件**。*
*   *在 AWS SageMaker-Inference 下创建一个**模型**。我们需要指定来自 AWS ECR 的**容器图像**和来自 AWS S3 存储桶的模型工件。*
*   *在 AWS SageMaker 推论下创建一个**端点配置**。我们需要为端点配置指定一个模型。*
*   *在 AWS SageMaker 推论下创建一个**端点**。我们需要为端点指定一个端点配置。*

*如果你在 AWS SageMaker 上面的步骤中有任何问题，下面的教程将会很有帮助！🏂🏻*

*[](https://aws.amazon.com/cn/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/) [## 使用 Amazon API Gateway 和 AWS Lambda | Amazon Web 调用 Amazon SageMaker 模型端点…

### 2021 年 4 月-此帖子已更新，以确保解决方案演练反映对相关 AWS 所做的更改…

aws.amazon.com](https://aws.amazon.com/cn/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/) 

# 设计 AWS Lambda 函数

如 AWS Lambda 开发人员[指南](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)中所述:

> Lambda 是一种计算服务，让您无需配置或管理服务器即可运行代码。Lambda 在高可用性计算基础设施上运行您的代码，并执行计算资源的所有管理，包括服务器和操作系统维护、容量供应和自动扩展、代码监控和日志记录。

就空间部署而言，我们将使用 AWS Lambda 作为接口，可以在用户输入和空间模型端点之间进行通信。具体来说，我们希望将推理文本作为 Lambda 事件输入，并在 Lambda execution 选项卡上接收模型响应。

关于如何在 AWS Lambda 上调用 SageMaker 端点的一些细节可以在与上面相同的教程[中找到。本质上，我们希望在 Lambda 上编写一个 python 处理函数，它可以执行以下步骤:](https://aws.amazon.com/cn/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/)

*   **输入事件:推理文本列表**
*   将事件读入数据帧(可选)
*   将数据帧写入 CSV 缓冲区(保持数据格式的一致性)
*   将缓冲区的值转换成 ASCII 格式(与 AWS 要求保持一致)
*   调用 SageMaker 端点并接收响应
*   将响应解码为 UTF 8 格式
*   将响应读入数据帧(可选)
*   **输出响应:模型预测列表**

关于创建 CSV 缓冲区或 ASCII / UTF-8 转换步骤的一些细节可能是不必要的，但最终它对我有用。如果你有更好的主意，请告诉我！😸

如果一切正常，现在，我们应该能够看到在 AWS Lambda **执行结果**选项卡上，模型预测的列表被打印出来。💯

# 摘要

这就结束了我关于使用 AWS 部署定制空间模型的文章。概括地说，工作流包括服务:

*   第一步:AWS ECR
*   第二步:AWS S3 铲斗(可选)
*   第三步:AWS SageMaker
*   第四步:AWS Lambda

希望它能帮助/激励任何从事类似工作的人😋

感谢您的阅读！

🏇🏼

# 相关阅读

[](https://xoelop.medium.com/deploying-big-spacy-nlp-models-on-aws-lambda-s3-2857bfc143ba) [## 在 AWS Lambda + S3 上部署大空间 NLP 模型

### Spacy 型号对于 250MB 的 Lambda 限制来说太大了。这向你展示了如何绕过它，从 S3 和…

xoelop.medium.com](https://xoelop.medium.com/deploying-big-spacy-nlp-models-on-aws-lambda-s3-2857bfc143ba) [](/load-a-large-spacy-model-on-aws-lambda-e1999e5e24b5) [## 在 AWS Lambda 上加载大型空间模型

### spaCy 是一个有用的工具，它允许我们执行许多自然语言处理任务。当将空间整合到一个…

towardsdatascience.com](/load-a-large-spacy-model-on-aws-lambda-e1999e5e24b5)*