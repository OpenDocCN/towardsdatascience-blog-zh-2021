# 药物 NER 使用 Python 中的空间

> 原文：<https://towardsdatascience.com/drugs-ner-using-spacy-in-python-f1f3091f8f4e?source=collection_archive---------13----------------------->

## 训练您自己的自定义 NER 组件来检测药品名称

![](img/4c9628ed738bab4839309897f8513d2d.png)

在 [Unsplash](https://unsplash.com/s/photos/drug?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [Myriam Zilles](https://unsplash.com/@myriamzilles?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

以前，我写过一篇关于使用 Python 中的 spaCy 进行[讽刺文本分类的文章。在本文中，您将了解更多关于命名实体识别(NER)组件的内容。](/sarcasm-text-classification-using-spacy-in-python-7cd39074f32e)

供您参考，NER 是 NLP 任务的一部分，用于定位非结构化文本中的实体并将其分类到不同的类别中。例如，给定以下句子:

```
John Doe bought 100 shares of Apple in 2020.
```

NER 模型预计将确定以下实体及其相应的类别:

*   无名氏
*   苹果公司(组织)
*   2020 年(时间)

本教程着重于训练一个自定义 NER 组件来识别药品名称。您可以很容易地修改它，使之适应您自己的用例。设置和训练过程是相同的，唯一的例外是数据集。

让我们继续下一节，开始安装必要的软件包。

# 设置

在继续之前，强烈建议您设置一个虚拟环境。

## 宽大的

您可以通过多种方式安装空间[。](https://spacy.io/usage)

您可以通过`pip install`轻松安装，如下所示:

```
pip install -U spacy
```

或者，您可以通过`conda`安装:

```
conda install -c conda-forge spacy
```

## 资料组

该数据集基于 [Reddit 评论](https://files.pushshift.io/reddit/comments/)中的药物名称，由 spaCy 开发者创建，作为示例项目的一部分。

创建一个名为`assets`的新文件夹，并从下面的存储库中下载相关的数据集:

*   [药物评价数据集](https://github.com/explosion/projects/blob/v3/tutorials/ner_drugs/assets/drugs_eval.jsonl)
*   [药物训练数据集](https://github.com/explosion/projects/blob/v3/tutorials/ner_drugs/assets/drugs_training.jsonl)

将两个数据集放置在资产文件夹中，如下所示:

*   drugs_eval.jsonl
*   drugs_train.jsonl

这两个文件都基于 JSONL 格式，每个数据点都包含以下重要字段:

*   `text` —原弦。
*   `tokens` —表示标记化单词的词典列表。
*   `spans` —标记实体的字典列表。每个条目包含开始索引、结束索引、开始令牌的 id、结束令牌的 id 和标签注释。

以下是单个数据点的示例:

```
{
  "text": "Idk if that Xanax or ur just an ass hole",
  "tokens": [
    { "text": "Idk", "start": 0, "end": 3, "id": 0 },
    { "text": "if", "start": 4, "end": 6, "id": 1 },
    { "text": "that", "start": 7, "end": 11, "id": 2 },
    { "text": "Xanax", "start": 12, "end": 17, "id": 3 },
    { "text": "or", "start": 18, "end": 20, "id": 4 },
    { "text": "ur", "start": 21, "end": 23, "id": 5 },
    { "text": "just", "start": 24, "end": 28, "id": 6 },
    { "text": "an", "start": 29, "end": 31, "id": 7 },
    { "text": "ass", "start": 32, "end": 35, "id": 8 },
    { "text": "hole", "start": 36, "end": 40, "id": 9 }
  ],
  "spans": [
    {
      "start": 12,
      "end": 17,
      "token_start": 3,
      "token_end": 3,
      "label": "DRUG"
    }
  ],
  "_input_hash": -2128862848,
  "_task_hash": -334208479,
  "answer": "accept"
}
```

在构建自己的数据集时，您可以放心地忽略其他字段。相比之下，与文本分类相比，为 NER 构建数据集要繁琐得多。

## 预处理脚本

此外，您将需要一个预处理脚本来将 JSONL 数据集转换为 spaCy 二进制格式以进行训练。在工作目录中创建一个名为`scripts`的新文件夹。

然后，创建一个名为`preprocess.py`的新 Python 文件，并在其中添加以下代码:

## 配置文件

之后，在您的工作目录中创建另一个新文件夹，并将其命名为`configs`。在其中，创建一个名为`config.conf`的新配置文件，并在文件中添加以下代码。

它包含项目的所有相关配置。您可以修改的几个关键区域在`training`部分:

```
[training]
...
patience = 1600
max_epochs = 0
max_steps = 20000
eval_frequency = 200
...
```

*   `patience` —早期停止机制无改善的步数。在这种情况下，如果 1600 步后分数没有提高，它将停止训练。
*   `max_epochs` —训练时期的最大数量。
*   `max_steps` —最大训练步数。
*   `eval_frequency` —每次评估的步骤数。在这种情况下，它将每 200 步评估一次模型。

此外，您可以在`training.score_weights`部分更改首选评分标准。

```
[training.score_weights]
ents_per_type = null
ents_f = 1.0
ents_p = 0.0
ents_r = 0.0
```

当前设置基于最佳`f-score`。如果您更喜欢拥有一辆性能良好的`precision`NER 车型，而不考虑召回，您可以对其进行如下调整:

```
[training.score_weights]
ents_per_type = null
ents_f = 0.0
ents_p = 1.0
ents_r = 0.0
```

## 项目文件

拼图的最后一块是项目文件。在同一个工作目录中，创建一个名为`project.yml`的新文件，并用以下代码填充它:

项目文件包含以下详细信息:

*   与项目相关的元数据
*   命令
*   工作流程

只需根据自己的需求对它们进行修改。例如，在训练模型时，您可以指定不同的`version`。因此，spaCy 将使用给定的`version`对输出进行后缀。

```
vars:
  config: "config.conf"
  name: "ner_drugs"
  version: "0.0.0"
  train: "drugs_train.jsonl"
  dev: "drugs_eval.jsonl"
```

如果您打算为配置文件和数据集使用不同的名称，请在`vars.train`和`vars.dev`对其进行相应的修改。

另一方面，命令是预定义的动作，可以通过以下方式触发:

```
spacy project run [command]
```

在这种情况下，项目包含以下命令:

*   `preprocess` —将数据转换为 spaCy 的二进制格式。
*   `training` —训练 NER 模型。
*   `evaluate` —评估模型并导出相关指标。
*   `package` —将训练好的模型打包成 Python 包。

要单独运行`preprocess`，只需使用以下命令:

```
spacy project run preprocess
```

除此之外，您还可以使用 workflow 将一些命令分组为一个包，如下所示:

```
workflows:
  all:
    - preprocess
    - train
    - evaluate
    - package
```

您可以通过以下命令运行上面的工作流:

```
spacy project run all
```

它将按顺序运行所有指定的命令。在这种情况下，它将首先运行`preprocess`命令，并以`package`命令结束。

一旦你完成了它，就可以进入下一部分进行实现了。

# 履行

让我们利用您之前创建的命令来训练这个模型。

## 使用工作流

让我们使用工作流来为您完成这些技巧，而不是一个一个地运行所有的命令。

```
spacy project run all
```

它将执行以下操作:

*   将数据转换为空间二进制格式
*   训练模型
*   评估训练最好的模型
*   将训练好的模型打包成 Python 模块

## 检查文件夹

在实际训练过程之前，它将检查并创建以下文件夹(如果不存在):

*   资产
*   文集
*   配置
*   培养
*   剧本
*   包装

## 预处理数据集

`preprocess`命令将加载您的 JSONL 数据集，并将其转换为`corpus`文件夹中空间所需的二进制格式。您应该得到以下文件:

*   drug_eval.jsonl.spacy
*   drug_train.jsonl.spacy

## 训练模型

一旦预处理步骤完成，它将初始化所需的管道组件`tok2vec` 和`ner`:

```
=========================== Initializing pipeline ===========================←[0m
[2021-06-24 16:36:47,013] [INFO] Set up nlp object from config
[2021-06-24 16:36:47,024] [INFO] Pipeline: ['tok2vec', 'ner']
[2021-06-24 16:36:47,029] [INFO] Created vocabulary
[2021-06-24 16:36:47,029] [INFO] Finished initializing nlp object
[2021-06-24 16:36:47,545] [INFO] Initialized pipeline components: ['tok2vec', 'ner']
✔ Initialized pipeline
```

默认情况下，spaCy 会在定型模型之前将整个数据集加载到内存中。如果您有一个大型数据集，该过程可能需要一段时间。随着训练的进行，您应该会在控制台中看到以下输出:

```
============================= Training pipeline =============================←[0m
ℹ Pipeline: ['tok2vec', 'ner']
ℹ Initial learn rate: 0.0
E    #       LOSS TOK2VEC  LOSS NER  ENTS_F  ENTS_P  ENTS_R  SCORE
---  ------  ------------  --------  ------  ------  ------  ------
  0       0          0.00     45.83    0.23    0.20    0.28    0.00
  0     200          9.60  15156.42    0.00    0.00    0.00    0.00
  0     400         21.03   1491.74    0.00    0.00    0.00    0.00
  1     600         13.16    836.43    0.00    0.00    0.00    0.00
  1     800         22.12   1227.63    0.00    0.00    0.00    0.00
  2    1000         26.70   1117.34   20.34   82.35   11.60    0.20
  3    1200         43.24   1095.54   46.64   81.94   32.60    0.47
  4    1400         67.99   1048.39   59.73   78.12   48.34    0.60
  5    1600         94.88   1039.05   65.81   78.03   56.91    0.66
```

*   `LOSS TOK2VEC` —`tok2vec`组件的损耗值。
*   `LOSS NER`—`ner`组件的损耗值。
*   `ENTS_F` — f 值从 0 到 100。
*   `ENTS_P` —精度分数从 0 到 100。
*   `ENTS_R` —回忆分数从 0 到 100。
*   `SCORE`—0.0-1.0 分，小数点后两位(四舍五入)。它基于您在`config.conf`中定义的`training.score_weights`。

训练将继续进行，直到满足以下任一条件:

*   达到了定义的`max_epochs`
*   达到了定义的`max_steps`
*   达到规定的`patience`(分数没有任何提高的步数)

然后，将最佳模型和最后模型保存在`training`文件夹中。

## 模型评估

工作流中的下一个命令是`evaluate`命令，它使用最佳模型计算指标。结果将作为`metrics.json`导出到`training`文件夹中。

```
{
  "token_acc":0.9999332072,
  "ents_p":0.7820895522,
  "ents_r":0.7237569061,
  "ents_f":0.7517934003,
  "speed":22459.7479096054,
  "ents_per_type":{
    "DRUG":{
      "p":0.7820895522,
      "r":0.7237569061,
      "f":0.7517934003
    }
  }
}
```

*   `p` —表示精度值
*   `r` —代表召回值
*   `f`—f1 值

## 打包模型

最后一步是把最好的模型打包成 Python 包。它会将打包的模型保存在`packages`文件夹中，如下所示:

```
en_ner_drugs-0.0.0
```

当您打开它时，您会找到以下文件和文件夹:

*   距离
*   能量药物
*   en_ner_drugs.egg-info
*   清单. in
*   meta.json
*   setup.py

## 将模型部署为 Python 包

通过在`setup.py`所在的目录中运行以下命令，您可以很容易地将它作为 Python 包安装在您的虚拟环境中。

```
python setup.py
```

不仅如此，你还可以复制出`en_ner_drugs`文件夹，把它当作一个普通的 Python 模块。只需将它放在 Python 应用程序的同一个目录中，并按如下方式正常调用它:

```
import en_ner_drugsnlp = en_ner_drugs.load()
```

或者，您可以通过传入以下文件路径来使用 spacy 进行加载:

```
import spacynlp = spacy.load('en_ner_drugs/en_ner_drugs-0.0.0')
```

模型加载后，您可以在任何文本上运行它，如下所示:

```
doc = nlp("This is a text")
```

输出是一个空间的 Doc 对象，它包含代表实体的`ents`属性。您可以用下面的代码调用它:

```
doc.ents
```

当处理大量文本时，您应该使用`pipe`批量运行它:

```
texts = ["This is a text", "These are lots of texts", "..."]# bad
docs = [nlp(text) for text in texts]# good
docs = list(nlp.pipe(texts))
```

请看下面的代码片段，它说明了如何在 Python 应用程序中使用该模型来获得药品名称的相关 NER。

运行该脚本时，您应该会在控制台上看到以下输出。

```
promethazine DRUG
weed DRUG
heroin DRUG
```

# 结论

让我们回顾一下你今天所学的内容。

这篇文章首先简要介绍了命名实体识别(NER)。

接下来，通过`pip`或`conda`安装空间。此外，还提供了训练和评估数据集作为链接。

然后，对整个实现过程进行了说明，包括将 JSONL 格式的数据集预处理为 spaCy 的二进制格式、训练模型、模型评估以及将最佳模型打包为 Python 包。

最后，它提供了一个关于 Python 部署以及如何获取相关实体及其相应标签的示例。

感谢阅读这篇文章。希望在下一个教程中再见到你！

# 参考

# 参考

1.  [空间——命名实体](https://spacy.io/usage/linguistic-features#named-entities)
2.  [spaCy —解析器和 NER](https://spacy.io/api/architectures#parser)
3.  [spaCy Github — NER 药品](https://github.com/explosion/projects/tree/v3/tutorials/ner_drugs)