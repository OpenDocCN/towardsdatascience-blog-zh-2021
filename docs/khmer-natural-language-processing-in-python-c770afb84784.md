# Python 中的高棉语自然语言处理

> 原文：<https://towardsdatascience.com/khmer-natural-language-processing-in-python-c770afb84784?source=collection_archive---------35----------------------->

## 利用库美尔-nltk，库美尔的开源 NLP 工具包

![](img/7ce8975a0839f43bd18bb8d943de0a6f.png)

Paul Szewczyk 在 [Unsplash](https://unsplash.com/s/photos/khmer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

通过阅读这篇文章，你将学会用 Python 对高棉语执行自然语言处理任务。供您参考，高棉语是柬埔寨的官方语言，在泰国(东部和东北部)和越南(湄公河三角洲)广泛使用。

在构建任何支持多种语言的 NLP 相关应用程序时，拥有一个专门的语言处理工具包会有很大帮助。在本文中，您将利用一个名为。`khmer-nltk`。基于[官方文档](https://github.com/VietHoang1512/khmer-nltk)，`khmer-nltk`是一个使用条件随机字段构建的高棉语处理工具包。

在撰写本文时，它支持以下 NLP 任务:

*   句子分割
*   分词
*   词性标注

以下任务已经在路线图中，预计将在未来推出。

*   命名实体识别
*   文本分类

让我们继续下一节，开始安装所有需要的 Python 包。

# 设置

强烈建议您在继续安装之前创建一个新的虚拟环境。

## 高棉语

激活虚拟环境并运行以下命令来安装`khmer-nltk`:

```
pip install khmer-nltk
```

一旦安装完成，当您运行`pip list`时，您应该有以下包:

```
Package          Version
---------------- -------
colorama         0.4.4
khmer-nltk       1.5
pip              21.1.3
python-crfsuite  0.9.7
setuptools       57.1.0
six              1.16.0
sklearn-crfsuite 0.3.6
tabulate         0.8.9
tqdm             4.61.2
```

# 履行

在本节中，您将编写一些 Python 代码来测试`khmer-nltk`。在工作目录中创建新的 python 文件。

## 导入

您需要根据您的用例导入相应的函数:

```
from khmernltk import sentence_tokenize
from khmernltk import word_tokenize
from khmernltk import pos_tag
```

从 1.5 版本开始，`khmer-nltk`具有以下功能:

*   `sentence_tokenize` —用于将文本标记成句子
*   `word_tokenize` —用于将文本标记为单词
*   `pos_tag` —用于词性标注

## 句子标记化

对于句子标记化，只需调用`sentence_tokenize`函数并传入一个字符串，如下所示:

```
text = 'ខួបឆ្នាំទី២៨! ២៣ តុលា ស្មារតីផ្សះផ្សាជាតិរវាងខ្មែរនិងខ្មែរ ឈានទៅបញ្ចប់សង្រ្គាម នាំពន្លឺសន្តិភាព និងការរួបរួមជាថ្'result = sentence_tokenize(text)
print(result)
```

运行 Python 文件时，您应该会得到以下输出:

```
["ខួបឆ្នាំទី២៨!", "២៣ តុលា ស្មារតីផ្សះផ្សាជាតិរវាងខ្មែរនិងខ្មែរ ឈានទៅបញ្ចប់សង្រ្គាម នាំពន្លឺសន្តិភាព និងការរួបរួមជាថ្"]
```

如果有与 Python 中的文本编码相关的错误，只需将

```
# -*- coding: UTF-8 -*-
```

Python 文件顶部的以下文本(在 import 语句之前)如下:

```
# -*- coding: UTF-8 -*-from khmernltk import sentence_tokenize
from khmernltk import word_tokenize
from khmernltk import pos_tag
```

如果由于字体错误而无法在命令行中打印输出，尝试通过`json`模块将其写入文件，如下所示:

```
import json...with open('temp.txt', 'w', encoding='utf8') as f:
    f.write(json.dumps(result, ensure_ascii=False) + '\n')
```

记住将`ensure_ascii`选项设置为`False`，以防止`json`包将字符更改为 unicode。

## 单词标记化

另一方面，`word_tokenize`函数接受两个附加参数:

*   `separator` —标记化后用于分隔文本的分隔符。默认为`-`，仅在`return_tokens`设置为假时起作用。
*   `return_tokens`—是返回标记化字符串还是标记列表。默认为`True`。

```
result = word_tokenize(text, return_tokens=True)
print(result)
```

输出如下所示:

```
["ខួប", "ឆ្នាំ", "ទី", "២៨", "!", " ", "២៣", " ", "តុលា", " ", "ស្មារតី", "ផ្សះផ្សា", "ជាតិ", "រវាង", "ខ្មែរ", "និង", "ខ្មែរ", " ", "ឈាន", "ទៅ", "បញ្ចប់", "សង្រ្គាម", " ", "នាំ", "ពន្លឺ", "សន្តិភាព", " ", "និង", "ការរួបរួម", "ជា", "ថ្"]
```

在某些情况下，您可能希望删除列表中的所有空格。你可以通过列表理解和条件语句很容易地做到这一点:

```
result = word_tokenize(text, return_tokens=True)
result = [x for x in result if x != ' ']
print(result)
```

## 词性标注

对于词性标注，应该改用`pos_tag`函数。它接受单个字符串并返回一个元组列表(str，str)。

```
result = pos_tag(text)
print(result)
```

每个元组表示标记化的单词及其对应的词性标签。

```
[["ខួប", "n"], ["ឆ្នាំ", "n"], ["ទី", "n"], ["២៨", "1"], ["!", "."], [" ", "n"], ["២៣", "1"], [" ", "n"], ["តុលា", "n"], [" ", "n"], ["ស្មារតី", "n"], ["ផ្សះផ្សា", "v"], ["ជាតិ", "n"], ["រវាង", "o"], ["ខ្មែរ", "n"], ["និង", "o"], ["ខ្មែរ", "n"], [" ", "n"], ["ឈាន", "v"], ["ទៅ", "v"], ["បញ្ចប់", "v"], ["សង្រ្គាម", "n"], [" ", "n"], ["នាំ", "v"], ["ពន្លឺ", "n"], ["សន្តិភាព", "n"], [" ", "n"], ["និង", "o"], ["ការរួបរួម", "n"], ["ជា", "v"], ["ថ្", "n"]]
```

以下列表说明了所有可用的标签及其对 POS tag 的意义。

*   `.`:标点符号
*   `1`:编号
*   `a`:形容词
*   `n`:名词(空格被视为名词)
*   `o`:连词
*   `v`:动词

## 完全码

你可以在下面的[要点](https://gist.github.com/wfng92/c9af3c761b2994222c81bf008d6e3a0b)找到完整的代码。

这个包仍处于开发的早期阶段，将来可能会有更多的东西出现。

# 结论

让我们回顾一下你今天所学的内容。

本教程首先简要介绍了`khmer-nltk`包以及可用于高棉语相关 NLP 任务的特性。

接下来，通过`pip install`介绍安装过程。

接着详细解释了词标记化、句子标记化和词性标注。它还根据您的用例提供了一些代码片段作为参考。

感谢您阅读这篇文章！查看我的其他文章，了解更多关于自然语言处理任务的教程！

# 参考

1.  [Github——高棉语 nltk](https://github.com/VietHoang1512/khmer-nltk)