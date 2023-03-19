# 获取股票基本面数据的简单方法

> 原文：<https://towardsdatascience.com/a-simple-way-to-get-a-stocks-fundamental-data-26506adf1214?source=collection_archive---------5----------------------->

## 我如何使用 Python 和 API 调用来检索公司的财务报表和收益报告

![](img/fdfad15546a8f633db7dad6f2e531b8d.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

获取或搜集股票数据有时说起来容易做起来难。找到包含适当的相关数据的正确资源或网站可能相当困难。在我的数据科学项目中，在过去几年中，我需要使用许多不同的数据集，其中相当一部分涉及股票数据和分析。

根据我的经验，股票的价格历史是最容易找到和检索的数据之一。通常，我会使用 Yahoo Finance 和附带的 Python 库来访问这些历史价格数据。然而，我发现对于一只股票的基本面数据来说，情况并非如此。

为了找到基本数据，比如财务报表和收益报告，你可能需要进行大量的谷歌搜索和烦人的网络搜索。在我之前的一个项目中，我幸运地发现了一个现已关闭的网站，名为 stockpup.com。这个网站包含了数千种股票的数千行基本面数据。它们都被整齐地组织成一个简单的 CSV 文件，供我下载并用于我的机器学习项目。

由于那个网站不再可用，我不得不在其他地方寻找数据…

> [在这里注册一个中级会员，可以无限制地访问和支持像我这样的内容！在你的支持下，我赚了一小部分会费。谢谢！](https://marco-santos.medium.com/membership)

# 财务数据 API

有许多金融数据 API 可以让你访问股票的基本数据。就我个人而言，我发现并使用了一个网站，它不仅能够提供基本数据，而且可以免费注册，这个网站叫做——[**【eodhistoricaldata.com】**](https://eodhistoricaldata.com/?ref=31CX3ILN&utm_source=medium&utm_medium=post&utm_campaign=a_simple_way_to_get_a_stock_s_fundamental_data)**，也被称为 EOD 高清。*披露:我从通过上面*的链接购买的任何商品中赚取一小笔佣金。**

**使用这个财务数据 API，我能够从数千家公司检索基本数据。数据已经准备好并可用，但现在我需要将它组织并格式化成我能够使用的熊猫数据框架。为此，我利用 Python 做好了一切准备:**

```
# Libraries
import pandas as pd
from eod import EodHistoricalData
from functools import reduce
from datetime import datetime, timedelta# Importing and assigning the api key
with open("../eodHistoricalData-API.txt", "r") as f:
    api_key = f.read()

# EOD Historical Data client
client = EodHistoricalData(api_key)
```

**有了我提供的 API 键和上面的代码，我就可以开始检索和格式化基础数据了。**

# **获取基础数据**

**我使用的 API 能够为我提供季度基础数据，如资产负债表、损益表、现金流和收益报告，但它们都整齐地存储在 API 的 return 对象中各自的部分。我需要创建能够访问的功能，然后将所有这些数据整合到一个大的数据框架中。**

**所以我创建了一个函数，它能够将这些独立的数据转换成它们自己的数据帧，然后将它们合并成一个大的 DF:**

**在这个函数中，我能够从给定的股票报价机中检索基本数据。如您所见，每个财务报表都存储在我需要访问的特定属性中。一旦我将它们存储到各自的数据框架中，我就可以通过将它们合并在一起来整合它们。我还删除了任何多余的列，比如在我删除它们之前一直重复的“ *Date* 列，以及任何重复的列。**

# **获取历史价格数据**

**我发现返回的基本面数据中缺少的一点是当时的股价。我认为这是一条重要的信息。API 也能够很容易地检索历史价格，所以这不成问题，但是我需要将这些价格与我上面创建的数据框架结合起来。**

**我创建了一个能够检索每日历史价格的函数，并将它们添加到更大的数据框架中:**

**当我最初将价格添加到数据框架中时，我发现了一个问题，即在较大的 DF 中，某些日期的一些价格数据丢失了。我假设这些日期有时是假日、周末或类似的日子。不管怎样，我只是想知道在财务报表报告日期前后的股票价格。它不需要很精确，事实上，它可能就在报告日期的前一天或后两天。**

**正如你在上面的函数中看到的，我可以填写周末、假期等。以之前的股价。之后，我将它们添加到最终返回的更大的数据帧中。**

# **获取基本面和价格数据**

**因为我能够创建两个函数来检索基本面和价格数据，所以我决定将它们压缩成一个函数:**

**在这个函数中，我合并了前两个函数。我还添加了额外的数据清理选项，提供了在更大的 DF 中删除空值的选择。如果您需要在此数据上训练机器学习模型，并且无法使用数据集中的 n a 值，则此选项可能会很有用。**

# **从多只股票中获取数据**

**前面的函数在从一个给定的股票行情自动收录器中检索基本数据时都很有用，但是如果我想从不止一只股票中检索数据呢？为此，我创建了以下函数，它允许从多只股票中检索基本面数据:**

**这是一个相对简单的函数，利用了上面的函数。我需要做的第一件事是交叉引用 API 中可用的代码。这样做是为了防止给定的 ticker 不存在或给得不正确。然后，如果给定的报价器是有效的，那么它将从所有给定的股票中检索基本面数据，并将它们组合成一个更大的 DF，其中包含多个股票的基本面数据。**

# **结束语**

**通过使用 Python 和这个金融数据 API，我能够轻松地检索几乎任何股票的基本数据。除了对格式化过程进行编码，整个任务相当简单。**

**随着多只股票的基本面数据准备就绪，我现在可以将这个数据集应用于任何未来的数据科学项目。可以对该数据集使用分类或回归 ML 模型。或者使用 Python 的众多可视化库进行简单的分析。有各种各样的项目可以使用这样的数据集。**

**[*另外在 eodhistoricaldata.com 出版*](https://eodhistoricaldata.com/financial-academy/extracting-the-knowledge-from-the-data/a-simple-way-to-get-a-stocks-fundamental-data/)**

## **开源代码库**

**[](https://github.com/marcosan93/Medium-Misc-Tutorials/blob/main/Stock-Market-Tutorials/Analyze-Fundamental-Data.ipynb) [## Medium-Misc-Tutorials/Analyze-Fundamental-data . ipynb at main Marcos an 93/Medium-Misc-Tutorials

### 一组随机的中等教程。为 Marcos an 93/Medium-Misc-Tutorials 开发做出贡献，创建一个…

github.com](https://github.com/marcosan93/Medium-Misc-Tutorials/blob/main/Stock-Market-Tutorials/Analyze-Fundamental-Data.ipynb)**