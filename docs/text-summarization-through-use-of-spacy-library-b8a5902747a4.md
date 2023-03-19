# 利用空间库进行文本摘要

> 原文：<https://towardsdatascience.com/text-summarization-through-use-of-spacy-library-b8a5902747a4?source=collection_archive---------26----------------------->

![](img/6d6090f8e733e77b972b509a686ba37b.png)

图像[来源](https://unsplash.com/photos/7x13D6qqOho?utm_source=unsplash&utm_medium=referral&utm_content=creditShareLink)

自然语言处理中的文本摘要意味着用有限数量的单词简短地讲述一个长故事，并简单地传达一个重要的信息。

可以有许多策略来使大消息简短并向前给出最重要的信息，其中之一是计算词频，然后通过除以最大频率来归一化词频。

然后找出出现频率高的句子，选择最重要的句子来传达信息。

**为什么我们需要自动摘要？**

时间优化，因为它需要更少的时间来获得文本的要点。

自动摘要可以改进索引。

如果我们使用自动摘要，可以处理更多的文档。

自动摘要中的偏差比手动摘要中的偏差小。

**文本摘要可以有多种类型，例如:**

1)基于输入类型:可以是单个文档，也可以是多个文档，需要从中对文本进行汇总。

2)基于目的:总结的目的是什么，这个人需要回答问题，还是特定领域的总结，还是一般的总结。

3)基于输出类型:取决于输出是抽象的还是提取的。

**文本摘要步骤:**

1)文本清理:去除停用词、标点符号，使单词变成小写。

2)工作分词:对句子中的每个单词进行分词。

3)词频表:统计每个词出现的频率，然后用每个频率除以最大频率，得到归一化的词频计数。

4)句子标记化:根据句子出现的频率

5)总结

现在让我们看看如何使用 Spacy 库来完成它。

安装和导入空间库，也可以导入停用词。

```
pip install spacy
import spacy
from spacy.lang.en.stop_words import STOP_WORDS
```

从字符串中导入标点符号，并在其中添加额外的下一行标记。

```
stopwords=list(STOP_WORDS)
from string import punctuation
punctuation=punctuation+ '\n'
```

在可变文本中存储一个文本，我们需要从中总结文本。

```
text="""The human coronavirus was first diagnosed in 1965 by Tyrrell and Bynoe from the respiratory tract sample of an adult with a common cold cultured on human embryonic trachea.1 Naming the virus is based on its crown-like appearance on its surface.2 Coronaviruses (CoVs) are a large family of viruses belonging to the Nidovirales order, which includes Coronaviridae, Arteriviridae, and Roniviridae families.3 Coronavirus contains an RNA genome and belongs to the Coronaviridae family.4 This virus is further subdivided into four groups, ie, the α, β, γ, and δ coronaviruses.5 α- and β-coronavirus can infect mammals, while γ- and δ- coronavirus tend to infect birds.6 Coronavirus in humans causes a range of disorders, from mild respiratory tract infections, such as the common cold to lethal infections, such as the severe acute respiratory syndrome (SARS), Middle East respiratory syndrome (MERS) and Coronavirus disease 2019 (COVID-19). The coronavirus first appeared in the form of severe acute respiratory syndrome coronavirus (SARS-CoV) in Guangdong province, China, in 20027 followed by Middle East respiratory syndrome coronavirus (MERS-CoV) isolated from the sputum of a 60-year-old man who presented symptoms of acute pneumonia and subsequent renal failure in Saudi Arabia in 2012.8 In December 2019, a β-coronavirus was discovered in Wuhan, China. The World Health Organization (WHO) has named the new disease as Coronavirus disease 2019 (COVID-19), and Coronavirus Study Group (CSG) of the International Committee has named it as SARS-CoV-2.9,10 Based on the results of sequencing and evolutionary analysis of the viral genome, bats appear to be responsible for transmitting the virus to humans"""
```

从课文的句子中给单词做记号。

```
nlp = spacy.load('en_core_web_sm')
doc= nlp(text)
tokens=[token.text for token in doc]
print(tokens)
```

删除停用词和标点符号后，计算文本中的词频。

```
word_frequencies={}
for word in doc:
    if word.text.lower() not in stopwords:
        if word.text.lower() not in punctuation:
            if word.text not in word_frequencies.keys():
                word_frequencies[word.text] = 1
            else:
                word_frequencies[word.text] += 1
```

打印看看词频就知道重要的词了。

```
print(word_frequencies)
```

计算最大频率，除以所有频率，得到归一化的词频。

```
max_frequency=max(word_frequencies.values())
for word in word_frequencies.keys():
    word_frequencies[word]=word_frequencies[word]/max_frequency
```

打印标准化词频。

```
print(word_frequencies)
```

获取句子令牌。

```
sentence_tokens= [sent for sent in doc.sents]
print(sentence_tokens)
```

通过添加每个句子中的词频来计算最重要的句子。

```
sentence_scores = {}
for sent in sentence_tokens:
    for word in sent:
        if word.text.lower() in word_frequencies.keys():
            if sent not in sentence_scores.keys():                            
             sentence_scores[sent]=word_frequencies[word.text.lower()]
            else:
             sentence_scores[sent]+=word_frequencies[word.text.lower()]
```

打印句子分数

```
sentence_scores
```

from '*head HQ*' import '*n largest '*，计算 30%的文本的最高分。

```
from heapq import nlargest
select_length=int(len(sentence_tokens)*0.3)
select_length
summary=nlargest(select_length, sentence_scores,key=sentence_scores.get)
summary
```

获取课文摘要。

```
final_summary=[word.text for word in summary]
final_summary
summary=''.join(final_summary)
summary
```

我们得到如下输出:

```
'and β-coronavirus can infect mammals, while γ- and δ- coronavirus tend to infect birds.6 Coronavirus in humans causes a range of disorders, from mild respiratory tract infections, such as the common cold to lethal infections, such as the severe acute respiratory syndrome (SARS), Middle East respiratory syndrome (MERS) and Coronavirus disease 2019 (COVID-19).The coronavirus first appeared in the form of severe acute respiratory syndrome coronavirus (SARS-CoV) in Guangdong province, China, in 20027 followed by Middle East respiratory syndrome coronavirus (MERS-CoV) isolated from the sputum of a 60-year-old man who presented symptoms of acute pneumonia and subsequent renal failure in Saudi Arabia in 2012.8The World Health Organization (WHO) has named the new disease as Coronavirus disease 2019 (COVID-19), and Coronavirus Study Group (CSG) of the International Committee has named it as SARS-CoV-2.9,10 Based on the results of sequencing and evolutionary analysis of the viral genome, bats appear to be responsible for transmitting the virus to humans
```

**结论**

这只是通过使用最频繁使用的单词，然后计算最重要的句子来获得文本摘要的方法之一。

可以有各种其他方式，如使用库' *nltk'* ,通过使用词法分析、词性标记器和 n 元语法、命名实体识别和句子之间的余弦相似性来完成。我们将在我的下一篇博客中详细讨论。

*原载于 2021 年 3 月 2 日*[*【https://www.numpyninja.com】*](https://www.numpyninja.com/post/text-summarization-through-use-of-spacy-library)*。*