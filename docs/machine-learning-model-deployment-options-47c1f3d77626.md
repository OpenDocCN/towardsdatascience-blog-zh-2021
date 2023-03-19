# 机器学习模型部署选项

> 原文：<https://towardsdatascience.com/machine-learning-model-deployment-options-47c1f3d77626?source=collection_archive---------33----------------------->

![](img/9d0bf0cc514b23385dc323e49be3d49e.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com/s/photos/artificial-intelligence?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

机器学习模型部署可以分为 3 大类:

1.  实时推理:通常它涉及在 web 服务器上作为端点托管机器学习模型。然后，应用程序可以通过 https 提供数据，并在短时间内接收回模型的预测。
2.  批量推断:定期(由时间或事件触发，如数据进入数据湖/数据存储)启动资源，并部署机器学习模型来预测数据湖/数据存储中现在可用的新数据。
3.  模型部署到边缘:模型预测是在边缘设备上计算的，而不是要求将输入数据传递到后端。想想物联网和网络应用。

当决定采用哪种方法将机器学习模型部署到生产中时，需要考虑几个因素。

1.  延迟:应用程序/用户需要多快得到模型预测的结果？
2.  数据隐私:向后端发送数据有什么问题/顾虑吗？
3.  网络连接:某些部署选项需要访问互联网/网络。如果需要部署模型的环境具有有限的或没有互联网/网络连接，则选项是有限的。
4.  成本:某些部署选项比其他选项成本更高。想象一下，拥有一台全天候在线的服务器来提供预测服务。运行和维护该服务器的成本是多少？

**潜伏期**

假设您已经创建了一个 web/mobile 应用程序，它提供了一个机器学习服务，可以根据用户的输入进行预测。如果用户能够在提供一些输入后立即获得模型的结果，而不是等待几个小时，或者直到他们收到模型最终对他们的输入做出预测的通知，这将是更好的用户体验。在这种特定情况下，将机器学习模型部署到实时推理管道/边缘部署中会更理想。

另一方面，如果机器学习模型正被用于丰富作为数据管道的一部分的一些数据，并且数据不需要立即可用，也许新数据每天仅到达一次，则使用批量推理管道来预测新输入数据可能更具成本效益。

**数据隐私**

当将模型部署到实时推理或批量推理管道时，输入数据将需要传输到后端，并且通常会在由机器学习模型的创建者管理的云环境中。想象一下，提供的数据有可能成为个人身份信息，或者有人担心数据将存储在哪个国家。如果存在任何数据隐私问题，使用这种方法可能是不可行的。

解决这个问题的一种方法是将模型部署到边缘设备中。这要求边缘设备具有足够的计算能力来实际托管模型，并进行对边缘处的输入数据进行预测所需的计算。部署到边缘设备的缺点是，通常计算资源的数量是一个限制，因此模型架构的选择可能需要更加简单，以便节省资源，这可能导致模型预测能力的性能下降。

**网络连接**

将模型部署到实时推理或批量推理管道通常需要某种形式的网络连接，能够将输入数据从源传输到后端。如果输入数据源的网络连接有限/没有网络连接，这可能意味着为了解决业务用例，模型需要部署在离数据源更近的地方。

**成本**

某些部署方法会导致不同的成本计算。当部署到实时推理管道时，您需要端点全天候可用。此外，您可能需要在高负载期间扩大计算资源，在低负载期间缩小计算资源。这种扩大和缩小的管理可能还需要一个团队来管理实时推理管道。

批处理推理管道通常更便宜，因为您只需要在预测新的一批数据所需的时间内加速资源。根据数据量和模型对数据进行预测的速度，您可以确定想要利用的资源量(节点数量和节点类型)(有关使用 AWS 的批处理推理管道的示例，请查看我之前写的这篇文章:[https://towards data science . com/AWS-sagemaker-batch-transform-design-pattern-fa1d 60618 fa 8](/aws-sagemaker-batch-transform-design-pattern-fa1d60618fa8))。

就运营成本而言，边缘部署可能是最便宜的选择，因为您正在利用边缘设备的资源。成本可能更加间接。这包括将预测能力低于其他模型的劣质模型部署到生产中的潜在成本、管理将模型部署到一组边缘设备的成本、评估模型在边缘设备上的性能的成本(您如何在边缘设备上收集和跟踪一段时间内的推断？).

**AWS 中的机器学习部署选项**

1.  实时推理:[https://docs . AWS . Amazon . com/sage maker/latest/DG/inference-pipeline-real-time . html](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-real-time.html)
2.  批量推断:[https://docs . AWS . Amazon . com/sage maker/latest/DG/inference-pipeline-batch . html](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-pipeline-batch.html)
3.  边缘部署:可能需要使用 SageMaker Neo:[https://docs . AWS . Amazon . com/SageMaker/latest/DG/Neo-Edge-devices . html](https://docs.aws.amazon.com/sagemaker/latest/dg/neo-edge-devices.html)或使用 green grass:[https://docs . AWS . Amazon . com/green grass/v1/developer guide/what-is-gg . html](https://docs.aws.amazon.com/greengrass/v1/developerguide/what-is-gg.html)进行交叉编译和/或使用 SageMaker 边缘管理器进行模型管理:[https://docs.aws.amazon.com/sagemaker/latest/dg/edge.html](https://docs.aws.amazon.com/sagemaker/latest/dg/edge.html)

**Azure 中的机器学习部署选项**

1.  实时推理:[https://docs . Microsoft . com/en-us/azure/machine-learning/how-to-deploy-and-where？tabs=azcli](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-deploy-and-where?tabs=azcli)
2.  批量推断:[https://docs . Microsoft . com/en-us/azure/machine-learning/tutorial-pipeline-batch-scoring-class ification](https://docs.microsoft.com/en-us/azure/machine-learning/tutorial-pipeline-batch-scoring-classification)
3.  Edge 部署(截至 2021 年 4 月 12 日预览版):[https://docs . Microsoft . com/en-us/azure/IOT-edge/tutorial-deploy-machine-learning？view=iotedge-2020-11](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-deploy-machine-learning?view=iotedge-2020-11)