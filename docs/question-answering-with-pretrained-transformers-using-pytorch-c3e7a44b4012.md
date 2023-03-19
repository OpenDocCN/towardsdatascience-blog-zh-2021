# 使用 PyTorch 回答关于预调变压器的问题

> 原文：<https://towardsdatascience.com/question-answering-with-pretrained-transformers-using-pytorch-c3e7a44b4012?source=collection_archive---------10----------------------->

## 学会在几分钟内用任何语言建立一个问答系统

![](img/4062a7e44bb78307ab7ce845a1da9033.png)

乔恩·泰森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 介绍

[**问答**](https://en.wikipedia.org/wiki/Question_answering) 是在 [**信息检索**](https://en.wikipedia.org/wiki/Information_retrieval) 和 [**自然语言处理(NLP)**](https://en.wikipedia.org/wiki/Natural_language_processing) 中的一个任务，调查可以用自然语言回答人类提问的软件。在 [**抽取问题回答**](https://rajpurkar.github.io/SQuAD-explorer/) 中，提供了一个上下文，以便模型可以参考它并对答案在文章中的位置做出预测。

在这篇文章中，我们将向你展示如何使用[**hugging face Transformers 库提供的预训练模型来实现问题回答。由于实现非常简单，您可以在几分钟内让您的问答系统快速工作！**](https://github.com/huggingface/transformers)

现在，让我们开始吧！

# 教程概述

*   步骤 1:安装库
*   步骤 2:导入库
*   步骤 3:构建问题回答管道
*   步骤 4:定义背景和要问的问题
*   第五步:回答问题
*   奖金:任何语言的问题回答

# 步骤 1:安装库

我们将使用[变形金刚库](https://github.com/huggingface/transformers)来回答问题。要安装它，只需运行:

```
pip install transformers
```

注意:如果你还没有安装，记得去 PyTorch 官方网站看看！

# 步骤 2:导入库

成功安装转换器后，现在可以将库导入到 python 脚本中:

```
from transformers import pipeline
```

我们将只使用`[pipeline](https://huggingface.co/transformers/main_classes/pipelines.html)` [模块](https://huggingface.co/transformers/main_classes/pipelines.html)，它是一个抽象层，提供一个简单的 API 来执行各种任务。

# 步骤 3:构建问题回答管道

现在，我们可以开始建设管道。要构建问答管道，我们可以简单地做:

```
question_answering = pipeline(“question-answering”)
```

这将创建一个预先训练的问题回答模型，以及它的后台标记器。这种情况下使用的默认模型是`DistilBERT-base`，它是在[小队数据集](https://rajpurkar.github.io/SQuAD-explorer/)上微调的。你可以在其[论文](https://arxiv.org/abs/1910.01108)中了解更多关于 DistilBERT 的信息。

要使用您自己的模型和标记器，您可以将它们作为`model`和`tokenizer`参数传递给管道。

# 步骤 4:定义背景和要问的问题

现在，是时候创建我们的上下文和我们想问模型的问题了。让我们从维基百科中抓取一个快速的[机器学习定义作为上下文:](https://en.wikipedia.org/wiki/Machine_learning)

> 机器学习(ML)是对通过经验自动改进的计算机算法的研究。它被视为人工智能的一部分。机器学习算法基于样本数据(称为“训练数据”)建立模型，以便在没有明确编程的情况下进行预测或决策。机器学习算法被用于各种各样的应用中，例如电子邮件过滤和计算机视觉，在这些应用中，开发常规算法来执行所需的任务是困难的或不可行的。

```
context = """
Machine learning (ML) is the study of computer algorithms that improve automatically through experience. It is seen as a part of artificial intelligence. Machine learning algorithms build a model based on sample data, known as "training data", in order to make predictions or decisions without being explicitly programmed to do so. Machine learning algorithms are used in a wide variety of applications, such as email filtering and computer vision, where it is difficult or unfeasible to develop conventional algorithms to perform the needed tasks.
"""
```

问它以下问题:

> 机器学习模型基于什么？

```
question = "What are machine learning models based on?"
```

# 第五步:回答问题

最后，是时候测试我们的模型来回答我们的问题了！我们可以通过将上下文和问题作为参数传递给实例化的管道来运行问答模型，并打印出结果:

```
result = question_answering(question=question, context=context)print("Answer:", result['answer'])
print("Score:", result['score'])
```

输出应该是:

> 答案:样本数据，
> 得分:0 . 46860 . 46866868661

从输出中，我们应该能够看到作为“样本数据”的答案，这是正确的，还可以看到它的置信度得分，在这种情况下，我认为它相当高。

# 奖金:任何语言的问题回答

等等，但是你可能会问，除了英语，我们如何实现其他语言的问答？在你走之前，我想这可能是你想知道的。

幸运的是，我们有一个由我们亲爱的社区发布的模型库，这些模型可能已经用你们的语言进行了问答训练。我们可以去 [Huggingface 模特网站](https://huggingface.co/models?filter=question-answering)查看可供问答的模特。

假设我们想用中文回答问题。我们可以使用经过多种语言训练的多语言模型。例如，这个[多语言 BERT](https://huggingface.co/mrm8488/bert-multi-cased-finetuned-xquadv1) 是在 Deepmind 的 [xQuAD 数据集](https://github.com/deepmind/xquad)(一个[小队数据集](https://rajpurkar.github.io/SQuAD-explorer/)的多语言版本)上训练的，它支持 11 种语言:阿拉伯语、德语、希腊语、英语、西班牙语、印地语、俄语、泰语、土耳其语、越南语和中文。

现在，根据[模型文档](https://huggingface.co/mrm8488/bert-multi-cased-finetuned-xquadv1)，我们可以通过指定其`model`和`tokenizer`参数直接在管道中构建模型，如下所示:

```
question_answering = pipeline("question-answering", model="mrm8488/bert-multi-cased-finetuned-xquadv1",
    tokenizer="mrm8488/bert-multi-cased-finetuned-xquadv1")
```

然后，让我们将上下文设置为:

> 机器学习是人工智能的一个分支。 是个人很热门的专业。

```
context = """机器学习是人工智能的一个分支。 是个人很热门的专业。"""
## Machine Learning is a branch of Artificial Intelligence. It is a popular major.
```

要问的问题是:

> 机器学习是什么的分支？

```
question = "机器学习是什么的分支？"
## What is Machine Learning a branch of?
```

之后，我们可以使用与前面相同的代码来运行系统:

```
result = question_answering(question=question, context=context)print("Answer:", result['answer'])
print("Score:", result['score'])
```

系统将输出:

> Answer: 机器学习是人工智能的一个分支。
> Score: 0.9717233777046204

懂中文的就知道输出答案翻译成“机器学习是人工智能的一个分支”，没错！

# 结论

今天就到这里吧！现在，您应该知道如何使用预先训练的模型实现任何语言的问答系统。如果你想一次性查看全部代码，我在下面附上了一个 Jupyter 笔记本:

如果你有任何问题或讨论，欢迎在下面评论！希望很快见到大家。如果你喜欢我的作品，请随意浏览我以前的其他帖子:

</semantic-similarity-using-transformers-8f3cb5bf66d6>  </abstractive-summarization-using-pytorch-f5063e67510>  </machine-translation-with-transformers-using-pytorch-f121fe0ad97b>  </bert-text-classification-using-pytorch-723dfb8b6b5b>  </fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7>  

# 参考

[1] [变形金刚 Github](https://github.com/huggingface/transformers) ，拥抱脸

[2] [变形金刚官方文档](https://huggingface.co/transformers/)，拥抱脸

[3] [Pytorch 官网](https://pytorch.org/)，艾研究

[4] Sanh，Victor，et al. [“蒸馏伯特，伯特的蒸馏版:更小，更快，更便宜，更轻。”](https://arxiv.org/abs/1910.01108) *arXiv 预印本 arXiv:1910.01108* (2019)。

[5] [小队官网](https://rajpurkar.github.io/SQuAD-explorer/)，斯坦福

[6] [XQuAD 官方 Github](https://github.com/deepmind/xquad) ，谷歌 Deepmind