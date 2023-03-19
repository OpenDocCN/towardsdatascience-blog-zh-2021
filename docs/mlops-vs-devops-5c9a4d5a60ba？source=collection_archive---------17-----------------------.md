# MLOps 与 DevOps

> 原文：<https://towardsdatascience.com/mlops-vs-devops-5c9a4d5a60ba?source=collection_archive---------17----------------------->

## 本文介绍了 DevOps 和 mlop 之间的相似之处和不同之处，以及有助于实现 mlop 的平台

![](img/934fd23289dcbf6ad7815c96b394fc4e.png)

[HalGatewood.com](https://unsplash.com/@halacious?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/science?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

随着近年来机器学习领域的成熟，将自动持续集成(CI)、持续交付(CD)和持续培训(CT)集成到机器学习系统的需求已经增加。DevOps 哲学在机器学习系统中的应用被称为 MLOps。MLOps 的目标是将机器学习系统开发(ML)和机器学习系统操作(Ops)融合在一起。

**什么是 DevOps？**

DevOps 是个人和团队在开发软件系统时使用的实践。个人和团队可以通过 DevOps 文化和实践获得的好处包括:

1.  快速开发生命周期
2.  部署速度
3.  通过使用测试来保证代码质量。

为了实现这些好处，在软件系统的开发过程中使用了两个关键概念。

1.  持续集成:将代码库合并到一个中央代码库/位置，自动化软件系统的构建过程，并测试代码库的组件。
2.  连续交付:自动化软件部署。

机器学习系统与软件系统相似，但不完全相同。主要区别在于:

1.  团队中存在的技能:开发机器学习模型/算法的人通常没有软件工程背景，并且主要关注概念证明/原型阶段。
2.  机器学习系统本质上是高度实验性的。在没有首先尝试和做一些实验的情况下，不能保证算法会预先成功。因此，需要跟踪不同的实验、特征工程步骤、模型参数、度量等。
3.  测试机器学习系统不仅仅是单元测试。您还需要考虑诸如数据检查、模型漂移、评估部署到生产中的模型性能之类的事情。
4.  机器学习模型的部署是非常定制的，取决于它试图解决的问题的性质。它可能涉及多步骤管道，包括数据处理、特征工程、模型训练、模型注册和模型部署。
5.  需要随着时间的推移跟踪数据的统计和分布，以确保模型今天在生产环境中看到的与它被训练的数据一致。

**m lops 和 DevOps 的相似之处**

1.  开发人员、数据科学家和数据工程师之间代码库的持续集成。
2.  机器学习系统代码的代码和组件的测试。
3.  将系统持续交付生产。

【MLOps 和 DevOps 的区别

1.  在 MLOps 中，除了测试代码之外，您还需要确保在机器学习项目的整个生命周期中保持数据质量。
2.  在 MLOps 中，您可能不需要仅仅部署一个模型工件。机器学习系统的部署可能需要涉及数据提取、数据处理、特征工程、模型训练、模型注册和模型部署的机器学习流水线。
3.  在 MLOps 中，还有 DevOps 中没有的第三个概念，即持续培训(CT)。该步骤完全是关于自动识别由于当前部署的机器学习模型/系统中的性能退化而需要模型被重新训练和重新部署到生产中的场景/事件。

如果你有兴趣了解部署机器学习模型的不同方式，请查看我的文章([https://towards data science . com/machine-learning-model-deployment-options-47 C1 F3 d 77626](/machine-learning-model-deployment-options-47c1f3d77626))。

**协助 MLOps 的平台和工具**

1.  ml flow([https://mlflow.org/](https://mlflow.org/)):这是一个开源平台，它帮助和支持模型跟踪、模型注册和模型部署步骤。我也相信这是 Azure 机器学习在他们的平台上使用的。Databricks 已经将 mlflow 整合到他们的平台上。
2.  版本控制系统如:
    -github
    -git lab
    -Azure devo PS
3.  云服务进行实验和部署机器学习管道:
    -AWS SageMaker([https://aws.amazon.com/sagemaker/](https://aws.amazon.com/sagemaker/))
    -Azure 机器学习([https://docs . Microsoft . com/en-us/Azure/Machine-Learning/overview-what-is-Azure-ml](https://docs.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-ml))
    -data bricks([https://databricks.com/](https://databricks.com/))

如果你有兴趣了解更多关于 AWS SageMaker 和 Azure 机器学习之间的异同，请查看我的文章([https://towardsdatascience . com/AWS-sage maker-vs-Azure-Machine-Learning-3ac 0172495 da](/aws-sagemaker-vs-azure-machine-learning-3ac0172495da))