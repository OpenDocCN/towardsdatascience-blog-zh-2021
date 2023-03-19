# 微调用于语法纠正的转换器模型

> 原文：<https://towardsdatascience.com/fine-tune-a-transformer-model-for-grammar-correction-b5c8ca49cc26?source=collection_archive---------31----------------------->

## 学习如何训练一个名为 T5 的转换器模型成为你自己的语法校正器

![](img/851f5cbb6532e2dbb73476fe384154d6.png)

作者图片

在本文中，我们将讨论如何训练一个最先进的转换器模型来执行语法纠正。我们将使用一个名为 T5 的模型，它目前在通用语言理解评估(GLUE)基准上的表现优于人类基准，使其成为现有的最强大的 NLP 模型之一。T5 由 Google AI 创建，并向全世界发布，供任何人下载和使用。

在本教程中，我们将使用我自己的 Python 包 [Happy Transformer](https://github.com/EricFillion/happy-transformer) 。快乐变形金刚建立在[拥抱脸的变形金刚库](https://github.com/huggingface/transformers)之上，只需几行代码就可以轻松实现和训练变形金刚模型。因此，理解本教程的内容不需要对 NLP 或 Python 有复杂的理解，即使我们将训练世界上最有能力的人工智能模型之一。

*向下滚动到“预训练模型”部分，了解如何下载和使用我训练并上传到 Hugging Face 的模型分发网络的语法纠正模型。*

# 装置

快乐变压器在 PyPI 上可用，因此可以 pip 安装。

```
pip install happytransformer
```

# 模型

T5 有几种不同的尺寸，我们将使用[基本模型](https://huggingface.co/t5-base)，它有 2.2 亿个参数。最大的[可用模型有 110 亿个参数，而最小的](https://huggingface.co/t5-11b)[有 6000 万个参数。](https://huggingface.co/t5-small)

T5 是一个文本到文本的模型，意味着给定的文本，它根据输入生成一段独立的文本。因此，我们将从 Happy Transformer 导入一个名为 HappyTextToText 的类，我们将使用它来加载模型。我们将为第一个位置参数提供模型类型(T5 ),为第二个位置参数提供模型名称(t5-base)。

```
from happytransformer import HappyTextToText

happy_tt = HappyTextToText("T5", "t5-base")
```

# 数据收集

我们将使用一个名为 JFLEG 的著名数据集来训练该模型。根据其拥抱脸页面上的[描述](https://huggingface.co/datasets/jfleg)，它是“开发和评估 GEC 系统流畅度的黄金标准基准”(GEC 代表语法错误纠正。)此外，根据谷歌学术的说法，它的[论文](https://arxiv.org/abs/1702.04066)目前有 [106 次引用](https://scholar.google.com/scholar?cites=14012499899893589917&as_sdt=2005&sciodt=0,5&hl=en)，这表明它在 NLP 社区内确实受到尊重[1]。它受 CC BY-NC-SA 4.0 许可证的约束，这意味着您必须提供其归属，不得将其用于商业目的，并对任何衍生应用相同的许可证*。

**这不是法律建议。阅读* [*完全许可*](https://creativecommons.org/licenses/by-nc-sa/4.0/) *了解更多信息*

该数据集在 Hugging Face 的数据集分发网络上[可用](https://huggingface.co/datasets/jfleg)，并且可以使用他们的数据集库来访问。因为这个库是 Happy Transformer 的依赖项，所以我们不需要安装它，可以直接从库中导入一个名为 load_dataset 的函数。

```
from datasets import load_dataset
```

数据集的 id 是“jfleg ”,有两个分支“‌"validation”和“测试”我们将验证集用于训练，测试集用于评估。

```
train_dataset = load_dataset("jfleg", split='validation[:]') eval_dataset = load_dataset("jfleg", split='test[:]')
```

# 数据检查

我们刚刚成功下载了数据集。现在让我们通过迭代一些案例来探索它。训练和评估数据集都以相同的方式构造，并且具有两个特征，句子和修正。句子功能包含每个案例的单个字符串，而更正功能包含 4 个人工生成的更正列表。

```
for case in train_dataset["corrections"][:2]: 
    print(case)
    print(case[0]) 
    print("--------------------------------------------------")
```

*结果:*

*【所以我认为如果我们的祖先没有发展科学技术，我们就不会活着……】*

所以我认为如果我们的祖先没有发展科学技术，我们就不会活着。

— — — — — — — — — — — — — — — — — — — — — — — — —

*[‘不适用于汽车’，‘不要在车里使用。’，'车不能用'，‘不能用这辆车。’】*

*不适用于汽车。*

— — — — — — — — — — — — — — — — — — — — — — — — —

# 数据预处理

现在，我们必须将处理成 Happy Transformer 的适当格式。我们需要将训练和评估数据组织成相同的格式，这是一个包含两列的 CSV 文件:输入和目标。输入列包含语法不正确的文本，而目标列包含来自目标列的文本的正确版本。

下面是将数据处理成适当格式的代码。我们必须通过给每个输入添加相同的前缀来指定我们希望执行的任务。在这种情况下，我们将使用前缀“grammar:”。这样做是因为 T5 模型能够用单个模型执行多项任务，如[翻译](https://youtu.be/2tdUyez7a2s)和[总结](https://www.vennify.ai/summarize-text-with-transformer-models/)，并且为每项任务使用唯一的前缀，以便模型学习执行哪项任务。我们还需要跳过包含空白字符串的情况，以避免微调时出现错误。

```
import csv

def generate_csv(csv_path, dataset):
    with open(csv_path, 'w', newline='') as csvfile:
        writter = csv.writer(csvfile)
        writter.writerow(["input", "target"])
        for case in dataset:
     	    # Adding the task's prefix to input 
            input_text = "grammar: " + case["sentence"]
            for correction in case["corrections"]:
                # a few of the cases contain blank strings. 
                if input_text and correction:
                    writter.writerow([input_text, correction])

generate_csv("train.csv", train_dataset)
generate_csv("eval.csv", eval_dataset)
```

我们只是生成我们的训练和评估数据！我们总共生成了 3016 个训练样本和 2988 个评估样本。

# 培训前评估

我们将使用一个称为损失的通用指标来评估微调前后的模型。损失可以描述为模型的预测与正确答案相比有多“错误”。因此，如果微调后损失减少，那么这表明模型学习了。重要的是，我们使用单独的数据进行训练和评估，以表明该模型可以概括其获得的知识，以解决看不见的情况。

*你还可以使用其他指标来评估语法纠正模型。其中最流行的一个叫 GLEU，在这里可以了解更多*[](https://www.researchgate.net/publication/283810389_Ground_truth_for_grammatical_error_correction_metrics)**【2】*。Loss 是用 Happy Transformer 实现的最简单的方法，所以我们将使用它。***

**让我们在任何训练之前确定评估数据集上的模型损失。为此，我们将调用 happy_tt 的 eval()方法，并提供包含评估数据的 CSV 路径。**

```
**before_result = happy_tt.eval("eval.csv")**
```

**结果是一个 dataclass 对象，它只有一个名为 loss 的变量，我们可以如下所示对其进行隔离。**

```
**print("Before loss:", before_result.loss)**
```

***结果:*亏损前:1 . 54385 . 38383838661**

# **培养**

**现在让我们训练模型。我们可以通过调用 happy_tt 的 train()方法来实现。为了简单起见，我们将使用默认参数，而不是批量大小，我们将把批量大小增加到 8。如果您遇到内存不足的错误，那么我建议您减少批量大小。您可以访问此[网页](https://happytransformer.com/text-to-text/finetuning/)了解如何修改各种参数，如学习率和时期数。**

```
**from happytransformer import TTTrainArgs args = TTTrainArgs(batch_size=8) happy_tt.train("train.csv", args=args)**
```

# **培训后评估**

**像以前一样，让我们确定模型的损失。**

```
**before_loss = happy_tt.eval("eval.csv") print("After loss: ", before_loss.loss)**
```

***结果:损失后:*0 . 54687 . 68686868661**

**这就对了，你可以看到损失减少了！但是，现在让我们通过提供示例来更定性地评估该模型。**

# **推理**

**现在让我们用这个模型来纠正我们将提供的例子的语法。为此，我们将使用快乐 tt 的 generate_text()方法。我们还将使用一种称为*波束搜索*的算法进行生成。您可以在这个[网页](https://happytransformer.com/text-to-text/settings/)上查看您可以修改的不同文本生成参数，以及您可以用于通用算法的不同配置。**

```
**from happytransformer import TTSettingsbeam_settings = TTSettings(num_beams=5, min_length=1, max_length=20)**
```

## **示例 1**

```
**example_1 = "grammar: This sentences, has bads grammar and spelling!" result_1 = happy_tt.generate_text(example_1, args=beam_settings) print(result_1.text)**
```

***结果:这个句子有糟糕的语法和拼写！***

## **示例 2**

```
**example_2 = "grammar: I am enjoys, writtings articles ons AI." result_2 = happy_tt.generate_text(example_2, args=beam_settings) print(result_2.text)**
```

**结果:我喜欢写关于人工智能的文章。**

# **后续步骤**

**有一些方法可以潜在地提高性能。我建议将一些评估案例转移到训练数据中，然后通过应用网格搜索之类的技术来优化超参数。然后，您可以将评估案例包括在训练集中，以使用您的最佳超参数集来微调最终模型。**

**我还建议您应用基本的数据预处理。数据集中的某些案例包含多余的空间，如果不进行更正，模型将在不需要的时候生成空间。因此，您可以应用下面的代码来更正训练和评估数据的输入和输出文本。**

```
**replacements = [
  (" .", "."), 
  (" ,", ","),
  (" '", "'"),
  (" ?", "?"),
  (" !", "!"),
  (" :", "!"),
  (" ;", "!"),
  (" n't", "n't"),
  (" v", "n't"),
  ("2 0 0 6", "2006"),
  ("5 5", "55"),
  ("4 0 0", "400"),
  ("1 7-5 0", "1750"),
  ("2 0 %", "20%"),
  ("5 0", "50"),
  ("1 2", "12"),
  ("1 0", "10"),
  ('" ballast water', '"ballast water')
]

def remove_excess_spaces(text):
  for rep in replacements:
    text = text.replace(rep[0], rep[1])

  return text**
```

**现在，在 generate_csv()函数的底部进行以下更改。**

```
**input_text = remove_excess_spaces(input_text)
correction = remove_excess_spaces(correction)
writter.writerow([input_text, correction])**
```

**最后，您可以保存您的模型，并在其他时间加载它，如本[网页](https://happytransformer.com/save-load-model/)所述。**

# **测试你的技能:**

**你可以微调一个语法纠正模型，上传到 Hugging Face 的模型分发网络，增强学习。特别是，我建议你考虑使用谷歌新发布的语法纠错数据集，名为 [C4_200M 语法纠错合成数据集](https://github.com/google-research-datasets/C4_200M-synthetic-dataset-for-grammatical-error-correction) [3]*。然后，跟随这个[教程](https://www.vennify.ai/upload-happy-transformer/)在你训练完一个模特后，如何上传一个模特到拥抱脸的模特分销网络。**

**如果您使用本教程中讨论的技术发布了一个模型，请给我发电子邮件(eric@vennify.ca)。我可能会发表一篇如何使用它的文章。**

****许可证:* [*知识共享署名 4.0 国际*](https://creativecommons.org/licenses/by/4.0/)**

# **预训练模型**

**我[在 Hugging Face 的模型发布网络上发布了](https://huggingface.co/vennify/t5-base-grammar-correction)一个模型，使用了本教程中介绍的数据集和技术。我还应用了后续步骤部分中的建议。我在下面包含了演示如何使用它的代码。**

```
**happy_tt = HappyTextToText("T5", "vennify/t5-base-grammar-correction")

result = happy_tt.generate_text("grammar: I boughts ten apple.", args=beam_settings)print(result.text)**
```

***结果:我买了十个苹果。***

# **结论:**

**我希望你把你学到的东西应用到训练你自己的模型上，然后通过在拥抱脸的模型发布网络上发布给全世界。通过这样做，您将帮助许许多多渴望实现高质量语法纠正模型的人。或者，您可能只是滚动到本文的底部，以了解如何实现预训练模型。无论如何，希望你学到了有用的东西，并保持快乐！**

# **资源**

**快乐变形金刚的 [GitHub 页面](https://github.com/EricFillion/happy-transformer)**

**订阅我的 [YouTube 频道](https://www.youtube.com/channel/UC7-EWrr8YdcQgPPk76OiUVw?sub_confirmation=1)观看即将发布的语法纠正视频。**

**加入 Happy Transformer 的 [Discord](https://discord.com/invite/psVwe3wfTb) 社区进行交流，并向阅读过这篇文章并对 NLP 充满热情的人提问**

**本教程中使用的[代码](https://colab.research.google.com/drive/1SZWAe_ND1V_sbCEOCpLFL_HrgLtKHvLo?usp=sharing)**

# **参考**

**[1] C .纳波莱斯，k .坂口，j .泰特劳特， [JFLEG:一个流利度语料库和语法纠错的基准](https://arxiv.org/abs/1702.04066)，EACL 2017**

**[2] C .纳波莱斯，k .坂口，m .波斯特，j .泰特劳特，[语法纠错度量的基础真理](https://www.researchgate.net/publication/283810389_Ground_truth_for_grammatical_error_correction_metrics)，IJCNLP 2015**

**[3] F. Stahlberg，S. Kumar，[利用标记的讹误模型进行语法纠错的合成数据生成](https://aclanthology.org/2021.bea-1.4/)，ACL 2021**

***原载于 2021 年 8 月 18 日*[*https://www . venni fy . ai*](https://www.vennify.ai/fine-tune-grammar-correction/)*。***