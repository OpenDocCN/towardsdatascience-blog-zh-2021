# 在拥抱脸中使用数据加载器

> 原文：<https://towardsdatascience.com/using-a-dataloader-in-hugging-face-52388f552259?source=collection_archive---------15----------------------->

## PyTorch 版本

每一个深入 DL 世界的人都可能听说，相信，或者是被试图说服的目标，这是一个 [**变形金刚**](https://arxiv.org/pdf/1706.03762.pdf) 的时代。自从第一次出现，**变形金刚**就成了几个方向大量研究的对象:

*   研究人员寻找建筑的改进。
*   人们研究管理这个领域的理论。
*   搜索可能使用此方法的应用程序。

那些旨在深入研究**变形金刚**的读者可能会找到大量详细讨论[变形金刚](https://www.coursera.org/lecture/nlp-sequence-models/transformer-network-Kf5Y3)的资源。简而言之，转换器通常用于为 NLP 问题开发语言模型。这些模型用于构建句子、Q & A 和翻译等任务。在一个非常高级的描述中，转换器可以被认为是复杂的自动编码器，它接收键、值和查询(单词)三元组作为输入，并研究一个语言模型，其中每个单词都有一个依赖于其语义上下文的特定表示。

# 伯特&拥抱脸

BERT ( **来自变压器**的双向编码器表示)在此[引入](https://arxiv.org/pdf/1810.04805.pdf)。随着变形金刚的出现， **BERT** 的想法是采用变形金刚预先训练好的模型，并根据特定任务对这些模型的权重进行微调(**下游任务**)。这种方法产生了一类新的 NLP 问题，可以通过最初使用转换器来解决，例如分类问题(例如情感分析)。这是通过将网络的上层修改成集群的结构或不同类型的序列来实现的。因此，我们有许多伯特模型。这样一个伟大的“模特银行”就是[抱脸](https://huggingface.co/)。该框架提供了一个包含三个基本组件的包:

*   各种预先训练的模型和工具
*   令牌化引擎
*   框架灵活性(例如 Torch、Keras)

这个软件包可以处理大量的 NLP 任务。

# 那我为什么要写帖子呢？

当我开始使用拥抱脸时，我对它提供的优秀的“端到端”管道以及它提供的数据结构的便利性印象深刻。然而，我觉得他们的教程中有一部分没有很好地涵盖。在我自己设法找到解决方案后，我觉得作为一个“激进的开源”我必须分享它。

为了说明这个问题，我将简要描述拥抱脸提供的特征提取机制。我们给出的数据很简单:文档和标签。

最基本的函数是**记号赋予器:**

```
**from** transformers **import** AutoTokenizer
tokens = tokenizer.batch_encode_plus(documents )
```

这个过程将文档映射成**变形金刚的**标准表示，因此可以直接用于拥抱脸的模型。这里我们提出一个通用的特征提取过程:

```
**def** regular_procedure(tokenizer, documents , labels ):
    tokens = tokenizer.batch_encode_plus(documents )

    features=[InputFeatures(label=labels[j], **{key: tokens[key][j]   
    **for** key **in** tokens.keys()}) **for** j **in** range(len(documents ))]
    **return** features
```

该方法的输出列表:*特性*是一个可以用于训练和评估过程的列表。我发现的一个障碍是缺乏使用 **Dataloader 的教程。**

在所有教程中，假设我们使用训练/评估期间可用的数据。这个假设对于新兵训练营的需求来说是明确的，但是对于现实世界的任务来说是错误的。我们正在处理大数据:

**在大数据中，代码指向数据，而不是数据指向代码**

我开始尝试。我的目标是创建一个能够用 PyTorch **数据加载器访问的特性文件夹。**我的第一次尝试如下:

```
**def** generate_files_no_tensor(tokenizer, documents, labels ):
    tokens  = tokenizer.batch_encode_plus(documents )

    file_pref =**"my_file_"
    for** j **in** range(len(documents) ):
            inputs = {k: tokens[k][j] **for** k **in** tokens}
            feature = InputFeatures(label=labels[j], **inputs)
            file_name = file_pref +**"_"**+str(j)+**".npy"** np.save(file_name, np.array(feature))
    **return**
```

这段代码运行得很好，但不是最佳的。它的主要缺点是节省了 **numpy** 个对象，而抱紧的模型需要**个张量**。这意味着我的 **__getitem__** 函数将有额外的任务，但上传文件:

```
**def** __getitemnumpy__(self, idx):
    aa = np.load(self.list_of_files[idx], allow_pickle=**True**)
    cc = aa.data.obj.tolist()
    c1 = cc.input_ids
    c2 = cc.attention_mask
    c3 = cc.label
    **return** torch.tensor(c1), torch.tensor(c2), c3
```

在训练过程中，我们需要将数量级的对象转换成张量。

我决定以一种不同的方式工作:我开发了我的 **__getitem__** 并强迫数据“承认它的规则”。

```
**def** __getitem__(self, idx):
    aa = torch.load(self.list_of_files[idx])
    **return** aa[0], aa[1], aa[2]
```

现在让我们面对挑战。让我们试试这个:

```
**def** generate_files_no_tensor(tokenizer, documents, labels ):
    tokens  = tokenizer.batch_encode_plus(documents )

    file_pref =**"my_file_"
    for** j **in** range(len(documents) ):
            inputs = {k: tokens[k][j] **for** k **in** tokens}
            feature = InputFeatures(label=labels[j], **inputs)
            file_name = file_pref +**"_"**+str(j)+**".pt"** torch.save(file_name, feature)
    **return**
```

有用！我们甚至可以直接接触到张量。但是这个循环非常慢。当我观察这些数据时，我看到了两个“现象”:

*   所有文件都有相同的大小
*   文件很大！！

我花了一段时间才意识到所有的文件都保存了整个张量(它们可能保存了一个指向它的位置的点)。因此，我们必须将它切片。

```
**def** generate_files_with_tensor(tokenizer, documents, labels ):
    tokens = tokenizer.batch_encode_plus(doc0, return_tensors=**'pt'**)

    file_pref =**"my_file_"
    for** j **in** range(len(documents) ):
        file_name = file_pref +**"_"**+str(j)+**".pt"** input_t =  torch.squeeze(torch.index_select(tokens[**"input_ids"**],dim=0,
index=torch.tensor(j)))
        input_m =     torch.squeeze(torch.index_select(tokens[**"attention_mask"**],dim=0,
        index=torch.tensor(j)))
        torch.save([input_t, input_m, labels[j]], file_name)
    **return**
```

**index_select** 函数对张量进行切片，挤压允许移除尺寸为 1 的尺寸。它达到了要求。现在我有一个快速的 **__getitem__** 除了上传数据什么也不做。

具有数据结构和**数据加载器**的代码示例存在于[这里](https://github.com/natanka/Keras_this_and_that/blob/patch-1/huggings_data_loader_examp.py)。

希望你会觉得有用。