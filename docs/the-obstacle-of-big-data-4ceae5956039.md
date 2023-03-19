# 大数据的障碍

> 原文：<https://towardsdatascience.com/the-obstacle-of-big-data-4ceae5956039?source=collection_archive---------31----------------------->

## [理解大数据](https://towardsdatascience.com/tagged/making-sense-of-big-data)

## 克服 1 亿多个数据点的技术

![](img/917a250e3755eb2da06cacb83eaec5c8.png)

[奥古斯都·伯纳姆·舒特](https://commons.wikimedia.org/wiki/File:Moby_Dick_p510_illustration.jpg)，公共领域，通过维基共享

在浏览纽约市[、旧金山](https://opendata.cityofnewyork.us/)、西雅图和西雅图 T10 的开放数据平台，寻找一个令我着迷的时间序列项目时，我无意中发现了一个庞然大物——一个如此庞大的数据集，以至于我怀疑自己(和我的电脑)处理它的能力。

一点背景知识:自然地，作为我的朋友讽刺地称之为*书头*的人，我被图书馆的数据所吸引，我开始想我是否可以建立一个预测未来图书馆活动的项目，即整个图书馆系统的借阅数量。令我沮丧的是，纽约公共图书馆(NYPL)和三藩市公共图书馆(SFPL)都没有任何关于借阅的数据；但是数据嚯！西雅图公共图书馆(SPL)有。并且做到了。

![](img/bb7bf1e69bc2457f6798d9df1fbf6a11.png)

NOAA 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 进入庞然大物

标题为[按标题检验(物理项目)](https://data.seattle.gov/Community/Checkouts-By-Title-Physical-Items-/5src-czff)的数据集描述如下:

> 该数据集包括西雅图公共图书馆的所有物理项目结帐的日志。该数据集从 2005 年 4 月的结账开始。不包括续费。

虽然感觉是很久以前的事了(但是时间到底是什么？)，我似乎想起了一种故意的无知，没有真正考虑这个数据集从 2005 年就开始了意味着什么。但是在那个决定命运的日子——2020 年 12 月 15 日——我点击了`Download`和`CSV`，发现自己等了几个小时才下载了一个 26.93 GB 的文件。这是一个令人大开眼界的数字，当然也是我见过的最大的数据集。

![](img/8793bfcd617c20886f9dea8efdb886a2.png)

由[格雷格·拉科齐](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 我可以看看吗？

不完全确定我会找到什么，我把 CSV 文件放到我项目的`Data`文件夹中，并试着把它加载到一个熊猫数据帧中。我给我的电脑开了一张安非他命的处方，然后走开了，以为它需要一段时间来加载。*一会儿*是轻描淡写；一段不确定的时间后(至少一个小时，也许两个小时，也许更久？)，当我的电脑风扇间歇地开到最大时，这个文件仍然没有加载。啊，那时我年轻多了；我甚至没有想过要记录我是从什么时候开始运行这个单元的。

此后，我构建了一个名为`status_update`的函数，它以字符串形式接收一条消息，并打印出带有当前时间戳的状态更新(您猜对了)。对于任何运行时间不可忽略的代码来说，这都是救命稻草。在处理大型数据集或复杂代码时，没有比不确定性更糟糕的感觉了。到目前为止，将时间跟踪器和质量检查整合到我的管道中对于这个项目来说是至关重要的，并且是我在未来项目中将永远保持的习惯。

用我的状态更新器武装起来，我让我的电脑整夜运行，到了早上，我仍然在黑暗中，看不到熊猫的数据帧。

![](img/ef40c9240ef5b75f1a1bd840e4658b0b.png)

照片由 [Dmitry Ratushny](https://unsplash.com/@ratushny?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 创造一个窥视孔

在我最终意识到我正在处理的野兽的本质之后，我意识到我需要查看一小部分数据，并围绕这些数据建立一个数据转换管道。我查阅了 Pandas [文档](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html),看看是否有任何方法可以限制我从原始 CSV 加载的行数和列数。当然有！鉴于这个数据集最终比我以前处理过的任何数据集都大整整三个数量级，我过去并不真的需要使用`nrows`或`usecols`或`skiprows`参数。下面是我第一次用来窥视数据的代码示例。

```
# load data
df = pd.read_csv(
    'data/Checkouts_By_Title__Physical_Items_.csv', # file path
    nrows=1000000 # number of rows to load
)
```

因为我还没有看到数据，所以我不确定哪些列我可以考虑*不*加载；然而，最终我会使用`usecols`参数，它允许您选择从数据源加载哪些列。还有一个`skiprows`参数，如果您想要加载特定的数据块，可以使用它。在这种情况下，我只想看到一些东西！所以前一百万行，包括所有的列，就可以了。

幸运的是，我立即注意到了大量多余的列，并且知道通过不加载这些列，我可以大大减少所有行的加载时间。我没有加载的列包括:

*   `ID` —生成的标识号，元数据存在于数据的其他地方。
*   `CheckoutYear` —已经包含在更需要的列`CheckoutDateTime`中的信息。
*   `BibNumber`、`CallNumber`和`ItemBarcode`——每个项目都有唯一的数字，这有助于确定项目的哪个版本最受欢迎，但我更简单地想看看最受欢迎的书籍/电影/项目*总体情况*，而不考虑版本。为此，我可以使用`ItemTitle`列。
*   `ItemType` —与数据字典配对时可能有用的列，但它包含的信息比`Collection`列少。

因此，从最初的 10 个专栏中，我只选择了 4 个— `Collection`、`ItemTitle`、`Subjects`和`CheckoutDateTime`。然后，我可以在构建管道的其余部分之前修改我以前的代码。

```
# list of columns to load
usecols = ['Collection', 'ItemTitle', 'Subjects', 'CheckoutDateTime']# load data
df = pd.read_csv(
    'data/Checkouts_By_Title__Physical_Items_.csv', # file path
    nrows=1000000, # number of rows to load
    usecols=usecols # specify columns to load
)
```

*剧透警告*:通过指定这 4 列，我的所有 106，581，725 行的加载时间从数小时减少到了 15 分钟，这是一个巨大的改进，尽管对于加载*原始*数据来说仍然有相当多的时间。

![](img/597dd536b5c8075a1f1d3282a054cf53.png)

照片由[约格什·佩达姆卡尔](https://unsplash.com/@yogesh_7?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 用管道绕过这个庞然大物

在实际加载所有数据之前，有必要使用这个较小的百万次观测数据块来为所有必要的数据转换构建一个管道。以下是我的管道流程的简要概述:

*   检查任何 NaN 值
*   将`CheckoutDateTime`列从字符串转换为 datetime 对象(对于任何时序项目都是必要的)
*   从[数据字典](https://data.seattle.gov/Community/Integrated-Library-System-ILS-Data-Dictionary/pbt3-ytbc)中合并项目信息
*   删除任何不必要的列
*   将任何可以合并到更合适类别中的值进行转换
*   保存清理后的数据集

我知道最大的任务之一是合并数据字典中的数据，数据字典包含主要用于探索数据(EDA)的重要信息，例如:

*   `Format Group` —项目是`Print`、`Media`、`Equipment`还是`Other`
*   `Format Subgroup` —更具体的分类，有 15 个类别，前三个是`Book`、`Video Disc`和`Audio Disc`
*   `Category Group` —通常是指项目是`Fiction`还是`Nonfiction`，尽管也包括其他类别
*   `Age Group` —一个项目是否被视为`Adult`、`Teen`或`Juvenile`

所有这些列都包含 string 对象，解析这些对象会占用大量内存和时间。经过一些研究，我找到了一个通过转换为熊猫分类对象的解决方案。

![](img/418dc74f1c9192c68a7b8cdde609b8e1.png)

丹尼尔·法齐奥在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 将字符串转换为分类

为了在数据操作和子集化过程中节省内存和时间，我在 Pandas 中使用了[分类](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Categorical.html)类。要阅读更多关于分类类如何提高内存使用(以及其他好处)，请参考[文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/categorical.html#categorical-memory)。基本思想是，我可以有`n`个类别，而不是一个包含 1 亿多个字符串的列。在我的例子中，上面的列的值从 3 到 15 不等，明显少于 1 亿+百万。

*剧透警告*:我已经计算了分类数据和字符串数据的每一列的大小，发现分类数据正好是字符串数据大小的八分之一*(12.5%)！考虑到所有这四列，在合并过程中，以及在数据转换过程和随后的 EDA 过程中，我节省了超过 3GB 的内存和大量时间。*

## *一切都被 i̶l̶l̶u̶m̶i̶n̶a̶t̶e̶d̶拉长了*

*随着我的管道或多或少地完成(必须始终为进一步的优化和改进留出空间)，我准备好加载所有必要的数据并通过管道发送它。很快就清楚了，处理大数据不仅仅取决于加载原始数据需要多长时间；对于这种量级的数据集，一切都需要更长的时间。甚至检查 NaN 值也花了我将近四分钟。将日期列从字符串转换为 datetime 对象需要六分多钟。*

*从数据字典中合并数据的速度惊人地快，只需 4 分钟，但是删除合并所需的列(但之后就没用了)却花了 45 分钟。我无法在熊猫身上找到一种更快的方式来完成这项工作，尽管可能有一些优化的方法，我将在下面讨论。多亏了 NumPy，所有其他数据转换几乎都是瞬间完成的。*

*这让我想到了最后一个障碍…*

*![](img/43aba67e63ab3977d7d4aa1e837b1276.png)*

*[伊万·拉季奇](https://commons.wikimedia.org/wiki/File:Floppy_disk_(back).jpg)，[由 2.0](https://creativecommons.org/licenses/by/2.0) 抄送，经由维基共享*

## *储蓄的问题是*

*也许我面临的最大的障碍，也是我需要做更多工作的事情，实际上是保存我最终清理和转换的数据集。出于各种原因，包括稳定性和内存节省，我决定将数据集保存为压缩文件。尽管如此，即使我的管道在所有其他步骤上都工作得很好，我还是遇到了将数据保存在一个包中的问题。内存使用仍然被超过，我的内核在最后一个关键步骤上一直崩溃。*

*我现在的解决方案是分批保存 1000 万行，总共 11 个文件。非常大的缺点是这个过程需要大约 8 个小时！好的一面是，我以前大约 27GB 的数据现在是干净的，总共只有 3GB。加载现在干净的数据仍然需要一段时间(大约 20 分钟)，但我认为这是煮咖啡和吃早餐的时间。*

## *重要的一点*

*我应该注意，这个非常大的数据集需要很长时间保存和加载，从某种意义上说，它是可选的。它只对项目的探索性数据分析部分重要，对我来说，这确实非常重要。然而，实际的时间序列数据可以通过对您想要的任何类别列以及最重要的目标变量`total_checkouts`进行虚拟化和求和来几乎即时地创建(在上述数据转换之后)。这些数据可以放入一个总计 277KB 的 Pickle 文件中。*

## *总是有更多需要优化*

*根据您的用例，执行通常快速的任务(当处理较小的数据时)所花费的大量时间可能是不可接受的。对于我的个人项目，在构建了包含前 100 万行的数据转换管道之后，整夜在整个原始数据集上运行脚本并保存一个现在可以轻松访问的干净的数据帧并没有什么大不了的。*

*这种“多余的时间”很大程度上可以归因于熊猫在处理数据时只使用一个单核。我对一个名为[摩丁](https://github.com/modin-project/modin)的库很感兴趣，它可以并行化你的数据，并利用你计算机的所有内核。不幸的是，它目前在熊猫的最新版本中无法使用，所以降级是必要的。 [Dask](https://docs.dask.org/en/latest/) 和 [PySpark](https://spark.apache.org/docs/latest/api/python/index.html) 是我正在考虑的其他工具，尽管 Dask 的早期实验表明，在数据转换过程中，性能的提高可以忽略不计(有时甚至为零)。例如，检查 NaN 值也需要同样长的时间。*

*到目前为止，我已经有了可以运行的清理过的数据，但是在将来，让我的管道更加高效仍然是有益的。到目前为止，通过解决这些问题，我明白了总是有更多的工作要做，而且不是所有的野兽都能被驯服(或杀死)。*

*![](img/32cf22236cad11d2b10af99198efdcaa.png)*

*Gustave Doré ，公共领域，通过维基共享*

## *访问我的项目存储库*

*虽然我仍然处于早期(ish)阶段，但这篇文章中详细描述的过程，以及我迄今为止的 EDA，可以在我的项目的[库](https://github.com/p-szymo/library_usage_seattle)中看到。作为奖励，我最近执行了该项目的第一次 API 调用，以便获得 2020 年的剩余数据，然后我在管道中运行并轻松清理这些数据。*