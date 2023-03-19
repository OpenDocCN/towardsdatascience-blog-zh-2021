# 联合高棉语分词和词性标注

> 原文：<https://towardsdatascience.com/joint-khmer-word-segmentation-and-pos-tagging-cad650e78d30?source=collection_archive---------43----------------------->

## 深度学习方法和 PyTorch 的简单实现

分词和词性标注是自然语言处理(NLP)中的两个关键过程，因为这些任务的性能显著影响下游任务，特别是在没有特定分词书写规则的语言中，如高棉语、汉语、日语等。

过去，分词和词性标注是分开学习。然而，单词的分离方式与其词性密切相关。因此，在一个学习过程中考虑这两项任务有望取得更好的效果。

在本文中，我们将着眼于联合高棉语分词和词性标注任务。这里解释的方法是由 Bouy 等人在题为“ [**使用深度学习的联合高棉语分词和词性标注**](https://arxiv.org/abs/2103.16801) ”的论文中提出的。

![](img/d804eb15698a38ee5af2f3e3e68e842c.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

![](img/d2feec8ccac55ef38fb1bd4f60ef54ea.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

# 概观

*   输入:高棉语句子中的字符序列(图 6)
*   输出:每个字符的位置和单词分隔符的标签序列(图 6)
*   模型:双向长短期记忆(双 LSTM)神经网络(图 5)

## 预测过程

*   首先，将输入句子转换成字符级序列。

![](img/9030c891873dc6270f2b7dac4bb08a28.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

*   然后，该序列被编码成 132 维的一键向量。

![](img/9a8c8d6b8c71b8a042bf7571e7a81248.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

*   编码的表示向量被用作 2 栈双 LSTM 的输入。
*   在最后的时间步骤中，Bi-lstm 的隐藏向量被连接以获得单个特征向量。

![](img/f89df7e195fbad528a9ac21f0c2d63d0.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

*   最后一层是前馈神经网络，利用这个特征向量来预测每个字符的标签。这里，softmax 函数被用作激活函数，因此，输出序列是预定义标签(词性标签和单词边界标签)的概率序列。

![](img/aee717f3f6ed6e120517b9f863fb5360.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

*   对于输出标签，“NS”代表“没有空格”，这意味着被标记的字符不是一个单独的位置。除此之外的其他标签，比如“PRO”、“NN”、“VB”，同时代表一个词段的开头及其词性。在下图中，带有“PRO”标签的字符是单词“ខ្ញុំ”(表示我)的开头，其 POS 标签是“PRO”(代词)，而带有“VB”标签的字符是单词“ស្រលាញ់”(表示爱情)的开头，其 POS 标签是“VB”(动词)。

![](img/4a9bc129c5220b274950c1ea0a3311c8.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

![](img/7c83012543dccff2dd5ad764fc989fec.png)

图自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

## 学问

*   作者在提出的模型的训练过程中使用了交叉熵损失函数。
*   学习目标是最小化交叉熵损失函数。

![](img/1b6e18e283c1a4cbe3f68a1b16aa9cc2.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

学习配置

![](img/65243638da8ba8784610c018cabaeee0.png)

图来自[【1】](https://arxiv.org/pdf/2103.16801.pdf)

我已经在 PyTorch 中实现了所提出的方法，并使用相同的超参数在本文中介绍的数据集上训练了模型。下图显示了使用引入方法的联合分词和词性标注的示例。有用！！！！

![](img/85b60b4de36593c825306ad72de32a11.png)

使用引入方法的联合分词和词性标注示例(图由 Mengsay 提供)

# 履行

现在让我们在 PyTorch 中实现它。在下面的实现中，我使用了一个来自 https://github.com/yekyaw-thu/khPOS[的数据集，如论文中所介绍的。](https://github.com/yekyaw-thu/khPOS)

*   本文的完整实现可以在 [Github](https://github.com/loem-ms/KhmerNLP/tree/main/JointKhmerSegmentationPOSTagging) 上获得。
*   [PyTorch 教程](https://pytorch.org/tutorials/)

## 数据预处理

可用数据集为“[word1]/[POS] [word2]/[POS]……”格式。所以，我们需要把它转换成字符序列和位置序列。这里的另一个警告是，在论文中，作者通过组合一些 POS 标签来修改原始数据集。详情请看论文。

![](img/a1151efcf441335c18792cc64fea6c74.png)

示例代码(数据处理 1/3)

![](img/32a04f2aef9a5dbe7fbfebd78117470a.png)

示例代码(数据处理 2/3)

![](img/e77d3c6b096343656445ceaf638d2d33.png)

示例代码(数据处理 3/3)

## 数据集和数据加载器

![](img/f6dec27803c8414f71ce16b4cc464bd8.png)

示例代码(数据集 1/1)

![](img/60455c041aba52c5db704cf04435bf6e.png)

示例代码(数据加载器 1/2)

![](img/ab80c9730ca43601b6f90d942889e2e8.png)

示例代码(数据加载器 2/2)

## 模型

![](img/7e1cf559895b1fb0d9d5bbf3e63b3698.png)

示例代码(模型)

## 培训和测试

![](img/8a28e8ff7db88c018eed23c2e7f0f551.png)

准备数据加载器

![](img/09f6c8fe11859adb21f2a334caca98ce.png)

初始化模型和相关的东西

![](img/348ba207905c5a49b4c175c4ac57baa9.png)

火车步

![](img/2c6bd7fe8340a6895d32c2a204233bfb.png)

测试步骤

## 参考

[1] Rina 浮标和 Nguonly Taing 和 Sokchea Kor。使用深度学习的联合高棉语分词和词性标注。 [arXiv:2103.16801](https://arxiv.org/abs/2103.16801) ，2021。