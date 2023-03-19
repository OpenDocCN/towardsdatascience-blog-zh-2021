# 通过微调 GPT-2 的条件文本生成

> 原文：<https://towardsdatascience.com/conditional-text-generation-by-fine-tuning-gpt-2-11c1a9fc639d?source=collection_archive---------3----------------------->

## 给定一个标题和一系列关键词，GPT-2 能产生令人信服的假新闻吗？

![](img/7a67e9897455edd354e3469683687bb7.png)

照片由[在](https://unsplash.com/@retrosupply?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上反推

虽然基于 transformer 的模型近年来在一系列自然语言处理任务上取得了良好的效果，但是文本生成仍然是一个令人好奇的案例。早在 2020 年 9 月，《卫报》发表了一篇令人印象深刻的[文章](https://www.theguardian.com/commentisfree/2020/sep/08/robot-wrote-this-article-gpt-3)，据称是由 OpenAI 的 GPT-3 从无到有撰写的，在主流媒体上广受好评，但许多人工智能专家仍然[持怀疑态度](https://www.technologyreview.com/2020/07/20/1005454/openai-machine-learning-language-generator-gpt-3-nlp/?fbclid=IwAR1SXByhKhOgL-L43KxigExJDdoCQEMJ7CPCW4J5W6GXDTuHE0PP_8R3AKs)。

文本生成的一个问题是缺乏对其方向的控制。使用 GPT-3，你可以给模型一个介绍和说明，但即使这样，也需要一个人类编辑从多个输出中挑选和安排文本，使之具有凝聚力。还有其他方法，例如 Salesforce 的 [CTRL](https://arxiv.org/abs/1909.05858) 和 UberAI 的 [PPLM](https://arxiv.org/pdf/1912.02164.pdf) 。

在本文中，我们将微调 Huggingface 预训练的 GPT-2，并提出我们自己的解决方案:通过选择数据集，我们可能会更好地控制文本样式和生成的内容。

# 数据

我们将使用来自[新闻聚合数据集](https://archive.ics.uci.edu/ml/datasets/News+Aggregator)的样本。它包含来自著名新闻出版商的 40 多万篇新闻文章的标题和超链接。为了减少训练时间，我从 4 个新闻类别中随机抽取了大约 10k 篇文章:商业、科学、娱乐和健康。文章是预先下载的，以减少运行时间。

# 关键词提取

我们需要在训练过程中从每篇文章的关键字列表。有一系列可用的方法，从 [Rake](https://medium.com/datadriveninvestor/rake-rapid-automatic-keyword-extraction-algorithm-f4ec17b2886c) 到使用 BERT 的[等等，但是我们在这里将坚持一个简单的](/keyword-extraction-with-bert-724efca412ea) [TFIDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) ，因为这不是我们的主要关注点。在我们的关键字选择中，我们还将允许 2 个字母的短语组成一个专有名词短语，例如，“内容创建者”。此过程也是离线执行的，因为只需执行一次。

数据下载和关键词提取的代码可以在我的 [GitHub 库](https://github.com/ivanlai/Conditional_Text_Generation)中找到。

# 管道

管道设置包括定义记号化器、模型和数据集，然后使用训练器类进行微调，最后是文本生成。我假设您熟悉这个一般设置— [本文](https://medium.com/swlh/fine-tuning-gpt-2-for-magic-the-gathering-flavour-text-generation-3bafd0f9bb93)详细介绍了管道，如果您需要提醒的话。我将重点介绍为了实现条件文本生成，我做了哪些不同的工作。

您可以访问我的 [Colab 笔记本](https://colab.research.google.com/drive/1vnpMoZoenRrWeaxMyfYK4DDbtlBu-M8V?usp=sharing)中的完整代码了解更多详细信息，也欢迎您进行复制和实验。

# 模型

在这个实验中，我们将使用 12 层解码器的小型版 GPT-2。该模型在 800 万个网页上进行了训练，在语言任务方面已经相当强大。为了在采用我们的数据集时保持其在语言建模中的一般能力，我们将通过设置其参数来冻结底部 6 层。requires_grad 为 False，并且仅训练顶部 6 层。这也将加速训练，因为向后传球的次数减少了。

模型层的名称可以简单地通过打印(模型)找到。

# 培训设置

在标准的文本生成微调中，由于我们是在给定我们到目前为止看到的文本的情况下预测下一个标记，所以标签只是移位编码的标记化输入(注意，如果我们设置 labels=input_ids，则标签会在模型内自动移位—参见下面的参考文献 1)。

但是在这里，我们希望有更多的控制——除了文章文本，我们还会给模型一个标题和一个微调的关键字列表。我们在定制的数据集类中实现了这一点。

在标准设置中，我们在文本前面加上 bos_token，在后面加上 eos_token:

我们还将在文本前插入标题和关键字列表，用特殊的分隔符号分隔:

为了帮助模型泛化，我们通过使用函数 join_keywords()在训练期间对关键字列表进行采样和洗牌来引入数据扩充。但是在验证期间没有应用任何增加来保持跨时期的一致性。然后，按照标准实践，使用训练器类为少量时期(在我们的例子中是 4 个时期)训练模型。

# 结果

为了测试我们新的假新闻生成器的威力，我从 BBC 新闻中挑选了一条最近热门的轻松新闻:[“当我们的照片成为一种模因时，我们感到非常悲伤”](https://www.bbc.com/news/newsbeat-55704468)。通过使用相同的标题并(相当主观地)从文章中挑选几个关键词，让我们看看我们的模型能给它带来什么样的变化。要用的关键词是:‘火车’，‘小伙子’，‘喝酒’，‘图片’，‘搞笑’和‘insta gram’。

我们为语料库设置的关键字的大小相当大，但是如果我们想要使用的关键字不在这个集合中，我们总是可以使用集合中的同义词，或者在一些单词相似性度量方面最接近的一个。

有几种方法可以从模型中生成文本。如果我们使用波束搜索和采样，我们得到样本如下:

```
Unfortunately, all we have right now is an image that has gone viral.

The picture was taken by one of the boys on Instagram and quickly went viral after it appeared to show a group of friends getting drunk at a New York City bar.

As soon as the picture came out online, many people took to Twitter to express their disgust with the picture.

"What's wrong with this f**king train??," one user wrote.

Another said: "I can't believe my eyes didn't even open."

A third added: "Just saw some pictures of men being mugged in NYC... I'm so sick of these dumbasses."

This content is created and maintained by a third party, and imported onto this page to help users provide their email addresses. You may be able to find more information about this and similar content at piano.io
```

每一句都是语义连贯的，前三句也确实与标题有关，但随后文章发生了意想不到的转折。“这混账火车怎么了？”…我也在想这件事。随着“思想”列车慢慢偏离轨道，我们似乎有了一些控制，但可能还不够。

一些关键词(‘火车’、‘喝酒’、‘图片’和‘insta gram’，或者它们的同义词或内涵)按照指示被用在生成的文本中。但是，‘滑稽’没有用；相反，我们用了“厌恶”这个词，它有效地概括了作品的情绪，这很可能是因为我们选择的标题。

这与原始的 GPT-2 相比如何？使用标题作为第一句话，下面是一个来自原始模型的输出示例以供比较:

```
We got a lot of grief when our photo became a meme.

"I'm not going to lie, I don't know how many times we've been in that kind of situation," he said. "It's hard for me to put it into words."

He added: "There was no way this could have happened without the help of my family and friends who were there with us on Saturday night. We're all so proud of what they did as well."
```

这一段开头不错，但最后一句无意的转折泄露了秘密。不出所料，这些关键字根本不重要。

在某种程度上，我们试图做的是与文本摘要相反的事情——我们试图将标题扩展成一篇以关键词为特色的文章，希望标题的效果会渗透到整篇文章中。相比之下，原始的 GPT-2 只是从第一句话开始，标题的记忆效果可能更短暂。

回到我们的模型，我们也可以使用像 top-p (nucleus)采样这样的方法生成文本，这往往会产生更多的变体。这里有一些有趣的，听起来部分真实，部分有趣和令人不安。它们可能没有说服力，但至少很有趣:

```
It was like we were being mugged and all over again.

“I think it's really sad that this happened to us because people are so used by the pictures on their Instagram feeds... they just want attention," said Dr Emily Loughner-Bullock from Kings College London in England who works as an epidemiologist at King Mungol University School Of Public Health (KMU). ‘When you look back through history there have been many examples where celebrities can make headlines for quite controversial things - such is how social media has affected public health research into mental illness or addiction."

The story spread online after one famous photograph emerged which showed two young men with guns talking about drug use while drinking together before another took off his shirt revealing something he had hidden under her skirt: "Nice impression but I wonder if girls don't understand what 'hooking up' looks exactly?" wrote Shilpa Khetrapal Singh Khalsa Mukherjee following Twitter users sharing similar images showing them laughing happily out loud without any justification whatsoever behind some sort action picture taken during dinner parties held between friends...

There will be no further comment here due today afternoon..
```

```
When I was 12 years old my friends and family started seeing pictures from the train accident. It just made them cry so much that they took to Instagram after it happened:

As far as their reactions are concerned this is all very funny but if you take out your cell phone or tablet then these people will be talking about what went down in Boston today - there's no way we could have imagined how bad things would look with photos like those...
```

```
“It's hard to remember the day you started your life in this world and it was just too much for one kid. It is really sad that we can no longer celebrate those days with these photos because they were meant only as fun pictures."
```

```
Join us for the world’s leading event about accelerating enterprise transformation with AI and Data by signing up today
The internet was flooded in on Saturday morning after one man posted an extremely disturbing photograph to his Twitter account. In it he is seen sitting at home surrounded only wearing headphones – just as we have done before: The picture has gone viral! Here are some highlights from that hilarious moment…
1) It's been nearly six months since I saw this image
```

```
It was all very funny. It’s not like I'm going to stop posting photos because people will be more than happy with it and the memes are still growing every day on Twitter

A new poster for an American Idol-themed picture that appeared in The New York Post is showing up at this week's event where fans can get drunk together (and drink too) before their favorite shows!
```

仔细看看人工智能写作的现状，似乎记者不会很快被技术取代。

毕竟，这是我首先发表这篇文章的原因——否则我怎么能在道德上有理由在互联网上这么容易地制造一个有效的假新闻生成器呢？

# 参考和鸣谢

1.  [拥抱脸 GPT-2 文件](https://huggingface.co/transformers/model_doc/gpt2.html#gpt2lmheadmodel)
2.  [拥抱脸变形金刚示例](https://huggingface.co/transformers/examples.html)
3.  [如何生成文本:用变形金刚使用不同的解码方法进行语言生成](https://huggingface.co/blog/how-to-generate)
4.  杰伊·阿拉玛的插图版 GPT-2
5.  [为万智牌风味文本生成微调 GPT-2](https://medium.com/swlh/fine-tuning-gpt-2-for-magic-the-gathering-flavour-text-generation-3bafd0f9bb93)
6.  [伊恩·波特的 GPT-2 教程](https://snappishproductions.com/blog/2020/03/01/chapter-9.5-text-generation-with-gpt-2-and-only-pytorch.html.html)