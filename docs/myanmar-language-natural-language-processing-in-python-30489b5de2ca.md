# Python 中的缅甸语自然语言处理

> 原文：<https://towardsdatascience.com/myanmar-language-natural-language-processing-in-python-30489b5de2ca?source=collection_archive---------23----------------------->

语言检测、Zawgyi-Unicode 转换和标记化

![](img/4adffe0ad183076bf28f24b465ff5316.png)

照片由[在](https://unsplash.com/@tsawwunna24?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/s/photos/myanmar?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上看到 Wunna

今天的主题是一个面向缅甸语言的开源免费 NLP 工具包，名为 [pyidaungsu](https://github.com/kaunghtetsan275/pyidaungsu) 。根据官方文件，pyiduangsu 是一个

> …缅甸语 Python 库。在缅甸语言的自然语言处理和文本预处理中很有用。

在撰写本文时，它支持:

*   语言检测(通用缅甸语、扎吉语、克伦语、孟语、掸语)
*   Zawgyi 和 Unicode 文本之间的转换
*   基于音节或单词的标记化

# 设置

建议在继续安装之前创建一个新的虚拟环境。

## Pip 安装

激活它并运行以下安装命令:

```
pip install pyidaungsu
```

# 语言检测

## 导入

在文件顶部添加以下导入语句:

```
import pyidaungsu as pds
```

然后，您可以调用`detect`函数并传入您想要的文本:

```
pds.detect("ထမင်းစားပြီးပြီလား")
# "mm_uni"
```

它将返回一个指示检测到的缅甸语言的字符串。例如，看看下面的例子和结果:

```
pds.detect("ထမင္းစားၿပီးၿပီလား")
# "mm_zg"
pds.detect("တၢ်သိၣ်လိတၢ်ဖးလံာ် ကွဲးလံာ်အိၣ်လၢ မ့ရ့ၣ်အစုပူၤလီၤ.")
# "karen"
pds.detect("ဇၟာပ်မၞိဟ်ဂှ် ကတဵုဒှ်ကၠုင် ပ္ဍဲကဵုဂကောံမွဲ ဖအိုတ်ရ၊၊")
# "mon"
pds.detect("ၼႂ်းဢိူင်ႇမိူင်းၽူင်း ၸႄႈဝဵင်းတႃႈၶီႈလဵၵ်း ၾႆးမႆႈႁိူၼ်း ၵူၼ်းဝၢၼ်ႈ လင်ၼိုင်ႈ")
# "shan"
```

在撰写本文时，它支持以下输出:

*   mm_uni
*   mm_zg
*   凯伦
*   孟族人
*   掸族

# Zawgyi-Unicode 转换

这个包提供了两个方便的函数供你在 Zawgyi 和 Unicode 之间转换。

## Unicode 到 Zawgyi

对于从 Unicode 到 Zawgyi 的转换，请使用以下函数:

```
pds.cvt2zgi("ထမင်းစားပြီးပြီလား")
# "ထမင္းစားၿပီးၿပီလား"
```

## Zawgyi 到 Unicode

至于 Zawgyi 转 Unicode，可以这样调用:

```
pds.cvt2uni("ထမင္းစားၿပီးၿပီလား")
# "ထမင်းစားပြီးပြီလား"
```

# 标记化

这个包的主要核心特性之一是标记缅甸语文本的能力。在撰写本文时，它支持:

*   音节级标记化(缅甸语、克伦族、掸族、孟族)
*   单词级标记化(缅甸语)

## 音节级标记化

这种标记化基于正则表达式(regex)。它支持缅甸语，克伦族，掸族和孟族语言。叫它如下:

它将返回一个记号列表(记号化的单词)。

## 单词级标记化

另一方面，单词级标记化只支持缅甸语。它基于条件随机场(CRF)预测。照常调用 tokenize 函数，并将`form`参数指定给`word`。

根据输入的文本，输出与音节标签略有不同。在包含英语单词的第二个例子中，注意单词标记化将`Alan Turing`组合成一个单词。

# 未来的工作

虽然这个包相当新，目前提供的功能有限，但作者在官方库中概述了以下路线图:

*   支持新的标记化方法(BPE，文字块等)。)
*   缅甸语的词性标注
*   缅甸语命名实体识别(NER)分类器
*   适当的文件

如果您对其他亚洲语言的语言分析感兴趣，请随意查看以下文章:

*   高棉语—[Python 中的高棉语自然语言处理](/khmer-natural-language-processing-in-python-c770afb84784)
*   泰语—[python 语言入门指南](/beginners-guide-to-pythainlp-4df4d58c1fbe)
*   朝鲜语—[Python 中的朝鲜语自然语言处理](/korean-natural-language-processing-in-python-cc3109cbbed8)
*   日语—[Suda chipy:Python 中的日语词法分析器](/sudachipy-a-japanese-morphological-analyzer-in-python-5f1f8fc0c807)
*   越南语—[pyvi 简介:Python 越南语 NLP 工具包](/introduction-to-pyvi-python-vietnamese-nlp-toolkit-ff5124983dc2)

# 结论

让我们回顾一下你今天所学的内容。

本文首先简要介绍了这个包提供的特性。

然后，它通过`pip install`进入安装过程。

它继续深入解释了语言检测、Zawgyi-Unicode 转换以及标记化。此外，标记化分为音节级和词级。

感谢你阅读这篇文章。祝你有美好的一天！

# 参考

1.  [Github — pyidaungsu](https://github.com/kaunghtetsan275/pyidaungsu)