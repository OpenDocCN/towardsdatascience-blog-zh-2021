# 使用正则表达式的文本提取(Python)

> 原文：<https://towardsdatascience.com/text-extraction-using-regular-expression-python-186369add656?source=collection_archive---------7----------------------->

## 正则表达式文本抽取指南

## 用正则表达式提取带有关键字的文本的例子。

![](img/5a9726dd8f571859556a9263df100ca0.png)

凯利·西克玛在 [Unsplash](https://unsplash.com/s/photos/regular-expression?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

> “正则表达式(RegEx)是计算机科学标准化中的一个默默无闻的成功案例，”[1]。

在我的[上一篇文章](/text-processing-in-python-29e86ea4114c)的例子中，正则表达式是用来清理噪声和对文本进行记号化的。好吧，在文本分析中我们可以用正则表达式做的远不止这些。在这篇文章中，我分享了如何使用正则表达式从文本数据或语料库中提取包含定义列表中任何关键字的句子。例如，您可能想要提取对特定产品功能的评论，或者您可能想要提取讨论紧急或关键主题的所有电子邮件。

对我来说，我在文本分析项目中使用的文本文档非常大，如果我要在我的设备上训练模型，这需要很长时间。此外，我对数据集中的每一句话都不感兴趣，因此文本提取是我项目中数据清理过程的一部分。仅提取包含已定义关键词的句子不仅可以减少词汇库的大小，还可以根据我的需要使模型更加精确。

然而，这种方法并不适合所有的文本分析项目。例如，如果你正在研究人们对特定主题、物体或事件的总体情绪，提取句子可能会影响总体情绪的准确性。另一方面，如果你确定你想研究的重点是什么，这种方法是合适的。例如，你经营一家销售家用产品的电子商务，你想知道人们对你的新产品的感觉，你可以直接提取所有包含新产品名称或型号的评论。

例如，电子商务商店推出了一款与以往产品高度不同的吧台凳，并希望了解客户的反应。下面突出显示的句子将是带有关键字“酒吧凳子”和“高度”的文本提取的输出

![](img/2399ba7e67ae8f9c44b2da6250937e8d.png)

背景:Joshua Bartell 在 [Unsplash](https://unsplash.com/s/photos/plain-background?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

嗯，这个介绍比我预想的要长。让我们不要再浪费时间了，开始吧！

# 1.读取包含文本数据和关键字的文本文件

```
## read sentences and extract only line which contain the keywords
import pandas as pd
import re
# open file
keyword = open('keyword.txt', 'r', encoding = 'utf-8').readlines()
texts = open('sent_token.txt', 'r', encoding = 'utf-8').readlines()
# define function to read file and remove next line symbol
def read_file(file):
    texts = []
    for word in file:
        text = word.rstrip('\n')
        texts.append(text) return texts# save to variable        
key = read_file(keyword)
corpus = read_file(texts)
```

在上面的脚本中，输入是存储在文本文件中的句子标记和关键字列表。您可以将文档中的数据集标记为段落或句子，然后提取包含关键字的段落或句子。句子标记化可以通过下面的`nltk.tokenize`中的`sent_tokenize`轻松完成。

```
from nltk.tokenize import sent_tokenize
text = open('Input/data.txt', 'r', encoding = 'utf-8')text_file = open("Output/sent_token.txt", "w", encoding='utf-8')### This part to remove end line break
string_without_line_breaks = ""
for line in text:
    stripped_line = line.rstrip() + " "
    string_without_line_breaks += stripped_linesent_token = sent_tokenize(string_without_line_breaks)
for word in sent_token:
    text_file.write(word)
    text_file.write("\n")text_file.close()
```

由于我使用的文本数据是从 PDF 文件中提取的，因此有许多换行符，因此我将在句子标记化之前删除换行符。

# 2.编写提取直线的函数

```
# open file to write line which contain keywords
file = open('Output/keyline.txt', 'w', encoding = 'utf-8') 
def write_file(file, keyword, corpus):    
    keyline = []      
    for line in corpus:
        line = line.lower()
        for key in keyword:
            result = re.search(r"(^|[^a-z])" + key + r"([^a-z]|$)", line)
            if result != None:
                keypair = [key, line]
                keyline.append(keypair) 
                file.write(line + " ")                                
                break                 
            else:
                pass                                            

    return(keyline)output = write_file(file,key,corpus)
```

上面的函数是我用来提取所有包含关键字的句子的函数。添加了一个`break` 来防止复制同一行的多个关键字来降低文件大小。这样做的关键脚本只是一行代码。

`result = re.search(r”(^|[^a-z])” + key + r”([^a-z]|$)”, line)`

关键字周围的`”(^|[^a-z])”`和`”([^a-z]|$)”` 是为了确保单词与关键字一致。例如，如果没有脚本的这两个部分，当我们搜索一个像“act”这样的关键字时，返回的结果可能是带有前缀或后缀的单词“act”，例如“react”或“actor”。

```
# create DataFrame using data 
df = pd.DataFrame(output, columns =['Key', 'Line']) 
```

提取行后，您可以从关键字和提取的相应行创建数据框架，以供进一步分析。请注意，在我的示例中，如果同一行包含多个关键字，我不会再次提取它。如果您需要这样做，您可以从上面的脚本中删除`break` 命令。

以上就是关于如何提取带关键词的那一行句子。简单吧？

最后但同样重要的是，分享一个我最近一直在使用的脚本，`os.listdir.startswith()`和`os.listdir.endswith()`可以帮助你有效地获得所有你需要的文件。这有点类似于正则表达式的概念，我们定义一个特定的模式，然后搜索它。

```
# collect txt file with name start with 'data' 
import os
path = 'C:/Users/Dataset txt'
folder = os.fsencode(path)
filenames_list = []for file in os.listdir(folder):
    filename = os.fsdecode(file)
    if filename.startswith( ('data') ) and filename.endswith( ('.txt') ): 
        filenames1.append(filename)filenames_list.sort() 
```

# 一些旁注

如果你对 NLTK 和 SpaCy 的文本处理感兴趣:
[Python 中的文本处理](/text-processing-in-python-29e86ea4114c)。

如果你有兴趣探索文字云的巨大文本:
[文字云探索](/text-exploration-with-python-cb8ea710e07c)。

# 保持联系

订阅 [YouTube](https://www.youtube.com/channel/UCiMtx0qbILP41Ot-pkk6eJw)

# 参考

[1] D. Jurafsky 和 J. H. Martin，“语音和语言处理”，2020 年 12 月 3 日。【在线】。可用:[https://web.stanford.edu/~jurafsky/slp3/.](https://web.stanford.edu/~jurafsky/slp3/.)

*祝贺并感谢你阅读到最后。希望你喜欢这篇文章。* ☺️

![](img/37ccf36830e82b26c8ae39442065b324.png)

照片由[威廉·冈克尔](https://unsplash.com/@wilhelmgunkel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/thank-you?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄