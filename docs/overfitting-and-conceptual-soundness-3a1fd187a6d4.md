# 过度拟合和概念合理性

> 原文：<https://towardsdatascience.com/overfitting-and-conceptual-soundness-3a1fd187a6d4?source=collection_archive---------15----------------------->

## [行业笔记](https://towardsdatascience.com/tagged/notes-from-industry)

## 特征使用如何影响我们对深层网络中过度拟合的理解

![](img/baf280cbb25c3df0b3d59bebf3aeeb4a.png)

由[谢恩·奥尔登多夫](https://unsplash.com/@pluyar?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

过度拟合是机器学习中的一个中心问题，当它被部署在看不见的数据上时，它与学习模型的可靠性密切相关。过度拟合通常通过模型在其训练数据上获得的精度与之前未见过的验证数据相比的差异来衡量，甚至定义。虽然这是一个有用的度量，广泛地捕捉了模型在新的点上犯错误的程度(过度拟合的关键问题之一)，但我们将改为对过度拟合采取更一般和细微的看法。特别是，本文将使用 [TruLens](https://www.trulens.org) 解释性框架来检查过度拟合背后的一个关键机制:编码和使用*不健全的特性*。

在高层次上，深度网络通过学习提取高层次的特征来工作，这些特征使它们能够对新的输入进行预测。虽然这些特征中的一些可能真的是可概括的预测器，但其他的可能只是巧合地帮助训练集上的分类。在前一种情况下，我们说学习到的特征在概念上是合理的。后一种类型的学习特征在概念上*不是*合理的，因此可能导致在看不见的点上的异常或不正确的行为，即过拟合。

在本文的剩余部分，我们将提供支持这种过度拟合观点的证据，并展示 TruLens 如何用于评估神经网络学习和使用的特征。有关 TruLens 的更全面介绍，请参见本文。

# 一个例证

我们的假设是，过度拟合通过特殊的特征使用在模型中表现出来。为了说明这一点，我们将考虑来自野生的(LFW)数据集中的“[标记的人脸的例子。LFW 数据集包含许多名人和杰出公众人物大约在 21 世纪初的图像，任务是识别每张照片中的人。我们选择了包含五个最频繁出现的身份的子集。完整的数据集可以通过](http://vis-www.cs.umass.edu/lfw/) [scikit-learn](https://scikit-learn.org/stable/datasets/real_world.html#labeled-faces-in-the-wild-dataset) 获得。

![](img/d44014481f4aafa4ebc2af5c840c4e00.png)

LFW 培训点示例。我们看到右上角的图像有一个独特的粉红色背景。(图片来自[利用模型记忆进行校准的白盒成员推理](https://arxiv.org/pdf/1906.11798.pdf))

在训练集中，我们发现托尼·布莱尔的一些图像具有独特而鲜明的粉红色背景。我们的假设表明，一个模型可以通过学习使用粉红色背景作为托尼·布莱尔的特征来过度拟合，因为该特征确实在训练集上预测托尼·布莱尔*。当然，尽管它对训练集非常有用，但背景显然在概念上是不合理的，并且不太可能对新数据有用。*

如果模型以这种方式过度拟合，那么通过检查模型在具有粉红色背景的实例上编码和使用的特征，这将是明显的。这可以通过 TruLens 使用*内部影响* [1]来完成。复制本文中图像和实验的笔记本可以在[这里](https://colab.research.google.com/drive/1Iswyxd4rorKqqQWkC4kieAwFpfqBS25v?usp=sharing)找到。

我们将从在 LFW 训练集上训练一个简单的卷积神经网络(CNN)开始。例如，使用张量流:

```
from trulens.nn.models import get_model_wrapper# Define our model.
x = Input((64,64,3))z = Conv2D(20, 5, padding='same')(x)
z = Activation('relu')(z)
z = MaxPooling2D()(z)z = Conv2D(50, 5, padding='same')(z)
z = Activation('relu')(z)
z = MaxPooling2D()(z)z = Flatten()(z)
z = Dense(500)(z)
z = Activation('relu')(z)y = Dense(5)(z)keras_model = Model(x, y)# Compile and train the model.
keras_model.compile(
    loss=SparseCategoricalCrossentropy(from_logits=True),
    optimizer='rmsprop',
    metrics=['sparse_categorical_accuracy'])keras_model.fit(
    x_train, 
    y_train, 
    epochs=50, 
    batch_size=64, 
    validation_data=(x_test, y_test))# Wrap the model as a TruLens model.
model = get_model_wrapper(keras_model)
```

接下来，我们将使用*内部影响*来检查模型用于决策的主要学习特征。根据我们的假设，过度拟合模型将编码粉红色背景作为用于分类的特征。如果这个假设成立，我们将需要找到这个特征在模型中的编码位置，以及它是否用于分类。

CNN 的卷积层由许多*通道*或*特征图*组成，而这些特征图又由单个神经元网格构成。每个通道代表一种类型的特征，而每个通道内的神经元代表图像中特定位置处的那种类型的特征*。有可能高级特征(例如，粉色背景)由网络编码，但是它不对应于单个通道。例如，它可以由多个通道的线性组合形成。在我们的例子中，为了简单起见，我们将我们的搜索限制在考虑单个通道，这恰好对我们有用。*

我们训练出来的网络不是特别深，所以我们没有太多的选择层来搜索。典型地，更深的层编码逐渐更高级的特征；为了找到我们的粉红色背景特征，我们将从搜索第二个卷积层开始。

我们使用下面的过程:首先，我们在第二卷积层(我们的模型的实现中的第 4 层)中找到最有影响的信道。我们有很多方法可以做到这一点；在我们的例子中，我们将根据通道中每个神经元之间的*最大*影响为每个通道分配影响。一旦我们确定了最有影响力的通道，我们将通过找到对该通道贡献最大的输入像素来可视化它。总之，这个过程告诉我们哪个特征(在我们选择的层)对模型的预测最有影响，以及图像的哪些部分是该特征的一部分。

```
from trulens.nn.attribution import InternalInfluence
from trulens.visualizations import HeatmapVisualizerlayer = 4# Define the influence measure.
internal_infl_attributer = InternalInfluence(
    model, layer, qoi='max', doi='point')internal_attributions = internal_infl_attributer.attributions(
    instance)# Take the max over the width and height to get an attribution for
# each channel.
channel_attributions = internal_attributions.max(
    axis=(1,2)
).mean(axis=0)target_channel = int(channel_attributions.argmax())# Calculate the input pixels that are most influential on the
# target channel.
input_attributions = InternalInfluence(
    model, (0, layer), qoi=target_channel, doi='point'
).attributions(instance)# Visualize the influential input pixels.
_ = HeatmapVisualizer(blur=3)(input_attributions, instance)
```

![](img/8e5a8f26fde856768e4d9af692be2618.png)

(图片由作者提供)

最重要的像素以红色突出显示。我们看到，实际上，背景被我们的模型大量使用。使用另一种可视化技术，我们可以再次确认解释集中在这些独特训练点的背景上:

```
from trulens.visualizations import ChannelMaskVisualizer
from trulens.visualizations import Tilervisualizer = ChannelMaskVisualizer(
    model,
    layer,
    target_channel,
    blur=3, 
    threshold=0.9)visualization = visualizer(instance)
plt.imshow(Tiler().tile(visualization))
```

![](img/875ceea4f1728472ba0cc3643aef9fc4.png)

(图片由作者提供)

为了便于比较，我们可以在一个不同的模型上遵循相同的程序，该模型在训练期间没有看到任何粉色背景。这个模型没有理由对粉红色背景特征进行编码，更不用说用它来识别托尼·布莱尔了。正如预期的那样，我们看到结果完全不同:

![](img/129334fc41d009c15b6cc3d5901dfd08.png)

(图片由作者提供)

# 用解释捕捉错误

解释有助于增加我们对概念上合理的模型的信任，或者有助于我们预测未来可能因使用不合理的特性而出现的错误。

再次考虑我们的运行示例。模特了解到粉色背景是托尼·布莱尔的特色。碰巧的是，在我们的测试集中，没有托尼·布莱尔或其他任何人的图片是粉色背景的。因此，我们的测试集在识别这种概念不健全的情况下是没有用的。但是模型应该被信任吗？据推测，粉色背景很容易在部署中出现，即使在测试集中没有出现。

用粉色背景训练的模型和不用粉色背景训练的模型都达到了大致相同的验证准确度(在 83%和 84%之间)。从验证指标的角度来看，我们应该对其中任何一个都满意。但是，前面几节中生成的解释应该清楚地表明，一个模型有一个缺点，而另一个模型没有。

事实上，我们可以直接证明不健全的功能使用的含义，这可以通过检查解释来预见。虽然我们在测试集中没有显示粉红色背景的例子，但这可以通过一些基本的照片编辑很容易地解决。在这里，我们编辑了一个来自 LFW 的非托尼-布莱尔派人士 Gerhard Schroeder 的图像，使其背景为粉红色。当然，像编辑过的图像这样的图片很容易在现实生活中实现。

![](img/495a9c237126b4c145693b2e04150256.png)

来自 LFW 的 Gerhard Schroeder 的原始图像(左)和编辑版本(右)。(图片由作者提供)

我们看到，在原始图像上，模型正确预测了类别 3，对应于 Gerhard Schroeder。但是，在编辑过的图像上，模型预测的是 class 4，对应的是 Tony Blair。

```
>>> keras_model.predict(original).argmax(axis=1)
array([3])
>>>
>>> keras_model.predict(edited).argmax(axis=1)
array([4])
```

而且，可以预见的是，如果我们问模型为什么它在编辑过的图像上预测到了托尼·布莱尔，我们会看到粉红色的背景再次被突出显示。

![](img/6acb9e7b0173e8ca6b6b8cd5bfd81684.png)

(图片由作者提供)

最后，如果我们转向没有粉红色背景的替代模型，我们观察到我们编辑的图像不会导致相同的错误行为。毕竟，另一种模式没有理由将粉红色背景与托尼·布莱尔(或任何其他人)联系起来，而且似乎也没有这样做。

```
>>> keras_model_no_pink.predict(original).argmax(axis=1)
array([3])
>>>
>>> keras_model_no_pink.predict(edited).argmax(axis=1)
array([3])
```

# 过度拟合的其他含义

我们已经看到过过度拟合如何导致模型在看不见的数据上犯特殊的错误。除了导致错误分类之外，过度拟合还会带来隐私风险。直观地说，通过学习过于特定于训练集的特征，模型将会无意中泄露关于它们的训练数据的信息。[我与](http://www.cs.cmu.edu/~kleino/)[马特·弗雷德里克松](https://www.cs.cmu.edu/~mfredrik/)(出现在《2020 年 USENIX》中)在这个话题上的研究【T1【2】)使用了与这里类似的见解来展示攻击者如何能够对用于训练模型的数据做出推断。

特别是，我们设计的攻击甚至在模型的训练集和验证集之间基本上没有差距的情况下也能工作*。这强调了一点，即过度拟合不一定需要在验证数据上显示为错误来导致问题。通过检查我们的模型使用特性的方式，我们可以在它们的功效上获得比我们从简单的性能度量中推断的更多的信任；相反，我们可以发现潜在的问题，否则可能会被忽视。*

# 摘要

机器学习模型容易学习不健全的特征，这些特征可能导致预测错误、隐私漏洞等。解释有助于识别不合理的特性使用情况，否则即使在验证数据中也不会被发现。另一方面，我们应该努力寻找那些解释表明合理特征使用的模型，增加我们对模型在未来未知数据上的表现的信任。TruLens 是一个功能强大且易于使用的工具，可以帮助我们应用这种有价值的模型分析形式。

# 参考

1.  深度卷积网络的影响导向解释。ITC 2018。 [ArXiv](https://arxiv.org/pdf/1802.03788.pdf)
2.  雷诺&弗雷德里克松。"偷来的记忆:利用模型记忆校准白盒成员推理."USENIX 2020。 [ArXiv](https://arxiv.org/pdf/1906.11798.pdf)