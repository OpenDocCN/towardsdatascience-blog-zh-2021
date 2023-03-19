# 通过构建 NLP 管道清理和预处理文本数据

> 原文：<https://towardsdatascience.com/cleaning-preprocessing-text-data-by-building-nlp-pipeline-853148add68a?source=collection_archive---------6----------------------->

## 用 python 处理文本数据的完整演练

![](img/98e835adac456ab74c518b877dbb07b3.png)

艾莉娜·格鲁布尼亚克在 [Unsplash](https://unsplash.com/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

有一段时间，我在处理文本数据，我意识到在当今世界，有必要知道自然语言处理是如何工作的，以及为了从文本数据中获得洞察力需要涉及的主要步骤。
众所周知，为了从数字数据中获得洞察力，我们可以应用许多统计学和数学。
但是说到文本数据的繁琐形式，我们在很多地方都缺乏。

**什么是 NLP 文本预处理？**

NLP 文本预处理是一种清理文本的方法，目的是使文本可以提供给模型。文本中的噪音有多种形式，如表情符号、标点符号、不同的大小写。所有这些噪音对机器都没有用，因此需要清理。

**为什么 NLP 文本预处理很重要？**

**重要性:**

经过适当清理的数据将有助于我们进行良好的文本分析，并帮助我们对业务问题做出准确的决策。因此，机器学习的文本预处理是一个重要的步骤。

在我让你们了解 NLP 文本预处理的主要步骤之前，我想说的是，你们可以根据自己的数据和需求增加或删除一些步骤。

在本教程中，我几乎不会给你任何步骤的定义，因为互联网上有很多。相反，我将解释为什么您应该应用特定的步骤，如何使用 python 进行文本预处理，我在每个步骤中理解了什么，以及我的数据集的结果是什么。

好吧！！说够了。现在让我们深入研究代码。😄

**库&所需的文本预处理工具:**

```
*# Importing Libraries* 
**import** **unidecode** 
**import** **pandas** **as** **pd** 
**import** **re** **import** **time** 
**import** **nltk** **from** **nltk.corpus** 
**import** stopwords 
nltk.download('stopwords') 
**from** **nltk.tokenize** **import** word_tokenize 
**from** **nltk.stem** **import** WordNetLemmatizer 
**from** **autocorrect** **import** Speller 
**from** **bs4** **import** BeautifulSoup 
**from** **nltk.corpus** **import** stopwords 
**from** **nltk** **import** word_tokenize **import** **string** 
```

**读取数据集:**

```
*# Read Dataset* Df = pd.read_csv('New Task.csv', encoding = 'latin-1')
*# Show Dataset*
Df.head()
```

以下是一些文本预处理步骤，您可以根据您拥有的数据集添加或删除这些步骤:

## **步骤 1:移除换行符&标签**

您可能会在文本数据集和选项卡中无缘无故地遇到许多新行。因此，当您抓取数据时，网站上结构化内容所需的换行符和制表符在您的数据集中是不需要的，并且还会被转换为无用的字符，如\n，\t。因此，我编写了一个函数来删除所有这些无意义的内容。

## **步骤 2:剥离 HTML 标签**

当您抓取数据时，如果您在抓取时没有处理它，您可能会在数据集的文本中看到 HTML 标记。因此，有必要在以后处理这些标签。在这个函数中，我删除了文本中所有匹配 HTML 标签的内容。

## **步骤 3:移除链接**

这一步将删除所有与任何类型的超链接相似的内容。我在这里添加了这个函数，因为我已经在数据集上处理了它。

## **第四步:删除空白**

如下所述，可以执行单行功能来移除额外的空白。在执行进一步的 NLP 任务之前，这一步至关重要。

**自然语言处理文本预处理的主要步骤是什么？**

下面列出的文本预处理步骤非常重要，我已经按顺序写了所有这些步骤。

## **第一步:删除重音字符**

这是将所有类似重音字符的字符转换成机器可理解语言的关键一步。以便可以容易地实施进一步的步骤。重音字符是指字符上方有音调符号的字符，如“、”或“”。

## **步骤 2:案例转换**

这是系列中真正重要的下一步，因为 case 是机器的两个不同的词。因此，您应该将文本的大小写转换为小写或大写才能继续。

## **步骤 3:减少重复字符和标点**

这一步很重要，因为可能会出现字符重复过多的情况，这是拼写检查器无法检测到的。因此，在应用拼写检查功能之前，需要预先处理这种情况。我在工作中还会遇到另一种情况，也可能会出现重复的标点符号。所以也需要遇到他们。

当我们非常兴奋时，我们确实会覆盖那些让读者不知所措的东西。

示例:- Cheeeeeerrrrrrss！！！！！！

**在上述正则表达式中使用某些符号的说明**

**\1** →相当于重新搜索(…)。组(1)。它指的是第一个捕获组。\1 匹配与第一个捕获组匹配的完全相同的文本。

**{1，}** →这意味着我们正在匹配出现不止一次的重复。

**DOTALL** - >它也匹配换行符，不像点运算符那样匹配给定文本中除换行符以外的所有内容。

**sub()** →此函数用于用另一个子字符串替换特定子字符串的出现。该函数将以下内容作为输入:要替换的子字符串。要替换的子字符串。

**r'\1\1'** →将所有重复限制在两个字以内。

**r'\1'** →将所有重复限制为仅一个字符。

**{2，}** →表示匹配出现两次以上的重复

## **第四步:扩张收缩**

为了在下一步中删除停用词，首先处理缩写是至关重要的。缩写只不过是诸如“不要”、“不会”、“它是”等词的速记形式。宫缩是任何类似这些例子不要，不会，它的。

## **步骤 5:删除特殊字符**

在这一步中，我们将学习如何删除特殊字符，为什么要删除它们，以及应该保留哪些特殊字符。

所以，我写了一个函数，它将删除一组指定的特殊字符，并保留一些重要的标点符号，如(，。？！)不包括括号。特殊字符应该被删除，因为当我们要对文本进行标记时，以后，标点符号不会以更大的权重出现。

**从文本中删除数字:**

您所需要做的就是修改给定的正则表达式

```
Formatted_Text = re.sub(r"[^a-zA-Z:$-,%.?!]+", ' ', text)
```

只需排除 0-9 范围，以便从文本中删除所有数字表示。我没有在我的数据集上执行这一特定步骤，因为这些数字对我的情况非常重要。

**根据我的数据集，我正在考虑的标点符号很重要，因为我稍后必须执行文本摘要。**

**，。？！** →这是一些经常出现的标点符号，需要保留下来，以便理解文本的上下文。

**:** →根据数据集，这个也很常见。保持是很重要的，因为每当出现像晚上 9:05 这样的时间时，它就有了意义

这个词也经常出现在许多文章中，更准确地讲述数据、事实和数字。

**$** →这个用在很多考虑价格的文章里。因此，省略这个符号对那些只剩下一些数字的价格没有太大意义。

## **步骤 6:移除停用词**

如果您正在执行标记化、文本摘要、文本分类或任何类似的任务，则应该删除停用词。如同没有停用字词一样，您可以理解呈现给您的文本数据的上下文。需要移除它们以减轻它们的重量。

你应该考虑这一步删除停用词，因为它不适合我的进一步分析。正如我在生成 n-gram 时发现的那样，有停用词的文件往往比没有停用词的文件给出更可靠的结果。

## **步骤 7:纠正拼写错误的单词**

尝试这一步时，你应该非常小心。因为这个功能可能会改变单词的真正含义。所以你必须非常小心，试着看看在应用这个函数时事情是如何发展的。如果您正在处理一些特定于行业的数据集，那么您可能需要考虑关联字典，该字典明确地告诉该函数保持那些特定的单词不变。

## **步骤 8:词汇化/词干化**

在这一步，我将准确地谈论两件事，词汇化和词干化。

人们对这两种技术感到困惑，比如哪一种更有效，该用什么。那么，让我告诉你我的经历，我喜欢什么，为什么？

因此，这两种技术实际上都将单词修整为其词根形式，就像 Planning 将被修整为 plan，但词干不会在单词“Better”的情况下工作，而如果您应用词汇化，它会将单词转换为其词根形式，即“good”。这是一个主要的区别，即 Lemmatization 工作效率高，我只在工作中使用过它。

虽然我已经写了这个函数，但是经过进一步的分析，我发现它的性能并不好，反而产生了噪音。因此，在对一些令牌进行位分析时，我意识到最好不要应用术语化。

例如:-有一个词“饲料”，这是频繁出现在所有的文章，这也是非常重要的。但是在词条化上，“饲料”→简化为→“饲料”。你可以看到它是如何改变了它的整个含义。

## **主要调查结果:**

数据清理步骤完全取决于数据集的类型。根据数据，可以包括更多的步骤。必须删除多余的空间以减小文件大小。

## **结论:**

这些是我在对文本数据进行预处理时一直考虑的一些重要步骤。如果您也必须处理文本数据，您可以利用共享的代码，或者不要忘记让我知道您是否尝试了其他方法来清理文本数据。这些步骤是特定于我所使用的数据集的，所以在方便的时候可以随意添加或删除它。

您还可以访问完整的 Github 资源库，其中包含一系列实现中的所有步骤，并附有简要说明。

[](https://github.com/techykajal/Data-Pre-processing/blob/main/Text_Preprocessing.ipynb) [## 数据预处理/Text _ preprocessing . ipynb at main techykajal/数据预处理

### 通过在 GitHub 上创建帐户，为 techykajal/数据预处理开发做出贡献。

github.com](https://github.com/techykajal/Data-Pre-processing/blob/main/Text_Preprocessing.ipynb) 

干杯！！你已经坚持到最后了。😉

所以，如果我的博客帖子对你有所帮助，而你此刻觉得很慷慨，请不要犹豫，请给我买杯咖啡。☕😍

[![](img/d0d4f4d264b3d53ab60b92ba81600f19.png)](https://www.buymeacoffee.com/techykajal)

是的，点击我。

```
And yes, buying me a coffee **(and lots of it if you are feeling extra generous)** goes a long way in ensuring that I keep producing content every day in the years to come.
```

您可以通过以下方式联系我:

1.  订阅我的 [**YouTube 频道**](https://www.youtube.com/channel/UCdwAaZMWiRmvIBIT96ApVjw) 视频内容即将上线 [**这里**](https://www.youtube.com/channel/UCdwAaZMWiRmvIBIT96ApVjw)
2.  跟我上 [**中**](https://medium.com/@TechyKajal)
3.  通过 [**LinkedIn**](http://www.linkedin.com/in/techykajal) 联系我
4.  跟随我的博客之旅:-[**https://kajalyadav.com/**](https://kajalyadav.com/)
5.  成为会员:[https://techykajal.medium.com/membershipT21](https://techykajal.medium.com/membership)

也可以看看我的其他博客:

[](/8-ml-ai-projects-to-make-your-portfolio-stand-out-bfc5be94e063) [## 8 ML/AI 项目，让您的投资组合脱颖而出

### 有趣的项目想法与源代码和参考文章，也附上一些研究论文。

towardsdatascience.com](/8-ml-ai-projects-to-make-your-portfolio-stand-out-bfc5be94e063) [](/scraping-1000s-of-news-articles-using-10-simple-steps-d57636a49755) [## 用 10 个简单的步骤搜集 1000 篇新闻文章

### 如果你遵循这 10 个简单的步骤，使用 python 进行网络抓取是非常简单的。

towardsdatascience.com](/scraping-1000s-of-news-articles-using-10-simple-steps-d57636a49755) [](/a-guide-to-scrape-tweet-replies-from-twitter-2f6168fed624) [## 从推特上收集回复的指南

### 使用 Octoparse 抓取 tweet 回复的初学者指南

towardsdatascience.com](/a-guide-to-scrape-tweet-replies-from-twitter-2f6168fed624) [](/15-free-open-source-data-resources-for-your-next-data-science-project-6480edee9bc1) [## 为您的下一个数据科学项目提供 15 种免费开源数据资源

### 为初学者和专业人士按不同类别组织的免费数据集的合并列表

towardsdatascience.com](/15-free-open-source-data-resources-for-your-next-data-science-project-6480edee9bc1)