# 从社会公正的角度看如何用 NLP 创建标签

> 原文：<https://towardsdatascience.com/how-to-transform-technical-jargon-into-simple-bi-tri-grams-with-nlp-on-a-public-dataset-2081f5609c1f?source=collection_archive---------16----------------------->

![](img/132d810e9e75751c867f205f295585c0.png)

[粘土银行](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

> 那么，互联网是一种人工制品，既是正式教育过程的延伸，也是“非正式文化”，因此它是一种“主体性的发挥”。这一想法提供了另一个有利的角度，可以理解媒体中的代表性(和虚假陈述)是权力关系的一种表达方式——Safiya Umoja Noble(压迫的算法:搜索引擎如何强化种族主义，2018 年)

屏幕另一边的人工智能对谦逊声音的压制正在扩大。我最近遇到一个小企业主。她的商业吸引力减少了，因为她的网站不再符合最近更新的搜索引擎优化算法。她的问题的解决方案比用于故障排除的标准技术更复杂。与谷歌**自然语言**或“搜索引擎”算法合作的最佳方式是什么？人类可以“学习人工智能来设计他们的方式来提高谷歌搜索的排名吗？

谷歌使用复杂的语料库和单词包算法来绘制现有网站，每秒钟可以理解 40，000 次搜索。让少数民族、残疾人、退伍军人和女性拥有的企业等不太为人所知的声音发出来的方法是，让这些社区拥有一个能够理解并向 SEO 算法提供微调参数(词)的人工智能。有一种方法可以借鉴 Google NLP 算法，优化并动态更新自报能力。在本文结束时，您将会实现以下目标:

*   将句子转换成有意义的二元模型和三元模型。
*   轻松添加和更改数据框和数据类型。
*   对公开报道的利基数据应用 PMI、[单词包](https://machinelearningmastery.com/gentle-introduction-bag-words-model/)、n-grams 和非结构化数据清理，以生成标签。
*   文本摘要、模型偏差和改进后端数据集的方法。

我们会从这里学习如何处理事情-

到这里-

让我们直接进入解决方案-

![](img/961169bd4f859d45dcea0fb2bb5a77ef.png)

照片由[Christian Wied](https://unsplash.com/@christianw?utm_source=medium&utm_medium=referral)Let ' s son[Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 数据集

纽约州在州属企业数据库中维护**D**isabled/**W**omen/**M**Bbusiness**E**E。审计官办公室托管主数据库，帝国审计官根据具体情况授予企业认证，并将其纳入[法律登记处](https://ny.newnycontracts.com/FrontEnd/SearchCertifiedDirectory.asp)。纽约市为特定城市的公司维护另一个数据库。接下来的部分将介绍我们如何合并这些数据库和数据可视化。

州数据库可能很难导航。应该与自我报告功能相对应的 Google 搜索可能不会映射州数据库，因为它具有受保护的性质。在定义了问题之后，让我们更深入地研究数据库的核心。对于这个问题，我将使用的数据库是[SDVOB](https://online.ogs.ny.gov/SDVOB/search)——截至 2022 年 1 月 27 日，数据库中有 **932 家认证的伤残退伍军人所有的企业**。您可以通过按下**搜索**按钮下载 excel 文件。

![](img/1a3bcabff28ea8a9ab4c2ac8285f1148.png)

乔希·阿佩尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 配方的模块

```
import numpy as np
import pandas as pd
from pandas import DataFrame, Series  # for convenience
import re
import glob
import copy
import os
import xlrd
import nltk
import unicodedata
import re
from collections import Counter
from nltk.collocations import *
bigram_measures = nltk.collocations.BigramAssocMeasures()
trigram_measures = nltk.collocations.TrigramAssocMeasures()
from nltk.tokenize import word_tokenize
from nltk.tag import pos_tag
import spacy
#!pip install -U pip setuptools wheel
#!pip install -U spacy
#!python -m spacy download en_core_web_sm
nlp = spacy.load("en_core_web_sm")
import en_core_web_sm
nlp = en_core_web_sm.load()
from bs4 import BeautifulSoup
from html import unescape
```

# 探索数据集

数据集包含 936 行× 21 列，其中一列包含自我报告功能-

-以及关键字和关于列。由于每一列都包含关键信息，这些信息可以让我们深入了解企业在做什么，因此我们需要训练一个模型，该模型使用这些信息的**组合作为之前的**，而没有任何嵌入的**分类**(或偏差)。这意味着我们的模型没有考虑信息是否来自关键字或关于列；该模型假设**自我报告的信息与公共记录认证和验证的信息一样有价值**。

因此，下一步是将信息组合成一个列表；列表的索引对应于企业在数据库中的位置。正如你可能看到的，特征数据是**不干净的，**有几个不需要的 ASCII 字符，但是如果你仔细观察，这些字符是分隔关键字的，例如:

```
string1 = "Marijuana Dispensary, Online Retail Sales"
string2 = "Environmental remediation services; Fire and or Water Damage Restoration Services"print(string1.split(sep =', '))
>>['Environmental remediation services',  'Fire and or Water Damage Restoration Services']#try - string2.split(sep='; ')
```

因此，要在没有 NLP 的情况下提取所需的二元模型和三元模型，我们可以使用分隔符，分隔符的选择取决于我们！这里有一些我可以观察到的分隔符`&, --,: etc.`,所以在我们下一步清理数据之前，让我们列出一个可以循环的列表。

# 数据集分离

下面是我如何制作一个总数据的**列表**

在我们进入清理和提取 hashtags 的循环之前，我将定义**关键函数来清理、提取和整理数据。**

## 基本功能

0. **Basic clean** 是最重要的函数，它使用 NLP 并返回一个没有 Unicode、ASCII 和停用词的字符串列表！通过使用 Beautifulsoup 来清理 HTML 语法或定义停用词，您可以将它提升到一个更高的水平。当我们**在将成为数据框单元一部分的字符串列表中获得重复的**时，该函数获取一组列表并将它们转换成一个字符串。

现在我们有了基本的清理，我们可以创建这个整洁的函数来大写单词-

1.  下面的函数将帮助我们**删除位于数据框单元格内的字符串列表**中的重复项，并吐出组合了这些单词的**字符串。**

比如说—

```
`s = “ Hello Hello Sir”
strsetoflist(s.split())
>>'Hello Sir'
```

2.不幸的是， **np nan** 一直是我所有数据方的客人，我不得不使用这个函数**将其从字符串列表**中移除——

3.**如果你没有字符串列表，那么在字符串中删除重复项**就有点复杂了，所以为了安全起见，这里是第一个函数的扩展。

4.移除非类型是我们需要考虑的另一个麻烦。

# 标签生成

欢迎使用机器！这里有几个移动部分——选择和调整参数。其中第一个是**字数***(len(q))***第二个是**[**三元组和二元组**](https://blog.xrds.acm.org/2017/10/introduction-n-grams-need/)**第三个是[**PMI**](https://medium.com/dataseries/understanding-pointwise-mutual-information-in-nlp-e4ef75ecb57a)**(**点态互信息)。我们使用 PMI 来识别“*搭配*的单词。首先，让我们解开双循环，走向 if-else。******

****因为数据列表的长度等于行数，所以让我们在原始数据框中创建一个单独的列来开始操作。我们可以通过更改列表本身并将其附加到数据框来实现相同的目标。****

1.  ****首先，我们将对数据中的每个字符串元素使用`split()`函数来创建一个双列表。然后我们将`basic_cleanandcapitalize()`这些单词，并开始一个接一个地删除和操作双列表中的每个单词/字符串。****

****2.我们将**分割字符串(总数据中的单词)并创建一个我们想要检查的虚拟**列表‘q’。****

****3.正如您可能看到的，我们希望将单词限制在长度 2–4 之间，所以我们想要一个长度在(1，5) 之间的**字符串。然后，我们将列表放回字符串中，并将其与原始数据框连接起来。******

****4.否则——我们使用列表中的三元模型查找器，并*应用过滤器* `apply_freq_filter` ( **移除频率小于 min_freq** 的候选元模型)。— 1 表示单词至少出现一次，这是另一个可以考虑的值。这里你也可以使用一个 bigram_measures 和[bigram collocation finder](https://tedboy.github.io/nlps/generated/generated/nltk.BigramCollocationFinder.html)。****

****5.我们还使用 **finder 和 nbest 来测量 PMI** 以检查单词**一起是否有意义**，并从该元组([0:1]) 中选择**前两个候选项，这里的机制是位扭曲的，试错法将比 PMI 的[数学解释得更多。](https://courses.engr.illinois.edu/cs447/fa2018/Slides/Lecture17HO.pdf)******

****6.最后，我们将**元组转换成字符串**，并在将临时变量合并到数据帧之前使用 tidy 函数。我们可以通过制作标签来总结:****

```
**df1['Hashtags'] = df1['Data_total_grams'].apply(nonetype_remove )**
```

# ****结论****

****上述过程解释了如何将句子转换成有意义的两个或三个单词的组合，并选择适合应用的单词。这篇文章的总体目标是解释简单的人工智能算法如何帮助和授权资源有限的社区进行“标记”和“标注”。关于[人工智能的社会挑战](https://www.nature.com/articles/s41599-019-0278-x.pdf)和[组织人工智能伦理](https://hbr.org/2021/07/everyone-in-your-organization-needs-to-understand-ai-ethics)的定性文章阐明了这个问题。这篇文章是将事情付诸行动的一个小小尝试。我们可以在 https://toughleaf.com[的](https://toughleaf.com)找到算法的应用****