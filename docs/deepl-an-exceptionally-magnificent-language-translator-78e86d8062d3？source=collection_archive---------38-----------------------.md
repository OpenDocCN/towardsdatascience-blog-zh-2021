# DeepL:一个非常出色的语言翻译器

> 原文：<https://towardsdatascience.com/deepl-an-exceptionally-magnificent-language-translator-78e86d8062d3?source=collection_archive---------38----------------------->

## DeepL 将翻译的边界推向了人类层面

![](img/2707b6efcddc560cba2d9db19565d711.png)

[杆长](https://unsplash.com/@rodlong?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

DeepL 将机器翻译带到了一个全新的水平。到目前为止，我们已经习惯了基于递归神经网络和“嵌入”形式的单词表示的机器翻译，这些翻译听起来往往机械而不自然。这就是为什么很多人只是在紧急情况下，旅行时才使用谷歌翻译。DeepL 是一个游戏改变者。建立在神经网络上的算法可以翻译文本，这与一个合格的翻译几乎没有区别。

与谷歌翻译类似，网络上有一个[应用](https://www.deepl.com/translator)，可以让你免费翻译 26 种不同语言之间的文本。翻译结果非常出色，很可能会改变人们习惯的文本翻译方式。在本文中，我比较了使用 DeepL 和 [Google Translate](https://translate.google.com/?sl=cs&tl=en&text=Andrej%20Babi%C5%A1%3A%20Nejsem%20idiot%2C%20%C5%99%C3%ADd%C3%ADm%20chytrou%20karant%C3%A9nu%20a%20m%C3%A1v%C3%A1m%20z%20%C5%98ecka.%0A%0APan%20premi%C3%A9r%20se%20n%C3%A1m%20rozjel%2C%20a%20to%20ve%20v%C5%A1ech%20v%C3%BDznamech%20tohoto%20slova.%20S%20rodinkou%20se%20rozjel%20do%20%C5%98ecka%2C%20na%20soci%C3%A1ln%C3%ADch%20s%C3%ADt%C3%ADch%20se%20rozjel%20s%20vyj%C3%A1d%C5%99en%C3%ADmi%2C%20kter%C3%BDm%20se%20u%C5%BE%20snad%20ani%20nejde%20sm%C3%A1t.%20Andrej%20Babi%C5%A1%20pr%C3%BD%20p%C5%99evzal%20kontrolu%20nad%20chytrou%20karant%C3%A9nou%2C%20m%C3%A1me%20probl%C3%A9my%2C%20ale%20ve%20skute%C4%8Dnosti%20se%20nic%20ned%C4%9Bje%2C%20a%20proto%20jako%C5%BEto%20p%C5%99edseda%20vl%C3%A1dy%20v%20krizov%C3%A9%20situaci%2C%20kter%C3%A1%20nen%C3%AD%20krizov%C3%A1%2C%20odj%C3%AD%C5%BEd%C3%AD%20na%20dovolenou%2C%20co%C5%BE%20je%20divn%C3%A9%2C%20proto%C5%BEe%20nap%C5%99%C3%ADklad%20ministryn%C4%9B%20financ%C3%AD%20Alena%20Schillerov%C3%A1%20podle%20sv%C3%BDch%20slov%20%C5%99e%C5%A1%C3%AD%20krizi%2C%20a%20tak%20nikam%20nejede.%0A%0ANo%20nic%2C%20p%C4%9Bkn%C4%9B%20popo%C5%99%C3%A1dku.%20Ministr%20zdravotnictv%C3%AD%20Adam%20Vojt%C4%9Bch%20po%20sedmist%C3%A9%20osmdes%C3%A1t%C3%A9%20p%C3%A1t%C3%A9%20selhal%20a%20s%20n%C3%ADm%20i%20%C3%BAdajn%C3%BD%20projekt%20chytr%C3%A9%20karant%C3%A9ny.%20%C5%98%C3%ADk%C3%A1me%20%C3%BAdajn%C3%A9%20proto%C5%BEe%20ji%20nikdo%20nevid%C4%9Bl.%20Babi%C5%A1%20se%20na%C5%A1tval.%20Nejprve%20se%20na%C5%A1i%20filutov%C3%A9%20pokusili%20situaci%20%C5%99e%C5%A1it%20t%C3%ADm%2C%20%C5%BEe%20prost%C4%9B%20a%20jednodu%C5%A1e%20%C5%A1krtli%20ze%20statistiky%20po%C4%8Dty%20naka%C5%BEen%C3%BDch%2C%20co%C5%BE%20bylo%20divn%C3%A9%2C%20ale%20op%C4%9Bt%20to%20vzbudilo%20pouze%20v%C3%BDsm%C4%9Bch.%20Lid%C3%A9%20nav%C3%ADc%20za%C4%8Dali%20b%C3%BDt%20nerv%C3%B3zn%C3%AD%2C%20proto%C5%BEe%20na%20testy%20se%20i%20po%20p%C5%AFl%20roce%20od%20po%C4%8D%C3%A1tku%20pandemie%20stoj%C3%AD%20fronty%20a%20nic%20nefunguje.&op=translate) 翻译不那么简单的文本，并给出了 DeepL 在不久的将来可能会被使用的例子。

# 让我们测试一下

我先用 DeepL 翻译了下面的文字，然后用 Google 翻译。文章是用捷克语写的，由于其极其复杂的语法，捷克语被认为是最难学的语言之一。这篇文章是以一种相当口语化的方式写的，结构不太好，包含引文，并且来自新闻门户[https://www.forum24.cz/.](https://www.forum24.cz/andrej-babis-nejsem-idiot-ridim-chytrou-karantenu-a-mavam-z-recka/)

```
Andrej Babiš: Nejsem idiot, řídím chytrou karanténu a mávám z Řecka.Pan premiér se nám rozjel, a to ve všech významech tohoto slova. S rodinkou se rozjel do Řecka, na sociálních sítích se rozjel s vyjádřeními, kterým se už snad ani nejde smát. Andrej Babiš prý převzal kontrolu nad chytrou karanténou, máme problémy, ale ve skutečnosti se nic neděje, a proto jakožto předseda vlády v krizové situaci, která není krizová, odjíždí na dovolenou, což je divné, protože například ministryně financí Alena Schillerová podle svých slov řeší krizi, a tak nikam nejede.No nic, pěkně popořádku. Ministr zdravotnictví Adam Vojtěch po sedmisté osmdesáté páté selhal a s ním i údajný projekt chytré karantény. Říkáme údajné protože ji nikdo neviděl. Babiš se naštval. Nejprve se naši filutové pokusili situaci řešit tím, že prostě a jednoduše škrtli ze statistiky počty nakažených, což bylo divné, ale opět to vzbudilo pouze výsměch. Lidé navíc začali být nervózní, protože na testy se i po půl roce od počátku pandemie stojí fronty a nic nefunguje.
```

首先，让我们看看谷歌翻译如何翻译文本:

```
Andrej Babiš: I'm not an idiot, I run a smart quarantine and I'm waving from Greece.The Prime Minister **has left us (1)**, in all meanings of the word. He went to Greece with his family, **he started on (2)** social networks with expressions that are hardly laughable. Andrej Babiš is said to have taken control of clever quarantine, we have problems, but nothing really happens, and therefore as prime minister in a non-crisis crisis, he goes on vacation, which is strange because, for example, Finance Minister Alena Schillerová says crisis, so it goes nowhere.**Well, nothing, all right (3).** Minister of Health Adam Vojtěch failed for the seventy-eighty-fifth time, and with him the alleged project of clever quarantine. We say alleged because no one saw **her(4)**. **Grandma(5)** was angry. At first, our filets tried to solve the situation by **simply and simply(6)** deleting the number of infected from the statistics, which was strange, but again it only aroused ridicule. In addition, people have become nervous, because even after six months from the beginning of the pandemic, there are queues for tests and nothing works.
```

我用数字标记了翻译中最高的语法错误:(1)这是单词的语义混乱，英文文本想说的是人类对情况的反应过度，(2)这又是一个单词的不准确翻译，(3)翻译“嗯，没什么，好吧”是人类可能不会说的机器翻译的例子，(4)这是一个错误，因为我们正在谈论一场危机， 因此，我们使用代词“它”，而不是“她”，(5)完全不准确，文本指的是 Babi，而译者翻译了“祖母”一词，(6)“简单而简单”又是一个英国人或美国人可能不会说的表达。

在文本中可能有更多的文体和语言错误可以描述。

我们来看看来自 DeepL 的翻译:

```
Andrej Babiš: I'm not an idiot, I run a smart quarantine and wave from Greece.The Prime Minister is on a roll, in all senses of the word. He took off for Greece with his family, and on social media he made statements that are impossible to laugh at anymore. He says that Andrej Babiš has taken control of a clever quarantine, that we have problems, but that nothing is really happening, and that is why, as Prime Minister in a crisis situation that is not a crisis, he is going on holiday, which is strange because, for example, Finance Minister Alena Schiller says she is dealing with a crisis and so is not going anywhere.Anyway, first things first. Health Minister Adam Vojtěch has failed for the eighty-fifth time, and with him the alleged smart quarantine project. We say alleged because no one has seen it. Babiš got angry. At first our philots tried to solve the situation by **simply and simply (1)** crossing out the number of infected from the statistics, which was strange, but again it only drew ridicule. Moreover, people have become nervous because there are still queues for tests six months after the pandemic began and nothing is working.Translated with [www.DeepL.com/Translator](http://www.DeepL.com/Translator) (free version)
```

翻译中的主要错误在于“简单而简单”这一表达方式，这是一种捷克特有的表达方式，该表达方式的直接翻译可能并不存在。

那不是不真实的吗？:-)文本的翻译差异巨大。

# 结论

一个使用 DeepL 的简单演示展示了这个工具有多棒。与 Google translator 相比，它在文本的机器翻译方面有了多方面的改进。

**这篇文章最初是用捷克语写的，然后用免费的 DeepL 版本**翻译成了英语，在[这里](https://www.deepl.com/translator)可以找到。除了文章标题和最后一句话，**我没有以任何方式编辑过语言翻译。我不确定你是否认出了它🙂**

DeepL 很有可能在不久的将来被使用，其中包括:

*   公司:网站、产品信息、内部文件、公司介绍等文本的翻译。
*   毕业论文和学位论文
*   网站的语言变异

在我看来，在 5 -7 年的时间里，人们只会翻译包含特定情感元素和具有艺术潜台词的文本，如诗歌、小说或其他非常具体的文本。其他文本由人类翻译在经济上是不可行的。最贵的许可证 DeepL 目前每月收费 40 欧元。如果它的创造者能把它的价格控制在翻译的成本之内，那么大多数翻译就不需要人类了。

*PS:你可以订阅我的* [*邮箱列表*](https://medium.com/subscribe/@petrkorab) *每次我写新文章都会收到通知。如果你还不是中等会员，你可以在这里加入*[](https://medium.com/@petrkorab/membership)**。**