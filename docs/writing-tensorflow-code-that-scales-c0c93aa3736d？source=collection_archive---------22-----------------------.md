# 编写可伸缩的张量流代码

> 原文：<https://towardsdatascience.com/writing-tensorflow-code-that-scales-c0c93aa3736d?source=collection_archive---------22----------------------->

## 添加两行代码来增强您的脚本

大多数时候，我们在本地编写和调试代码。通过任何测试后，我们将脚本部署到远程环境中。如果幸运的话，我们可能可以访问多个 GPU。这种转变曾经是错误的来源，但是对复杂计算设置的支持已经显著增加。

![](img/d6da96b64fcdec927cb27e8121c2da58.png)

杰里米·贝赞格在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

如果您主要使用 Keras 的 *model.fit()* 调用，那么您可以通过添加两行代码来快速地让您的脚本分发知晓。

# 在分配工作负载之前

最初，我们的代码可能是这样的:

# 分配工作负载后

为了让代码准备好发布，我们必须修改第 1 行到第 4 行。为此，我们创建了一个*策略*对象，这是 TensorFlow 处理计算环境的方式。然后，我们使用它来包装我们的代码，如下面的代码片段所示:

在**我们添加的第一行**，第一行，我们设置了我们的策略对象。在示例中，我们使用了一个 *MirroredStrategy* ，它告诉 TensorFlow 在一台机器上的多个计算设备上复制模型。对于其他环境，我们可以选择不同的策略，如文档中列出的[。](https://www.tensorflow.org/guide/distributed_training#types_of_strategies)

除了我们将模型和优化器创建例程包装在所选的*策略*对象的范围内之外，其余代码基本相同。这是**我们添加的第二行代码**并且是必要的修改:*作用域*修改变量是如何创建的以及在哪里放置它们。

之后，我们可以照常进行，调用 *model.fit()* 。然后，工作量会自动分配。我们在这里什么都不用做，因为 TensorFlow 在内部处理所有事情，这非常舒服！这也是一个巨大的进步:仅仅几年前，研究人员和从业者还必须自己编写分发算法。

现在，必须一直手动实例化*策略*对象是令人厌烦的。然而，我们可以用下面的代码自动创建正确的对象，我通常喜欢把它放在一个单独的 *utils.py* 文件中:

这段代码[取自 Hugginface 的存储库](https://github.com/huggingface/transformers/blob/1c06240e1b3477728129bb58e7b6c7734bb5074e/src/transformers/training_args_tf.py#L191)，在创建分发策略之前检查计算环境。根据设置，返回的*策略*对象处理 TPU、GPU 以及混合精度训练。

如果您使用这种方法，那么您只需更改这一行:

# 摘要

这就是你要在代码中修改的全部内容。总而言之，你

*   制定分销策略
*   将所有变量创建例程都包含在策略范围内
*   像平常一样调用 model.fit()

这些步骤涵盖了使用 Keras 的高级 API 进行任何计算的情况。但是，如果您正在使用自定义训练循环，则需要注意更多事项。在这种情况下，[我这里有你罩着](/a-template-for-custom-and-distributed-training-c24e17bd5d8d)。