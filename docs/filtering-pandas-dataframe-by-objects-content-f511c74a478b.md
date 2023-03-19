# 根据对象内容过滤熊猫数据帧

> 原文：<https://towardsdatascience.com/filtering-pandas-dataframe-by-objects-content-f511c74a478b?source=collection_archive---------12----------------------->

## 如果你对筛选[熊猫](https://pandas.pydata.org/docs/user_guide/index.html#user-guide)数据框的许多方法感到困惑，这篇短文就是为你准备的！

![](img/26909d8f2979517722420bc57814dc63.png)

来自维基百科[的](https://en.wikipedia.org/wiki/Fram#/media/File:Fram_Model_1898-1902.jpg)[米尔科·葛军](https://commons.wikimedia.org/wiki/User:DrJunge)博士拍摄的*弗雷姆*模型。[根据 CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0) 获得许可。

首先，什么是[数据帧](https://pandas.pydata.org/pandas-docs/stable/user_guide/dsintro.html#dataframe)？DataFrame 是带有潜在不同类型的列的二维标记数据结构。这个概念类似于关系数据库中的表。也就是说，如果你只存储原子值。然而，作为一个 Python 库，DataFrame 很自然地将对象存储在其单元中。在关系数据库中，这通常被认为是一种不好的做法，因为它打破了第一范式，大大增加了 SQL 查询的复杂性。在 Python 中不是这样！本文用工作代码示例解释了在存储 Python 对象时不同的过滤方法是如何工作的。

## 使用掩码过滤标量数据

让我们看看如何从数据帧中选择数据的不同方法。

我们可以选择该数据帧中的行并进行投影(选择列)。行选择应该产生一个布尔掩码，它只是一个长度与 DataFrame 的行数匹配的布尔数组。我们将使用这个掩码，通过 DataFrame 的 **[]** 操作符来说明我们希望保留哪些行。

## 使用访问者

掩码通过操作符重载的魔力工作。通过对投影结果和一个标量值应用 **>** 操作符，我们自动映射一个一元函数，在前者上有一个布尔返回类型。这不适用于更复杂的数据类型。熊猫通过使用 [**访问器**](/pandas-dtype-specific-operations-accessors-c749bafb30a4) 对象来解决它。让我们看看字符串访问器 **str** 是如何工作的:

对于列表和字典等 Python 对象，Pandas 中提供的默认访问器可能还不够。虽然我们可以定义一个自定义的访问器，但是这样做可能不值得，因为有很多方法可以在没有访问器的情况下处理行。

让我们扔掉我们的玩具数据框架，创建一个新的适合为监督机器学习标记样本的任务。如果您已经做到了这一步，我将假设您熟悉这个用例。我们将创建一个 DataFrame，它可以存储关于哪个注释者用哪个标签标记了样本的信息。有了这个数据模型，很容易计算统计数据，比如注释者协议和 kappas！

哇，那里发生了很多事！让我们一起一点一点地熬过来。

通过使用 **iloc** 索引器选择一对熊猫 [**切片对象**](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#slicing-with-labels) (第一个用于行，第二个用于列)的标签列，开始屏蔽。 [**apply**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas-dataframe-apply) 函数对由 [**系列**](https://pandas.pydata.org/pandas-docs/stable/user_guide/dsintro.html#series) 对象表示的行(由**轴=1** 参数决定)进行迭代，并将一元函数映射到每一行。本质上是一个映射函数在一个列表上做的事情。我们将函数定义为 Python lambda 函数，没有什么特别的。在函数内部，我们访问每个单元格中的数据并测量其长度。为了创建行的过滤掩码，我们需要使用 Python **any** 函数连接每个单元格的结果。就是这样！

## 我们可能需要做更复杂的事情，这些事情不适合 lambda 函数。

没问题！创建您自己的接收行的函数，并自己处理它。我们可以使用这样一个函数来确定不同的注释者不同意哪一行。

## 我们还可以根据条件选择列。

允许它的巧妙技巧是 [**栈**](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.stack.html#pandas-dataframe-stack) 函数。与显式指定列相比，我们不仅可以选择整行，还可以根据对数据评估的一些条件来选择列。stack 函数旋转数据帧，使该列成为新的多级索引的一部分，并将该行拆分为具有单个值的多行。让我们把这个相当抽象的想法付诸实践。

到目前为止，我们存储的所有对象都是列表，但是我们也可以使用其他类型和自定义类。例如，让我们使用字典。

## 总之，

我们已经介绍了通过应用过滤掩码来过滤标量和 Python 对象上的 Pandas 数据帧。对于标量，我们通过使用重载操作符来创建它们，对于对象，通过使用内置的访问器和 *apply* 函数来创建它们。我们还描述了通过定义一个定制的过滤函数来基于列的动态组合进行过滤。最后，我们探索了通过*栈*函数使用旋转来动态选择列。

作为结束语，我想讨论一下在数据帧中存储对象的用处。在我看来，当建立一个关系数据库是多余的时候，它可能是一个合适的数据存储解决方案。这可能是典型的几百行长的用于机器学习的 Python 脚本，或者是一个快速而肮脏的 Flask 注释工具。从磁盘存储(和加载)数据帧只需要一次函数调用(例如*df . to _ CSV(‘文件名’)*)。我认为这是将这些数据存储在 Python 字典中的更好的替代方法，因为后续的查询更加优雅。使用字典时，查询很可能采用 for 循环的形式，这种形式比 DataFrame 查询可读性差，也更容易出错。通过为每一列指定一种数据类型，使用 DataFrames 还可以强化良好的数据模型设计实践。

*希望这篇文章能帮到你。欢迎任何意见和改进建议！*