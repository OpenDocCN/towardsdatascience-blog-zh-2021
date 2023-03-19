# 用于释义生成的训练 T5

> 原文：<https://towardsdatascience.com/training-t5-for-paraphrase-generation-ab3b5be151a2?source=collection_archive---------14----------------------->

![](img/b289703c2b012e40633a40c401b1c172.png)

使用 [Imgflip](https://imgflip.com/memegenerator) 生成的图像

在我之前的博客[中](/textgenie-augmenting-your-text-dataset-with-just-2-lines-of-code-23ce883a0715)谈到了 [TextGenie](https://github.com/hetpandya/textgenie) ，我提到了我在从零开始收集文本数据和使用 T5(文本到文本转换转换器)生成的释义作为扩充文本数据的方法之一时所面临的问题。看过模型的运行后，让我们来体验一下培训过程😉

如果您希望全程跟随我，您可以在我的 Github repo 上的[这里](https://github.com/hetpandya/paraphrase-datasets-pretrained-models/blob/main/examples/t5_paraphrase_model_training_example.ipynb)找到培训笔记本。

**提示:**如果你没有 GPU，我建议使用 [Google Colaboratory](https://colab.research.google.com) 来训练模型。

# 安装依赖项

在继续之前，让我们准备好所有需要的包，使用:

```
pip install simpletransformers datasets tqdm pandas
```

# 资料组

我们将使用 [TaPaCo](https://huggingface.co/datasets/tapaco) 数据集来完成我们的任务。该数据集由 73 种语言的总共 190 万个句子组成，我们将从中提取`English`语言的句子。

## 预处理数据集(可选)

在将数据集输入模型之前，需要将其转换成成对的输入句子和目标句子。预处理的代码可以在[这里](https://github.com/hetpandya/paraphrase-datasets-pretrained-models/tree/main/datasets/tapaco#storing-original-dataset-as-csv)以及笔记本中找到。

## 下载已经预处理的数据集

如果您不想对数据进行预处理，我已经为您完成了任务。你可以直接从[这里](https://github.com/hetpandya/paraphrase-datasets-pretrained-models/raw/main/datasets/tapaco/tapaco_paraphrases_dataset.csv)下载数据集的预处理版本。

## 加载数据集

完成后，您可以按以下方式加载数据集:

```
import pandas as pddataset_df = pd.read_csv("tapaco_paraphrases_dataset.csv",sep="\t")
```

加载后，需要重命名数据的列。另外，我们需要给每个句子加一个前缀。这里，前缀可以是作为列添加的任何文本，每行具有相同的值。

```
# Renaming the columns
dataset_df.columns = ["input_text","target_text"]# Adding a prefix. Here we shall keep "paraphrase" as a prefix.
dataset_df["prefix"] = "paraphrase"
```

## 分割数据集

我们将以 90%-10%的比例分割数据集

```
from sklearn.model_selection import train_test_splittrain_data,test_data = train_test_split(dataset_df,test_size=0.1)
```

# 训练模型

该模型需要调整某些参数，如下所示:

从`simpletransformers`初始化`T5Model`类对象:

```
from simpletransformers.t5 import T5Model
import sklearnmodel = T5Model("t5","t5-small", args=args)
```

我们现在将采用`t5-small`模式。让我们继续培训:

```
model.train_model(train_data, eval_data=test_data, use_cuda=True,acc=sklearn.metrics.accuracy_score)
```

# 使用训练好的模型进行加载和预测

模型训练可能需要几个小时。一旦培训完成，您可能会在`outputs`目录中找到最终的模型。它可以加载为:

## 加载已训练的模型

```
from simpletransformers.t5 import T5Model
import osroot_dir = os.getcwd()
trained_model_path = os.path.join(root_dir,"outputs")args = {
"overwrite_output_dir": True,
"max_seq_length": 256,
"max_length": 50,
"top_k": 50,
"top_p": 0.95,
"num_return_sequences": 5
}trained_model = T5Model("t5",trained_model_path,args=args)
```

## 使用所训练的模型生成释义

让我们看看模型在我们的自定义输入下表现如何:

```
prefix = "paraphrase"
pred = trained_model.predict([f"{prefix}: The house will be cleaned by me every Saturday."])print(pred)#Output:[['My home will be cleaned on Saturdays.',   
'I will clean the house every Saturday.',   
'The house is going to be clean every Saturday.',   
"I'll clean the house every Saturday.",   
'I will clean the house every Saturday.']]
```

而且很管用！！耶！

T5 车型培训到此为止。我已经开源了预训练模型和预处理数据集，以便在我的 [Github repo](https://github.com/hetpandya/paraphrase-datasets-pretrained-models) 上解释，如果你想探索它们的话。

感谢您的阅读😄