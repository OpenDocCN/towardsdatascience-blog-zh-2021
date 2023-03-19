# 健忘的熊猫🐼

> 原文：<https://towardsdatascience.com/forgetful-pandas-98b50c1193a9?source=collection_archive---------20----------------------->

## 熊猫的内存报告可能会误导-学习如何看到真正的使用和节省内存！

![](img/4d9b6ed49d34531301144468a4c4477b.png)

资料来源:pixabay.com

pandas 库是 Python 中用于数据清理、数据准备和数据分析的工具。一旦你找到了它的扩展 API，使用它是一种乐趣。🎉

Pandas 将您的数据存储在内存中，使操作变得敏捷！🏎不利的一面是，一个大的数据集可能不适合你的机器的内存，使你的工作陷入停顿。☹️

通常，知道你的熊猫数据帧占用了多少内存是很方便的。几年来，我一直在研究熊猫，并把它教给学生。我甚至写了一本关于入门的小书。我听说默认显示的内存使用并不总是准确的。🤔但是有如此多的数据需要探索，而时间又如此之少，以至于我最近才开始着手调查。我很震惊地了解到内存欠计数可以有多大。😲

在这篇文章中，我将向你展示几种方法来获得你的熊猫对象的真实内存使用情况。然后我会分享八个解决你的数据放不进内存的方法。🎉

![](img/402070c1fb7f03d21d852597f475d157.png)

小熊猫也很可爱。资料来源:pixabay.com

我用文本数据制作了一个 40 列 100，000 行的数据框架——仅仅是一些狗和猫品种的名字。🐶🐈没什么疯狂的。

让我们一起探索吧！🚀

## df.info()🔎

我总是使用`[df.info()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html)`来获得一个新数据帧的基本信息。下面是该方法的相关输出:`memory usage: 31.3+ MB`。

注意`+` 符号。嗯。你可能认为这意味着实际的内存使用比 31.3 MB 多一点——比如 32 或 33MB。保持这种想法。😉

## df.memory_usage()

我们用`[df.memory_usage()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.memory_usage.html)`函数看看能不能挖一点。

结果是以字节为单位报告每列内存使用情况的 Series 对象。我们把结果求和，换算成 MB。

`df.memory_usage().sum() / 1_000_000`结果在`32.8`

好吧。这比`df.info()`报道的要高一点。但是我们在同一个邮编。我可以忍受。但是等等，好像还有一个`deep`关键词。这是怎么回事？🤔我们来深入挖掘一下！

## df.memory_usage(deep=True)

让我们将`deep=True`参数传递给`df.memory_usage()`方法，求和并将结果转换为 MB。`df.memory_usage(deep=True).sum() / 1_000_000`导致`256.8` MB。这几乎是 8 倍的内存！😲

## 这是怎么回事？

与其他数据类型不同，对象数据类型列没有固定的内存分配，每个单元格的内存分配都是相同的。使用的内存量取决于字符的数量。更多的字符意味着更多的内存。有点像这样:

莫人物，莫记忆。资料来源:giphy.com

除了莫字符，莫记忆！

## 变量检查器

请注意，如果您使用方便的[变量检查器 JupyterLab 扩展](https://github.com/lckr/jupyterlab-variableInspector)，它也会显示不准确的 32.8 MB 大小。

## df.info(内存使用=真)

让我们回到`df.info()`。你可以把参数`memory_usage=True`传给它。然后它报告了一个接近`df.memory_usage(deep=True)` : `244.9 MB`的东西。

## 想让 df.info()每次返回更准确的数字？

使用`pd.options.display.memory_usage = 'deep'`。请注意，该设置不会影响`df.memory_usage()`的报告。

## 老式的 Python 系统函数说明了什么？

让我们检查一下。

```
import sys
sys.getsizeof(df) / 1_000_000
```

`sys.getsizeof()`函数报告`256.800016` MB，几乎与`df.memory_usage(deep=True)`相同。这个数字看起来可信。🎉

## 为什么真实内存总是不报？

清点人数需要资源。还有熊猫开发团队——顺便说一下，我们都应该感谢他们出色的工作👏—默认情况下，决定节省资源。

只是不要被误导！⚠️

![](img/e9faedf96d0425521fd6f2af9f72fb32.png)

熊猫可能健忘，也可能不健忘，但据报道，大象有极好的记忆力。资料来源:pixabay.com 和阿加莎·克里斯蒂

您已经看到了几种获得更准确内存信息的方法。酷毙了。但是当你想节省一些内存的时候应该怎么做呢？

# 我没有足够的内存，我该怎么办？

1.  **只读入你需要的栏目。** `pd.read_csv()`有一个`usecols`参数，可以用来指定列的子集。Pro move:提前检查您的文件，只引入您需要的列。😎
2.  **向下转换数值。如果你不需要所有的数字，节省的费用会很可观。从 float64 迁移到 float16 将会减少 75%的内存使用量！⭐️将`pd.to_numeric()`与`downcast`一起使用。**
3.  **将分类数据转换为类别数据类型。**根据`memory_usage()`的说法，将我们的宠物对象列更改为分类将内存占用减少到 4.8 MB。通过`deep=True`会导致显示的内存使用量略有增加。最大的问题是，对象数据类型占用的内存是分类数据类型的 50 多倍！😲如果转换整个数据帧，使用`df.astype(‘category’)`。
4.  **如果您有稀疏数据，请使用稀疏数组。**稀疏数据是主要具有相同值的数据。1000 万个 float64 值使用 80MB 内存。如果这些值中的大部分是相同的，那么在转换为稀疏数据时可以节省大量内存。使除了 10 个值之外的所有值都相同并转换为稀疏值会将内存降低到. 000248MB！👍用`pd.arrays.SparseArray()`转换成稀疏。
5.  **使用** [***生成器表达式***](https://www.python.org/dev/peps/pep-0289/) 代替*循环遍历列表以节省内存。*
6.  **用`[read_csv()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)`函数的`chunksize`参数一次读入数据行的子集**。这将创建一个熊猫 TextFileReader 对象。这就像一个 Python 生成器。然后，遍历一大块行，直到遍历完所有行。
7.  **检出**[**Dask**](https://dask.org/)**。**它提供分布式计算解决方案，将您的数据和处理分散到多台机器上。dask 系列包模仿了 pandas API 和其他常见的 Python 数据科学 API。
8.  **获取更多内存**——要么从亚马逊、谷歌或微软的云中租借，要么增加本地机器的内存。如果你在本地工作，8GB 通常可以应付，但 16GB 更好，尤其是如果你使用视频会议和浏览网页。🕸

你有其他节省记忆的方法吗？请在 discdiver 的评论或 [Twitter 上与我分享。📣](https://twitter.com/discdiver)

# 包装

现在你知道如何找到熊猫的真实内存使用情况了。您还看到了处理内存限制的方法。像往常一样，不要过早地进行优化——如果您的数据很小，这不是什么大问题。但是当你的数据很大的时候，这是一个非常大的问题。😀

我希望这篇获得准确的内存报告和节省内存的指南对你有所帮助！如果你有，请分享到你最喜欢的社交媒体上。🎉

我写关于数据科学、 [Python](https://memorablepython.com/) 、[熊猫](https://memorablepandas.com)和其他技术主题的文章。如果你对此感兴趣，请阅读更多内容并加入我在 Medium 上的 [15，000 多名粉丝。👍](https://jeffhale.medium.com/)

![](img/f752f920d0797d028065848e6fdef7e7.png)

大象也很可爱！资料来源:pixabay.com

快乐智能记忆使用！🧠