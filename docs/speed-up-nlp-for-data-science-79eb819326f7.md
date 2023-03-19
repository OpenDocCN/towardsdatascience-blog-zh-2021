# 加速数据科学的自然语言处理

> 原文：<https://towardsdatascience.com/speed-up-nlp-for-data-science-79eb819326f7?source=collection_archive---------20----------------------->

## 为自然语言处理任务编写更快代码的 5 种简单方法

![](img/9467d2e29722c80c066d313ea9dba395.png)

由 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/speed?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

不久前，我写了一篇关于我创建的过滤相似文本的程序的文章。但是当我把它投入生产时，我的程序的 NLP 部分造成了巨大的瓶颈。他们花了大约 1000 万来运行一个程序，而我希望它的总运行时间是 10 分钟！

但是在一些简单的性能改进之后，它们迅速成为程序中最快的过去，并且在不到 1 秒的时间内执行。

外卖？为 NLP 任务写快速代码真的很有用！它防止了产品瓶颈，并使您的代码更容易调试，因为您可以更快地接收程序输出。

在本文的其余部分，我将介绍五个步骤，您可以将它们应用到许多 NLP 风格的程序中，以提高它们的速度。我将使用 spacy 作为自然语言处理库来说明这些例子，因为它是我在我的相似文本程序中使用的，还因为它非常快——如果你正确使用它的话！

## 超级列表理解

所以，你可能已经知道列表理解比循环更快。如果没有，这里有一个关于如何写清单理解的简要回顾:

```
*# Traditional for loop* one_to_a_hundred = []
**for** x **in** range(1, 101):
    one_to_a_hundred.append(x)

*# List comprehension equivalent* one_to_a_hundred = [x **for** x **in** range(1, 101)]
```

但是实际上在这里你可以做更多的事情。列表理解语法非常强大，如果您充分利用它，您可以最大限度地提高代码的性能。

举以下例子:

```
*# Traditional for loop* evens_times_two_and_zeros_from_one_to_four = []
**for** x **in** range(1, 101):
    **if** x **in** [1, 2, 3, 4]:
        **if** x % 2 == 0:
            evens_times_two_and_zeros_from_one_to_four.append(x * 2)
        **else**:
            evens_times_two_and_zeros_from_one_to_four.append(0)

*# List comprehension equivalent* evens_times_two_and_zeros_from_one_to_four = [(x * 2) **if** x % 2 == 0 **else** 0 **for** x **in** range(1, 101) **if** x **in** [1, 2, 3, 4]]
```

明白我的意思吗？你可以在函数中工作，比如 times two，if else 子句比如测试 x 是否是偶数，另一个 if 子句看看 x 是否在列表`[1, 2, 3, 4]`中。

这里的语法非常强大，打开了许多大门，所以值得看看如何充分利用它。

## 尽可能使用发电机！

这是你会经常看到的事情，不是没有原因的。生成器极大地提高了代码性能，并大大加快了运行时间。

加快代码速度的一个非常直接的方法是使用生成器理解，而不是 for 循环。

这使用了与上面几乎相同的语法:

```
*# Traditional for loop* one_to_a_hundred = []
**for** x **in** range(1, 101):
    one_to_a_hundred.append(x)

*# Generator comprehension equivalent* one_to_a_hundred = (x **for** x **in** range(1, 101))

**for** number **in** one_to_a_hundred:
    print(number)
```

你实际上只是用普通的方括号代替了方括号。generator comprehension 返回的输出是一个 generator 对象，您可以一次迭代一个项目。

因为它们与列表具有不同的基础数据类型，所以您不能基于索引访问生成器对象中的项目，也不能对列表执行您可能喜欢的许多其他功能。

但是如果你有一个简单的用例，需要迭代输出——比如在预处理文本之后做一些事情——那么生成器可能是一个很好的选择！

例如:

```
*# Possible preprocessing pipeline* preprocessed_tests = (preprocess_function(x) **for** x **in** texts)

**for** text **in** preprocessed_tests:
    some_nlp_function(text)
```

## 使用缓存装饰器！

这听起来可能比实际复杂得多…

您可能已经通过浏览互联网了解了什么是缓存。当你打开一个网页时，你的浏览器会缓存有用的信息，这样当你返回到那个网页时，浏览器就不需要重新加载所有的内容，只需要加载新的内容。

因此，如果你在脸书上访问一个你经常访问的页面，你的浏览器可能已经缓存了该页面的一些基本布局。在这种情况下，它不必重新加载数据，而只需重新加载页面上的新帖子，这意味着它的工作速度会快得多。

如果在 python 函数中添加一个缓存装饰器，基本上就会发生这种情况。我不打算深究装饰器是什么——如果你感兴趣，这里有一篇[很棒的文章](https://realpython.com/primer-on-python-decorators/)——所以只要把它想成是在不改变函数本身的情况下向函数添加额外内容的一种很酷的方式。

实际上是什么样的呢？

```
**import** functools

@functools.lru_cache(maxsize=128)
**def** preprocess_texts(input_texts):
    *"""
    Generic preprocessing function.* **:param** *input_texts: tuple of input documents
    """
    # do some pre-processing*
```

很简单，对吧？您可以根据需要调整缓存大小。

注意，如果你向这个函数输入一个列表——也就是说，如果你输入了`preprocess_texts(input_texts=['a', 'b'])`,那么你会得到一个错误，因为列表不是一个可散列的类型，所以它必须是一个元组。

简而言之，这意味着由于列表是一种我们可以改变的数据类型——例如，通过添加或删除值——列表不能被转换成特定的字母数字标识符，也称为哈希。

functools decorator 要求函数的输入是可散列的，所以这里最好使用元组，因为一旦创建了元组，就不能添加或删除元素或以任何方式真正改变它。元组可能是 python 最顽固的数据类型，这也使得它们——你猜对了——是可散列的。

## 禁用不必要的 NLP 开销

这个例子是专门关于空间的，但也适用于其他库。

默认情况下，spacy 包含了很多很酷的东西，比如命名实体识别和依赖解析器。

但是这需要花费大量的时间*,你可能不需要它来完成一个简单的预处理任务，甚至是你可能正在做的许多其他任务。*

*您可以在 spacy 中很容易地改变这一点，但这里的主要教训是— *阅读文档*！*

*在你从某个很酷的新库中调用一个方法之前，确保你理解这个库在做什么，这样你就可以根据你的用例来调整它。*

*撇开恐吓不谈，你可以这样做:*

```
***import** spacy

text = **"Net income was $9.4 million compared to the prior year of $2.7 million."** nlp = spacy.load(**"en_core_web_sm"**)

*# Default* doc = nlp(text)
*# Modified* doc = nlp(text, disable=[**"tagger"**, **"parser"**, **"ner"**]*
```

## *加载预训练模型*

*这个问题很容易解决，但是我发现它是我运行时瓶颈的主要原因。*

*看到上面那行代码了吗— `nlp = spacy.load("en_core_web_sm")`？你做这个的时候要小心。*

*如果你把它放在你的函数中，你调用这个函数 100 次，那么你将会加载这个令人印象深刻但通常很重要的预训练模型，这个模型是这个库附带的 100 次。*

*然而，如果你把它放在你的脚本的顶部，并使 nlp 成为程序的一个全局变量，你只是做了一次…*

*因此，开始时的一些繁重工作会导致后来运行时间的大幅减少。*

## *结论！*

*这些只是一些简单的步骤，我采取这些步骤来改进我的程序的运行时间，使其长度满足生产需求。*

*概括来说，它们是:*

1.  *增强你的列表理解能力*
2.  *更好的是，使用生成器理解*
3.  *向函数中添加缓存装饰器*
4.  *根据您选择的库，禁用不必要的 NLP 开销*
5.  *考虑在程序中的什么地方加载预先训练好的模型*

*这里您可以做更多的事情，比如为运行时分析您的代码或者运行并行作业，但是希望这提供了一个简单且容易操作的起点。*