# 正则表达式对 NLP 至关重要

> 原文：<https://towardsdatascience.com/regex-essential-for-nlp-ee0336ef988d?source=collection_archive---------9----------------------->

## 理解各种正则表达式，并将其应用于自然语言处理中经常遇到的情况

![](img/b56982946ad0714d6d1abdb0702b0538.png)

照片由[纳撒尼尔·舒曼](https://unsplash.com/@nshuman1291?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 为什么正则表达式对 NLP 至关重要？

每当我们处理文本数据时，它几乎总是不以我们想要的形式出现。文本可能包含我们想要删除的单词、不需要的标点符号、可以删除的超链接或 HTML 以及可以简化的日期或数字实体。在本文中，我将使用 re 模块描述 Python 中的一些基本正则表达式，以及在 NLP 中我最终使用它们的一些常见情况。**请注意，本文中的正则表达式是按照复杂度的递增顺序提到的。**

# 一些基本术语

在我们开始之前，让我们先弄清楚一些基本情况。我将只解释本文后面用到的术语，这样就不会太难理解了。

```
**\w  represents any alphanumeric characters (including underscore)****\d  represents any digit**.   **represents ANY character (do not confuse it with a period )****abc literally matches characters abc in a string****[abc] matches either a or b or c (characters within the brackets)****?   after a character indicates that the character is optional*****   after a character indicates it can be repeated 0 or more times****+   after a character indicates it can be repeated 1 or more times****\   is used to escape special characters (we'll use this alot!)**
```

# 转义特殊字符

从上一节你可以看到许多字符被 re 作为特殊字符使用，并且有它们自己的意思。比如**。？甚至/也是一些可以避免文字匹配的字符。在这种情况下，如果我们真的想要匹配这些字符，我们必须在它们前面加一个反斜杠(\)。**考虑 **\？**这将从字面上匹配字符串为 a？

# 让我们把手弄脏吧！

我将使用 python 中的 re 模块来替换和搜索字符串中的模式。简单地使用它`import re.` **下面的正则表达式和用例是按照复杂程度递增的顺序排列的，所以你可以随意跳来跳去。**

# 情况 1:删除出现在字符串开头或结尾的单词

说我们有一句话**友好的男孩有一只漂亮的狗，这只狗很友好**

现在，如果我们想删除第一个“the ”,我们可以简单地使用正则表达式 **^the.在使用 re.sub 时，第二个参数是被替换的单词，因为我们想完全删除它，所以我们用空引号替换我们的单词。**

同样，为了删除 **friendly** ，我们在最后使用正则表达式 **friendly$**

请注意，只有开头和结尾的单词被删除，而其他出现的单词保持不变。

# 情况 2:删除数字和货币

这个很简单。在 NLP 中，我们经常删除数值，因为模型并没有真正地从中学习，特别是当我们有很多不同的数值时。

在句子中，**我有 500 卢比。**我们可以使用表达式 **d+** 来匹配一个或多个数字。

现在我们来看一个微小的变化。假设我们有一个句子**我有 500 美元，你有 200 美元。注意美元符号的位置是如何不同的。为了处理这个问题，我们可以使用 **[\$\d+\d+\$]** ，它使用\来逐字匹配$并确保出现在美元符号之前或之后的数字都匹配。**

# 场景 3:处理各种日期格式

日期的问题在于你可以用不同的方式书写它们。2021 年 7 月 14 日、2021 年 7 月 14 日和 2021 年 7 月 14 日的含义相同。下面的表达式处理所有格式。

# 情况 4:删除超链接

当我们遇到完全想要删除的 URL 时，我们可以使用表达式`https?:\/\/.*[\r\n]*`来匹配以 **https://** 开头的 URL

# 情况 5:提取 URL 的主域名

在 NLP 中，我们可能需要分析 URL。**如果我们只对域名感兴趣，而对特定页面或查询参数的链接不感兴趣，那么我们需要使用一个表达式来使所有这样的链接统一。**

说我要从[**https://rajsangani.me/about**](https://rajsangani.me/about)**和**[**https://rajsangani.me/**](https://rajsangani.me/)**或者 www.rajsangani.me(这最后一个不存在)******

****在这种情况下，我可以使用下面的代码****

****注意我们在这里是如何使用 re.search 而不是 re.sub 的。我们要求代码在“**”之后搜索 URL**或一个' **/'** ，并在一个**'之前结束。'******

# ****场景 6:去掉一些标点符号****

****在自然语言处理中，我们必须根据手头的任务对文本进行预处理。在某些情况下，我们需要删除字符串中的所有标点符号，但是在情感分析这样的任务中，保留一些标点符号是很重要的，比如“！”表达了强烈情感。****

******如果我们想删除所有标点符号，我们可以简单地使用[^a-za-z0–9 ],它匹配除字母数字字符之外的所有内容。******

******但是说我们要把感叹号以外的东西都去掉。然后我们可以使用下面这段代码。******

```
****#Removing all punctuation except exclamation marks**re.sub(r'[\.;:,\?\"\'\/]','','''Hi, I am :" Raj Sangani ! Nice to meet you!!!!!!''')**#Result: Hi I am  Raj Sangani ! Nice to see you!!!!!!****
```

****根据需要，可以随意使用表达式中的标记。****

# ****情况 7:用一个感叹号代替两个或更多的感叹号****

****同样，在进行情感分析时，我们可能会遇到有不止一个标点符号组合在一起的句子。感叹号很常见，尤其是在 Twitter 这样的社交媒体平台上。在句子**嗨！我是拉吉·桑加尼！！很高兴见到你！！！！！！**有多余的感叹号。*如果我们想用一个感叹号代替两个或更多的感叹号，我们可以使用下面的表达式。* **{2，}表示 2 个以上。******

# ****情况 8:用一个空格替换两个或多个连续的空格****

****当把 pdf 抓取或转换成文本文件时，我们经常会遇到单词间有不止一个空格的字符串。更糟糕的是，在一些地方，单词被完全分开(有一个空格)，而在另一些地方，有 2 个甚至 3 个空格。为了使间距一致，我们可以使用下面的代码。****

******再次注意引号内{2，}前的空格，这是与空格匹配的部分。******

# ****情况 9:组合用连字符或空格分隔的单词****

****这是我最近在为一个项目做预处理时遇到的一个非常有趣的情况。像 cheesecake **这样的单词有时被写成两个间隔的单词 cheesecake，或者出现在连字符形式 cheese-cake 中。如果我们希望两个事件都被芝士蛋糕**取代，我们可以使用下面的代码。****

****例如，在句子中，**比起蓝莓芝士蛋糕或巧克力芝士蛋糕，我更喜欢草莓芝士蛋糕。******

*******那个？这里表示空格和连字符是可选的。*******

# ****结论****

****我希望这对在预处理文本时经常遇到这些情况的人有所帮助。请让我知道我是否错过了其他一些人们经常遇到的情况。****

****如果你喜欢这篇文章，这里有更多！****

****</representing-5-features-in-a-single-animated-plot-using-plotly-e4f6064c7f46>  </powerful-text-augmentation-using-nlpaug-5851099b4e97>  </effortless-exploratory-data-analysis-eda-201c99324857> [## 轻松的探索性数据分析(EDA)

towardsdatascience.com](/effortless-exploratory-data-analysis-eda-201c99324857) </replacing-lewa-5808cf020bfa>  

查看我的 [**GitHub**](https://github.com/rajlm10) 其他一些项目。你可以在这里联系我<https://rajsangani.me/>****。*** 感谢您的配合！*****