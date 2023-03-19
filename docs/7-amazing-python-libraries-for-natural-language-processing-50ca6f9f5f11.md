# 用于自然语言处理的 7 个惊人的 Python 库

> 原文：<https://towardsdatascience.com/7-amazing-python-libraries-for-natural-language-processing-50ca6f9f5f11?source=collection_archive---------20----------------------->

## 这些库将帮助你处理文本数据

![](img/eda9c40bda5be87a96e8ab141a0887a4.png)

照片由 [**Skylar 康**发自](https://www.pexels.com/@skylar-kang?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[Pexels](https://www.pexels.com/photo/pieces-of-paper-with-words-6045344/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

自然语言处理(NLP)是深度学习的一个领域，其目标是教会计算机如何理解人类语言。该领域是数据科学和机器学习的结合，主要处理文本数据的提取和分析，以从中提取一些值。

NLP 是 2021 年学习的一项令人惊叹的技术，因为许多大公司都在关注客户的情感分析，或使用原始文本数据制作高级聊天机器人。

因此，如果这个领域让您感兴趣，在本文中，我介绍了 7 个令人惊叹的 Python 库，它们可能会帮助您实现 NLP 算法并使用它们构建项目。

# **1。**NLTK

自然语言工具包(NLTK)是目前最流行的构建 NLP 相关项目的平台。它为超过 50 个语料库和词汇资源提供了一个易于使用的界面，并附带了一系列文本处理库，如分类、词干、标记、分析、标记化等。

这个库也是一个开源库，几乎可以用于各种操作系统。所以不管你是 NLP 的初学者还是 ML 的研究者，你都可以学习 NLTK。

> **安装**

```
pip install nltk
```

**了解更多:**[https://www.nltk.org/](https://www.nltk.org/)

# **2。** **多语种**

Polyglot 是一个用于 NLP 的 python 库，特别有用，因为它支持广泛的多语言应用程序。根据 polyglot 的文档，它支持 165 种语言的标记化，196 种语言的语言检测，16 种语言的词性标注和 130 多种语言的情感分析。

因此，如果有人使用非主流语言，这可能会很有用。此外，它的工作速度非常快，因为它使用 NumPy。

> **安装**

```
pip install polyglot
```

**了解更多:**[http://polyglot.readthedocs.org/](http://polyglot.readthedocs.org/)

# **3。** **空间**

SpaCy 是一个 Python NLP 库，特别适用于包含大量文本数据的行业级真实项目。

使用这个库的主要优势是它的速度。SpaCy 比其他库快得多，因为它是用 Cython 编写的，这也使它能够有效地处理大量数据。

支持超过 64 种语言，19 种语言的 60 +训练管道，使用 BERT 等预训练转换器的多任务学习，以及对 Pytorch 和 Tensorflow 等现代 ML/DL 框架的支持，使 SpaCy 成为专业项目的良好选择。

> **安装(以及依赖关系)**

```
pip install –U setuptools wheelpip install –U spacypython -m spacy download en_core_web_sm
```

**了解更多:**【https://spacy.io/】T4

# **4。** **GenSim**

GenSim 是用 Python 编写的 NLP 库，由于其惊人的速度和内存优化而广受欢迎。GenSim 中使用的所有库都是独立于内存的，并且可以轻松运行大型数据集。它附带了小型有用的自然语言处理算法，如随机投影(RP)，潜在语义分析(LSA)，分层狄利克雷过程(HDP)等。

GenSim 使用 SciPy 和 NumPy 进行计算，并用于聊天机器人和语义搜索应用等应用中。

> **安装**

```
pip install — upgrade gensim
```

**了解更多:**[https://radimrehurek.com/gensim/](https://radimrehurek.com/gensim/)

# **5。** **Textblob**

Textblob 是一个由 NLTK 支持的 Python 库。它提供了 NLTK 的几乎所有功能，但是以一种更加简单和初学者友好的方式，并且它的 API 可以用于一些常见的任务，如分类、翻译、单词变形等。

许多数据科学家也使用 textblob 进行原型开发，因为它使用起来要轻便得多。

> **安装**

```
pip install -U textblobpython -m textblob.download_corpora
```

**了解更多:**https://textblob.readthedocs.io/en/dev/

# 6。 **PyNLPI**

PyNLPI 也读作菠萝，是一个 Python NLP 库，主要用于构建基本的语言处理模型。它分为不同的模型和包，可以用于不同种类的 NLP 任务。PyNLPI 最突出的特性之一是它附带了一个完整的库，用于处理 FoLiA XML(语言注释格式)

> **安装**

```
pip install pynlpl
```

**了解更多:**[**https**://pynlpl . readthedocs . io/en/latest/](https://pynlpl.readthedocs.io/en/latest/)

# **7。**图案**图案**

*Pattern* 是一个多用途的 Python 库，可用于不同的任务，如自然语言处理(标记化、情感分析、词性标注等)。)、来自网站的数据挖掘和使用内置模型的机器学习，例如 K-最近邻、支持向量机等。

这个库对于初学者来说很容易理解和实现，因为它的语法简单明了，对于需要处理文本数据的 web 开发人员来说也很有帮助。

> **安装**

```
pip install pattern
```

**了解更多:**[https://github.com/clips/pattern](https://github.com/clips/pattern)

# **结论**

尽管列表中提到的几乎所有 NLP 库都形成了类似的任务，但它们在某些特定情况下可能会派上用场。就像 SpaCy 在处理具有大型数据集的真实项目时可能会有所帮助一样，GenSim 将在有严格的内存限制时发挥作用。

NLTK 肯定会成为最受学生和研究人员欢迎的图书馆。所以我们应该总是选择最适合问题陈述的库。

> 在你走之前…

如果你喜欢这篇文章，并且想继续关注关于 **Python &数据科学**的更多**精彩文章**——请点击这里[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)考虑成为一名中级会员。

请考虑使用[我的推荐链接](https://pranjalai.medium.com/membership)注册。通过这种方式，会员费的一部分归我，这激励我写更多关于 Python 和数据科学的令人兴奋的东西。

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。