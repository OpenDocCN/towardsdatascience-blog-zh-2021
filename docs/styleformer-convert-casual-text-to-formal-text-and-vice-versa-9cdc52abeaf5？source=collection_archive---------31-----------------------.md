# Styleformer:将非正式文本转换为正式文本，反之亦然

> 原文：<https://towardsdatascience.com/styleformer-convert-casual-text-to-formal-text-and-vice-versa-9cdc52abeaf5?source=collection_archive---------31----------------------->

## *如何利用强大的 T5 Transformer 模型改变文本的形式*

![](img/2c464b15ba2fc391342f925608ce986f.png)

作者图片

[Styleformer](https://github.com/PrithivirajDamodaran/Styleformer) 是一个全新的 Python 库，允许你使用一个强大的名为 T5 的 Transformer 模型来改变文本的样式。本教程重点介绍它将非正式文本转换为正式文本的能力，反之亦然。改变文本的形式有许多应用。例如，您可能希望增加工作邮件的正式程度——只需运行几行代码的 Styleformer。

# 装置

您可以使用以下命令直接从 GitHub 下载 Styleformer。

```
pip install git+https://github.com/PrithivirajDamodaran/Styleformer.git
```

# 从休闲到正式

现在，让我们导入一个名为 Styleformer 的类，我们将使用它来加载模型。

```
from styleformer import Styleformer
```

让我们加载将非正式文本转换为正式文本的模型。这个模型的样式 ID 是 0，所以在实例化 Syleformer 对象时，我们将把它的“style”参数设置为 0。

```
styleformer_c_t_f = Styleformer(style=0)
```

从这里我们可以通过调用 styleformer_c_t_f.transfer()立即开始转换文本。我们将提供希望转换的文本作为第一个也是唯一的位置输入。然后，如果我们想使用 CPU，我们将把“推论 _ 开”参数设置为 0，如果我们想使用 GPU，则将参数设置为 1。

```
text_1 = "Yo, I love coding in Python. "result_1 = styleformer_c_t_f.transfer(text_1, inference_on=1) print(result_1)
```

*输出:我喜欢用 Python 编码。*

这是另一个例子。

```
text_2 = "I'm going to go buy some stuff like apples at the store "result_2 = styleformer_c_t_f.transfer(text_2, inference_on=1) print(result_2)
```

结果:我要去商店买一些东西，比如苹果。

不算太差！现在，让我们讨论相反的情况——将正式文本转换成非正式文本。

# 正式到休闲

让我们加载模型，将正式文本转换为非正式文本。此型号的样式 ID 为“1”。

```
styleformer_f_t_1 = Styleformer(style=1)
```

我们现在可以像以前一样调用“transfer()”方法。

```
text_3 = "Let's discuss our plans for this evening"
result_3 = styleformer_f_t_c.transfer(text_3, inference_on=1) 
print(result_3)
```

结果:让我们谈谈今晚我们要做什么

# 多个句子

虽然文档中没有说明，但我相信这个包一次只能用于一个句子。在查看了源代码之后，我注意到启用了“early_stopping ”,这意味着当到达“句子结束”标记时，模型停止生成文本。此外，内部设置设置每次迭代最多 32 个标记，其中标记通常是单词或符号。下面是如何使用名为 TextBlob 的 Python 包按句子分解输入的快速概述。

PiPI 上有 TextBlob，我们可以用一个简单的 pip 命令安装它。

```
pip install textblob
```

现在，让我们导入一个名为 TextBlob 的类，我们将使用它来按句子分解字符串。

```
from textblob import TextBlob
```

让我们通过向 TextBlob 类提供一个字符串来创建一个 TextBlob 对象。注意这个字符串是如何包含多个句子的。我们将按句子分解文本，然后通过使用 syleformer_c_t_f 对象将文本从随意转换为正式。

```
text_4 = "Hey man, what's up? We should hang out and watch the Olympics. Then maybe go grab some food" blob = TextBlob(text_4)
```

TextBlob 构建在名为 NLTK 的 Python 框架之上。有时我们需要在用 TextBlob 执行操作之前从 NLTK 安装资源。因为 NLTK 是 TextBlob 的依赖项，所以我们不需要安装它。让我们从 NLTK 下载一个名为‘punkt’的标记器。

```
import nltk 
nltk.download('punkt')
```

我们现在有了开始逐句标记字符串所需的一切。

```
result_4_list = [] 
for sent in blob.sentences: 
    temp_result = styleformer_c_t_f.transfer(sent.string) 
    result_4_list.append(temp_result) result_4 = " ".join(result_4_list) 
print(result_4)
```

*输出:你好，你在做什么？我建议我们花时间在一起看奥运会。之后，也许去吃点东西。*

# 带快乐变压器的造型器

你可能想用一个更成熟的包，像[拥抱脸的变形金刚](https://github.com/huggingface/transformers)库或者我自己的[快乐变形金刚](https://github.com/EricFillion/happy-transformer)包来运行这个模型。与 Styleformer 不同，这两个包都可以在 PyPI 上获得，并允许您修改文本生成设置。

您可能希望修改文本生成设置有几个原因。例如，默认情况下，Styleformer 包每次推理最多生成 32 个标记，其中标记通常是单词或符号，没有办法调整这一点。此外，通过修改设置，您可以更改不太可能被选中的单词的比率，这允许您改变文本的“创造性”。

下面是一个 GitHub 要点，演示了如何将 Styleformer 模型与 Happy Transformer 一起使用。也许我会写一篇关于如何将 Styleformer 与 Happy Transformer 一起使用的完整文章，但是现在，下面的要点对大多数人来说应该足够了。

您还可以查看这篇文章，这篇文章深入解释了如何将 Happy Transformer 用于 Styleformer 作者创建的类似模型。这种模型被称为“文法形成器”,用于纠正输入文本的语法。

# 结论

希望你学到了很多！我期待着阅读这个模型的未来应用。在给你的老板发邮件之前，一定要使用 Styleformer，让每个人都开心！

# 资源

[订阅](https://www.youtube.com/channel/UC7-EWrr8YdcQgPPk76OiUVw?sub_confirmation=1)我的 YouTube 频道，了解 Styleformer 上即将发布的内容。

本教程中使用的[代码](https://colab.research.google.com/drive/16FbY6Q_F_5SVtyEQyAtqsiIvUh1TL8aN?usp=sharing)

*原载于 2021 年 8 月 2 日*[*https://www . vennify . ai*](https://www.vennify.ai/how-to-use-styleformer/)*。*