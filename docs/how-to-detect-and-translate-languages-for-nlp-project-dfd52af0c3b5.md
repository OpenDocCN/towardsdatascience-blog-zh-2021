# 如何为 NLP 项目检测和翻译语言

> 原文：<https://towardsdatascience.com/how-to-detect-and-translate-languages-for-nlp-project-dfd52af0c3b5?source=collection_archive---------1----------------------->

## 从具有多种语言的文本数据到单一语言

![](img/db04727d3c0c98e61934f59a1e403996.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=20860)

*本文更新于 2022 年 6 月 20 日*

对于说西班牙语的人，你可以在这里阅读这篇文章的翻译版本

祝你新年快乐，2021 年到了，你做到了💪**。** 2020 年已经过去，尽管对世界各地的许多人来说，2020 年是艰难而奇怪的一年，但仍有许多值得庆祝的事情。2020 年，我明白了我们所需要的只是我们所爱的人、家人和朋友的爱和支持。

> “面对逆境，我们有一个选择。我们可以痛苦，也可以变得更好。那些话是我的北极星。”-卡林·沙利文

这将是我 2021 年的第一篇文章，我将谈论数据科学家或机器学习工程师在从事 NLP 项目时可能面临的一些语言挑战，以及如何解决这些挑战。

假设你是一名数据科学家，被分配到一个 NLP 项目中，分析人们在社交媒体(如 Twitter)上发布的关于新冠肺炎的内容。你的首要任务之一是为新冠肺炎找到不同的标签(例如 **#covid19** ),然后开始收集所有与新冠肺炎相关的推文。

当您开始分析收集到的与新冠肺炎相关的数据时，您会发现这些数据是由世界各地的不同语言生成的，例如*英语、斯瓦希里语、* [*西班牙语*](https://www.ibidem-translations.com/edu/traduccion-idiomas-nlp/) *、中文、印地语*等。在这种情况下，在开始分析数据集之前，您将有两个问题需要解决，第一个是**识别**特定数据的语言，第二个是如何**翻译**这些数据

***那么如何才能解决这两个问题呢？***

![](img/164b5d01fe21fd36821c84885fdca96c.png)

图片来自 [Pixabay](https://pixabay.com/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5723449) 的[图米苏](https://pixabay.com/users/tumisu-148124/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=5723449)

## 第一个问题:语言检测

第一个问题是知道如何检测特定数据的语言。在这种情况下，您可以使用一个名为 **langdetect 的简单 python 包。**

l [angdetect](https://pypi.org/project/langdetect/) 是一个由 [Michal Danilák](https://pypi.org/user/Michal.Danilk/) 开发的简单 python 包，支持开箱即用的 **55** 不同语言**([ISO 639-1 代码](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes))的检测:**

```
af, ar, bg, bn, ca, cs, cy, da, de, el, en, es, et, fa, fi, fr, gu, he,
hi, hr, hu, id, it, ja, kn, ko, lt, lv, mk, ml, mr, ne, nl, no, pa, pl,
pt, ro, ru, sk, sl, so, sq, sv, sw, ta, te, th, tl, tr, uk, ur, vi, zh-cn, zh-tw
```

## **安装语言检测**

**要安装 langdetect，请在您的终端中运行以下命令。**

```
pip install langdetect
```

## **基本示例**

**检测文本的语言:例如“*Tanzania ni nchi inayoongoza kwa utalii barani Afrika*”。首先，从 langdetect 导入 **detect** 方法，然后将文本传递给该方法。**

**输出:“软件”**

**该方法检测到所提供的文本是用**斯瓦希里**语(‘SW’)编写的。**

**你也可以使用 **detect_langs** 方法找出排名靠前的语言的概率。**

**输出:[sw:0.9999971710531397]**

****注:**你还需要知道，语言检测算法是不确定的，如果你对一个太短或太不明确的文本运行它，你可能每次运行它都会得到不同的结果。**

**在语言检测之前调用下面的代码，以便得到一致的结果。**

**现在，您可以通过使用 langdetect python 包来检测数据中的任何语言。**

## **第二个问题:语言翻译**

**你需要解决的第二个问题是将文本从一种语言翻译成你选择的语言。在这种情况下，你将使用另一个有用的 python 包，名为[**Google _ trans _ new**](https://pypi.org/project/google-trans-new/)。**

**google_trans_new 是一个免费的、无限制的 python 包，它实现了 Google Translate API，还执行自动语言检测。**

## **安装 google_trans_new**

**要安装 google_trans_new，请在您的终端中运行以下命令。**

```
pip install google_trans_new
```

## **基本示例**

**要将文本从一种语言翻译成另一种语言，您必须从`google_trans_new`模块导入`google_translator`类。然后你必须创建一个`google_translator`类的对象，最后将文本作为参数传递给**翻译**方法，并使用 **lang_tgt** 参数指定目标语言，例如 lang_tgt="en "。**

**在上面的例子中，我们将一个斯瓦希里语的句子翻译成英语。下面是翻译后的输出。**

```
Tanzania is the leading tourism country in Africa
```

**默认情况下，`translate()`方法可以检测所提供文本的语言，并将英语翻译返回给它。如果要指定文本的源语言，可以使用 **lang_scr** 参数。**

**这里是所有语言的名称及其简写符号。**

```
{'af': 'afrikaans', 'sq': 'albanian', 'am': 'amharic', 'ar': 'arabic', 'hy': 'armenian', 'az': 'azerbaijani', 'eu': 'basque', 'be': 'belarusian', 'bn': 'bengali', 'bs': 'bosnian', 'bg': 'bulgarian', 'ca': 'catalan', 'ceb': 'cebuano', 'ny': 'chichewa', 'zh-cn': 'chinese (simplified)', 'zh-tw': 'chinese (traditional)', 'co': 'corsican', 'hr': 'croatian', 'cs': 'czech', 'da': 'danish', 'nl': 'dutch', 'en': 'english', 'eo': 'esperanto', 'et': 'estonian', 'tl': 'filipino', 'fi': 'finnish', 'fr': 'french', 'fy': 'frisian', 'gl': 'galician', 'ka': 'georgian', 'de': 'german', 'el': 'greek', 'gu': 'gujarati', 'ht': 'haitian creole', 'ha': 'hausa', 'haw': 'hawaiian', 'iw': 'hebrew', 'hi': 'hindi', 'hmn': 'hmong', 'hu': 'hungarian', 'is': 'icelandic', 'ig': 'igbo', 'id': 'indonesian', 'ga': 'irish', 'it': 'italian', 'ja': 'japanese', 'jw': 'javanese', 'kn': 'kannada', 'kk': 'kazakh', 'km': 'khmer', 'ko': 'korean', 'ku': 'kurdish (kurmanji)', 'ky': 'kyrgyz', 'lo': 'lao', 'la': 'latin', 'lv': 'latvian', 'lt': 'lithuanian', 'lb': 'luxembourgish', 'mk': 'macedonian', 'mg': 'malagasy', 'ms': 'malay', 'ml': 'malayalam', 'mt': 'maltese', 'mi': 'maori', 'mr': 'marathi', 'mn': 'mongolian', 'my': 'myanmar (burmese)', 'ne': 'nepali', 'no': 'norwegian', 'ps': 'pashto', 'fa': 'persian', 'pl': 'polish', 'pt': 'portuguese', 'pa': 'punjabi', 'ro': 'romanian', 'ru': 'russian', 'sm': 'samoan', 'gd': 'scots gaelic', 'sr': 'serbian', 'st': 'sesotho', 'sn': 'shona', 'sd': 'sindhi', 'si': 'sinhala', 'sk': 'slovak', 'sl': 'slovenian', 'so': 'somali', 'es': 'spanish', 'su': 'sundanese', 'sw': 'swahili', 'sv': 'swedish', 'tg': 'tajik', 'ta': 'tamil', 'te': 'telugu', 'th': 'thai', 'tr': 'turkish', 'uk': 'ukrainian', 'ur': 'urdu', 'uz': 'uzbek', 'vi': 'vietnamese', 'cy': 'welsh', 'xh': 'xhosa', 'yi': 'yiddish', 'yo': 'yoruba', 'zu': 'zulu', 'fil': 'Filipino', 'he': 'Hebrew'}
```

## **检测和翻译 Python 函数**

**我创建了一个简单的 python 函数，您可以检测文本并将其翻译成您选择的语言。**

**python 函数接收文本和目标语言作为参数。然后，它检测所提供的文本的语言，如果文本的语言与目标语言相同，则它返回相同的文本，但如果不相同，则它将所提供的文本翻译成目标语言。**

**示例:**

**在上面的源代码中，我们将句子翻译成斯瓦希里语。以下是输出结果:-**

```
Natumai kwamba, nitakapojiwekea akiba, nitaweza kusafiri kwenda Mexico
```

# **包扎**

**在本文中，您了解了当您拥有不同语言的文本数据并希望将数据翻译成您选择的单一语言时，如何解决两种语言难题。**

**恭喜👏，你已经做到这篇文章的结尾了！**

**你可以在这里下载本文用到的笔记本:[https://github.com/Davisy/Detect-and-Translate-Text-Data](https://github.com/Davisy/Detect-and-Translate-Text-Data)**

**如果你学到了新的东西或者喜欢阅读这篇文章，请分享给其他人看。在那之前，下期帖子再见！也可以通过 Twitter [@Davis_McDavid](https://twitter.com/Davis_McDavid) 联系到我。**

*****最后一件事:*** *在以下链接中阅读更多类似这样的文章。***

**<https://medium.com/datadriveninvestor/how-to-deploy-your-nlp-model-to-production-as-an-api-with-algorithmia-e4081854d524>  <https://chatbotslife.com/how-to-use-texthero-to-prepare-a-text-based-dataset-for-your-nlp-project-734feea75a5a>  <https://davis-david.medium.com/meet-the-winners-of-swahili-news-classification-challenge-60f5edd7aa9> **