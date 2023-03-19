# 使用 Pytorch 的抽象摘要

> 原文：<https://towardsdatascience.com/abstractive-summarization-using-pytorch-f5063e67510?source=collection_archive---------5----------------------->

## 总结任何文本使用变压器在几个简单的步骤！

![](img/3e1c6fad52c36f06db738bc3c0fe65cf.png)

照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

[**抽象摘要**](https://paperswithcode.com/task/abstractive-text-summarization) 是 [**自然语言处理(NLP)**](https://en.wikipedia.org/wiki/Natural_language_processing) 中的任务，目的是生成源文本的简明摘要。与抽象概括不同，**抽象概括不只是简单地从源文本中复制重要的短语，还可能提出相关的新短语**，这可以被视为释义。抽象概括在不同的领域产生了大量的应用，从书籍和文献，到科学和研发，到金融研究和法律文件分析。

迄今为止，**最新最有效的抽象摘要方法是使用** [**转换器**](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)) **模型，这些模型是在摘要数据集**上专门调整的。在本文中，我们演示了如何通过几个简单的步骤，使用强大的模型轻松地总结文本。我们将要使用的模型已经过预训练，因此不需要额外的训练:)

所以事不宜迟，我们开始吧！

# 步骤 1:安装变压器库

我们将要使用的库是 Huggingface 的 [Transformers。如果你不熟悉变形金刚，你可以继续阅读我以前的文章，看看它能做什么:](https://github.com/huggingface/transformers)

[](/top-nlp-libraries-to-use-2020-4f700cdb841f) [## 2020 年将使用的顶级 NLP 库

### AllenNLP，Fast.ai，Spacy，NLTK，TorchText，Huggingface，Gensim，OpenNMT，ParlAI，DeepPavlov

towardsdatascience.com](/top-nlp-libraries-to-use-2020-4f700cdb841f) 

要安装变压器，您只需运行:

```
pip install transformers
```

*注意:变压器要求事先安装 Pytorch。如果您还没有安装 Pytorch，请前往* [*Pytorch 官网*](https://pytorch.org/) *并按照其说明进行安装。*

# 步骤 2:导入库

成功安装 transformers 后，现在可以开始将其导入到 Python 脚本中。我们还可以导入`os`来设置环境变量，供 GPU 在下一步中使用。请注意，这完全是可选的，但如果您有多个 GPU(如果您使用的是 Jupyter 笔记本)，这是一个防止溢出到其他 GPU 的好做法。

```
from transformers import pipeline
import os
```

# 步骤 3:设置要使用的 GPU 和模型

如果您决定设置 GPU(例如 0)，那么您可以如下所示进行设置:

```
os.environ["CUDA_VISIBLE_DEVICES"] = "0"
```

现在，我们可以选择要使用的总结模型了。Huggingface 提供了两个强大的总结模型可以使用: **BART** (bart-large-cnn)和 **t5** (t5-small，t5-base，t5-large，t5–3b，t5–11b)。你可以在他们的官方论文里读到更多关于他们的内容([巴特论文](https://arxiv.org/abs/1910.13461)、 [t5 论文](https://arxiv.org/abs/1910.10683))。

要使用在 [CNN/Daily Mail 新闻数据集](https://www.tensorflow.org/datasets/catalog/cnn_dailymail)上训练的 BART 模型，您可以通过 Huggingface 的内置管道模块直接使用默认参数:

```
summarizer = pipeline("summarization")
```

如果您想使用 t5 模型(例如 t5-base)，它是在 [c4 通用抓取网页语料库](https://www.tensorflow.org/datasets/catalog/c4)上训练的，那么您可以这样做:

```
summarizer = pipeline("summarization", model="t5-base", tokenizer="t5-base", framework="tf")
```

你可以参考 [Huggingface 文档](https://huggingface.co/transformers/main_classes/pipelines.html#transformers.SummarizationPipeline)了解更多信息。

# 步骤 4:输入要总结的文本

现在，在我们准备好模型之后，我们可以开始输入我们想要总结的文本。想象一下，我们想总结以下关于新冠肺炎疫苗的文字，摘自[medicine net](https://www.medicinenet.com/script/main/art.asp?articlekey=250899)的一篇文章:

> 一个月前，美国开始了全国 COVID 疫苗接种运动，这已经成为一个麻烦，这项努力终于获得了真正的动力。
> 
> 美国疾病控制和预防中心(u . s . Centers for Disease Control and Prevention)周三报告称，在过去 24 小时内，近 100 万剂——更准确地说，超过 95.1 万剂——流入了美国人的手中。据哥伦比亚广播公司新闻报道，这是自推出以来一天内注射数量最多的一次，也是前一天的一大飞跃，前一天注射了不到 34 万剂。
> 
> 在联邦政府周二批准各州为 65 岁以上的人接种疫苗，并表示将发放所有可用的疫苗剂量后，这一数字可能会迅速上升。与此同时， *CBS 新闻*报道，许多州已经开放了大规模疫苗接种点，以努力让更多的人接种疫苗。

我们定义我们的变量:

```
text = """One month after the United States began what has become a troubled rollout of a national COVID vaccination campaign, the effort is finally gathering real steam.
Close to a million doses -- over 951,000, to be more exact -- made their way into the arms of Americans in the past 24 hours, the U.S. Centers for Disease Control and Prevention reported Wednesday. That's the largest number of shots given in one day since the rollout began and a big jump from the previous day, when just under 340,000 doses were given, CBS News reported.
That number is likely to jump quickly after the federal government on Tuesday gave states the OK to vaccinate anyone over 65 and said it would release all the doses of vaccine it has available for distribution. Meanwhile, a number of states have now opened mass vaccination sites in an effort to get larger numbers of people inoculated, CBS News reported."""
```

# 第五步:总结

最后，我们可以开始总结输入的文本。这里，我们声明了我们希望摘要输出的 min_length 和 max_length，并关闭了采样以生成固定的摘要。我们可以通过运行以下命令来做到这一点:

```
summary_text = summarizer(text, max_length=100, min_length=5, do_sample=False)[0]['summary_text']
print(summary_text)
```

瞧啊。我们得到我们的摘要文本:

> 疾病预防控制中心称，在过去的 24 小时内，一天内注射了超过 951，000 剂疫苗。这是自推出以来一个月内拍摄数量最多的一次。周二，联邦政府批准各州为 65 岁以上的人接种疫苗。哥伦比亚广播公司新闻报道，许多州现在已经开放了大规模疫苗接种点，以努力让更多的人接种疫苗。

如摘要文本所示，我们可以看到模型知道 24 小时相当于一天，并且足够聪明地将美国疾病控制和预防中心缩写为 CDC。此外，该模型成功地链接了第一段和第二段的信息，表明这是自上个月开始展示以来给出的最大数量的镜头。我们可以看到，这个总结模型的性能相当不错。

# 结论

恭喜你，你现在知道如何对任何文本进行抽象概括了！将所有这些放在一起，下面是 Jupyter 笔记本形式的完整代码:

仅此而已！希望你喜欢这篇文章。如果你喜欢我的作品，请随意看看我的其他文章，敬请期待更多内容:)

[](/top-nlp-books-to-read-2020-12012ef41dc1) [## 2020 年最佳 NLP 读物

### 这是我个人为自然语言处理推荐的书籍列表，供实践者和理论家参考

towardsdatascience.com](/top-nlp-books-to-read-2020-12012ef41dc1) [](/bert-text-classification-using-pytorch-723dfb8b6b5b) [## 使用 Pytorch 的 BERT 文本分类

### 文本分类是自然语言处理中的一项常见任务。我们应用 BERT，一个流行的变压器模型，对假新闻检测使用…

towardsdatascience.com](/bert-text-classification-using-pytorch-723dfb8b6b5b) [](/fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7) [## 使用 Pytorch 微调用于文本生成的 GPT2

### 使用 Pytorch 和 Huggingface 微调用于文本生成的 GPT2。我们在 CMU 图书摘要数据集上进行训练，以生成…

towardsdatascience.com](/fine-tuning-gpt2-for-text-generation-using-pytorch-2ee61a4f1ba7) [](/implementing-transformer-for-language-modeling-ba5dd60389a2) [## 为语言建模实现转换器

### 使用 Fairseq 训练变压器模型

towardsdatascience.com](/implementing-transformer-for-language-modeling-ba5dd60389a2) [](/semantic-similarity-using-transformers-8f3cb5bf66d6) [## 使用转换器的语义相似度

### 使用 Pytorch 和 SentenceTransformers 计算两个文本之间的语义文本相似度

towardsdatascience.com](/semantic-similarity-using-transformers-8f3cb5bf66d6) 

# 参考

[1] [抽象的文本摘要](https://paperswithcode.com/task/abstractive-text-summarization)，带代码的论文

[2] [自动摘要在企业中的 20 个应用](https://blog.frase.io/20-applications-of-automatic-summarization-in-the-enterprise/#6_Legal_contract_analysis)，Fraise

【3】[变形金刚 Github](https://github.com/huggingface/transformers) ，拥抱脸

[4] [变形金刚文档](https://huggingface.co/transformers/)，拥抱脸

[5] [Pytorch 官网](https://pytorch.org/)，脸书艾研究

[6] Lewis，Mike，et al .[“Bart:用于自然语言生成、翻译和理解的去噪序列间预训练”](https://arxiv.org/abs/1910.13461) *arXiv 预印本 arXiv:1910.13461* (2019)。

[7] Raffel，Colin，et al. [“用统一的文本到文本转换器探索迁移学习的极限”](https://arxiv.org/abs/1910.10683) *arXiv 预印本 arXiv:1910.10683* (2019)。

[8] [Tensorflow 数据集](https://www.tensorflow.org/datasets)，谷歌

[9] [美国 COVID 疫苗首次推出日均接近 100 万支](https://www.medicinenet.com/script/main/art.asp?articlekey=250899)，MedicineNet