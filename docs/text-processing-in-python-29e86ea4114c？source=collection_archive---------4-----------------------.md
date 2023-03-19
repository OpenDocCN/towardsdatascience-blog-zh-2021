# Python 中的文本处理

> 原文：<https://towardsdatascience.com/text-processing-in-python-29e86ea4114c?source=collection_archive---------4----------------------->

## 面向所有人的 Python 文本处理指南。

## 使用 NLTK 和 spaCy 的文本处理示例

![](img/e0c3a0e742553044bf9567a5c408a623.png)

布雷特·乔丹在 [Unsplash](https://unsplash.com/s/photos/computer-process?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

互联网连接了世界，而脸书、Twitter 和 Reddit 等社交媒体为人们表达对某个话题的看法和感受提供了平台。然后，智能手机的普及直接增加了这些平台的使用率。例如，有 96%或 22.4 亿脸书活跃用户通过智能手机和平板电脑使用脸书[1]。

社交媒体使用的增加增加了文本数据的大小，并促进了自然语言处理(NLP)中的学习或研究，例如，信息检索和情感分析。很多时候，待分析的文档或文本文件非常庞大，包含大量噪声，直接使用原始文本进行分析是不适用的。因此，文本处理对于为建模和分析提供清晰的输入是必不可少的。

文本处理包含两个主要阶段，即标记化和规范化[2]。**记号化**是将一个较长的文本串分割成较小的片段或记号的过程[3]。**规范化**指将数字转换为对应的单词，去除标点符号，将所有文本转换为相同的大小写，去除停用词，去除噪音，去除词条和词干。

*   词干-删除词缀(后缀、前缀、中缀、抑扬)，例如，连读
*   引理化——根据单词的引理获取标准形式。例如，更好的到好的[4]

在本文中，我将演示用 **Python 进行文本处理。**

以下是从丹尼尔·茹拉夫斯基和詹姆斯·h·马丁的《语音和语言处理》一书中摘录的一段话[6]。

> “赋予计算机处理人类语言能力的想法和计算机本身的想法一样古老。这本书是关于这个令人兴奋的想法的实现和含义。我们介绍了一个充满活力的跨学科领域，其许多方面有许多对应的名称，如语音和语言处理、人类语言技术、自然语言处理、计算语言学以及语音识别和合成。这个新领域的目标是让计算机执行涉及人类语言的有用任务，如实现人机交流、改善人与人之间的交流，或者只是对文本或语音进行有用的处理。”

这一段将在下面的文本处理示例中使用。

# Python 中的文本处理

对于 Python 中的文本处理，演示中将使用两个自然语言处理(NLP)库，即 NLTK(自然语言工具包)和 spaCy。之所以选择这两个库而不是其他文本处理库，如 Gensim 和 Transformer，是因为 NLTK 和 spaCy 是最受欢迎的库，对自然语言处理(NLP)初学者来说非常友好。

对于 NLTK 和 spaCy，首先需要将文本保存为变量。

```
text = """The idea of giving computers the ability to process human language is as old as the idea of computers themselves. This book is about the implementation and implications of that exciting idea. We introduce a vibrant interdisciplinary field with many names corresponding to its many facets, names like speech and language processing, human language technology, natural language processing, computational linguistics, and speech recognition and synthesis. The goal of this new field is to get computers to perform useful tasks involving human language, tasks like enabling human-machine communication, improving human-human communication, or simply doing useful processing of text or speech."""
```

## 使用 NLTK 进行文本处理

1.  导入所有需要的库

```
import re
import pandas as pd
import nltk
from nltk.tokenize import WordPunctTokenizer
nltk.download(’stopwords’)
from nltk.corpus import stopwords
# needed for nltk.pos_tag function nltk.download(’averaged_perceptron_tagger’)
nltk.download(’wordnet’)
from nltk.stem import WordNetLemmatizer
```

1.  **标记化**

使用 tokenizer 将句子分成一系列单词(记号)。

```
word_punct_token = WordPunctTokenizer().tokenize(text)
```

除了上面使用的`WordPunctTokenizer`，NLTK 库中还有几个记号赋予器模块。比如`word_tokenize`和`RegexpTokenizer`。`RegexpTokenizer`能够通过设置`RegexpTokenizer(‘\w+|\$[\d\.]+|\S+’)`将 9.99 美元这样的货币分离为一个单独的令牌。所有提到的记号赋予器将以列表形式返回记号。NLTK 还有一个名为`sent_tokenize` 的模块，它能够将段落分成句子列表。

**2。标准化**

下面的脚本删除了不是单词的标记，例如，符号和数字，以及只包含少于两个字母或只包含辅音的标记。这个脚本在这个例子中可能没有用，但是在处理大量文本数据时非常有用，它有助于清除大量干扰。每当我处理文本数据时，我总是喜欢包含它。

```
clean_token=[]
for token in word_punct_token:
    token = token.lower()
    *# remove any value that are not alphabetical*
    new_token = re.sub(r'[^a-zA-Z]+', '', token) 
    *# remove empty value and single character value*
    if new_token != "" and len(new_token) >= 2: 
        vowels=len([v for v in new_token if v in "aeiou"])
        if vowels != 0: # remove line that only contains consonants
            clean_token.append(new_token)
```

**2a。移除停用字词**

停用词指的是没有太多含义的词，如介词。NLTK 和 spaCy 在库中有不同数量的停用词，但是 NLTK 和 spaCy 都允许我们添加任何我们认为必要的词。例如，当我们处理电子邮件时，我们可以添加`gmail`、`com`、`outlook` 作为停用词。

```
# Get the list of stop words
stop_words = stopwords.words('english')
# add new stopwords to the list
stop_words.extend(["could","though","would","also","many",'much'])
print(stop_words)
# Remove the stopwords from the list of tokens
tokens = [x for x in clean_token if x not in stop_words]
```

**2b。词性标注(POS Tag)**

这个过程指的是用词类位置来标记单词，例如，动词、形容词和名词。模块以元组的形式返回结果，为了方便以后的工作，通常我会将它们转换成数据帧。在[6]中，POS 标签是在标记化之后直接执行的任务，当你知道你只需要像形容词和名词这样的特定词类时，这是一个聪明的举动。

```
data_tagset = nltk.pos_tag(tokens)
df_tagset = pd.DataFrame(data_tagset, columns=['Word', 'Tag'])
```

**2c。词汇化**

词汇化和词干化都有助于通过将单词返回到其词根形式(词汇化)或删除所有后缀、词缀、前缀等(词干化)来减少词汇的维数。词干对于减少词汇的维度是很好的，但是大多数时候这个单词变得毫无意义，因为词干只是砍掉了后缀，而不是将单词还原成它们的基本形式。比如*宅*词干化后会变成*宅*，完全失去意义。因此，词汇化对于文本分析更为可取。

以下脚本用于获取名词、形容词和动词的词根形式。

```
# Create lemmatizer object 
lemmatizer = WordNetLemmatizer()# Lemmatize each word and display the output
lemmatize_text = []
for word in tokens:
    output = [word, lemmatizer.lemmatize(word, pos='n'),       lemmatizer.lemmatize(word, pos='a'),lemmatizer.lemmatize(word, pos='v')]
    lemmatize_text.append(output)# create DataFrame using original words and their lemma words
df = pd.DataFrame(lemmatize_text, columns =['Word', 'Lemmatized Noun', 'Lemmatized Adjective', 'Lemmatized Verb']) 

df['Tag'] = df_tagset['Tag']
```

将形容词、名词和动词分别进行词汇化的原因是为了提高词汇化的准确性。

```
# replace with single character for simplifying
df = df.replace(['NN','NNS','NNP','NNPS'],'n')
df = df.replace(['JJ','JJR','JJS'],'a')
df = df.replace(['VBG','VBP','VB','VBD','VBN','VBZ'],'v')'''
define a function where take the lemmatized word when tagset is noun, and take lemmatized adjectives when tagset is adjective
'''df_lemmatized = df.copy()
df_lemmatized['Tempt Lemmatized Word']=df_lemmatized['Lemmatized Noun'] + ' | ' + df_lemmatized['Lemmatized Adjective']+ ' | ' + df_lemmatized['Lemmatized Verb']df_lemmatized.head(5)
lemma_word = df_lemmatized['Tempt Lemmatized Word']
tag = df_lemmatized['Tag']
i = 0
new_word = []
while i<len(tag):
    words = lemma_word[i].split('|')
    if tag[i] == 'n':        
        word = words[0]
    elif tag[i] == 'a':
        word = words[1]
    elif tag[i] == 'v':
        word = words[2]
    new_word.append(word)
    i += 1df_lemmatized['Lemmatized Word']=new_word
```

上面的脚本是根据单词的 POS 标签将正确的词汇化单词分配给原始单词。

**3。获取清理后的令牌**

```
# calculate frequency distribution of the tokens
lemma_word = [str(x) for x in df_lemmatized['Lemmatized Word']]
```

在对文本进行了标记化和规范化之后，现在我们获得了一个干净标记的列表，可以插入到 WordCloud 或其他文本分析模型中。

## 空间文本处理

1.  **标记化+词条化**

与 NLTK 相比，SpaCy 的优势在于它简化了文本处理过程。

```
# Import spaCy and load the language library
import spacy
#you will need this line below to download the package
!python -m spacy download en_core_web_sm
nlp = spacy.load('en_core_web_sm')# Create a Doc object
doc = nlp(text)
token_list = []
# collect each token separately with their POS Tag, dependencies and lemma
for token in doc:
    output = [token.text, token.pos_, token.dep_,token.lemma_]
    token_list.append(output)# create DataFrame using data 
df = pd.DataFrame(token_list, columns =['Word', 'POS Tag', 'Dependencies', 'Lemmatized Word']) 
```

在 spaCy 中，您可以在执行标记化时获得 POS 标签和 lemma(单词的词根形式),这样可以节省一些精力。

**2。标准化**

由于词汇化是在最开始执行的，因此规范化步骤只剩下去除噪声和停用词。

**2a。去除噪音**

```
df_nopunct = df[df['POS Tag']!='PUNCT']
df_nopunct
```

spaCy POS 标签包含一个显示为`PUNCT`的标点符号，因此我们可以通过删除带有`PUNCT`标签的标记来删除所有标点符号。

**2b。移除停用字词**

```
import numpy as np
lemma_word = df_nopunct['Lemmatized Word'].values.tolist()stopword = nlp.Defaults.stop_words
# Add the word to the set of stop words. Use lowercase!
nlp.Defaults.stop_words.add('btw')is_stopword_list = []
for word in lemma_word:
    is_stopword = nlp.vocab[word].is_stop
    is_stopword_list.append(is_stopword)
df_nopunct["is_stopword"] = is_stopword_list
df_nopunct
clean_df = df_nopunct[df_nopunct["is_stopword"]==False]
```

SpaCy 有一个类`.is_stop`，可以用来检测一个令牌是否是一个停用词，我们可以用它来删除停用词，如上面的脚本所示。

**3。获取清理后的令牌**

```
clean_list = clean_df["Lemmatized Word"].values.tolist()
```

现在我们获得了已清理令牌的列表！

spaCy 比 NLTK 更快、更先进，但是 NLTK 更适合初学者理解文本处理中的每个过程，因为它发生的时间更长，并且有很多文档和解释。您可以在这里找到 spaCy 和 NLTK 在其他特性方面的比较，如 GPU 支持、效率、性能、艺术水平、词向量和灵活性[。](/text-normalization-with-spacy-and-nltk-1302ff430119)

谢谢你读到这里！希望上面的演示能帮助你理解 Python 中的文本处理。

对于文本处理，理解数据结构与正则表达式(RegEx)一样重要。例如，类或模块的返回可能是元组和列表的形式，要操作返回输出，我们首先必须理解它们。下面是我经常使用的一些脚本，用于根据不同的目的改变变量的数据结构。

1.  **从数据框架(多列)到嵌套列表，从嵌套列表到列表**

当我们创建主题建模或情感分析模型时，通常我们根据句子或段落单独保存令牌，例如，评论和推文单独保存，以获得每个评论或推文的准确情感或主题。因此，如果我们有 10 个评论，就会有 10 个标记列表，将它们保存在一个变量中创建一个嵌套列表。另一个例子是一个数据框架，它有几列包含文本数据，可以是调查的问答题，也可以是对某个产品的评论。我们可能希望将这些列直接转换成列表，但是将几列数据转换成列表也会创建一个嵌套列表。

```
# df_essays is a dataframe with few columns of text data
essay_list = df_essays.values.tolist() # this create a nested list# this create a flat list
flatEssayList = [item for elem in essay_list for item in elem if str(item) != 'nan']
```

**2。从数据框架(单列)或列表到文本**

文本分析中最流行的应用之一是 WordCloud，它主要用于分析文本中最常见的讨论。这个模块只接受文本作为输入，所以在这个场景中，我们需要改变列表或数据帧。列到文本。

```
# dataframe.column to text
text = ‘ ‘.join(str(x) for x in df[‘review’])
# list to text
text = ‘ ‘.join(str(x) for x in any_list)
```

**3。从数据帧(单列)到列表**

```
reviews_list = df["reviews"].values.tolist()
```

**4。从文本到列表**

这既简单又方便。

```
token_list = text.split()
```

**5。从元组到数据帧**

我上面提到过这个，在这里重复一遍，以便更好的参考。

`nltk.pos_tag`模块以`(word, pos tag)`的形式返回标签集作为元组。

```
data_tagset = nltk.pos_tag(tokens)
df_tagset = pd.DataFrame(data_tagset, columns=['Word', 'Tag'])
```

# 一些旁注

如果你对从关键词列表中提取包含关键词的句子感兴趣:
[使用正则表达式(Python)的文本提取](/text-extraction-using-regular-expression-python-186369add656)。

如果你有兴趣用词云探索巨大的文本:
[用词云探索文本](/text-exploration-with-python-cb8ea710e07c)。

# 保持联系

订阅 [YouTube](https://www.youtube.com/channel/UCiMtx0qbILP41Ot-pkk6eJw)

# 参考

[1] M. Iqbal，“脸书收入和使用统计(2020 年)”，2021 年 3 月 8 日。【在线】。可用:[https://www.businessofapps.com/data/facebook-statistics/.](https://www.businessofapps.com/data/facebook-statistics/.)

[2] M. Mayo，“预处理文本数据的一般方法”，2017。【在线】。可用:[https://www . kdnugges . com/2017/12/general-approach-预处理-text-data . html](https://www.kdnuggets.com/2017/12/general-approach-preprocessing-text-data.html.)【2020 年 6 月 12 日访问】。

[3] D. Subramanian，“Python 中的文本挖掘:步骤和示例”，2019 年 8 月 22 日。【在线】。可用:[https://medium . com/forward-artificial-intelligence/text-mining-in-python-steps-and-examples-78 B3 F8 FD 913 b。](https://medium.com/towards-artificial-intelligence/text-mining-in-python-steps-and-examples-78b3f8fd913b.)【2020 年 6 月 12 日进入】。

[4] M. Mayo，《自然语言处理关键术语解释》，2017。【在线】。可用:[https://www . kdnugges . com/2017/02/natural-language-processing-key-terms-explained . html .](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html.)

[5]《Julia 中的自然语言处理(文本分析)》，JCharisTech，2018 年 5 月 1 日。【在线】。可用:[https://jcharistech . WordPress . com/2018/05/01/natural-language-processing-in-Julia-text-analysis/。](https://jcharistech.wordpress.com/2018/05/01/natural-language-processing-in-julia-text-analysis/.)

[6] D. Jurafsky 和 J. H. Martin，“语音和语言处理”，2020 年 12 月 3 日。【在线】。可用:[https://web.stanford.edu/~jurafsky/slp3/.](https://web.stanford.edu/~jurafsky/slp3/.)

[7] M.F. Goh，“使用 spaCy 和 NLTK 进行文本规范化”，2020 年 11 月 29 日。【在线】。可用:[https://towardsdatascience . com/text-normalization-with-spacy-and-nltk-1302 ff 430119。](/text-normalization-with-spacy-and-nltk-1302ff430119.)

*祝贺并感谢你阅读到最后。希望你喜欢这篇文章。* ☺️

![](img/26eb9950ccaeafba69e8f996cfb2dedb.png)

照片由[Courtney hedge](https://unsplash.com/@cmhedger?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/thank-you?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄