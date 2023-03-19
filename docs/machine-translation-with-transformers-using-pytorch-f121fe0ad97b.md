# 使用 Pytorch 的变压器机器翻译

> 原文：<https://towardsdatascience.com/machine-translation-with-transformers-using-pytorch-f121fe0ad97b?source=collection_archive---------23----------------------->

## 只需简单的几个步骤就可以将任何语言翻译成另一种语言！

![](img/0ee67c058338b7ae9595a094cf39f026.png)

[皮斯特亨](https://unsplash.com/@pisitheng?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

[**翻译**](https://en.wikipedia.org/wiki/Machine_translation) ，或者更正式地说，机器翻译，是处理从一种语言到另一种语言的翻译的[**【NLP】**](https://en.wikipedia.org/wiki/Natural_language_processing)中最流行的任务之一。在早期，翻译最初只是简单地用一种语言中的单词替换另一种语言中的单词。然而，这样做不会产生好的结果，因为语言是根本不同的，所以需要更高层次的理解(如短语/句子)。**随着深度学习的出现，现代软件现在采用了统计和神经技术，这些技术被证明在做翻译时更有效。**

当然，每个人都可以使用强大的 [Google Translate](https://translate.google.com/) ，但是如果你想知道如何用代码实现翻译，这篇文章将教你如何实现。**本文将展示如何使用**[**hugging face Transformers**](https://huggingface.co/transformers/)**提供的简单 API 轻松实现翻译，这是一个基于**[**py torch**](https://pytorch.org/)**的库。**

现在事不宜迟，让我们开始吧！

# 教程概述

*   安装库
*   英语到德语的翻译示例
*   自定义语言翻译示例

# 安装库

在安装[变形金刚](https://github.com/huggingface/transformers)库之前，你需要安装一个 [Pytorch](https://pytorch.org/) 的工作版本。你可以去它的[官网](https://pytorch.org/)安装 Pytorch。

安装 Pytorch 后，您可以通过以下方式安装变压器:

`pip install transformers`

# 英语到德语的翻译示例

现在，我们准备好做翻译了！如果你想做英语到德语的翻译，那么你可以从导入变形金刚中相关的`pipeline`模块开始:

```
from transformers import pipeline
```

`pipeline`是一种通过使用简单的 API 对不同任务进行推理的简单方法。你可以在这里了解更多关于`pipeline`模块[的信息。](https://huggingface.co/transformers/main_classes/pipelines.html)

要进行英语到德语的翻译，您需要一个专门针对这项任务进行微调的模型。 [**T5**](https://arxiv.org/abs/1910.10683) 是一个已经在包含英德翻译数据集的大规模 [c4 数据集](https://www.tensorflow.org/datasets/catalog/c4)上训练的模型，因此我们可以直接将该模型用于翻译管道(我们使用的是`t5-base`变体):

```
translation = pipeline(“translation_en_to_de”)
## same with 
## translation = pipeline("translation_en_to_de", model="t5-base", tokenizer="t5-base")
```

注意，我们没有在这行代码中指定任何模型，因为默认情况下，`t5-base`用于翻译。如果您想要指定您自己的模型和记号赋予器，您可以通过指定模型和记号赋予器参数来添加一个`model`和`tokenizer`(如果它们在 Huggingface 中提供的话)，或者构建您自己的模型和记号赋予器，如下例所示(如果社区提供的话)。关于翻译管道的更多细节，你可以在这里参考官方文档[。](https://huggingface.co/transformers/main_classes/pipelines.html#transformers.TranslationPipeline)

然后，您可以定义要翻译的文本。让我们试着翻译一下:

> 我喜欢研究数据科学和机器学习

```
text = "I like to study Data Science and Machine Learning"
```

最后，现在您可以使用管道提供的 API 来翻译和设置一个`max_length`(例如 40 个令牌):

```
translated_text = translation(text, max_length=40)[0]['translation_text']
print(translated_text)
```

瞧吧！几十秒后，我们得到德文翻译:

> 我学的是现代科学和机械

# **自定义语言翻译示例**

如果你想翻译任意两种自定义语言，比如说从英语到汉语，那么你需要一个专门针对这一特定任务进行优化的模型。幸运的是，有了 Huggingface 建立的社区，你很可能不需要收集自己的数据集，并在其上微调你的模型。你可以直接前往 [Huggingface 的模型网站](https://huggingface.co/models?filter=translation)查看不同语言对的翻译模型列表。

对于我们从英文翻译成中文的例子，我们可以使用 HelsinkiNLP 的[英汉预训练模型并直接使用它。首先，我们首先导入必要的模块:](https://huggingface.co/Helsinki-NLP/opus-mt-en-zh)

```
from transformers import AutoModelWithLMHead, AutoTokenizer
```

然后，我们可以通过以下方式构建我们的模型和标记器:

```
model = AutoModelWithLMHead.from_pretrained("Helsinki-NLP/opus-mt-en-zh")
tokenizer = AutoTokenizer.from_pretrained("Helsinki-NLP/opus-mt-en-zh")
```

现在，我们可以将我们想要翻译、建模和标记化的语言对输入管道:

```
translation = pipeline("translation_en_to_zh", model=model, tokenizer=tokenizer)
```

类似于前面的例子，我们可以使用相同的代码来定义我们的文本和翻译:

```
text = "I like to study Data Science and Machine Learning"
translated_text = translation(text, max_length=40)[0]['translation_text']
print(translated_text)
```

等待几秒钟后，您应该会看到中文版的文本！

> 我喜欢学习数据科学和机器学习

# 结论

恭喜你！现在，您应该能够知道如何使用 Huggingface 及其社区提供的预训练模型来实现翻译。如果您想看看完整的代码是什么样子，这里是 Jupyter 代码:

仅此而已！如果您有任何问题，请随时在下面提问。如果你喜欢我的作品，你可以关注我并注册我的时事通讯，这样每当我发表一篇新文章，你都会得到通知！喜欢的话也可以看看我之前的文章。下次再见，:D

</abstractive-summarization-using-pytorch-f5063e67510>  </semantic-similarity-using-transformers-8f3cb5bf66d6>  </bert-text-classification-using-pytorch-723dfb8b6b5b>  </fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7>  </implementing-transformer-for-language-modeling-ba5dd60389a2>  

# 参考

[1] [变形金刚 Github](https://github.com/huggingface/transformers) ，拥抱脸

[2] [变形金刚官方文档](https://huggingface.co/transformers/)，拥抱脸

[3] [Pytorch 官网](https://pytorch.org/)，脸书艾研究

[4] Raffel，Colin，et al. [“用统一的文本到文本转换器探索迁移学习的极限”](https://arxiv.org/abs/1910.10683) *arXiv 预印本 arXiv:1910.10683* (2019)。

[5] [Tensorflow 数据集](https://www.tensorflow.org/datasets)，谷歌