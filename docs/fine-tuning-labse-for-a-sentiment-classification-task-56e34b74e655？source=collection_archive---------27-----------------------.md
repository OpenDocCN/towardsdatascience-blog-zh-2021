# 针对情感分类任务微调“LaBSE”

> 原文：<https://towardsdatascience.com/fine-tuning-labse-for-a-sentiment-classification-task-56e34b74e655?source=collection_archive---------27----------------------->

![](img/23a5feca6992ed0277d8406be2506b99.png)

照片由[丹尼斯·莱昂](https://unsplash.com/@denisseleon?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**一些背景**

多语言语言模型(让我们称之为“MLM ”)由于其提供多语言单词嵌入(或句子、文档等)的能力，近来已经成为 NLP 领域的趋势。)在单个模型内。“预训练”是与 MLMs 一起出现的另一个术语，它告诉我们，模型已经在不同领域的大型语料库上训练过，因此我们不必从头开始再次训练它们，但我们可以针对所需的目标任务“微调”它们，同时利用来自预训练知识的“知识转移(转移学习)”。传销主要由谷歌、脸书、百度等科技巨头发布供公众使用，因为他们有资源来训练这些具有数百万、数十亿甚至数万亿参数的大型模型。LaBSE[1]就是 Google 发布的这样一个模型，基于 BERT 模型。

LaBSE 或“语言不可知的 BERT 句子嵌入”专注于双文本挖掘、句子/嵌入相似性任务。它使用“单词块”标记化，可以为 109 种语言生成句子嵌入([CLS]标记从模型的最终层嵌入表示句子嵌入)。虽然，他们还没有报道该模型在其他下游任务如分类或命名实体识别(NER)中的性能，也没有太多用于这类下游任务。LaBSE 的架构是一个“双编码器”模型(带附加余量 Softmax 的双向双编码器)，这意味着它有两个基于“BERT-base”模型编码器的编码器模块。这两个编码器分别对源句子和目标句子进行编码，并提供给一个评分函数(余弦相似度)来对它们的相似度进行排序。LaBSE 的训练损失函数就是建立在这个评分的基础上的，这个评分就是前面提到的“加性边际 Softmax”。

**设置事物**

(*我会尽量保持简单，同时包括重要的观点*)LaBSE 的官方或原始模型由作者发布到“tensor flow Hub”([https://www.tensorflow.org/hub/](https://www.tensorflow.org/hub/))，我将使用它。该模块依赖于 Tensorflow (2.4.0+将是伟大的，我正在使用 2.5.0)有其他必要的库，他们可以使用 pip 安装。

> ***注意:*** *到目前为止(据我所知)Conda 环境不支持 tfhub 模型+ GPU。如果您尝试使用这样的设置，它将总是(自动)退回到 Tensorflow 的 CPU 版本或抛出错误。因此，如果使用 GPU，Conda 应该不在考虑范围内。(*[)https://github.com/tensorflow/text/issues/644](https://github.com/tensorflow/text/issues/644)

首先，需要安装一些必要的库，(我用的是 Ubuntu 机器)

```
!pip install tensorflow-hub!pip install tensorflow-text # Needed for loading universal-sentence-encoder-cmlm/multilingual-preprocess!pip install tf-models-official
```

我们可以进口它们，

```
import tensorflow as tfimport tensorflow_hub as hubimport tensorflow_text as text from official.nlp import optimization 
```

显然，如果需要的话，你也应该导入其他的公共库(numpy，pandas，sklearn ),我不会在这里提到。

对于分类任务，我们可以使用预先训练的 109 种语言的任何标记数据集(或者也可以是不支持的，这没有关系！但是有一些性能下降)。我们要做的是对模型进行微调，即使用额外的数据集(与庞大的预训练数据集或语料库相比，较小的数据集)训练预训练模型，以便将模型微调到我们特定的分类(或任何其他)任务。作为起点，我们可以使用 IMDb 电影评论数据集。([https://ai.stanford.edu/~amaas/data/sentiment/](https://ai.stanford.edu/~amaas/data/sentiment/))。因此，我们的任务将变成“二元情感分类”任务。数据集由 25k 训练和 25k 测试数据组成。

```
!wget -c [https://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz](https://ai.stanford.edu/~amaas/data/sentiment/aclImdb_v1.tar.gz) -O — | tar -xzAUTOTUNE = tf.data.AUTOTUNE
batch_size = 32 #8 #16
seed = 42raw_train_ds = tf.keras.preprocessing.text_dataset_from_directory(
 ‘aclImdb/train’,
 batch_size=batch_size,
 validation_split=0.2,
 subset=’training’,
 seed=seed)class_names = raw_train_ds.class_names
train_ds = raw_train_ds.cache().prefetch(buffer_size=AUTOTUNE)val_ds = tf.keras.preprocessing.text_dataset_from_directory(
 ‘aclImdb/train’,
 batch_size=batch_size,
 validation_split=0.2,
 subset=’validation’,
 seed=seed)val_ds = val_ds.cache().prefetch(buffer_size=AUTOTUNE)test_ds = tf.keras.preprocessing.text_dataset_from_directory(
 ‘aclImdb/test’,
 batch_size=batch_size)test_ds = test_ds.cache().prefetch(buffer_size=AUTOTUNE)
```

我们可以使用 Tensorflow 构建的数据管道作为输入数据。(*如果使用 TF 的早期版本，该功能可能无法完全或至少不能直接使用*。).接下来，我们需要“预处理”这些数据，然后输入到我们将要构建的模型中。之后，预处理的数据可以被编码或嵌入向量空间。为此，我们可以定义以下变量，我们将使用 LaBSE 的版本 2([https://tfhub.dev/google/LaBSE/2](https://tfhub.dev/google/LaBSE/2)，版本 1 是发布到 TFhub 的初始模型)

```
tfhub_handle_preprocess=”[https://tfhub.dev/google/universal-sentence-encoder-cmlm/multilingual-preprocess/2](https://tfhub.dev/google/universal-sentence-encoder-cmlm/multilingual-preprocess/2)"
tfhub_handle_encoder=”[https://tfhub.dev/google/LaBSE/2](https://tfhub.dev/google/LaBSE/2)"
```

![](img/9c82a6afaba6f71a63d2ba5235e134f1.png)

照片由[斯蒂夫·约翰森](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**建立模型**

接下来，我们可以建立模型。下面，已经定义了一个函数来建立具有一些特定层的模型。

```
def build_classifier_model():
 text_input = tf.keras.layers.Input(shape=(), dtype=tf.string, name=’text’)
 preprocessing_layer = hub.KerasLayer(tfhub_handle_preprocess, name=’preprocessing’)
 encoder_inputs = preprocessing_layer(text_input)
 encoder = hub.KerasLayer(tfhub_handle_encoder, trainable=True, name=’LaBSE_encoder’)
 outputs = encoder(encoder_inputs)
 net = outputs[‘pooled_output’]
 net = tf.keras.layers.Dropout(0.1)(net)
 net = tf.keras.layers.Dense(1, name=’classifier’)(net)
 return tf.keras.Model(text_input, net)
```

你可以注意到模型中有一个术语“pooled_outputs ”,它指的是本文前面提到的句子的[CLS]令牌表示。(另一种输出形式是‘sequence _ outputs’)。编码器层上的“trainable = True”参数或标志意味着，在不使用数据集进行微调的同时，我们也可以更新原始模型的权重/参数的权重。(这也被称为“全局微调”)。

```
encoder = hub.KerasLayer(tfhub_handle_encoder, trainable=True, name=’LaBSE_encoder’)

net = outputs[‘pooled_output’]
```

如果我们希望保持原始模型的权重不变，那么它将被称为“基于特征的微调”或“固定维度方法”(在文献中有不同的术语)。此外，在充当模型的分类器层的编码器(下降和密集)之后添加了一些附加层。这是在文献中经常发现的这种分类的组合。

对于优化器，Adam 或 AdamW(更多)是首选，我们可以像下面这样设置。基于数据集或任务，学习率可能会改变(在大多数情况下会降低)。Keras([https://keras.io/guides/keras_tuner/](https://keras.io/guides/keras_tuner/))或类似“Wandb”([https://wandb.ai/site](https://wandb.ai/site))的服务中已经提供的超参数优化方法也可用于寻找最佳参数，如学习速率和批量大小。

```
from tensorflow_addons.optimizers import AdamW
step = tf.Variable(0, trainable=False)
schedule = tf.optimizers.schedules.PiecewiseConstantDecay(
 [1000], [5e-5,1e-5])
lr = 1 * schedule(step)
wd = lambda: 1e-6 * schedule(step)
optimizer=AdamW(learning_rate=lr,weight_decay=wd)
```

接下来，可以用 mode.fit()方法对模型进行编译和训练。

```
classifier_model.compile(loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),
 optimizer=optimizer,
 metrics=tf.keras.metrics.BinaryAccuracy(threshold=0.0))history = classifier_model.fit(train_ds,validation_data=val_ds,
                               epochs=epochs, batch_size=32)
```

时期的数量可以设置为 3、4 或 5，这通常是足够的。我们也可以包括像“提前停止”这样的方法。对这样的大型模型使用“K-Fold 交叉验证”并不常见。相反，我们可以对输入数据选择使用不同的随机种子多次运行这个模型。从模型中构建、运行和获得结果是相当容易的。经过训练的模型可以被保存并用于预测新的数据等。如张量流模型通常所做的那样。

*在我看来，*与 XLM-R、*或*等模型相比，LaBSE 模型在文本分类等任务上可能表现不佳，因为 LaBSE 最初是为双文本挖掘或句子相似性任务而构建和训练的，为了获得更好的结果，可能需要更多的训练数据(微调数据)。LaBSE 在文献中也没有太多用于分类任务(根据我的知识和论文本身对谷歌学术的 38 次引用*)*。对于这里的任务，我获得了略高于 50%的准确度、精确度、召回率和 f1 分数。这也是用一些随机选择的超参数完成的，因此如果超参数也被改变/调整，结果可能会得到改善。(使用 LaBSE 版本 1([https://medium . com/swlh/language-agnostic-text-classification-with-LaBSE-51 a4 f 55 dab 77](https://medium.com/swlh/language-agnostic-text-classification-with-labse-51a4f55dab77))进行了一些类似的工作，但是使用了更大的训练数据集。)

无论如何，我希望听到反馈，评论或其他人的经验。所以。请随意评论您的想法和建议。感谢阅读！！！

[](https://github.com/VinuraD) [## VinuraD -概述

### 阻止或报告基于 Python 的 web 应用程序，用于查看 SDWAN 网络隧道 Python 1 2 一个基于 GoLang 的程序，用于解析…

github.com](https://github.com/VinuraD) 

**参考文献**

[1] —语言不可知的伯特语句嵌入，冯，杨，丹尼尔 Cer，纳文阿里瓦扎甘，，[arXiv:2007.01852 v1](https://arxiv.org/abs/2007.01852v1)**【cs .CL]**