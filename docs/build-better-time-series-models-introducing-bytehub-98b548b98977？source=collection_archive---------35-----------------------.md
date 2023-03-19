# 构建更好的时间序列模型:ByteHub 简介

> 原文：<https://towardsdatascience.com/build-better-time-series-models-introducing-bytehub-98b548b98977?source=collection_archive---------35----------------------->

## 了解如何使用要素存储来简化您的数据科学工作流

![](img/d5d05f6a7b53d027eca40c8c0fe0b6b6.png)

数据管道对机器学习至关重要。[西格蒙德](https://unsplash.com/@sigmund?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/pipes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照。

本文介绍 ByteHub:一个开源的**特性库，**，旨在帮助数据科学家建立更好的时间序列数据模型，准备用于预测和异常检测等任务。虽然我们经常关注用于这些任务的机器学习技术和算法，但**数据管道**经常被忽视，尽管它对于开发高质量的模型以及能够将其作为更大项目的一部分进行部署和维护来说至关重要。

我们将探索在构建时间序列模型时改进数据科学工作流的三种方式，并通过示例演示如何使用 ByteHub。

# 获取数据

模型的好坏取决于它们接受训练的输入数据。对于任何使用时间序列模型的数据科学家来说，一项重要的任务是确定要使用的数据源。结果可能是戏剧性的:例如，在处理零售、能源、运输和其他部门的各种建模问题时，包含单个天气变量的结果通常会大大提高准确性。

添加天气数据会使伦敦自行车租赁数据的时间序列模型产生很大的性能差异[1]。

不幸的是，向模型中添加新的数据源通常不是一件容易的事情。数据集经常被锁在不同的数据库中或复杂的 API 后面，这使得很难快速迭代不同的数据输入并找到最好的。

ByteHub 通过两种方式解决了这个问题。首先，通过提供一种简单的方法来存储时间序列数据，以便可以轻松地访问、过滤、重新采样并输入模型。例如，以下代码片段演示了如何加载多个时间序列要素并将其重新采样到每小时分辨率，以便在模型训练中使用，而无需任何额外准备。

快速加载和重新采样时间序列特征。

其次，这些数据集存储有描述和额外的元数据，并且可以从简单的 Python 接口中进行搜索。数据科学家可以更轻松地搜索和重用现有数据，在更短的时间内开发出更好的模型，而不是在每个新项目上重新发明轮子。

![](img/2df0d28ea3005147b8678a0c2d6bee51.png)

在 Python 中快速搜索数据集和要素。

# 准备数据

找到正确的数据后，下一步是在将数据输入模型之前准备和转换数据。被称为特征工程，它允许我们使用我们自己的领域专业知识来提高模型性能。

ByteHub 引入了一个叫做**特征转换**的概念:可以应用于原始时间序列数据的 Python 代码片段。这些可以链接在一起，允许您构建非常复杂的功能工程管道，然后发布这些管道，以便在不同的项目和模型之间重用。

例如，在这个[天气预报示例](https://github.com/bytehub-ai/code-examples/blob/main/tutorials/06_timeseries_forecasting.ipynb)中，我们需要将风速和风向转换为 x 和 y 风速，因为否则我们使用的模型[将很难正确解释风的角度](https://www.tensorflow.org/tutorials/structured_data/time_series#wind)。下面的 Python 函数包含了实现这一点所需的数学知识，而 decorators 将这些转换添加到特性库中，以便可以重用它们。

使用 Python 函数将原始时序数据转换为模型要素。

# 保持整洁！

在过去，所有这些数据管道很容易产生难以理解和维护的代码鸟巢。通过使用特征库，可以**将数据准备从模型训练**代码中分离出来。这不仅使模型**更容易理解**，也使**更容易部署**:数据加载和准备可以用一行代码完成。例如，ByteHub 中的典型模型训练脚本如下所示，其中包含加载数据、构建和训练模型的简单而清晰的步骤。

一旦所有的数据准备都转移到特征存储中，模型训练代码就会变得更加简单和易于理解。

# 最后的想法

我希望这篇文章能激发你在下一个时间序列建模项目中尝试 ByteHub。如果你想试一试，[完整的文档](https://docs.bytehub.ai/)和安装说明在 [GitHub](https://github.com/bytehub-ai/bytehub) 上。

[](https://github.com/bytehub-ai/bytehub) [## bytehub-ai/bytehub

### 易于使用的功能商店。特征存储是用于数据科学和机器学习的数据存储系统。它可以…

github.com](https://github.com/bytehub-ai/bytehub) 

所有的[教程和示例](https://github.com/bytehub-ai/code-examples/tree/main/tutorials)都可以从 Google Colab 笔记本上运行。

如果您有任何想法、意见或问题，请在下方留言或直接联系我们。

[1]伦敦交通局:[桑坦德循环雇佣计划的雇佣总人数，按日、月、年统计](https://data.london.gov.uk/dataset/number-bicycle-hires)。