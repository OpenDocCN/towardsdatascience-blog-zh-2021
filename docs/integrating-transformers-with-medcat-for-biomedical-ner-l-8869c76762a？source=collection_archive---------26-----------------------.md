# (使)融入🤗带 MedCAT 的变压器，用于生物医学 NER+L

> 原文：<https://towardsdatascience.com/integrating-transformers-with-medcat-for-biomedical-ner-l-8869c76762a?source=collection_archive---------26----------------------->

![](img/7a2582a634b4492303dc26acac99c6fd.png)

由 [Irwan iwe](https://unsplash.com/@aboutiwe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/medical-record?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

生物医学 NER+L 致力于从电子健康记录(EHRs)中的自由文本中提取概念，并将它们链接到大型生物医学数据库，如 SNOMED-CT 和 UMLS。

医学概念注释工具包(MedCAT)使用基于 Word2Vec 的浅层神经网络来检测和消除生物医学概念的歧义。这种方法给了我们:1)无监督训练；2)检测数百万个独特概念的可能性；3)训练速度快，资源要求低；4)仅从正面例子中学习的能力；

对于一些我们有足够训练样本的用例，基于变压器(如 BERT)的监督学习方法可能更合适。

在这项工作中，我们展示了如何整合🤗使用 MedCAT 的数据集/转换器，或者更准确地说，我们展示了如何将 MedCATtrainer 导出(手动注释的项目)转换为🤗数据集和训练 a🤗变压器型号。

先决条件:

*   熟悉 [MedCAT](https://github.com/CogStack/MedCAT) ( [TDS 教程](/medcat-introduction-analyzing-electronic-health-records-e1c420afa13a))、 [MedCATtrainer](https://github.com/CogStack/MedCATtrainer) 、[拥抱脸变形金刚和数据集](https://huggingface.co/transformers/)

随附的 Jupyter 笔记本可以在 MedCAT [资源库](https://github.com/CogStack/MedCAT/blob/master/notebooks/BERT%20for%20NER.ipynb)中找到。

# 数据准备

[MedCATtrainer](https://github.com/CogStack/MedCATtrainer/) 用于手动注释任何生物医学概念的自由文本文档(例如 [SNOMED](https://www.snomed.org/) 或 [UMLS](https://www.nlm.nih.gov/research/umls/index.html) )。注释过程完成后，项目可以以如下所示的`.json`格式导出(有限视图):

```
{'projects': [{'name': '<something>', 
               'documents': [{'text': '<some text>', 
                              'annotations': [{'start': 23,
                                               'end': 32,
                                               'cui': <label>},
                                              ...]}]}]}
```

简化对一个🤗Transformer 模型，我们将 JSON 输出转换成🤗数据集。我们只保留 JSON 中的重要特性，而丢弃其他特性:

```
features=datasets.Features(
{
"id": datasets.Value("int32"),
"text": datasets.Value("string"),
"ent_starts": datasets.Sequence(datasets.Value("int32")),
"ent_ends": datasets.Sequence(datasets.Value("int32")),
"ent_cuis": datasets.Sequence(datasets.Value("string")),
}),
```

这里，`id`是文档的 ID，`text`是文档的文本，`ent_starts`是文档中所有被手工注释的实体的起始字符的位置列表，`ent_ends`是结束字符的位置，`ent_cuis`是标签。请注意，MedCATtrainer 使用在线学习，虽然用户能够创建新实体，但大多数实体都由 MedCAT 预先注释，并由用户简单验证(正确/不正确)。因此，当在 dataset 类中生成示例时，我们只保留`correct`示例和那些由用户添加的`manually_created`示例，换句话说:

```
for entity in document['annotations']:
    if entity.get('correct', True) or entity.get('manually_created', False):
        # Use the annotation
        ...
```

加载`.json`文件现在很简单:

```
import os
import datasets
from medcat.datasets import medcat_nerDATA_PATH = '<path to my .json export from medcattrainer>'
dataset=datasets.load_dataset(os.path.abspath(medcat_ner.__file__), 
                              data_files=DATA_PATH, 
                              split=datasets.Split.TRAIN)
```

一旦数据集被加载，我们需要把它转换成正确的格式🤗变形金刚模型。也就是说，对文本进行标记并分配标签。我们写了一个包装器🤗tokenizers，这将照顾一切:

```
from medcat.datasets.tokenizer_ner import TokenizerNER
from transformers import AutoTokenizerhf_tokenizer = AutoTokenizer.from_pretrained('<name>')
id2type = {}
for i in range(hf_tokenizer.vocab_size):
    id2type[i] = 'sub' if hf_tokenizer.convert_ids_to_tokens(i).startswith("##") else 'start'
tokenizer = TokenizerNER(hf_tokenizer, id2type=id2type)
```

使用新的标记器，我们可以将数据集转换成所需的格式:

```
encoded_dataset = dataset.map(
 lambda examples: tokenizer.encode(examples, ignore_subwords=True),
 batched=True,
 remove_columns=['ent_cuis', 'ent_ends', 'ent_starts', 'text'])
```

数据集现在看起来像这样:

```
Dataset({
    features: ['id', 'input_ids', 'labels'],
    num_rows: 4935
})
```

# 培训 a🤗变形金刚模型

如果我们的用例具有相对较少数量的独特概念(不是几十个，几千个)，我们可以使用来自🤗使用 TokenClassification 头:

```
# It is important to set the num_labels, which is the number of unique conceptsmodel = AutoModelForTokenClassification.from_pretrained("emilyalsentzer/Bio_ClinicalBERT", num_labels=len(tokenizer.label_map))
```

为了对数据进行批处理并填充到需要的地方，我们使用了来自`MedCAT.datasets`的`CollateAndPadNER`，对于`metrics`，我们编写了一个简单的函数来打印令牌分类报告。现在一切都准备好了，我们从🤗并运行培训:

```
trainer = Trainer(
    model=model,                         
    args=training_args,                 
    train_dataset=encoded_dataset['train'],       
    eval_dataset=encoded_dataset['test'],     
    compute_metrics=metrics,
    data_collator=collate_fn,
    tokenizer=None # We've tokenized earlier
)
trainer.train()
```

# 结果

我们在 MedMentions (MM)上测试性能，因为它是一个相当完整的数据集，有大量的注释(它并不完美，因为注释者有一些分歧，但它已经足够好了)。

该模型在三个不同版本的 MM 上进行测试:1)整个数据集；2)只有频率在 300 以上的概念；3)只有 1000 以上的频率。

## 完整的 MM 数据集(测试集中的 13069 个概念)

BERT 最糟糕的用例，大量的概念，其中大多数有<10 occurrences. As it can be seen BERT cannot handle this use-case at all — at least not in this form. All scores are macro averaged.

*   MedCAT (unsupervised): F1=0.67, P=0.80, R=0.69
*   BERT: F1=0.0, R=0.01, P=0.0

## Only concepts with frequency > 300 个(测试集中有 107 个概念)

第一个用例在医疗保健领域是相当标准的，大量的概念带有不同数量的注释。稍微不太标准的是，我们已经为每个概念添加了不少注释。有趣的是表演几乎是一样的。

*   MedCAT(监督):P=0.50，R=0.44，F1=0.43
*   伯特:P=0.47，R=0.46，F1=0.43

## 仅频率> 1000 的概念(测试集中有 12 个概念)

这个用例应该最适合 BERT，因为我们只关注具有大量训练数据的概念。像这样的用例非常罕见，这一个有点特殊，因为出现这么多次的概念主要是定性概念——它们非常多样(许多不同的术语属于同一个概念),更适合类似 BERT 的模型。

*   MedCAT(受监督):F1=0.34，P=0.24，R=0.70
*   伯特:F1=0.59，P=0.60，R=0.59

# 结论

生物医学 NER+1 是一项艰巨的任务，就像其他任何事情一样，一个模型并不适合所有情况。我们表明，对于具有有限数量的训练样本或相对较低的术语方差的用例，基于 Word2Vec 的浅层神经网络是更好的选择。但是，如果我们有大量的训练样本和较高的长期方差，基于 BERT 的模型表现更好。

最后，这两种方法现在都是 MedCAT 的一部分。