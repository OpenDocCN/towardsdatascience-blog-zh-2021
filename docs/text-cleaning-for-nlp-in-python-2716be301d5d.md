# 如何在 Python 中像 Boss 一样为 NLP 清理文本

> 原文：<https://towardsdatascience.com/text-cleaning-for-nlp-in-python-2716be301d5d?source=collection_archive---------19----------------------->

## 自然语言处理的关键步骤变得简单！

![](img/17bf9ff855321b5ee796e4d76fd8b5a8.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=medium&utm_medium=referral) 拍摄

# 清理文本

自然语言处理(NLP)中最常见的任务之一是清理文本数据。为了最大化你的结果，重要的是从你的文本中提取出语料库中最重要的*词根*，并清除掉*不需要的噪音*。这篇文章将展示我是如何做到这一点的。以下是文本预处理的一般步骤:

*   **标记化**:标记化将文本分解成更小的单元，而不是大块的文本。我们将这些单元理解为*单词*或*句子*，但是机器只有将它们分开才能理解。分解术语时必须特别小心，以便创建逻辑单元。大多数软件包处理边缘情况(美国闯入美国，而不是美国和美国)，但确保它正确完成总是至关重要的。
*   **清洗**:清洗过程对于去除对分析不重要的文本和字符至关重要。诸如 URL 之类的文本、诸如连字符或特殊字符之类的非关键项目、网页抓取、HTML 和 CSS 信息都将被丢弃。
*   **删除停用词**:接下来是删除停用词的过程。停用词是出现但不增加任何理解的常用词。像“a”和“the”这样的词就是例子。这些词也非常频繁地出现，在你的分析中成为主导，模糊了有意义的词。：
*   **拼写**:拼写错误也可以在分析过程中纠正。根据通信媒介的不同，可能会有更多或更少的错误。官方的企业或教育文档很可能包含较少的错误，而社交媒体帖子或电子邮件等更非正式的交流可能会有更多的错误。根据期望的结果，纠正或不纠正拼写错误是关键的一步。
*   **词干化和词汇化**:词干化是从一个单词的开头或结尾移除字符，以将其缩减为词干的过程。词干化的一个例子是将“runs”简化为“run”，因为基本单词去掉了“s”，而“ran”不会在同一个词干中。然而，词汇化会将“然”归入同一词汇中。

以下是我用来清理大部分文本数据的脚本。

# 进口

```
import pandas as pd
import re
import string
from bs4 import BeautifulSoup
import nltk
from nltk.stem import PorterStemmer
from nltk.stem.wordnet import WordNetLemmatizer
import spacy
```

# 清理 HTML

移除 HTML 是可选的，这取决于您的数据源是什么。我发现美丽的汤是最好的清洗方法。

```
def clean_html(html):

    # parse html content
    soup = BeautifulSoup(html, "html.parser")

    for data in soup(['style', 'script', 'code', 'a']):
        # Remove tags
        data.decompose()

    # return data by retrieving the tag content
    return ' '.join(soup.stripped_strings)
```

**注意:**在`for`循环中，你可以指定你想要清理的不同 HTML 标签。例如，上面的步骤包括`style`、`script`、`code`和`a`标签。试验并扩充这个列表，直到你得到你想要的结果。

# 清洁其余部分

现在是老黄牛。

1.  将文本**变成小写**。您可能知道，python 是区分大小写的，其中`A != a`。
2.  移除**断线**。同样，根据您的源代码，您可能已经编码了换行符。
3.  移除**标点符号**。这是使用字符串库。其他标点符号可以根据需要添加。
4.  使用`NLTK`库删除**停止字**。下一行有一个列表，可以根据需要向函数中添加额外的停用词。这些可能是嘈杂的领域词或任何使上下文清晰的东西。
5.  移除**数字**。根据您的数据选择。
6.  **词干化**或**词干化**。这个过程是函数中的一个自变量。您可以使用`Stem`或`Lem`选择一个过孔。默认情况下使用 none。

```
# Load spacy
nlp = spacy.load('en_core_web_sm')

def clean_string(text, stem="None"):

    final_string = ""

    # Make lower
    text = text.lower()

    # Remove line breaks
    # Note: that this line can be augmented and used over
    # to replace any characters with nothing or a space
    text = re.sub(r'\n', '', text)

    # Remove punctuation
    translator = str.maketrans('', '', string.punctuation)
    text = text.translate(translator)

    # Remove stop words
    text = text.split()
    useless_words = nltk.corpus.stopwords.words("english")
    useless_words = useless_words + ['hi', 'im']

    text_filtered = [word for word in text if not word in useless_words]

    # Remove numbers
    text_filtered = [re.sub(r'\w*\d\w*', '', w) for w in text_filtered]

    # Stem or Lemmatize
    if stem == 'Stem':
        stemmer = PorterStemmer() 
        text_stemmed = [stemmer.stem(y) for y in text_filtered]
    elif stem == 'Lem':
        lem = WordNetLemmatizer()
        text_stemmed = [lem.lemmatize(y) for y in text_filtered]
    elif stem == 'Spacy':
        text_filtered = nlp(' '.join(text_filtered))
        text_stemmed = [y.lemma_ for y in text_filtered]
    else:
        text_stemmed = text_filtered

    final_string = ' '.join(text_stemmed)

    return final_string
```

# 例子

要将此应用于标准数据框，请使用 Pandas 的`apply`函数，如下所示。让我们来看看起始文本:

```
<p>
  <a
    href="https://forge.autodesk.com/en/docs/data/v2/tutorials/download-file/#step-6-download-the-item"
    rel="nofollow noreferrer"
    >https://forge.autodesk.com/en/docs/data/v2/tutorials/download-file/#step-6-download-the-item</a
  >
</p>
\n\n
<p>
  I have followed the tutorial and have successfully obtained the contents of
  the file, but where is the file being downloaded. In addition, how do I
  specify the location of where I want to download the file?
</p>
\n\n
<p>
  Result on Postman\n<a
    href="https://i.stack.imgur.com/VrdqP.png"
    rel="nofollow noreferrer"
    ><img
      src="https://i.stack.imgur.com/VrdqP.png"
      alt="enter image description here"
  /></a>
</p>
```

让我们从清理 HTML 开始。

```
# To remove HTML first and apply it directly to the source text column.
df['body'] = df['body'].apply(lambda x: clean_html(x))
```

将该函数应用于清理 HTML 后，结果如下——非常令人印象深刻:

```
I have followed the tutorial and have successfully obtained the contents 
of the file, but where is the file being downloaded. In addition, how 
do I specify the location of where I want to download the file? Result 
on Postman
```

接下来，让我们应用`clean_string`函数。

```
# Next apply the clean_string function to the text
df['body_clean'] = df['body'].apply(lambda x: clean_string(x, stem='Stem'))
```

最后得到的文本是:

```
follow tutori success obtain content file file download addit 
specifi locat want download file result postman
```

完全干净，随时可以在您的 NLP 项目中使用。您可能会注意到，去掉停用词后，单词的长度大大缩短了，而且单词的词干也变成了它们的词根形式。

**注意:**我经常创建一个新的专栏，就像上面的`body_clean`，所以我保留了原来的，以防需要标点符号。

大概就是这样。上述函数中的顺序很重要。您应该在其他步骤之前完成某些步骤，例如先制作小写字母。该函数包含一个删除数字的正则表达式示例；一个可靠的实用函数，您可以调整它来使用 RegEx 删除文本中的其他项目。

# 空间与 NLKT 符号化

上面的函数包含了两种不同的方法来对你的文本进行词汇化。NLTK `WordNetLemmatizer`需要一个词性(POS)参数(`noun`，`verb`)，因此要么需要多次传递来获取每个单词，要么只捕获一个词性。另一种方法是使用`Spacy`，它将自动对每个单词进行词条分类，并确定它属于哪个词性。问题是 Spacy 的性能会比 NLTK 慢很多。

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。一个月 5 美元，让你可以无限制地访问成千上万篇文章。如果你用[我的链接](https://medium.com/@broepke/membership)注册，我会赚一小笔佣金，不需要你额外付费。