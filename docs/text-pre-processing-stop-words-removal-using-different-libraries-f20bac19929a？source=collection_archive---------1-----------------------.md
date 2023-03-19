# 文本预处理:使用不同的库停止单词删除

> 原文：<https://towardsdatascience.com/text-pre-processing-stop-words-removal-using-different-libraries-f20bac19929a?source=collection_archive---------1----------------------->

## Python 中英文停用词移除的便捷指南

![](img/119eb7e7c655daaa59720c0ccc2112c4.png)

图像由 [Kai](https://unsplash.com/@kaip) 在 [Unsplash](https://unsplash.com/photos/1k3vsv7iIIc)

我们很清楚这样一个事实，如果编程好，计算机可以很容易地处理数字。🧑🏻‍💻然而，我们拥有的大部分信息都是文本形式的。📗我们通过直接与他们交谈或使用短信、社交媒体帖子、电话、视频通话等方式相互交流。为了创造智能系统，我们需要利用我们丰富的信息。

**自然语言处理** **(NLP)** 是人工智能的一个分支，允许机器解读人类语言。👍🏼但是，相同的不能被机器直接使用，我们需要先对相同的进行预处理。

**文本预处理**是准备文本数据的过程，以便机器可以使用这些数据来执行分析、预测等任务。文本预处理有许多不同的步骤，但在本文中，我们将只熟悉停用词，我们为什么要删除它们，以及可以用来删除它们的不同库。

那么，我们开始吧。🏃🏽‍♀️

## **什么是停用词？**🤔

在处理自然语言之前通常被过滤掉的单词被称为**停用词**。这些实际上是任何语言中最常见的单词(如冠词、介词、代词、连词等)，不会给文本增加太多信息。英语中一些停用词的例子有“the”、“a”、“an”、“so”、“what”。

## 为什么我们要删除停用词？🤷‍♀️

任何人类语言中都有大量的停用词。通过删除这些单词，我们从文本中删除了低级信息，以便将更多的注意力放在重要信息上。换句话说，我们可以说，移除这样的单词不会对我们为任务训练的模型产生任何负面影响。

停用词的移除无疑减小了数据集的大小，并且由于训练中涉及的标记数量更少，因此减少了训练时间。

## 我们总是删除停用词吗？它们对我们来说总是无用的吗？🙋‍♀️

答案是否定的！🙅‍♂️

我们并不总是删除停用词。停用词的去除高度依赖于我们正在执行的任务和我们想要实现的目标。例如，如果我们正在训练一个可以执行情感分析任务的模型，我们可能不会删除停用词。

***影评:*** “电影一点都不好。”

***文字删除后的停止词:****【电影好】*

*我们可以清楚地看到，对这部电影的评论是负面的。但是去掉停用词之后，评论就变成正面了，现实并不是这样。因此，停用词的删除在这里可能会有问题。*

*像文本分类这样的任务通常不需要停用词，因为数据集中存在的其他词更重要，并且给出了文本的总体思想。因此，我们通常会在这类任务中删除停用词。*

*简而言之，NLP 有很多任务在去除停用词后无法正常完成。所以，在执行这一步之前要三思。这里的问题是，没有通用的规则，也没有通用的停用词列表。对一项任务不传达任何重要信息的列表可以对另一项任务传达大量信息。*

***忠告:**在去掉停止语之前，先研究一下你的任务和你试图解决的问题，然后再做决定。*

## ***删除停用词有哪些不同的库？**🙎‍♀️*

*自然语言处理是当今研究最多的领域之一，在这个领域已经有了许多革命性的发展。NLP 依赖于先进的计算技能，世界各地的开发人员已经创建了许多不同的工具来处理人类语言。在这么多的库中，有几个非常受欢迎，并在执行许多不同的 NLP 任务时提供了很大的帮助。*

*下面给出了一些用于删除英语停用词的库、停用词列表以及代码。*

***自然语言工具包(NLTK):***

*NLTK 是一个非常棒的自然语言库。当您开始 NLP 之旅时，这是您将使用的第一个库。下面给出了导入库和英语停用词列表的步骤:*

```
***import** nltk
**from** nltk.corpus **import** stopwords
sw_nltk = stopwords.words('english')
**print**(sw_nltk)*
```

*输出:*

```
*['i', 'me', 'my', 'myself', 'we', 'our', 'ours', 'ourselves', 'you', "you're", "you've", "you'll", "you'd", 'your', 'yours', 'yourself', 'yourselves', 'he', 'him', 'his', 'himself', 'she', "she's", 'her', 'hers', 'herself', 'it', "it's", 'its', 'itself', 'they', 'them', 'their', 'theirs', 'themselves', 'what', 'which', 'who', 'whom', 'this', 'that', "that'll", 'these', 'those', 'am', 'is', 'are', 'was', 'were', 'be', 'been', 'being', 'have', 'has', 'had', 'having', 'do', 'does', 'did', 'doing', 'a', 'an', 'the', 'and', 'but', 'if', 'or', 'because', 'as', 'until', 'while', 'of', 'at', 'by', 'for', 'with', 'about', 'against', 'between', 'into', 'through', 'during', 'before', 'after', 'above', 'below', 'to', 'from', 'up', 'down', 'in', 'out', 'on', 'off', 'over', 'under', 'again', 'further', 'then', 'once', 'here', 'there', 'when', 'where', 'why', 'how', 'all', 'any', 'both', 'each', 'few', 'more', 'most', 'other', 'some', 'such', 'no', 'nor', 'not', 'only', 'own', 'same', 'so', 'than', 'too', 'very', 's', 't', 'can', 'will', 'just', 'don', "don't", 'should', "should've", 'now', 'd', 'll', 'm', 'o', 're', 've', 'y', 'ain', 'aren', "aren't", 'couldn', "couldn't", 'didn', "didn't", 'doesn', "doesn't", 'hadn', "hadn't", 'hasn', "hasn't", 'haven', "haven't", 'isn', "isn't", 'ma', 'mightn', "mightn't", 'mustn', "mustn't", 'needn', "needn't", 'shan', "shan't", 'shouldn', "shouldn't", 'wasn', "wasn't", 'weren', "weren't", 'won', "won't", 'wouldn', "wouldn't"]*
```

*让我们检查一下这个库有多少停用词。*

```
***print**(**len**(sw_nltk))*
```

*输出:*

```
*179*
```

*让我们把课文中的停用词去掉。*

```
*text = "When I first met her she was very quiet. She remained quiet during the entire two hour long journey from Stony Brook to New York."words = [word **for** word **in** text.**split()** **if** word.**lower()** **not in** sw_nltk]
new_text = " ".**join**(words)**print**(new_text)
**print**("Old length: ", **len**(text))
**print**("New length: ", **len**(new_text))*
```

*上面的代码很简单，但我仍然会为初学者解释它。我有文本，我把文本分成单词，因为停用词是一个单词列表。然后我将单词改为小写，因为停用词列表中的所有单词都是小写的。然后我创建了一个不在停用词列表中的所有词的列表。然后将结果列表连接起来，再次形成句子。*

*输出:*

```
*first met quiet. remained quiet entire two hour long journey Stony Brook New York.
Old length:  129
New length:  82*
```

*我们可以清楚地看到，停用词的删除将句子的长度从 129 减少到 82。*

*请注意，我将在每个库中使用类似的代码来解释停用词。*

***空间:***

*spaCy 是一个用于高级 NLP 的开源软件库。这个库现在非常流行，NLP 从业者用它来以最好的方式完成他们的工作。*

```
***import** spacy
#loading the english language small model of spacy
en = **spacy.load**('en_core_web_sm')
sw_spacy = en.**Defaults.stop_words**
**print**(sw_spacy)*
```

*输出:*

```
*{'those', 'on', 'own', '’ve', 'yourselves', 'around', 'between', 'four', 'been', 'alone', 'off', 'am', 'then', 'other', 'can', 'regarding', 'hereafter', 'front', 'too', 'used', 'wherein', '‘ll', 'doing', 'everything', 'up', 'onto', 'never', 'either', 'how', 'before', 'anyway', 'since', 'through', 'amount', 'now', 'he', 'was', 'have', 'into', 'because', 'not', 'therefore', 'they', 'n’t', 'even', 'whom', 'it', 'see', 'somewhere', 'thereupon', 'nothing', 'whereas', 'much', 'whenever', 'seem', 'until', 'whereby', 'at', 'also', 'some', 'last', 'than', 'get', 'already', 'our', 'once', 'will', 'noone', "'m", 'that', 'what', 'thus', 'no', 'myself', 'out', 'next', 'whatever', 'although', 'though', 'which', 'would', 'therein', 'nor', 'somehow', 'whereupon', 'besides', 'whoever', 'ourselves', 'few', 'did', 'without', 'third', 'anything', 'twelve', 'against', 'while', 'twenty', 'if', 'however', 'herself', 'when', 'may', 'ours', 'six', 'done', 'seems', 'else', 'call', 'perhaps', 'had', 'nevertheless', 'where', 'otherwise', 'still', 'within', 'its', 'for', 'together', 'elsewhere', 'throughout', 'of', 'others', 'show', '’s', 'anywhere', 'anyhow', 'as', 'are', 'the', 'hence', 'something', 'hereby', 'nowhere', 'latterly', 'say', 'does', 'neither', 'his', 'go', 'forty', 'put', 'their', 'by', 'namely', 'could', 'five', 'unless', 'itself', 'is', 'nine', 'whereafter', 'down', 'bottom', 'thereby', 'such', 'both', 'she', 'become', 'whole', 'who', 'yourself', 'every', 'thru', 'except', 'very', 'several', 'among', 'being', 'be', 'mine', 'further', 'n‘t', 'here', 'during', 'why', 'with', 'just', "'s", 'becomes', '’ll', 'about', 'a', 'using', 'seeming', "'d", "'ll", "'re", 'due', 'wherever', 'beforehand', 'fifty', 'becoming', 'might', 'amongst', 'my', 'empty', 'thence', 'thereafter', 'almost', 'least', 'someone', 'often', 'from', 'keep', 'him', 'or', '‘m', 'top', 'her', 'nobody', 'sometime', 'across', '‘s', '’re', 'hundred', 'only', 'via', 'name', 'eight', 'three', 'back', 'to', 'all', 'became', 'move', 'me', 'we', 'formerly', 'so', 'i', 'whence', 'under', 'always', 'himself', 'in', 'herein', 'more', 'after', 'themselves', 'you', 'above', 'sixty', 'them', 'your', 'made', 'indeed', 'most', 'everywhere', 'fifteen', 'but', 'must', 'along', 'beside', 'hers', 'side', 'former', 'anyone', 'full', 'has', 'yours', 'whose', 'behind', 'please', 'ten', 'seemed', 'sometimes', 'should', 'over', 'take', 'each', 'same', 'rather', 'really', 'latter', 'and', 'ca', 'hereupon', 'part', 'per', 'eleven', 'ever', '‘re', 'enough', "n't", 'again', '‘d', 'us', 'yet', 'moreover', 'mostly', 'one', 'meanwhile', 'whither', 'there', 'toward', '’m', "'ve", '’d', 'give', 'do', 'an', 'quite', 'these', 'everyone', 'towards', 'this', 'cannot', 'afterwards', 'beyond', 'make', 'were', 'whether', 'well', 'another', 'below', 'first', 'upon', 'any', 'none', 'many', 'serious', 'various', 're', 'two', 'less', '‘ve'}*
```

*相当长的名单。让我们检查一下这个库有多少停用词。*

```
***print**(**len**(sw_spacy))*
```

*输出:*

```
*326*
```

*哇，326！让我们把前面课文中的停用词去掉。*

```
*words = [word **for** word **in** text.**split()** **if** word.**lower()** **not in** sw_spacy]
new_text = " ".**join**(words)
**print**(new_text)
**print**("Old length: ", **len**(text))
**print**("New length: ", **len**(new_text))*
```

*输出:*

```
*met quiet. remained quiet entire hour lomg journey Stony Brook New York.
Old length:  129
New length:  72*
```

*我们可以清楚地看到，停用词的删除将句子的长度从 129 减少到 72，甚至比 NLTK 还短，因为 spaCy 库的停用词比 NLTK 多。在这种情况下，结果是非常相似的。*

***Gensim:***

*Gensim (Generate Similar)是一个使用现代统计机器学习的开源软件库。*根据维基百科的说法，Gensim 旨在使用数据流和增量在线算法来处理大型文本集合，这与大多数其他只针对内存处理的机器学习软件包有所不同。**

```
***import** gensim
**from** gensim.parsing.preprocessing **import** remove_stopwords, STOPWORDS
**print**(STOPWORDS)*
```

*输出:*

```
*frozenset({'those', 'on', 'own', 'yourselves', 'ie', 'around', 'between', 'four', 'been', 'alone', 'off', 'am', 'then', 'other', 'can', 'cry', 'regarding', 'hereafter', 'front', 'too', 'used', 'wherein', 'doing', 'everything', 'up', 'never', 'onto', 'how', 'either', 'before', 'anyway', 'since', 'through', 'amount', 'now', 'he', 'cant', 'was', 'con', 'have', 'into', 'because', 'inc', 'not', 'therefore', 'they', 'even', 'whom', 'it', 'see', 'somewhere', 'interest', 'thereupon', 'thick', 'nothing', 'whereas', 'much', 'whenever', 'find', 'seem', 'until', 'whereby', 'at', 'ltd', 'fire', 'also', 'some', 'last', 'than', 'get', 'already', 'our', 'doesn', 'once', 'will', 'noone', 'that', 'what', 'thus', 'no', 'myself', 'out', 'next', 'whatever', 'although', 'though', 'etc', 'which', 'would', 'therein', 'nor', 'somehow', 'whereupon', 'besides', 'whoever', 'thin', 'ourselves', 'few', 'did', 'third', 'without', 'twelve', 'anything', 'against', 'while', 'twenty', 'if', 'however', 'found', 'herself', 'when', 'may', 'six', 'ours', 'done', 'seems', 'else', 'call', 'perhaps', 'had', 'nevertheless', 'fill', 'where', 'otherwise', 'still', 'within', 'its', 'for', 'together', 'elsewhere', 'throughout', 'of', 'eg', 'others', 'show', 'sincere', 'anywhere', 'anyhow', 'as', 'are', 'the', 'hence', 'something', 'hereby', 'nowhere', 'latterly', 'de', 'say', 'does', 'neither', 'his', 'go', 'forty', 'put', 'their', 'by', 'namely', 'km', 'could', 'five', 'unless', 'itself', 'is', 'nine', 'whereafter', 'down', 'bottom', 'thereby', 'such', 'both', 'she', 'become', 'whole', 'who', 'yourself', 'every', 'thru', 'except', 'very', 'several', 'among', 'being', 'be', 'mine', 'further', 'here', 'during', 'why', 'with', 'just', 'becomes', 'about', 'a', 'co', 'using', 'seeming', 'due', 'wherever', 'beforehand', 'detail', 'fifty', 'becoming', 'might', 'amongst', 'my', 'empty', 'thence', 'thereafter', 'almost', 'least', 'someone', 'often', 'from', 'keep', 'him', 'or', 'top', 'her', 'didn', 'nobody', 'sometime', 'across', 'hundred', 'only', 'via', 'name', 'eight', 'three', 'back', 'to', 'all', 'became', 'move', 'me', 'we', 'formerly', 'so', 'i', 'whence', 'describe', 'under', 'always', 'himself', 'more', 'herein', 'in', 'after', 'themselves', 'you', 'them', 'above', 'sixty', 'hasnt', 'your', 'made', 'everywhere', 'indeed', 'most', 'kg', 'fifteen', 'but', 'must', 'along', 'beside', 'hers', 'computer', 'side', 'former', 'full', 'anyone', 'has', 'yours', 'whose', 'behind', 'please', 'mill', 'amoungst', 'ten', 'seemed', 'sometimes', 'should', 'over', 'take', 'each', 'don', 'same', 'rather', 'really', 'latter', 'and', 'part', 'hereupon', 'per', 'eleven', 'ever', 'enough', 'again', 'us', 'yet', 'moreover', 'mostly', 'one', 'meanwhile', 'whither', 'there', 'toward', 'give', 'system', 'do', 'quite', 'an', 'these', 'everyone', 'towards', 'this', 'bill', 'cannot', 'un', 'afterwards', 'beyond', 'make', 'were', 'whether', 'well', 'another', 'below', 'first', 'upon', 'any', 'none', 'many', 'various', 'serious', 're', 'two', 'less', 'couldnt'})*
```

*又是一个很长的列表。让我们检查一下这个库有多少停用词。*

```
***print**(**len**(STOPWORDS))*
```

*输出:*

```
*337*
```

*嗯！类似于 spaCy。让我们从课文中去掉无用的词。*

```
*new_text = **remove_stopwords**(text)
**print**(new_text)**print**("Old length: ", **len**(text))
**print**("New length: ", **len**(new_text))*
```

*我们可以看到，使用 Gensim 库删除停用词非常简单。*

*输出:*

```
*When I met quiet. She remained quiet entire hour long journey Stony Brook New York.
Old length:  129
New length:  83*
```

*停用词的删除将句子长度从 129 个减少到 83 个。我们可以看到，即使 spaCy 和 Gensim 中的停用词长度相似，但得到的文本却大不相同。*

***Scikit-Learn:***

*Scikit-Learn 无需介绍。这是一个免费的 Python 软件机器学习库。它可能是机器学习最强大的库。*

```
***from** sklearn.feature_extraction.text **import** ENGLISH_STOP_WORDS
**print**(ENGLISH_STOP_WORDS)*
```

*输出:*

```
*frozenset({'those', 'on', 'own', 'yourselves', 'ie', 'around', 'between', 'four', 'been', 'alone', 'off', 'am', 'then', 'other', 'can', 'cry', 'hereafter', 'front', 'too', 'wherein', 'everything', 'up', 'onto', 'never', 'either', 'how', 'before', 'anyway', 'since', 'through', 'amount', 'now', 'he', 'cant', 'was', 'con', 'have', 'into', 'because', 'inc', 'not', 'therefore', 'they', 'even', 'whom', 'it', 'see', 'somewhere', 'interest', 'thereupon', 'nothing', 'thick', 'whereas', 'much', 'whenever', 'find', 'seem', 'until', 'whereby', 'at', 'ltd', 'fire', 'also', 'some', 'last', 'than', 'get', 'already', 'our', 'once', 'will', 'noone', 'that', 'what', 'thus', 'no', 'myself', 'out', 'next', 'whatever', 'although', 'though', 'etc', 'which', 'would', 'therein', 'nor', 'somehow', 'whereupon', 'besides', 'whoever', 'thin', 'ourselves', 'few', 'third', 'without', 'anything', 'twelve', 'against', 'while', 'twenty', 'if', 'however', 'found', 'herself', 'when', 'may', 'ours', 'six', 'done', 'seems', 'else', 'call', 'perhaps', 'had', 'nevertheless', 'fill', 'where', 'otherwise', 'still', 'within', 'its', 'for', 'together', 'elsewhere', 'throughout', 'of', 'eg', 'others', 'show', 'sincere', 'anywhere', 'anyhow', 'as', 'are', 'the', 'hence', 'something', 'hereby', 'nowhere', 'de', 'latterly', 'neither', 'his', 'go', 'forty', 'put', 'their', 'by', 'namely', 'could', 'five', 'itself', 'is', 'nine', 'whereafter', 'down', 'bottom', 'thereby', 'such', 'both', 'she', 'become', 'whole', 'who', 'yourself', 'every', 'thru', 'except', 'very', 'several', 'among', 'being', 'be', 'mine', 'further', 'here', 'during', 'why', 'with', 'becomes', 'about', 'a', 'co', 'seeming', 'due', 'wherever', 'beforehand', 'detail', 'fifty', 'becoming', 'might', 'amongst', 'my', 'empty', 'thence', 'thereafter', 'almost', 'least', 'someone', 'often', 'from', 'keep', 'him', 'or', 'top', 'her', 'nobody', 'sometime', 'across', 'hundred', 'only', 'via', 'name', 'eight', 'three', 'back', 'to', 'all', 'became', 'move', 'me', 'we', 'formerly', 'so', 'i', 'whence', 'describe', 'under', 'always', 'himself', 'in', 'herein', 'more', 'after', 'themselves', 'you', 'above', 'sixty', 'them', 'hasnt', 'your', 'made', 'indeed', 'most', 'everywhere', 'fifteen', 'but', 'must', 'along', 'beside', 'hers', 'side', 'former', 'anyone', 'full', 'has', 'yours', 'whose', 'behind', 'please', 'amoungst', 'mill', 'ten', 'seemed', 'sometimes', 'should', 'over', 'take', 'each', 'same', 'rather', 'latter', 'and', 'hereupon', 'part', 'per', 'eleven', 'ever', 'enough', 'again', 'us', 'yet', 'moreover', 'mostly', 'one', 'meanwhile', 'whither', 'there', 'toward', 'give', 'system', 'do', 'an', 'these', 'everyone', 'towards', 'this', 'bill', 'cannot', 'un', 'afterwards', 'beyond', 'were', 'whether', 'well', 'another', 'below', 'first', 'upon', 'any', 'none', 'many', 'serious', 're', 'two', 'couldnt', 'less'})*
```

*又是一个很长的列表。让我们检查一下这个库有多少停用词。*

```
***print**(**len**(ENGLISH_STOP_WORDS))*
```

*输出:*

```
*318*
```

*让我们从课文中去掉无用的词。*

```
*words = [word **for** word **in** text.**split()** **if** word.**lower()** **not in** ENGLISH_STOP_WORDS]
new_text = " ".**join**(words)
**print**(new_text)
**print**("Old length: ", **len**(text))
**print**("New length: ", **len**(new_text))*
```

*输出:*

```
*met quiet. remained quiet entire hour long journey Stony Brook New York.
Old length:  129
New length:  72*
```

*停用词的删除将句子长度从 129 个减少到 72 个。我们可以看到 Scikit-learn 和 spaCy 产生了相同的结果。*

## *我可以在列表中添加我自己的停用词吗？✍️*

*是的，我们也可以将自定义的停用词添加到这些库中可用的停用词列表中，以达到我们的目的。*

*下面是向 NLTK 的停用词列表添加一些自定义停用词的代码:*

```
*sw_nltk.**extend**(['first', 'second', 'third', 'me'])
**print**(**len**(sw_nltk))*
```

*输出:*

```
*183*
```

*我们可以看到 NLTK 停止字的长度现在是 183 而不是 179。而且，我们现在可以使用相同的代码从文本中删除停用词。*

## *我可以从预制列表中删除停用词吗？👋*

*是的，如果我们愿意，我们也可以从这些库中的可用列表中删除停用词。*

*下面是使用 NLTK 库的代码:*

```
*sw_nltk.**remove**('not')*
```

*停用词“not”现在已从停用词列表中删除。*

*根据您使用的库，您可以执行相关操作，在预先制作的列表中添加或删除停用词。我指出这一点是因为 NLTK 返回一个停用词列表，而其他库返回一组停用词。*

*如果我们不想使用这些库，我们也可以创建我们自己的自定义停用词列表，并在我们的任务中使用它。当我们在自己的领域有专业知识，并且知道在执行任务时应该避免哪些词时，通常会这样做。*

*看看下面的代码，看看这有多简单。*

```
*#create your custom stop words list
my_stop_words = ['her','me','i','she','it']words = [word **for** word **in** text**.split()** **if** word**.lower()** **not in** my_stop_words]
new_text = " ".**join**(words)
**print**(new_text)
**print**("Old length: ", **len**(text))
**print**("New length: ", **len**(new_text))*
```

*输出:*

```
*When first met was very quiet. remained quiet during the entire two hour long journey from Stony Brook to New York.
Old length:  129
New length:  115*
```

*以类似的方式，你可以根据你的任务创建你的停用词列表并使用它。🤟*

*我们在这篇文章中观察到，不同的库有不同的停用词集合，我们可以清楚地说停用词是任何语言中使用最频繁的词。*

*虽然您可以使用这些库中的任何一个来删除文本中的停用词，但是强烈建议您对整个文本预处理任务使用同一个库。*

*谢谢大家阅读这篇文章。请分享您对这篇文章的宝贵反馈或建议！快乐阅读！📗 🖌*

*[LinkedIn](https://www.linkedin.com/in/chetna-khanna/)*