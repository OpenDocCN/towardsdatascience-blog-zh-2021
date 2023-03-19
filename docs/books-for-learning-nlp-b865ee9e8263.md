# 用 Python 学习自然语言处理的阅读列表

> 原文：<https://towardsdatascience.com/books-for-learning-nlp-b865ee9e8263?source=collection_archive---------14----------------------->

![](img/17ea3e86f68f7f539feb416dd203ef3d.png)

劳拉·卡弗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

一位前同事最近向他寻求建议，如何最好地从头开始学习自然语言处理(NLP)。

“优秀！”我以为；我已经为另一位前同事回答了完全相同的问题，所以这应该和重新整理之前的回答一样简单，对吗？

嗯，除了一个小细节之外，它本来是可以的:我完全不知道我把资源列表保存在哪里了。

为此，为了避免再次违反 [DRY 原则](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself#:~:text=The%20DRY%20principle%20is%20stated,their%20book%20The%20Pragmatic%20Programmer.)(即“不要重复自己”)，我写了这篇文章，这样如果/当我收到另一个请求时，我就可以把链接转发给他们。

以下是我非常固执己见的观点。不同意也可以留下你推荐的课程的链接。

## 免责声明#1

我假设你对线性代数、统计学和语言学有所了解。

如果没有，停止。别说了。为什么？三个原因:

1.  为了分析句子，识别词性，以及任何其他任务，你需要对语言学有一个坚实的理解，所以考虑参加一个课程，比如这个 [one](https://www.coursera.org/learn/human-language) 会给你一个概述。
2.  统计意义、相关性、分布和概率是机器学习的基本概念。为了填补这个空白，给自己挑一本关于统计学的好书，比如[数据科学实用统计学](https://www.goodreads.com/book/show/48816585-practical-statistics-for-data-scientists)并努力阅读。
3.  至于线性代数，正如下面的文章巧妙地解释的那样，它是深度学习和计算文本相似性的基础:

</linear-algebra-for-deep-learning-506c19c0d6fa>  </a-complete-beginners-guide-to-document-similarity-algorithms-75c44035df90>  

为此，帮自己一个忙，看看 3Brown1Blue 的这个令人惊叹的视频系列

## 免责声明#2

下面概述的道路是我希望我会选择的道路。

> “你走了什么路？”

好吧，简而言之，我花了几年(是的，几年)试图找出如何使用编码作为工具来完成我想要的项目。

听起来不错，对吧？

嗯，问题是我没有学习 Python 的基础知识，所以当我的代码抛出一个错误，或者，上帝保佑，我试图从 StackOverflow 和/或 Github 中重新使用别人的代码，但它不能开箱即用时，我不知道该怎么办。

> 为什么？

简单地说，我并不精通 Python，因此，我无法适应不熟悉的情况。

为此，如果你想专门学习 NLP 或一般的机器学习，你需要做的第一件事就是**学习中级水平的编码**。

> 为什么是中级？

因为你遇到的几乎每一个 Python for NLP 资源都会推荐你“熟悉”、“了解”、“精通”或其他一些难以置信的模糊标准，以便从他们的产品或平台中获得最大收益。

简而言之，你越擅长编码，就越容易将你在旅途中学到的概念付诸实践。

> 那么我如何达到“中级”Python 呢？

谢谢你的关心😄

# Python 基础:Python 3 实用介绍

这本书由 [Real Python](https://realpython.com/) 的团队编写，前 10 章涵盖了从在机器上安装 Python、运行脚本(不是笔记本)、数据类型、函数、调试和通过定义类进行面向对象编程(OOP)的所有内容。

至于本书的后半部分，它是一系列编码问题的集合，比如从网站上抓取和解析文本(第 16 章)以及为你的程序创建 GUI(第 18 章)。

因此，我建议你仔细阅读前十章的每一页，如果你感兴趣的话，可以阅读剩下的任何一章。

你可以在他们的[网站](https://realpython.com/products/python-basics-book/)上拿一本。

# 用于数据分析的 Python

当你管理数据时，你永远不会对数字或熊猫太精通，所以你花在研究韦斯·麦金尼的这本极其通俗易懂的书的每一分钟都应该被看作是对你未来的帮助。

再一次，帮你自己一个忙，在深入研究 NumPy 和 pandas 之前，先用第三章回顾一下 Python 中内置的数据结构，同时很好地理解 matplotlib。然而，这本书最有用的部分是第 14 章，其中包含了五个实例。

一定要在 GitHub 上访问该书的[回购](https://github.com/wesm/pydata-book#readme)以留下一颗星，查看每一章的笔记本，并找到在哪里拿起一本。

# 使用 Scikit-Learn、Keras 和 TensorFlow 进行机器实践学习

既然您的 Python 已经达到标准，并且可以操作数据，那么是时候学习机器学习了，没有比 Aurélien Geron 的这本宝石更好的学习机器学习基础的书了。虽然这本书涵盖了从逻辑回归到神经网络的所有内容(包括关于 NLP 注意力的讨论)，但真正的优势在于作者除了简单解释*如何*使用不同技术之外，还讨论了*为什么要使用*不同技术。因此，第 2 章的端到端机器学习项目，以及附录 B 的机器学习项目清单，值得这本书的成本。

要了解关于这篇文章的更多信息，以及在不下载或安装任何东西的情况下运行附带的笔记本，请访问作者的 [repo](https://github.com/ageron/handson-ml2/blob/master/README.md) 。

# 实用自然语言处理

我会读的第一本彻头彻尾的 NLP 书就是这本书。是什么让它这么好？这本书采用基于任务的方法，提出一个问题，然后询问如何使用 NLP 解决这个问题。

此外，它还展示了社交媒体、金融和法律等不同领域的应用。此外，几乎每个问题都通过多种方法解决，因此读者可以了解主要的 NLP 库，如 [Spacy](https://spacy.io/) 、 [NLTK](https://www.nltk.org/) 、 [Textblob](https://textblob.readthedocs.io/en/dev/) 、 [Gensim](https://radimrehurek.com/gensim/) 等等。

请访问该书的网站[了解更多信息。](http://www.practicalnlp.ai/index.html#home)

# 使用 Python 进行文本分析的蓝图

现在您已经掌握了 NLP，让我们来扩展一下您可以用它做什么。了解什么是可能的一种方法是通过观察人们如何使用 NLP 来寻找商业问题的解决方案。

这正是这本书所扮演的角色；它包含大约 100 个带有蓝图的常见用例。

> 但是什么是蓝图呢？"

作者指出，“蓝图[……]是一个常见问题的最佳实践解决方案。”本质上，它们是模板，因为您精通 Python，所以您可以最小程度地适应您的特定用例。

请务必前往该书的[回购](https://github.com/blueprints-for-text-analytics-python/blueprints-text)留下一颗星，看看工作的例子。

# 自然语言处理转换器

最后，既然您已经有了坚实的 Python 基础，并且了解了可以用 NLP 解决哪些问题以及如何解决这些问题，那么是时候了解最先进的架构:transformers 了。

> 什么是变形金刚？

抱歉，这超出了本文的范围；然而，如果你真的想知道，我推荐阅读开创性的作品 [*注意力是你需要的全部*](https://arxiv.org/abs/1706.03762) 和/或[插图变压器](https://jalammar.github.io/illustrated-transformer/)来获得想法的要点。

一旦你有了这些，一定要看看 Denis Rothman 的翻页文本，它涵盖了像微调和预训练模型以及用例这样的主题。要了解更多关于本文的信息，请点击[此处](https://github.com/PacktPublishing/Transformers-for-Natural-Language-Processing)。

# 填补空白的书籍

虽然我认为上面的文本是核心，但你永远也不会太了解你的工具是如何工作的。

因此，以下书籍提供了一些有用工具的深度挖掘。

[通过](https://github.com/PacktPublishing/Mastering-spaCy)[Duygu altn ok](https://www.linkedin.com/in/duygu-altinok-4021389a/)掌握空间应该是你学习所有空间知识的最佳指南。

通过[解决(几乎)任何机器学习问题](https://github.com/abhishekkrthakur/approachingalmost)由 [Abhishek Thakur](https://www.linkedin.com/in/abhi1thakur/) 完成，这类似于使用 Scikit-Learn、Keras & TensorFlow 的动手机器学习，但使用 PyTorch。

由 [Sudharsan Ravichandiran](https://www.linkedin.com/in/sudharsan1396/) 撰写的[Google BERT](https://github.com/sudharsan13296/Getting-Started-with-Google-BERT/blob/main/README.md)入门是一篇很好的关于 BERT 架构的文章。

# 结论

虽然我是上面列出的所有书籍的超级粉丝，但我毫不怀疑我忽略了一些应该包括在内的书籍。

为此，请在你推荐给那些想在新的一年开始 NLP 之旅的人的书中留下评论。

编码快乐！