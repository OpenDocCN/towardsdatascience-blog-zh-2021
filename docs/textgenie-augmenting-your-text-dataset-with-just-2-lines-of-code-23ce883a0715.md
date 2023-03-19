# text genie——仅用两行代码扩充您的文本数据集！

> 原文：<https://towardsdatascience.com/textgenie-augmenting-your-text-dataset-with-just-2-lines-of-code-23ce883a0715?source=collection_archive---------10----------------------->

![](img/63518f43d847f58a4ce4a0e425af1c54.png)

TextGenie 徽标-作者图片

通常在开发自然语言处理模型时，我们发现很难找到相关数据。更重要的是，找到大量的数据。

之前，在开发我们的[意图分类器](https://medium.com/analytics-vidhya/creating-your-own-intent-classifier-b86e000a4926)时，我们使用了 [CLINC150 数据集](https://github.com/clinc/oos-eval)，该数据集包含 150 个不同类别的 100 个样本。但是，如果我们需要更多的样本呢？另一个类似的场景是我在用 [Rasa](https://github.com/RasaHQ/rasa) 开发上下文助手的时候。当从头开始创建训练数据时，我必须为每个意图设想不同的样本，或者向我的朋友寻求一些帮助。根据领域的不同，每个类可能需要一定数量的样本。

这是我产生创建 [TextGenie](https://github.com/hetpandya/textgenie) 的想法的时候，这是一个增加文本数据的库。python 包在我的 [Github repo](https://github.com/hetpandya/textgenie) 是开源的。让我们看看图书馆的运作。

# 图书馆是如何工作的

到目前为止，该库使用以下方法来扩充文本数据:

## 使用 T5 解释

通过使用深度学习进行解释，可以生成大量不同的样本。我们将使用来自 huggingface 的这个 [T5 模型](https://huggingface.co/ramsrigouthamg/t5_paraphraser)来生成释义。

## 伯特遮罩填充

为了使用掩码填充来扩充文本，找到可以被掩码的第一个单词。为此，我们将使用 spacy 从一个句子中提取关键字。一旦找到关键词，它们就被替换为掩码，并被馈送到 BERT 模型，以预测一个单词来代替被屏蔽的单词。

## 将句子转换成主动语态

此外，我们还检查一个句子是否是被动语态。如果是，则转换为主动语态。

# 装置

使用以下命令安装库:

```
pip install textgenie
```

# 使用

让我们使用下面的代码初始化来自`TextGenie`类的增强器:

在这里，除了解释模型，我还提到了 BERT 模型的名称，它默认设置为`None`。但是可以通过提到模型的名称来启用它。**建议也使用掩码填充方法，因为这将有助于生成更多数据。**

您可以在下面找到`TextGenie`对象的完整参数列表:

*   `paraphrase_model_name`:T5 改述模型的名称。**编辑:**在这里可以找到用于释义生成的预训练模型列表[。](https://github.com/hetpandya/paraphrase-datasets-pretrained-models#pretrained-models)
*   `mask_model_name`:该参数是可选的 BERT 模型，将用于填充遮罩。`mask_model_name`的默认值设置为`None`，默认禁用。但是可以通过提及要使用的 BERT 模型的名称来启用它。面具填充模型列表可以在找到[。](https://huggingface.co/models?filter=en&pipeline_tag=fill-mask)
*   `spacy_model_name`:空间模型名称。可用型号可在这里找到[。默认值设置为`en`。虽然已经设置了空间模型名称，但是如果`mask_model_name`设置为`None`，则不会使用该名称。](https://spacy.io/models)
*   `device`:模型加载的设备。默认值设置为`cpu`。

## 使用 T5 的释义进行文本扩充:

`augment_sent_t5()`方法的参数列表如下:

*   `sent`:必须应用增强的句子。
*   `prefix`:T5 型号输入的前缀。
*   `n_predictions`:增加的次数，函数应该返回。默认值设置为`5`。
*   `top_k`:T5 模型应该生成的预测数。默认值设置为`120`。
*   `max_length`:输入模型的句子的最大长度。默认值设置为`256`。

## 使用 BERT 遮罩填充的文本增强；

**注意:**使用这种方法时，请注意标点符号。

请在下面找到`augment_sent_mask_filling()`方法的参数表:

*   `sent`:必须应用增强的句子。
*   `n_mask_predictions`:伯特遮罩填充模型应生成的预测数量。默认值设置为`5`。

## 将句子转换成主动语态

在第一个例子中，这个句子是主动语态。因此，它被原样归还。而在另一个例子中，句子从被动语态转换成了主动语态。

以下是`convert_to_active()`方法所需的参数:

*   `sent`:要转换的句子。

## 神奇的一次:将所有方法包装在一个方法中

受输出所占空间的限制，我将预测值放在一个较小的数字上。请随意和他们玩！😉

看起来，`textgenie.magic_once()`方法融合了上述所有技术的功能。

**由于该方法对单个文本数据进行操作，因此可以很容易地与需要数据扩充的其他框架合并。**

`magic_once()`的完整参数列表如下:

*   `sent`:必须扩充的句子。
*   `paraphrase_prefix`:T5 型号输入的前缀。
*   `n_paraphrase_predictions`:扩增次数，T5 型号应该返回。默认值设置为`5`。
*   `paraphrase_top_k`:T5 模型应该生成的预测总数。默认值设置为`120`。
*   `paraphrase_max_length`:输入模型的句子的最大长度。默认值设置为`256`。
*   `n_mask_predictions`:伯特遮罩填充模型应生成的预测数。默认值设置为`None`。
*   `convert_to_active`:句子是否要转换成主动语态。默认值设置为`True`。

# 神灯时间到了！

既然已经讨论了单个数据，让我们对整个数据集施展一下魔法吧！😋

`magic_lamp()`方法获取整个数据集，并根据输入生成一个包含扩充数据的`txt`或`tsv`文件。如果输入是一个`Python List`或`.txt`文件，扩充后的输出将存储在一个名为`sentences_aug.txt`的`txt`文件中，同时，包含扩充数据的`Python List`将被返回。如果数据在`csv`或`tsv`文件中，将保存一个名为`original_file_name_aug.tsv`的包含扩充数据的`tsv`文件，并返回一个熊猫`DataFrame`。如果这些文件包含带标签的数据，将返回增加的数据以及相应的标签。

首先，我们需要一个数据集来处理。我从[垃圾短信收集数据集](https://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection)中提取了 300 个样本进行测试，你可以从[这里](https://gist.github.com/hetpandya/0bfbf07d587d84c37d40e54199428547)下载。

## 扩充数据集

下载当前工作目录中的`hamspam.tsv`并运行以下代码:

`magic_lamp()`方法的完整列表如下:

*   `sentences`:需要扩充的数据集。这可以是一个`Python List`、`txt`、`csv`或`tsv`文件。
*   `paraphrase_prefix`:T5 型号输入的前缀。
*   `n_paraphrase_predictions`:扩增数，T5 型号应该回归。默认值设置为`5`。
*   `paraphrase_top_k`:T5 模型应该生成的预测总数。默认值设置为`120`。
*   `paraphrase_max_length`:输入模型的句子的最大长度。默认值设置为`256`。
*   `n_mask_predictions`:伯特遮罩填充模型应生成的预测数。默认值设置为`None`。
*   `convert_to_active`:句子是否要转换成主动语态。默认值设置为`True`。
*   `label_column`:包含带标签数据的列的名称。默认值设置为`None`。如果数据集在`Python List`或`txt`文件中，则不需要设置该参数。
*   `data_column`:包含数据的列的名称。默认值设置为`None`。如果数据集是一个`Python List`或`txt`文件，也不需要这个参数。
*   `column_names`:如果`csv`或`tsv`没有列名，必须传递一个 Python 列表来给列命名。由于该功能也接受`Python List`和一个`txt`文件，默认值被设置为`None`。但是，如果使用`csv`或`tsv`文件，则必须设置该参数。

代码需要一些时间来扩充数据。拿起你的咖啡，看精灵表演他的魔术！

一旦增强完成，输出将如下所示:

你可以在这里找到整个扩充数据集[。从 300 排到 43600 排！](https://gist.github.com/hetpandya/ad86da386cc98e8da195baaf319160fa)

# 包扎

目前就这些。图书馆随时欢迎各种建议和想法。

感谢阅读😃！