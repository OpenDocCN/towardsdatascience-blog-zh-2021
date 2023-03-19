# 知识民主化的文本简化

> 原文：<https://towardsdatascience.com/text-simplification-for-the-democratization-of-knowledge-5b3647e4a52?source=collection_archive---------16----------------------->

## **学习深度学习使用变形金刚进行文本简化**

![](img/a9295a6c1d1245cac5ab01ac0b0ad2a1.png)

[钳工](https://unsplash.com/@benchaccounting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/simple?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

语言无时无刻不在我们身边。它的一个先决条件是它的可理解性。所以没有相互理解，语言就失去了目的。干净清晰的言语和语言享有很高的声誉，这不是巧合。只有这样，我们才能跨越领域的边界分享知识和想法。只有这样，我们才能确保信息以民主的方式传播。在信息将成为下一个重要资源的时代，这一点比以往任何时候都更加重要。作为教育的基础，它必须平等地提供给每个人。

技术，特别是人工智能可以帮助我们达到这个目标。自然语言处理是最令人兴奋的机器学习领域之一，不仅仅是自变形金刚以来。它允许我们自动化复杂的语言任务，这些任务只能由具有特殊知识的人来执行。但是正如已经说过的，知识应该自由流动。

我将在文章中解释的项目应该是实现这个目标的一小步。它被设计成我参与的数据科学务虚会的最后一个项目。所以这个项目更多的是关于学习和发现，而不是立刻改变世界。

本文将是三篇系列文章中的第一篇。在这一篇中，我将讨论使用拥抱脸库的 transformer 实现。第二个是关于数据集的准备等。第三个是关于一个自制的变压器。他们是一个更大项目的一部分。查看相应的 [GitHub repo](https://github.com/chrislemke/deep-martin) 以获得概述。

现在让我们开始吧！

有[抱脸](https://huggingface.co)就没必要多此一举了🤗。对于每个不知道拥抱脸的人来说:它是 NLP 中使用变形金刚最著名和最常用的库之一。我不会深入细节，但我强烈建议检查他们的东西。
使用拥抱人脸序列到序列模型不仅节省了很多麻烦——从头编写一个转换器——而且通过使用他们预先训练的编码器和解码器打开了一个广阔的新领域。

## 编码器-解码器模型又名序列到序列模型

所以一切都是这样开始的:

我们创建一个 EncoderDecoderModel，用 BERT (bert-base-uncased)免费提供的检查点初始化编码器和解码器。编码器和解码器参数的哪种组合是最佳的，因使用情况而异。我开始使用 BERT 的权重作为编码器和解码器参数，最后使用 RoBERTa，因为它的训练语料更广泛。尝试和分析所有可能的组合是一项相当艰巨的任务。因此，我们应该感到高兴的是，Sascha Rothe、Shashi Narayan 和 Aliaksei Severyn 在他们出色的论文中为我们做了这些。

运行上面的代码会给我们一些消息:

```
Some weights of the model checkpoint at bert-base-uncased were not used when initializing BertModel: ['cls.predictions.decoder.weight', 'cls.seq_relationship.weight', 'cls.predictions.transform.dense.bias', 'cls.seq_relationship.bias', 'cls.predictions.transform.LayerNorm.bias', 'cls.predictions.transform.LayerNorm.weight', 'cls.predictions.bias', 'cls.predictions.transform.dense.weight']
- This IS expected if you are initializing BertModel from the checkpoint of a model trained on another task or with another architecture (e.g. initializing a BertForSequenceClassification model from a BertForPreTraining model).
- This IS NOT expected if you are initializing BertModel from the checkpoint of a model that you expect to be exactly identical (initializing a BertForSequenceClassification model from a BertForSequenceClassification model).
Some weights of the model checkpoint at bert-base-uncased were not used when initializing BertLMHeadModel: ['cls.seq_relationship.bias', 'cls.seq_relationship.weight']
- This IS expected if you are initializing BertLMHeadModel from the checkpoint of a model trained on another task or with another architecture (e.g. initializing a BertForSequenceClassification model from a BertForPreTraining model).
- This IS NOT expected if you are initializing BertLMHeadModel from the checkpoint of a model that you expect to be exactly identical (initializing a BertForSequenceClassification model from a BertForSequenceClassification model). Some weights of BertLMHeadModel were not initialized from the model checkpoint at bert-base-uncased and are newly initialized: ['bert.encoder.layer.7.crossattention.output.dense.bias', 'bert.encoder.layer.9.crossattention.self.query.bias'...]
You should probably TRAIN this model on a down-stream task to be able to use it for predictions and inference.
```

这条消息在开始时可能看起来令人困惑。但实际上，它只是告诉我们 CLS 层——我们的 seq2seq 模型不需要它——没有初始化。它还告诉我们，来自交叉注意层的许多权重是随机初始化的。如果我们看看编码器(BERT)，这是有意义的，它没有交叉注意层，因此不能为它提供任何参数。

现在，我们已经有了一个编码器-解码器模型，它提供了与这种类型的其他模型相同的功能，如[巴特](https://huggingface.co/transformers/model_doc/bart.html)、[先知](https://huggingface.co/transformers/model_doc/prophetnet.html)或 [T5](https://huggingface.co/transformers/model_doc/t5.html) 。唯一的区别是我们的模型使用了预先训练好的 BERT 权重。

下一步是设置记号赋予器。抱脸也让这一步变得极其简单。我们现在需要它来与模型配置共享一些参数，但主要任务我们将在稍后看到——一旦我们谈到培训。在 tokenizer 的帮助下，我们现在可以配置模型的一些参数。

要配置的激励参数是第二块中的参数。
用 *length_penalty* 我们推模型，使简化文本自动比原始文本短。 *num_beams* 参数解释起来有点复杂。综上所述，就是关于序列中要考虑多少个接续词来计算概率的问题。请查看[这个](http://The exciting parameters to configure are the ones in the second block. with 'length_penalty' we push the model in the direction that the simplified text is automatically shorter than the original text. The 'num_beams' parameter is a bit more complicated to explain. In summary, it is about how many continuation words should be considered in the sequence to calculate the probability.  Please check out this block post to get a detailed picture about Beam search: https://huggingface.co/blog/how-to-generate#beam-search)伟大的 block 帖子，获得光束搜索的详细图片。

## 模型

就是这个！我们热启动的 seq2seq 模型现在可以进行微调了。下一步是设置所有必要的训练参数。有关完整列表，请参考[文档](https://huggingface.co/transformers/main_classes/trainer.html#trainingarguments)。我只谈其中的一部分。

*predict_with_generate* 应为其中之一，将其设置为“真”时，将在训练时计算诸如流星或胭脂等度量。稍后我们将讨论度量标准。重要的是要知道，损失和评估损失作为度量在文本简化中不像在其他深度学习应用中那样有意义。
为了加快训练速度，减少 GPU 的内存使用，我们启用 *fp16* 使用 16 位精度，而不是 32 位。我们希望这个设置不会引起渐变消失，这在使用变形金刚时是很危险的。
*gradient _ accumulation _ steps*走的方向差不多。它决定了在执行反向路径之前累积了多少个更新步骤。当在单个 GPU 上使用大模型时，这是在缺乏 GPU 内存的情况下无法立即运行的一种可能性。

接收训练参数作为参数的 *Seq2SeqTrainer* 也需要一个 *compute_metrics* 函数。

[METEOR](https://en.wikipedia.org/wiki/METEOR) metric(使用显式排序评估翻译的度量)，顾名思义，它实际上是为翻译而设计的。而且在文本简化中，评价文本的质量也是一个很有实用价值的问题。该指标基于单字精度和召回率的调和平均值，召回率的权重高于精度。

从*预测*对象中提取出*标签标识*和*预测*后，我们对它们进行解码。第 11 行确保我们正确地替换了 pad 令牌。之后，我们计算流星和胭脂度量，并将其作为字典返回。

现在一切都准备好了，我们可以开始训练了。根据数据集，这可能需要一段时间。我上一次使用 1.3M 行数据集时花了 60 个小时。所以让我们等着吃饼干吧🍪。

## 估价

最后，我们可以使用我们的模型来简化一些文本。

输入的文本显然应该有点复杂，否则，简化是没有意义的。我从维基百科上挑选了量子力学的介绍。这应该值得简化。

让我们快速浏览一下代码。幸运的是它再次使用拥抱脸:

在第八行，我们加载刚刚训练的模型。我们也可以使用之前的同一个实例。但是我们可能不会马上使用它。出于同样的原因，我们也再次加载记号赋予器。这次我们可以从我们的模型中加载它。

然后我们对 *input_text* 进行标记，这样它就为模型的处理做好了准备。我们给它的*最大长度*为 60——其余的将被填充或切断——取决于长度。

使用 *trained_model.generate* ，我们生成简化文本。在这里，我们可以调整参数，如*温度*或*光束数量*来改善结果。

## 接下来

这个对项目一部分的简明介绍仅仅是个开始。我已经写了以下关于数据集和所有需要的典型工作的文章，所以它符合模型。与此同时，请随意查看一下 [GitHub 库](https://github.com/chrislemke/deep-martin)。在那里，您可以找到我在文章中谈到的所有代码，以及后续文章的代码。