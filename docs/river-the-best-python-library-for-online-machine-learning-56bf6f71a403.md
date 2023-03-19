# River:在线机器学习的最佳 Python 库

> 原文：<https://towardsdatascience.com/river-the-best-python-library-for-online-machine-learning-56bf6f71a403?source=collection_archive---------11----------------------->

## 意见

## 流数据机器学习的“sklearn”

![](img/dbcb3edcc8294e8e5fa3d3c7999e3269.png)

图片由 [UnboxScience](https://pixabay.com/users/unboxscience-1306029/) 在 [Pixabay](https://pixabay.com/vectors/water-stream-river-creek-flow-wet-908813/) 拍摄

传统的机器学习算法，如线性回归和 xgboost，以“批处理”模式运行。也就是说，它们一次性使用完整的数据集来拟合模型。用新数据更新该模型需要使用新数据和旧数据从头开始拟合一个全新的模型。

在许多应用中，这可能是困难的或不可能的！它要求所有的数据都适合内存，这并不总是可能的。模型本身的重新训练可能会很慢。为模型检索旧数据可能是一个很大的挑战，尤其是在数据不断生成的应用程序中。存储历史数据要求数据存储基础设施能够快速返回完整的数据历史。

或者，模型可以“在线”或以“流”模式训练。在这种情况下，数据被视为一个流或一系列项目，一个接一个地传递给模型。

> ***增量学习、持续学习*** 和 ***连续学习*** 是“在线学习”的首选术语，因为对“在线学习”的搜索主要指向教育网站。

增量学习是许多用例的理想选择，例如在大型数据集上拟合模型、垃圾邮件过滤、推荐系统和物联网应用。

# 介绍河流

`[River](https://riverml.xyz/latest/getting-started/getting-started/)`是一个新的 python 库，用于在流设置中增量训练机器学习模型。它为不同的在线学习任务提供了最先进的学习算法、数据转换方法和性能指标。它是合并了`creme`和`scikit-multiflow`库的最好部分的产物，这两个库都是为了相同的目标而构建的:

> “为社区提供工具来提升流式机器学习的状态，并促进其在现实世界应用中的使用。”

River 是一个由从业者和研究人员组成的大型社区维护的开源包。源代码可在 [Github](https://github.com/online-ml/river.) 上获得。

## River 中的算法类型

`river`为回归、分类和聚类任务提供了一系列增量学习算法。可用的模型类型包括朴素贝叶斯、树集成模型、因式分解机、线性模型等等。有关已实现算法的完整列表，请参见 [API 参考](https://riverml.xyz/latest/api/overview/)。

River 还提供漂移检测算法。[概念漂移](https://riverml.xyz/latest/examples/concept-drift-detection/)发生在输入数据和目标变量之间的关系改变时。

River 提供了[处理不平衡数据集](https://riverml.xyz/latest/examples/imbalanced-learning/)和异常检测的方法。

最后，`river`提供了模型度量和数据预处理方法，它们已经被重构以处理增量更新。

## 增量学习算法有什么不同？

为了增量地转换数据和训练模型，必须重构大多数学习算法和数据转换方法来处理更新。

首先，考虑**如何缩放特征以获得零均值和单位方差。**在批量设置中，这很简单:计算平均值和标准差，从每个值中减去平均值，结果除以标准差。这种方法需要完整的数据集，并且不能随着新数据的到来而更新。在流设置中，使用*运行统计*完成特征缩放，这是一种数据结构，允许平均值和标准偏差增量更新。

**对于增量式训练模型，常见的学习算法是** [***随机梯度下降***](/stochastic-gradient-descent-clearly-explained-53d239905d31) **(SGD)。** SGD 是一种用于训练神经网络的流行算法，有多种变体。它还可以用于训练其他模型，如线性回归。SGD 的核心思想是在每个训练步骤中，模型参数权重在梯度的相反方向上进行调整，该梯度是使用该步骤中的模型预测误差来计算的。

# API 河

所有模型都能够从数据中学习并做出预测。`river`的模型具有从单一实例中学习的灵活性。每当新数据到达时，模型可以快速更新。

*   `learn_one(x, y)`更新模型的内部状态，给出一个包含输入特征`x`和目标值`y`的新观察值。
*   `predict_one`(分类、回归、聚类)返回单个观察值的模型预测值
*   `predict_proba_one`(分类)返回单次观察的模型预测概率
*   `score_one`(异常检测)返回单个观察值的异常值
*   `transform_one`转换一个输入观察值

要演示 API 的用法，请参见下面的示例。在该示例中，实例化了线性`LogisticRegression`模型和`ROCAUC`模型评分对象。然后，对于数据集中的每个观察值，模型使用`predict_proba_one`进行预测，并通过将观察值`x`和标签`y`传递给`learn_one`方法来更新其权重。最后，用真实值`y`和预测值`y_pred`更新模型评分对象`metric.update`。

# 字典数据结构

River 希望数据观察能够以 python 字典的形式呈现。代表功能名称的字典关键字。

字典数据结构有几个优点:

*   词典是灵活的，不是打字的。因此，它们可以处理稀疏数据和可能出现在数据流中的新功能。
*   字典数据是轻量级的，不需要复杂数据结构(如`numpy.ndarray`或`pandas.DataFrame`)所需的开销。这使得在流式环境中快速处理单个观察更容易。

River 扩展了原生 python 字典结构，以支持更高效的数据操作。

`river`中的`stream`模块提供了几个用于处理流数据的实用程序，包括将数据加载到预期的字典格式中。

例如，`stream.iter_csv`方法允许您将 CSV 文件加载到流数据结构中。在下面的例子中，“loaded”`data_stream`实际上是一个 python 生成器，它遍历 CSV 文件并解析每个值。整个数据集不是一次从磁盘中读取，而是一次读取一个样本。

*典例出自* [*河文献*](https://riverml.xyz/latest/user-guide/reading-data/) *。*

以上`stream.iter_csv`示例中使用的一些附加参数:

*   `converters`参数指定非字符串列的数据类型。如果在加载时没有指定数据类型，`stream.iter_csv`假设所有数据都是字符串。
*   `parse_dates`表示解析日期的预期格式。
*   `target`指定哪个变量是目标，或`y`列。如果排除，`y`返回为`None`。

在像 web 应用程序这样的应用程序中，字典数据结构非常直观。您可以简单地将一个 JSON 结构的有效负载传递给一个模型，以进行预测或更新模型。

*代码示例来自* [*河流文档*](https://riverml.xyz/latest/user-guide/reading-data/) *。*

# 河流管道

管道是`river`的核心组成部分。与`scikit-learn`类似，管道将模型的各个步骤，包括数据转换和预处理，链接到一个序列中。

河流用户指南中的代码示例([管道—河流(riverml.xyz)](https://riverml.xyz/latest/user-guide/pipelines/) )

如[使用管道的艺术](https://riverml.xyz/latest/examples/the-art-of-using-pipelines/)教程所示，当特征预处理步骤变得复杂时，管道简化了模型拟合代码。它使代码可读性更强，更不容易出错。

需要注意的重要行为:**当在管道上调用** `**predict_one**` **或** `**predict_proba_one**` **方法时，模型的非监督部分被更新**！无监督部分包括特征缩放和其他变换。这和批量机器学习有很大不同。像要素缩放这样的变换不依赖于地面实况标签，因此可以在没有地面实况标签的情况下进行更新。当然，除了转换方法之外，管道的`learn_one`方法还会更新受监控的组件。

# 简单的例子

以下示例演示了如何在乳腺癌数据集上拟合逻辑回归模型。

首先，实例化标准数据缩放器和逻辑回归模型。给逻辑回归模型一个学习率为 0.01 的随机梯度下降优化器进行训练。然后，初始化真实值和预测值的列表。

然后，代码遍历数据集；每个`xi`都是一个字典对象。在 for 循环的每一步中，代码需要 4 个步骤:

1.  用新的观测值更新定标器，并对观测值进行定标。
2.  该模型使用缩放后的观察值进行预测。
3.  模型将使用新的观察和标签进行更新。
4.  真实标签和预测标签存储在一个列表中。

当 for 循环结束时，计算总 ROC AUC。

改编自[河(riverml.xyz)](https://riverml.xyz/latest/examples/batch-to-online/) 文档的代码示例

这段代码可以改进和简化。在下面的示例中，为相同的任务训练了逻辑回归模型，但是使用管道和运行度量来减少步骤数量并增强可读性。

# 结论

`river`是增量学习和持续学习的首选库。它提供了一系列数据转换方法、学习算法和优化算法。其独特的数据结构非常适合流数据和 web 应用程序的设置。

# 参考资料和进一步阅读

如何使用 River 的例子:【https://riverml.xyz/latest/examples/batch-to-online/】T4

用户指南:[https://riverml.xyz/latest/user-guide/reading-data/](https://riverml.xyz/latest/user-guide/reading-data/)

河纸:[https://arxiv.org/abs/2012.04740](https://arxiv.org/abs/2012.04740)

 