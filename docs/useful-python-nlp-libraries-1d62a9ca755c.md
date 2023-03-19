# 有用的 Python NLP 库

> 原文：<https://towardsdatascience.com/useful-python-nlp-libraries-1d62a9ca755c?source=collection_archive---------31----------------------->

## 你不知道的事

![](img/ce5a2e70a674c540c428c86160962422.png)

图片由[神奇创意](https://pixabay.com/users/creativemagic-480360/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1041796)来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1041796)

**简介**

最近在玩自然语言处理吗？如果您有，您可能已经使用 spacy 或 NLTK 库构建了一些基本的 NLP 算法并处理了文本数据。然而，还有其他 python 库可以帮助进行数据预处理或可视化。

在本文中，我将向您展示我在处理文本时经常使用的三个鲜为人知的库。它们大多是为完成特定任务而设计的小型库。

让我们开始吧。

**数字器**

我想向你介绍的第一个库是 numerizer。顾名思义，它将书面数字转换成相应的数字表示。

您可以使用以下代码安装它:

```
pip install numerizer
```

这个图书馆很容易使用。你只需要在字符串编号上调用 numerize 函数。以下示例将“55”更改为其数字表示形式“55”。

```
>>> from numerizer import numerize
>>> numerize(‘fifty five’)
'55'
```

它也适用于分数和不太传统的分数表示。例如，当提到分数时，你可以使用像四分之一、一半等词，并且仍然应该正确转换。

```
>>> numerize(‘fifty and a quarter’)
'50.25'
```

这个库非常有用，我在处理转录文本时经常使用它。

**表情**

这是另一个有用的库，当你处理文本时，尤其是当人们使用大量表情符号和表情符号时，来自社交媒体的文本。

该库获取表情符号和表情符号，并返回它们的文本表示。就这么简单。

您可以使用以下代码安装 emot:

```
pip install emot
```

让我们看一个关于如何在一些包含表情符号的字符串上使用 emot 库的实际例子。我们将通过它'我爱蟒蛇☮🙂 ❤'.

```
>>> import emot 
>>> emot_obj = emot.emot() 
>>> text = “I love python ☮ 🙂 ❤” 
>>> emot_obj.emoji(text){'value': ['☮', '🙂', '❤'],
 'location': [[14, 15], [16, 17], [18, 19]],
 'mean': [':peace_symbol:', ':slightly_smiling_face:', ':red_heart:'],
 'flag': True}
```

正如你所看到的，在导入库之后，我们必须创建一个 emot_object。一旦我们这样做了，我们可以简单地调用表情符号文本上的 emoji()函数。

结果是字典具有关于包含在字符串中的表情符号和表情符号的详细信息，例如它们的文本表示和位置。

还有一个很有用的函数叫做 bulk_emoji()，可以让你从字符串列表中提取表情符号。这可能是很多 NLP 数据的样子，所以不需要手动遍历列表。只需调用 bulk_emoji():

```
>>> import emot
>>> emot_obj = emot.emot()
>>> bulk_text = ["I love python ☮ 🙂 ❤", "This is so funny 😂",]
>>> emot_obj.bulk_emoji(bulk_text, multiprocessing_pool_capacity=2)[{'value': ['☮', '🙂', '❤'],
  'location': [[14, 15], [16, 17], [18, 19]],
  'mean': [':peace_symbol:', ':slightly_smiling_face:', ':red_heart:'],
  'flag': True},
 {'value': ['😂'],
  'location': [[17, 18]],
  'mean': [':face_with_tears_of_joy:'],
  'flag': True}]
```

结果，您将得到一个字典列表，如上例所示。

**WordCloud**

另一个非常有用的库是 WordCloud。它帮助你在给定的文本中可视化词频。

你听说过“一图胜千言”这句话吗？

我认为这是这个图书馆成就的精髓。通过查看给定文本的单词云，你可以不用阅读就能猜出该文本的内容。

那么它是如何工作的呢？

您可以使用以下代码安装它:

```
pip3 install WordCloud
```

一旦你下载了这个库，使用起来就非常简单了。只需在 WorldCloud 对象上调用 generate()函数，并将一些文本作为参数传递给它。

让我们看看我从 [CNN 健康](https://edition.cnn.com/2021/08/08/health/how-to-teach-kids-cooking-wellness/index.html)中摘录的以下文字:

## “作为两个小女孩的母亲，我发现教孩子如何做饭——尽管这有挑战性——也是我作为父母最有收获和最愉快的经历之一。我的女儿们从蹒跚学步开始就在厨房里看着我——基本上是从她们能吃鳄梨泥开始。

## 这是我的第一点建议:早点开始。让你的孩子观察你做饭，让他们在小的时候参与简单的食物准备，这不仅仅是帮助他们在厨房里变得舒适。这也增加了他们享受健康食品的几率。"

您可以使用下面的代码为上面的文本创建一个单词云。

```
from wordcloud import WordCloudtext = “As a mom of two young girls, I’ve found that teaching kids how to prepare meals — while it comes with its challenges — has also been one of my most rewarding and enjoyable experiences as a parent. My daughters have watched me in the kitchen since they were toddlers — basically since they could eat mashed avocado. That’s my first bit of advice: start early. Allowing your children to observe you cooking and involving them in simple food prep at a young age will do much more than help them become comfortable in the kitchen. It also increases the odds they will enjoy eating healthy foods.”wordcloud = WordCloud().generate(text)
```

一旦你生成了云数据，显示它是非常容易的。你只需要从 matplotlib 导入 plt 模块，设置一些显示参数。

```
import matplotlib.pyplot as pltplt.imshow(wordcloud, interpolation=’bilinear’)
plt.axis(“off”)
plt.show()
```

![](img/75bed166f600cc44566134d020861d3a.png)

瞧啊。你可以立即看到这篇课文是关于:小孩、妈妈和厨房。

你可以玩这个库，通过改变云的大小和颜色以及指定文本中使用的停用词来根据你的需要设置它。

**总结**

在本文中，我分享了三个 NLP 库，在处理文本数据时，我用它们来进行一些文本处理和可视化。

我希望您喜欢这篇文章，并且有机会在一些真实的数据集上使用本教程中介绍的库。

*最初发布于 aboutdatablog.com:* [你不知道的有用的 Python NLP 库…](https://www.aboutdatablog.com/post/useful-python-nlp-libraries-that-you-did-not-know-about)**2021 年 8 月 26 日。**

**PS:我正在 Medium 和*<https://www.aboutdatablog.com/>**上撰写深入浅出地解释基本数据科学概念的文章。你可以订阅我的* [***邮件列表***](https://medium.com/subscribe/@konkiewicz.m) *每次我写新文章都会收到通知。如果你还不是中等会员，你可以在这里加入**[***。***](https://medium.com/@konkiewicz.m/membership)***

**下面是一些你可能会喜欢的帖子**

**</jupyter-notebook-autocompletion-f291008c66c>  </top-8-magic-commands-in-jupyter-notebook-c1582e813560>  </9-things-you-did-not-know-about-jupyter-notebook-d0d995a8efb3>  </top-9-jupyter-notebook-extensions-7a5d30269bc8> **