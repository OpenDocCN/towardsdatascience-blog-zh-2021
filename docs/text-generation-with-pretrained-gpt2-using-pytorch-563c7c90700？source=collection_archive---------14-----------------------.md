# 使用 PyTorch 通过预训练的 GPT2 生成文本

> 原文：<https://towardsdatascience.com/text-generation-with-pretrained-gpt2-using-pytorch-563c7c90700?source=collection_archive---------14----------------------->

## 使用 Huggingface 框架快速轻松地生成任何语言的文本

![](img/9624ebd561401ab86b4c238f9574e070.png)

照片由[布里吉特·托姆](https://unsplash.com/@brigittetohm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 介绍

[**文本生成**](https://en.wikipedia.org/wiki/Natural-language_generation) 是近年来 [**【自然语言处理】**](https://en.wikipedia.org/wiki/Natural_language_processing) 最激动人心的应用之一。我们大多数人可能听说过 [GPT-3](https://arxiv.org/abs/2005.14165) ，这是一个强大的语言模型，可以生成接近人类水平的文本。然而，像这样的模型由于其庞大的体积而极难训练，**因此在适用的情况下，通常首选预训练模型。**

**在这篇文章中，我们将教你如何使用预训练的**[**GPT-2**](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)**，GPT-3 的更轻的前身来生成文本。我们将使用由 [Huggingface](https://huggingface.co/) 开发的著名的[变形金刚库](https://github.com/huggingface/transformers)。如果你想知道如何在你自己的定制数据集上微调 GPT-2 以生成特定领域的文本，那么你可以参考我以前的帖子:**

[](/fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7) [## 使用 Pytorch 微调用于文本生成的 GPT2

### 使用 Pytorch 和 Huggingface 微调用于文本生成的 GPT2。我们在 CMU 图书摘要数据集上进行训练，以生成…

towardsdatascience.com](/fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7) 

如果使用预训练的 GPT-2 是足够的，你在正确的地方！事不宜迟，我们开始教程吧！

# 教程概述

*   步骤 1:安装库
*   步骤 2:导入库
*   步骤 3:构建文本生成管道
*   步骤 4:定义开始生成的文本
*   第五步:开始生成
*   额外收获:生成任何语言的文本

# 步骤 1:安装库

要安装[拥抱脸变形金刚](https://github.com/huggingface/transformers)，我们需要确保 PyTorch 已经安装。如果你还没有安装 PyTorch，去[它的官方网站](https://pytorch.org/)，按照它的指示安装。

安装 PyTorch 后，您可以通过运行以下命令来安装 Huggingface Transformers:

```
pip install transformers
```

# 步骤 2:导入库

成功安装变压器后，您现在可以导入其管道模块:

```
from transformers import pipeline
```

`[pipeline](https://huggingface.co/transformers/main_classes/pipelines.html)` [模块](https://huggingface.co/transformers/main_classes/pipelines.html)是一个抽象层，它消除了代码的复杂性，并允许以一种简单的方式执行不同的 NLP 任务。

# 步骤 3:构建文本生成管道

现在，我们可以开始构建文本生成的管道。我们可以通过以下方式做到这一点:

```
text_generation = pipeline(“text-generation”)
```

文本生成管道的默认模型是 GPT-2，这是最流行的基于解码器的语言生成转换模型。

# 步骤 4:定义开始生成的文本

现在，我们可以开始定义我们想要生成的前缀文本。让我们给它一个更一般的起始句:

> 这个世界是

```
prefix_text = "The world is"
```

# 第五步:开始生成

在我们定义了我们的起始文本之后，现在是时候进行生成了！我们可以通过运行以下命令来实现:

```
generated_text= text_generation(prefix_text, max_length=50, do_sample=False)[0]
print(generated_text[‘generated_text’])
```

上面的代码指定了 50 个令牌的 max_length，并关闭了采样。输出应该是:

> 如果你是个好人，世界会变得更好。
> 
> 我不是说你应该是个坏人。我是说你应该做个好人。
> 
> 我不是说你应该是个坏人

正如我们所看到的，计算机如何能够生成有意义的文本是非常令人着迷的，尽管它并不完美。输出的一个问题是它在最后是重复的。这可能可以通过使用不同的解码方案(例如[top-k](https://arxiv.org/abs/1805.04833)/[top-p sampling](https://arxiv.org/abs/1908.04319))和使用不同的值来解决，但这超出了本文的范围。要了解更多关于解码方案以及如何实现它的信息，请查看 [Huggingface 的官方教程](https://huggingface.co/blog/how-to-generate)和 [TextGeneration 管道文档](https://huggingface.co/transformers/main_classes/pipelines.html#transformers.TextGenerationPipeline)。

# 额外收获:生成任何语言的文本

首先，要生成另一种语言的文本，我们需要一个之前在该语言的语料库上训练过的语言模型；否则；我们将不得不自己进行微调，这是一项繁琐的任务。幸运的是，Huggingface 提供了一个由热情的 NLP 社区发布的模型列表([链接此处](https://huggingface.co/models))，并且有可能一个语言模型已经根据您选择的语言进行了微调。

假设我们想要生成中文文本。CKIPLab 的这个 [GPT2 模型是在中文语料库上预训练的，所以我们可以使用他们的模型，而不需要我们自己进行微调。](https://huggingface.co/ckiplab/gpt2-base-chinese)

遵循[他们的文档](https://github.com/ckiplab/ckip-transformers)，我们可以从导入相关的记号化器和模型模块开始:

```
from transformers import BertTokenizerFast, AutoModelWithLMHead
```

然后，我们可以相应地构建标记器和模型:

```
tokenizer = BertTokenizerFast.from_pretrained(‘bert-base-chinese’)
model = AutoModelWithLMHead.from_pretrained(‘ckiplab/gpt2-base-chinese’)
```

接下来，我们将新的标记器和模型作为参数来实例化管道:

```
text_generation = pipeline(“text-generation”, model=model, tokenizer=tokenizer)
```

之后，我们再次定义我们的前缀文本，这次用中文:

> 我 想 要 去

```
prefix_text = "我 想 要 去"
## I want to go 
```

使用与上面相同的代码，我们现在可以从前缀文本生成:

```
generated_text= text_generation(prefix_text, max_length=50, do_sample=False)[0]
print(generated_text['generated_text'])
```

现在，您应该会看到以下输出:

> 我 想 要 去 看 看 。 」 他 說 : 「 我 們 不 能 說, 我 們 不 能 說, 我 們 不 能 說, 我 們 不 能 說, 我 們 不 能 說, 我 們 不 能 說, 我 們
> 
> ##我想四处看看。他说:“我们不能说，我们不能说，我们不能说，我们不能说，我们不能说，我们不能说。”

尽管生成的文本远非完美，但这是另一篇文章的主题。

# 结论

现在你有了！希望您现在知道如何使用 Huggingface 提供的简单 API 接口和预训练模型来实现文本生成。为了您的方便，我在这里附上了一个 Jupyter 笔记本:

就是这样！希望你喜欢这篇文章。如果你有任何问题，欢迎在下面评论。此外，请订阅我的电子邮件列表，以接收我的新文章。有兴趣也可以看看我以前的帖子:)

[](/question-answering-with-pretrained-transformers-using-pytorch-c3e7a44b4012) [## 使用 Pytorch 回答关于预调变压器的问题

### 学会在几分钟内建立一个任何语言的问答系统！

towardsdatascience.com](/question-answering-with-pretrained-transformers-using-pytorch-c3e7a44b4012) [](/machine-translation-with-transformers-using-pytorch-f121fe0ad97b) [## 使用 Pytorch 的变压器机器翻译

### 只需简单的几个步骤就可以将任何语言翻译成另一种语言！

towardsdatascience.com](/machine-translation-with-transformers-using-pytorch-f121fe0ad97b) [](/abstractive-summarization-using-pytorch-f5063e67510) [## 使用 Pytorch 的抽象摘要

### 总结任何文本使用变压器在几个简单的步骤！

towardsdatascience.com](/abstractive-summarization-using-pytorch-f5063e67510) [](/semantic-similarity-using-transformers-8f3cb5bf66d6) [## 使用转换器的语义相似度

### 使用 Pytorch 和 SentenceTransformers 计算两个文本之间的语义文本相似度

towardsdatascience.com](/semantic-similarity-using-transformers-8f3cb5bf66d6) [](/bert-text-classification-using-pytorch-723dfb8b6b5b) [## 使用 Pytorch 的 BERT 文本分类

### 文本分类是自然语言处理中的一项常见任务。我们应用 BERT，一个流行的变压器模型，对假新闻检测使用…

towardsdatascience.com](/bert-text-classification-using-pytorch-723dfb8b6b5b) 

# 参考

语言模型是一次性学习者。 *arXiv 预印本 arXiv:2005.14165* (2020)。

拉德福德、亚历克等人[“语言模型是无人监督的多任务学习者。”](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) *OpenAI 博客* 1.8 (2019): 9。

[变形金刚 Github](https://github.com/huggingface/transformers) ，拥抱脸

[变形金刚官方文档](https://huggingface.co/transformers/)，拥抱脸

[Pytorch 官网](https://pytorch.org/)、脸书 AI 研究

范，安琪拉，，还有杨多芬。[“分层神经故事生成”](https://arxiv.org/abs/1805.04833) *arXiv 预印本 arXiv:1805.04833* (2018)。

韦勒克，肖恩，等人[“不太可能训练的神经文本生成”](https://arxiv.org/abs/1908.04319) *arXiv 预印本 arXiv:1908.04319* (2019)。

中国科学院信息科学研究所和语言研究所的中文知识与信息处理