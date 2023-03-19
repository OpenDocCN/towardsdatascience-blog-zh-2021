# pyvi 简介:Python 越南语 NLP 工具包

> 原文：<https://towardsdatascience.com/introduction-to-pyvi-python-vietnamese-nlp-toolkit-ff5124983dc2?source=collection_archive---------20----------------------->

## 利用“pyvi”包进行标记化、词性标注和重音标记修改

![](img/866bd14aba123cbab9455f2941084ae1.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/s/photos/vietnamese?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在过去，我已经发表了许多与各种亚洲语言的 NLP 工具包相关的文章:

*   [Python 中的高棉语自然语言处理](/khmer-natural-language-processing-in-python-c770afb84784)
*   [python 语言入门指南](/beginners-guide-to-pythainlp-4df4d58c1fbe)
*   [Python 中的朝鲜语自然语言处理](/korean-natural-language-processing-in-python-cc3109cbbed8)
*   [SudachiPy:一个用 Python 编写的日语词法分析器](/sudachipy-a-japanese-morphological-analyzer-in-python-5f1f8fc0c807)

今天，让我们来更深入地探讨一下越南语。通过阅读这篇文章，您将学会通过一个名为`pyvi`的开源 Python 包对越南语文本进行语言分析。

在撰写本文时，`pyvi`提供了以下功能:

*   标记化
*   词性标注
*   去除重音符号
*   重音符号添加

让我们继续下一节，开始安装必要的软件包。

# 设置

在继续安装之前，建议创建一个新的虚拟环境。激活它并运行以下命令:

```
pip install pyvi
```

# 标记化

## 标记化

在本节中，您将学习对越南语文本执行标记化。创建一个新的 Python 文件，并在其中添加以下代码。

```
from pyvi import ViTokenizertext = 'Xin chào! Rất vui được gặp bạn.'
result = ViTokenizer.tokenize(text)
print(result)
```

您应该得到以下输出:

```
Xin chào ! Rất vui được gặp bạn .
```

每个令牌将由一个空格分隔。通过用空格分割文本，可以很容易地将其转换为列表:

```
result.split(' ')
```

新的输出如下:

```
['Xin', 'chào', '!', 'Rất', 'vui', 'được', 'gặp', 'bạn', '.']
```

## spacy_tokenize

除此之外，`pyvi`确实提供了一个名为`spacy_tokenize`的替代功能，以便更好地与`spaCy`包集成。简单的叫它如下:

```
result = ViTokenizer.spacy_tokenize(text)
```

输出是一个包含以下项目的元组:

*   令牌化令牌的列表
*   一个布尔值列表，指示标记后面是否跟有一个空格

运行该文件时，您应该得到以下输出:

```
(['Xin', 'chào', '!', 'Rất', 'vui', 'được', 'gặp', 'bạn', '.'], [True, False, True, True, True, True, True, False, False])
```

使用索引 0 获取列表:

```
result[0]
```

# 词性标注

词性标注包括两个步骤:

*   将文本标记为由空白分隔的新文本
*   对标记化的文本执行词性标注

## 后标记

在对文本进行标记后，只需调用`postagging`函数:

```
from pyvi import ViPosTaggerresult = ViPosTagger.postagging(ViTokenizer.tokenize(text))
print(result)
```

以下文本将显示在您的终端上:

```
(['Xin', 'chào', '!', 'Rất', 'vui', 'được', 'gặp', 'bạn', '.'], ['V', 'V', 'F', 'R', 'A', 'R', 'V', 'N', 'F'])
```

同样，它包含一个具有以下项目的元组:

*   标记化文本的列表
*   对应于令牌的 POS 标签列表

只需遍历列表，获取令牌的相应标记:

```
for index in range(len(result[0])):
    print(result[0][index], result[1][index])
```

您应该会看到以下输出:

```
Xin V
chào V
! F
...
```

POS 标签的完整列表如下:

*   形容词
*   C —并列连词
*   e-介词
*   I —感叹词
*   L —限定词
*   M —数字
*   n——普通名词
*   名词分类器
*   Ny —名词缩写
*   Np —专有名词
*   Nu —单位名词
*   代词
*   R —副词
*   从属连词
*   T —助词、语气词
*   动词
*   X —未知
*   F —过滤掉(标点符号)

## 后置标记 _ 令牌

此外，还有一个名为`postagging_tokens`的替代函数，它接受一个令牌列表。您可以将其与`spacy_tokenize`结合使用，以获得相同的输出:

```
tokens = ViTokenizer.spacy_tokenize(text)[0]
result = ViPosTagger.postagging_tokens(tokens)
print(result)
```

# 去除重音符号

有时，可能会出现应该从文本中删除重音符号(音调符号)的情况。在这种情况下，您可以利用`remove_accents`功能来帮您完成:

```
from pyvi import ViUtilsresult = ViUtils.remove_accents(text)
print(result)
```

它返回一个字节字符串:

```
b'Xin chao! Rat vui duoc gap ban.'
```

如果您希望将输出作为字符串使用，您需要做的就是根据 UTF-8 编码对其进行编码:

```
result.encode('utf8')
```

# 添加重音符号

另一方面，如果没有重音符号，越南语文本可能会模糊不清。使用`add_accents`功能将非重音文本转换为重音文本:

```
unaccented_text = 'truong dai hoc bach khoa ha noi'
result = ViUtils.add_accents(unaccented_text)
print(result)
```

输出应该如下所示:

```
Trường Đại học Bách Khoa hà nội
```

# 结论

让我们回顾一下你今天所学的内容。

本文首先简要介绍了`pyvi`中可用的特性。

然后，通过`pip install`进入安装过程。

它继续深入解释了一些核心功能，如标记化和词性标注。此外，本教程还介绍了关于音调符号的部分，包括删除和添加越南语文本的重音符号。

感谢你阅读这篇文章。请随意阅读我的其他文章。祝你有美好的一天！

# 参考

1.  [Github — pyvi](https://github.com/trungtv/pyvi)