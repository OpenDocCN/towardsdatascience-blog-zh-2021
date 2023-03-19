# Python 的运算符模块的未知特性

> 原文：<https://towardsdatascience.com/the-unknown-features-of-pythons-operator-module-1ad9075d9536?source=collection_archive---------10----------------------->

## 借助 Python 鲜为人知的操作符模块，使您的代码更快、更简洁、可读性更强、功能更强大

![](img/a11767d52edf6d483978b75910415860.png)

[Bilal O.](https://unsplash.com/@lightcircle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

乍一看，Python 的`operator`模块似乎不是很有趣。它包括许多用于算术和二进制运算的操作函数，以及几个方便的帮助函数。它们可能看起来不那么有用，但是在这些函数的帮助下，你可以让你的代码更快、更简洁、更可读、功能更强。因此，在本文中，我们将探索这个伟大的 Python 模块，并充分利用其中包含的每个函数。

# 用例

该模块的最大部分由包装/模拟基本 Python 操作符的函数组成，如`+`、`<<`或`not`。当您可以只使用操作符本身时，为什么您需要或想要使用其中的任何一个可能不是很明显，所以让我们首先讨论所有这些函数的一些用例。

您可能希望在代码中使用其中一些的第一个原因是，如果您需要将运算符传递给函数:

我们需要这样做的原因是，Python 的操作符(`+`、`-`、…)不是函数，所以你不能直接把它们传递给函数。相反，您可以从`operator`模块传入版本。你可以很容易地实现包装函数来帮你做这件事，但是没有人想为每个算术运算符创建函数，对吗？另外，作为一个额外的好处，这允许更多的函数式编程。

你可能还会想，*我不需要* `*operator*` *模块来做这个，我可以只使用* `*lambda*` *表达式！。是的，但是这是你应该使用这个模块的第二个原因。这个模块中的函数比 lambdas 快。对于单次执行，您显然不会注意到这一点，但是如果您在循环中运行它足够多次，就会产生很大的不同:*

因此，如果你习惯于写类似于`(lambda x,y: x + y)(12, 15)`的东西，你可能想切换到`operator.add(12, 15)`来获得一点性能提升。

第三，对我来说，使用`operator`模块最重要的原因是可读性——这更多的是个人偏好，如果你一直使用`lambda`表达式，那么你可能会更自然地使用它们，但在我看来，一般来说在`operator`模块中使用函数比在 lambdas 中使用更具可读性，例如考虑以下内容:

显然，第二种选择可读性更强。

最后，与 lambdas 不同，`operator`模块函数是*可选择的*，这意味着它们可以被保存并在以后恢复。这可能看起来不是很有用，但是对于分布式和并行计算来说是必要的，这需要在进程之间传递函数的能力。

# 所有选项

正如我已经提到的，这个模块为每一个 Python 算法、按位和真值操作符以及一些额外的功能提供了一个函数。关于函数和实际操作符之间的映射的完整列表，参见文档中的[表。](https://docs.python.org/3/library/operator.html#mapping-operators-to-functions)

除了所有预期的函数之外，这个模块还具有实现操作的就地版本，例如`a += b`或`a *= b`。如果你想使用这些，你可以在基本版本前加上前缀`i`，例如`iadd`或`imul`。

最后，在`operator`中，你还会发现所有这些函数的*版本和*版本，例如`__add__`或`__mod__`。这些是由于遗留原因而出现的，没有下划线的版本应该是首选的。

除了所有实际的操作符之外，这个模块还有一些可以派上用场的特性。其中一个是鲜为人知的`length_hint`函数，它可以用来得到*迭代器长度的模糊*概念:

我想在这里强调一下*模糊的*关键字——不要依赖这个值，因为它实际上是一个*提示*并且不能保证准确性。

我们可以从这个模块中获得的另一个方便的函数是`countOf(a, b)`，它返回`b`在`a`中出现的次数，例如:

这些简单助手的最后一个是`indexOf(a, b)`，它返回`b`在`a`中第一次出现的索引:

# 关键功能

除了操作员功能和上述几个实用功能外，`operator`模块还包括用于处理高阶功能的功能。这两个键是`attrgetter`和`itemgetter`，它们通常与`sorted`或`itertools.groupby`等键一起使用。

为了了解它们是如何工作的，以及如何在代码中使用它们，让我们看几个例子。

假设我们有一个字典列表，我们希望通过一个公共关键字对它们进行排序。下面是我们如何用`itemgetter`做到这一点:

在这个代码片段中，我们使用了接受 iterable 和 key 函数的`sorted`函数。这个键函数必须是一个可调用的函数，它从 iterable ( `rows`)中提取单个项目，并提取用于排序的值。在这种情况下，我们传入`itemgetter`，它为我们创建了可调用的。我们还从`rows`中给它字典键，然后将这些键提供给对象的`__getitem__`，查找的结果用于排序。正如你可能注意到的，我们同时使用了`surname`和`name`，这样我们可以同时对多个字段进行排序。

代码片段的最后几行还显示了`itemgetter`的另一种用法，即查找 ID 字段中具有最小值的行。

接下来是`attrgetter`函数，它可以以与上面的`itemgetter`类似的方式用于排序。更具体地说，我们可以用它来对没有本地比较支持的对象进行排序:

这里我们使用`self.order_id`属性按照 id 对订单进行排序。

当与`itertools`模块中的一些函数结合使用时，上面显示的两个函数都非常有用，所以让我们看看如何使用`itemgetter`按其字段对元素进行分组:

这里我们有一个行列表(`orders`)，我们希望通过`date`字段对其进行分组。为此，我们首先对数组进行排序，然后调用`groupby`来创建具有相同`date`值的项目组。如果您想知道为什么我们需要先对数组进行排序，这是因为`groupby`函数通过寻找具有相同值的*连续*记录来工作，因此所有具有相同日期的记录需要预先分组在一起。

在前面的例子中，我们使用了字典数组，但是这些函数也可以应用于其他的可重复项。例如，我们可以使用`itemgetter`按值对字典进行排序，在数组中查找最小/最大值的索引，或者根据元组的一些字段对元组列表进行排序:

# 方法调用程序

需要提及的`operator`模块的最后一个功能是`methodcaller`。此函数可用于调用对象上的方法，方法使用以字符串形式提供的名称:

在上面的第一个例子中，我们主要使用`methodcaller`来调用`”some text”.rjust(12, “.”)`，它将字符串右对齐为 12 个字符，并用`.`作为填充字符。

使用该函数更有意义，例如，当您有一个所需方法的字符串名称，并希望反复向它提供相同的参数时，如上面的第二个示例所示。

使用`methodcaller`的另一个更实际的例子是下面的代码。在这里，我们将文本文件的行输入到`map`函数，并且我们还将我们想要的方法传递给它——在本例中是`strip`——该方法从每行中去除空白。此外，我们将结果传递给`filter`，它删除所有空行(空行是空字符串，它们是 *falsy* ，因此它们被过滤器删除)。

# 结束语

在本文中，我们快速浏览了一个被低估的`operator`模块(在我看来)。这表明，即使只有几个函数的小模块在您的日常 Python 编程任务中也会非常有用。Python 的标准库中有许多更有用的模块，所以我建议只需查看[模块索引](https://docs.python.org/3/py-modindex.html)并开始研究。您也可以查看我以前的文章，这些文章探讨了其中的一些模块，如 [itertools](/tour-of-python-itertools-2af84db18a5e) 或 [functools](/functools-the-power-of-higher-order-functions-in-python-8e6e61c6e4e4) 。

*本文原帖*[*martinheinz . dev*](https://martinheinz.dev/blog/54?utm_source=medium&utm_medium=referral&utm_campaign=blog_post_54)

</functools-the-power-of-higher-order-functions-in-python-8e6e61c6e4e4>  </the-correct-way-to-overload-functions-in-python-b11b50ca7336> [## Python 中重载函数的正确方法

towardsdatascience.com](/the-correct-way-to-overload-functions-in-python-b11b50ca7336) </scheduling-all-kinds-of-recurring-jobs-with-python-b8784c74d5dc> 