# 成为最受欢迎的数据科学家

> 原文：<https://towardsdatascience.com/upskill-to-be-a-most-sought-after-data-scientist-bde9f3bc32d7?source=collection_archive---------21----------------------->

## 成为成功的数据科学家的全面技能列表

![](img/6d3ef57f6fef0a9217189e1f4e5d52e2.png)

照片由 [**迈克尔·布隆维斯特**](https://www.pexels.com/@mikael-blomkvist?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**派克斯**](https://www.pexels.com/photo/woman-in-front-of-the-computer-6476582/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

随着技术的发展，保持行业领先地位所需的技能也在发展。过去几年来，成为一名数据科学家一直是许多人的梦想。不用说，这是一份高收入的工作。你们中的一些人可能是数据科学家，其他人可能会在这个领域寻找职位。在这篇简短的文章中，我将列出现代数据科学家的技术技能要求。如果你是一名在职的数据科学家，你将拥有其中一些技能，但是，你可能需要在许多新的领域提高技能。如果你是一个初学者，这个全面的列表将帮助你规划成为一名数据科学家的职业生涯。

我先说最本质的技巧。

# 编程语言的选择

任何机器学习项目，像任何其他软件项目一样，都需要选择合适的编程语言。没有一个软件开发者会想到用 Fortran 做商业应用；同样，您永远不会在科学应用程序中使用 COBOL。也就是说，每种编程语言在设计时都有特定的目的。

我不需要解释，但是 Python 是大多数 ML 开发的选择。如果你调查一下招聘信息，你会注意到 Python、SQL 和 R 可能是最热门的。我会推荐 Python。如果你是一个经验丰富的开发者，考虑一下 Julia 的高性能。

这里最重要的问题是，你需要了解 Python 到什么深度？初学者可能会惊讶地发现，每一个机器学习代码都只有几行——不到 100 到 200 行。框架代码在所有 ML 应用程序中保持不变。这是完整的骨架代码(算法):

*   数据清理
*   数据预处理
*   探索性数据分析
*   特征工程
*   创建培训、测试和验证数据集
*   选择 ML 算法
*   培养
*   评估模型的性能

训练后，如果您对模型对看不见的数据的推断感到满意，您可以将模型移动到生产服务器。

上述培训和测试代码在所有 ML 项目中保持不变；改变的只是 ML 算法。尽管如此，根据您的数据类型和您试图解决的问题，每个步骤都会有所不同。然而，顶层视图在所有项目中保持不变。我希望你说到点子上了。对于编写这种模型训练代码，您不需要掌握很深的 Python 技能。我很少在我的 ML 项目中使用 Python 的面向对象和函数式编程。

与其他编程语言形成对比。如果不使用这些高级特性，您将永远无法开发出高性能的软件应用程序。在 ML 中，什么是高性能？即使模特训练需要额外的时间，我也没意见。对我来说，重要的是结果，也就是模型的准确性。第二，通过使用函数式和面向对象的代码，我会让见习开发人员更难读懂。因此，我总是坚持简单的基本代码，即使是新手也能很容易理解。

你甚至可以为 400+ ML 算法找到现成的[模板](https://cloud.blobcity.com/)。使用这样的服务将会剥夺你写一个框架代码的工作。

接下来的技巧是机器学习库的选择。

# 机器学习库

scikit-learn 是许多 ML 开发者的选择。scikit-learn 为预测建模和分析提供了大量工具。需要用熊猫，NumPy，Matplotlib/Seaborn 来补充 sklearn。NumPy 和 Pandas 是创建机器可理解的数据集、要素/目标选择和创建数据集所必需的。Matplotlib 和 Seaborn 将协助 EDA。

任何 ML 项目中最大的挑战是选择一个合适的算法，最终给你看不见的数据最好的预测。下面是帮助你选择最佳算法的 AutoML。

# AutoML

对于算法选择，请使用 AutoML。AutoML 是一个工具，它可以将几个 ML 算法应用到您的数据集，并按照性能准确性的升序对它们进行排序。使用 AutoML，您不需要开发那些核心数据科学家的专业知识。在学习过程中节省了很多——你只需要学习各种算法背后的概念以及它们解决了什么问题。

即使是 ML 的初学者也可以学习使用这些工具来帮助选择最佳性能的模型及其所有微调的超参数。有一个巨大的清单，自动和超参数调整库。我已经生成了一个完整的列表供您快速参考。

## AutoML 库/工具:

[Autosklearn](https://automl.github.io/auto-sklearn/master/) ， [TPOT](http://epistasislab.github.io/tpot/) ， [Autokeras](https://autokeras.com/) ， [MLBox](https://mlbox.readthedocs.io/en/latest/) ，[autoglon](https://auto.gluon.ai/stable/index.html)， [autoweka](https://www.cs.ubc.ca/labs/beta/Projects/autoweka/) ， [H2OAutoML](https://docs.h2o.ai/h2o/latest-stable/h2o-docs/automl.html) ， [autoPyTorch](https://www.automl.org/automl/autopytorch/) ，[亚马逊 Lex](https://aws.amazon.com/lex/) ， [DataRobot](https://www.datarobot.com/) ， [TransmogrifAI](https://transmogrif.ai/) ，[Azur](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-configure-auto-train)

> 这一领域值得研究的新成员是 [BlobCity AutoAI](https://github.com/blobcity/autoai) 。我想提到的最有趣的特性是，它泄露了生成模型的项目源，数据科学家可以声称这是他自己的创造。已编辑—2022 年 1 月 17 日

## 超参数调节库/工具:

[Optuna](https://optuna.org/) ，[hyperpt](http://hyperopt.github.io/hyperopt/)， [Scikit-optimize](https://scikit-optimize.github.io/stable/) ， [Ray tune](https://docs.ray.io/en/latest/tune/index.html) ，[randomsearccv](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.RandomizedSearchCV.html)， [GridSearchCV](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) ，[微软 NNI](https://nni.readthedocs.io/en/stable/)

我不想为你背书或推荐一个。你可以自己尝试一下。在 ML 开发中，算法选择和微调超参数是最艰巨的任务，而这些工具的使用使这变得很容易。

接下来是统计建模和人工神经网络之间的选择。

# 经典 ML 和 ANN 之间的选择

在这里，我可以向你推荐我之前发表在 Medium 上的文章*现代数据科学家的建模方法*，或者你可以观看 YouTube 上关于 *@blobcity* 频道的演示，它讲述了数据科学家在建模中采用的方法。

如果你更喜欢神经网络而不是经典的 ML，开发者的首选是 TensorFlow。它不仅仅是一个人工神经网络开发库，而是一个完整的平台，允许您创建数据管道，使用分布式培训来减少培训时间，根据您拥有的资源使用 CPU/GPU/TPU，允许在本地服务器、云甚至边缘设备上部署。出于您的善意考虑，我可能会让您参考我的[书](https://www.apress.com/gp/book/9781484261491)以了解更多细节，其中也包含 25 个以上基于 ANN 的真实项目。

另一个流行的 ANN 开发工具是 PyTorch。

# 迁移学习

训练 DNN 需要巨大的计算资源，数周的训练时间，以及数百万张图像或数兆字节文本数据的数据集。许多科技巨头创造了几个 DNN 模型来解决某一类问题。这些公司创造的模式，在这个世界上没有人能做到。

这里有一个全面的列表供您快速参考。

**图像分类** : ImageNet，CIFAR，MNIST

**计算机视觉的 CNN 模型** : VGG19，Inceptionv3 (GoogLeNet)，ResNet50，EfficientNet

**NLP 机型** : OpenAI 的 GPT-3，Google 的 BERT，微软的 CodeBERT，ELMo，XLNet，Google 的 ALBERT，ULMFiT，脸书的 RoBERTa

幸运的是，这些公司已经公开了这些模型。作为一名数据科学家，您可以原样重用这些模型，或者扩展它们以满足您的业务需求。我们称之为*迁移学习*。他们为特定的目的训练每个模型。你只需要在将他们的学习转移到你的模型之前理解它。

现在出现了一个重大的变化，它将改变我们多年来开发模型的方式。

# 动态模式

到目前为止，我们一直在静态数据上开发 ML 模型。在今天的疫情情况下，你认为这些大约 2 年前根据静态数据训练的模型会帮助你预测你的客户访问模式和他们新的购买趋势吗？您需要根据新数据重新训练模型。这意味着所有基于静态数据开发的模型都必须定期重新训练。这需要消耗资源、时间和精力。这需要额外的成本。

于是就出现了动态建模的概念。您可以根据流数据不断地重新训练模型。您在流中收集的单位时间内的数据点数量将决定窗口大小。解决方案现在可以归结为时间序列分析。动态建模的主要优势之一是您可以使用有限的资源进行模型训练。在静态建模的情况下，它们需要您在内存中加载大量的数据点，这也会导致很长的训练时间。

开发流数据模型需要额外的技能。看看 PySpark。您需要将一些 web 框架应用到您的模型中。请注意，您会持续训练模型，并使用它进行即时实时预测。web 框架应该能够产生快速响应。

在我的下一篇文章中，我将更深入地讨论动态模型。

颠覆性创新来了。

# MLaaS

最后，我必须提到 MLaaS，它可能会取代数据科学家。亚马逊、谷歌、IBM 和微软等科技巨头提供完整的机器学习服务。

你只需要把你的数据上传到服务器上。他们做全部的数据预处理、模型建立、评估等等。最后，他们只是给你一个模型，可以直接移植到他们的产品服务器上。尽管这对数据科学家来说是一种威胁，但我仍然建议数据科学家应该习惯这些新技术，并为其所有者提供快速解决方案。毕竟，让老板满意有助于你保住工作。

# 摘要

我已经为您提供了在当前数据科学状态下成为顶尖数据科学家所需技能的全面观点。随着技术的进步，你不需要掌握每一项技能。比如到目前为止设计的统计 ML 算法就有几百种。您现在可以使用像 AutoML 这样的工具来为您的业务用例选择一个性能最好的。甚至就编程语言而言，你已经看到，要成为一名 ML 开发人员，你并不需要成为 it 专家。

人工神经网络的成功揭开了 ML 发展的新篇章。有几件事现在被过分简化了。创建数据管道、自动特征选择、无监督学习和在万亿字节数据上训练网络的能力只是 ANN 技术的几个优点。作为一名数据科学家，您需要获得这些技能，如果您目前还没有这样做的话。使用 DNN 和预先训练的模型进一步简化了数据科学家的工作。

另一个出现的重要需求，可能是因为当前的流行病，是需要开发动态模型。作为一名数据科学家，他们现在要求你开发流数据模型。开发高频数据的动态模型是当前研究的主题。

最后，你可以选择 MLaaS 作为快速解决方案。

祝你好运，努力成为最受欢迎的数据科学家。

你可能会喜欢在 YouTube 上观看我的演讲。

## 信用

![](img/aa5709d09a9bb5902bcfa679911f2676.png)