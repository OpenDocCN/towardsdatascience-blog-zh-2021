# 有志 ML 从业者的基本行话

> 原文：<https://towardsdatascience.com/essential-lingo-for-the-aspiring-ml-practitioner-f939176ad7f4?source=collection_archive---------33----------------------->

## 从层类型到功能，一个快速列表可以帮助你在开始机器学习时保持方向。

每个行业都有自己的行话，机器学习也不例外。这里有一个关键术语的简要清单，所有有抱负的 ML 开发人员在构建模型时都应该熟悉这些术语。

![](img/0f8216769494fd3b450ca554cf6bfe3f.png)

[图像来源](https://unsplash.com/photos/P0NuBF6nA7A)

**图层和模型类型**

在 ML 中，深度学习(DL)是当前的艺术状态。今天所有的 DL 模型都是基于[神经网络](https://blog.perceptilabs.com/four-common-types-of-neural-network-layers-and-when-to-use-them/)。神经网络包括*神经元*(又名*张量* ) 的**层，其存储某物的状态(例如，像素值)，并且连接到存储更高级实体(例如，像素组)的状态的后续层中的神经元。在这些层之间是 ***权重*** ，这些权重在训练期间被调整，并最终用于进行预测。**

下面是**要熟悉的神经网络层的常见类型**:

*   **输入层**:代表输入数据(如图像数据)的神经网络的第一层。
*   **输出层**:神经网络的最后一层，包含给定输入的模型结果(如[图像分类](https://blog.perceptilabs.com/top-five-ways-that-machine-learning-is-being-used-for-image-processing-and-computer-vision/))。
*   **隐藏层:**位于神经网络的*输入*和*输出*层之间的层，对数据进行转换。
*   **密集**(又名*全连接层*):其所有输出都连接到下一层所有输入的层。
*   **卷积**:构建[卷积神经网络](https://machinelearningmastery.com/convolutional-layers-for-deep-learning-neural-networks/) (CNN)的基础。
*   **递归**:提供*循环*功能，其中层的输入包括要分析的数据和对该层执行的先前计算的输出。递归层构成了[递归神经网络](/recurrent-neural-networks-d4642c9bc7ce) (RNNs)的基础。
*   **残差块**:残差神经网络(ResNets)将两层或更多层组合在一起作为*残差块*，以避免[消失梯度问题](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)。

ML 从业者使用**不同类型的层**以不同的方式构建神经网络。常见的神经网络示例包括 VGG16、ResNet50、InceptionV3 和 MobileNetV2。请注意，这些模型中有许多是由 [Keras 应用](https://docs.perceptilabs.com/perceptilabs/references/components/deep-learning)提供的，并且可以通过 PerceptiLabs 中的组件获得，允许您使用[迁移学习](https://blog.perceptilabs.com/when-to-use-transfer-learning-in-image-processing/)快速构建模型。

**基础知识**

模型的层和互连构成了模型的 ***内部*** 或 ***可学习的*参数**，这些参数在训练过程中**调整**。关键参数包括:

*   **权重**:一个给定的神经元影响它在下一层所连接的张量的程度。
*   **偏差**:向上或向下移动结果，使其更容易或更难激活神经元。

在**训练**期间，底层 **ML 引擎通过调整其权重和偏差来优化模型。**为了做到这一点，采用了以下算法:

*   **优化器**:更新模型，帮助它学习复杂的数据模式。常见的算法有: *ADAM* ，*随机梯度下降(SGD)* ， *Adagrad* ， *RMSprop* 。
*   **激活函数**:给神经元输出增加非线性的数学函数。例子包括:*乙状结肠*、 *ReLU* 和 *Tanh* 。
*   **损失函数**(又名*误差函数*):衡量优化器对单个训练样本的调整是否会产生更好的性能(即，指示给定一组权重和偏差值时模型的预测效果)。常见的损失函数有*交叉熵*、*二次*和*骰子*。
*   **成本函数**:衡量优化器对所有训练样本的调整是否会产生更好的模型性能。

训练通常包括将数据分成三组:

*   **训练数据**:用于通过提供*地面实况*数据(例如，具有已知正确的相应标签的图像)来优化模型的权重和偏差。
*   **验证数据**:用于测试模型在训练时的准确性。
*   **测试数据**:用于测试最终训练好的模型在以前没有见过的数据上的性能。

重要的是要避免在训练期间 ***欠拟合*** (即训练一个太简单而不能很好地从数据中学习的模型)，以及**过拟合**(即训练一个太复杂而不能根据新的、看不见的数据准确预测的模型)。ML 从业者可以调整许多用户可控的设置，称为 ***超参数*** ，以开发出尽可能好的模型:

*   **批量大小**:优化器更新模型参数之前要训练的训练样本数。
*   **时期:对整个训练数据集(包括所有批次)进行的次数。**
*   **损失:要使用的损失函数算法。**
*   **学习率**:模型参数更新后，根据计算的误差，改变模型参数的多少。
*   **优化器**:要使用的优化器算法。
*   **Shuffle** :随机改变训练数据顺序，使模型更健壮。
*   **Dropout** :通过忽略随机选择的层输出来减少过拟合的正则化技巧。然后，对层的每次更新都在该层的不同*视图*上有效地执行。

这个清单当然不是详尽无遗的——我们可以很容易地写出一整套关于基础知识的书！然而，这些关键要素是一个良好的开端。

对于那些想在 Perceptilabs 中尝试这些基础知识的人，请务必查看 PerceptiLabs [文档](https://docs.perceptilabs.com/perceptilabs/)和我们的[入门](https://docs.perceptilabs.com/perceptilabs/getting-started/quickstart-guide)指南。