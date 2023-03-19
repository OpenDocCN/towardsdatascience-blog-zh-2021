# 用 Torcheck 测试您的 PyTorch 模型

> 原文：<https://towardsdatascience.com/testing-your-pytorch-models-with-torcheck-cb689ecbc08c?source=collection_archive---------23----------------------->

## 一个方便的 PyTorch 健全性检查工具包

![](img/9c2a537f1dc7505c09b194cdfbd54007.png)

[斯科特·格雷厄姆](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

你是否有过长时间训练 PyTorch 模型，却发现自己在模型的`forward`方法中打错了一行的经历？你是否曾经遇到过这样的情况:你从你的模型中获得了某种程度上合理的输出，但不确定这是否表明你已经建立了正确的模型，或者只是因为深度学习非常强大，甚至错误的模型架构也产生了下降结果？

就我自己而言，测试一个深度学习模型时不时会让我抓狂。最突出的难点是:

*   它的黑盒性质使它很难测试。如果不是不可能的话，理解中间结果需要很多专业知识。
*   较长的训练时间大大减少了迭代次数。
*   没有专用的工具。通常，您会希望在一个小样本数据集上测试您的模型，这涉及到为设置优化器重复编写样板代码、计算损失和反向传播。

为了减少这种开销，我以前做过一些研究。我发现了一篇由 Chase Roberts 撰写的关于这个话题的精彩的 [medium 帖子](https://thenerdstation.medium.com/how-to-unit-test-machine-learning-code-57cf6fd81765)。核心思想是，我们永远不能百分之百确定我们的模型是正确的，但至少它应该能够通过一些健全性检查。换句话说，这些健全性检查是必要的，但可能还不够。

为了节省您的时间，以下是他提出的所有理智检查的总结:

*   在训练过程中，模型参数应该总是变化的，如果它不是故意冻结的话。这可以是 PyTorch 线性图层的权重张量。
*   如果模型参数被冻结，则在训练过程中不应改变。这可能是您不想更新的预训练层。
*   根据模型属性，模型输出的范围应符合某些条件。例如，如果它是一个分类模型，其输出不应该都在范围(0，1)内。否则，在计算损耗之前，很有可能错误地将 softmax 函数应用于输出。
*   (这个实际上不是那个帖子上的，而是一个常见的)一个模型参数在大多数情况下不应该包含`NaN`(不是数字)或者`Inf`(无穷大)。这同样适用于模型输出。

除了提出这些检查，他还构建了一个实现它们的 Python 包。这是一个很好的包，但仍然有未解决的痛点。该软件包是几年前构建的，不再维护。

因此，受这种健全性检查思想的启发，旨在创建一个易于使用的 Python 包，torcheck 应运而生！它的主要创新是:

*   不再需要额外的测试代码。只需在训练前添加几行指定检查的代码，torcheck 就会接管，在训练进行时执行检查，并在检查失败时发出一条信息性错误消息。
*   在不同的层次上检查你的模型是可能的。你可以指定检查一个子模块，一个线性层，甚至是权张量，而不是检查整个模型！这使得围绕复杂体系结构的检查有了更多的定制。

接下来，我们会给你一个关于 torcheck 的快速教程。以下是一些有用的链接:

*   [Torcheck GitHub 页面](https://github.com/pengyan510/torcheck)
*   [Torcheck PyPI 页面](https://pypi.org/project/torcheck/)

假设我们已经编写了一个用于分类 [MNIST 数据集](https://en.wikipedia.org/wiki/MNIST_database)的 ConvNet 模型。完整的训练程序如下所示:

模型代码中实际上有一个细微的错误。你们中的一些人可能已经注意到:在第 16 行，我们不小心把`x`放在了右手边，它应该是`output`。

现在让我们看看 torcheck 如何帮助您检测这个隐藏的错误！

# 步骤 0:安装

在我们开始之前，首先在一行中安装软件包。

```
$ pip install torcheck
```

# 步骤 1:添加 torcheck 代码

接下来，我们将添加代码。Torcheck 代码总是驻留在模型和优化器实例化之后，紧接在训练 for 循环之前，如下所示:

# 步骤 1.1:注册您的优化器

首先，向 torcheck 注册您的优化器:

```
torcheck.register(optimizer)
```

# 步骤 1.2:添加健全性检查

接下来，在这四个类别中添加您想要执行的所有检查。

## 1.参数改变/不改变

对于我们的例子，我们希望所有的模型参数在训练过程中改变。添加检查很简单:

```
# check all the model parameters will change
# module_name is optional, but it makes error messages more informative when checks fail
torcheck.add_module_changing_check(model, module_name="my_model")
```

## 边注

为了展示 torcheck 的全部功能，假设稍后您冻结了卷积层，而只想微调线性层。在这种情况下添加检查类似于:

```
# check the first convolutional layer's parameters won't change
torcheck.add_module_unchanging_check(model.conv1, module_name="conv_layer_1")# check the second convolutional layer's parameters won't change
torcheck.add_module_unchanging_check(model.conv2, module_name="conv_layer_2")# check the third convolutional layer's parameters won't change
torcheck.add_module_unchanging_check(model.conv3, module_name="conv_layer_3")# check the first linear layer's parameters will change
torcheck.add_module_changing_check(model.fc1, module_name="linear_layer_1")# check the second linear layer's parameters will change
torcheck.add_module_changing_check(model.fc2, module_name="linear_layer_2")
```

## 2.输出范围检查

由于我们的模型是一个分类模型，我们想要添加前面提到的检查:模型输出不应该都在范围(0，1)内。

```
# check model outputs are not all within (0, 1)
# aka softmax hasn't been applied before loss calculation
torcheck.add_module_output_range_check(
    model,
    output_range=(0, 1),
    negate_range=True,
)
```

`negate_range=True`的说法带有“并非全部”的意思。如果您只想检查模型输出是否都在某个范围内，只需删除该参数。

尽管 torcheck 不适用于我们的示例，但它也使您能够检查子模块的中间输出。

## 3.NaN 检查

我们肯定希望确保模型参数在训练期间不会变成 NaN，并且模型输出不包含 NaN。添加 NaN 检查很简单:

```
# check whether model parameters become NaN or outputs contain NaN
torcheck.add_module_nan_check(model)
```

## 4.Inf 检查

类似地，添加 Inf 检查:

```
# check whether model parameters become infinite or outputs contain infinite value
torcheck.add_module_inf_check(model)
```

添加所有感兴趣的检查后，最终的训练代码如下所示:

# 第二步:培训和固定

现在让我们像往常一样运行培训，看看会发生什么:

```
$ python run.pyTraceback (most recent call last):
  (stack trace information here)
RuntimeError: The following errors are detected while training:
Module my_model's conv1.weight should change.
Module my_model's conv1.bias should change.
```

砰！我们立即得到一条错误消息，说我们的模型的`conv1.weight`和`conv1.bias`没有改变。`model.conv1`肯定有问题。

正如预期的那样，我们转向模型代码，注意到错误，修复它，并重新运行培训。现在一切都像魔咒一样工作:)

# (可选)步骤 3:关闭检查

耶！我们的模型已经通过了所有的检查。为了摆脱它们，我们可以简单地调用

```
torcheck.disable()
```

当您想要在验证集上运行您的模型，或者您只想从您的模型训练中删除检查开销时，这是非常有用的。

如果你想再开支票，就打电话

```
torcheck.enable()
```

这就是开始使用 torcheck 所需的全部内容。更全面的介绍请参考其 [GitHub 页面](https://github.com/pengyan510/torcheck)。

由于该软件包仍处于早期阶段，可能会有一些错误。请不要犹豫报告他们。欢迎任何投稿。

希望你觉得这个包有用，让你对你的黑盒模型更有信心！