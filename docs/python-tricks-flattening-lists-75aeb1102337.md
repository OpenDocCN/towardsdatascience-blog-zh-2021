# Python 技巧:扁平化列表

> 原文：<https://towardsdatascience.com/python-tricks-flattening-lists-75aeb1102337?source=collection_archive---------28----------------------->

![](img/fd53a1c6e5fe2631c65105eab7695ce7.png)

由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 你还在遍历 for 循环吗？

欢迎阅读一系列短文，每篇短文都有方便的 Python 技巧，可以帮助你成为更好的 Python 程序员。在这篇博客中，我们将探讨如何扁平化列表。

## 情况

我们都处理过列表的列表，甚至更糟:嵌套列表的列表。

理论上，我们可以像剥洋葱一样一层一层地剥列表:

```
l = [0, 1, 2, [3, 4, 5, [6, 7, 8]]]from collections import Iterabledef unwrap(l):
    flat_list = []
    for item in l:
        if isinstance(item, Iterable):
            flat_list.extend(item)
        else:
            flat_list.append(item)
    return flat_listl = unwrap(l)
# [0, 1, 2, 3, 4, 5, [6, 7, 8]]l = unwrap(l)
# [0, 1, 2, 3, 4, 5, 6, 7, 8]
```

这也意味着无论有多少嵌套层，我们都需要运行`unwrap`。

> 有没有更有效的方法来展平嵌套列表？

## 候选解决方案:itertools

`itertools`是 Python 的标准库之一，它提供了创建迭代器以实现高效循环的便捷函数。其中有一个名为`chain`的函数，它创建了**一个链式对象**来连接一个可迭代列表。链对象可以理解为遍历每个可迭代对象的迭代器。因此，如果你需要的不仅仅是一个迭代器，你就需要把它显式地转换成一个链表。

```
from itertools import chainl = [[0, 1], [2, 3]]
list(chain(*l))# [0, 1, 2, 3]list(chain.from_iterable(l))# [0, 1, 2, 3]
```

然而，`chain`也有一些限制:它连接了一个可迭代的列表，但是没有进一步展开，并且它还要求所有元素都是可迭代的:

```
from itertools import chainl = [[0, 1], [2, 3, [4, 5]]]
list(chain(*l))# [0, 1, 2, 3, [4, 5]]l = [0, 1, [2, 3, [4, 5]]]
list(chain(*l))# TypeError: 'int' object is not iterable
```

## 候选解决方案:熊猫

作为`itertools.chain`的替代，熊猫在`pandas.core.common`下有一个`flatten`功能。与`itertools.chain`类似，`flatten`返回一个**生成器**而不是一个列表。生成器和迭代器在实现和好处上有一些不同。虽然我们将在以后介绍这一点，但它们之间的一个简单的区别是，生成器是使用 yield 语句的迭代器，并且只能读取一次。

```
from pandas.core.common import flattenl = [0, 1, [2, 3, [4, 5]]]
list(flatten(l))# [0, 1, 2, 3, 4, 5]
```

从上面的例子中，我们可以看到`flatten`也像`chain`一样处理嵌套列表！

## 结束语

> 那么我们应该用哪一个呢？

看情况。

如果你有一个嵌套列表的列表，要么重构代码以避免创建那个怪物，要么使用`flatten`一次完全展平它。

如果你有一个列表列表，那么`chain`就足够了。毕竟`chain`也比`flatten`快

```
l = [[0, 1], [2, 3, 4], [5, 6, 7, 8]]%timeit list(chain(l))
# 236 ns ± 7.68 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)%timeit list(flatten(l))
# 5.13 µs ± 164 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
```

这篇博文就讲到这里吧！我希望你已经发现这是有用的。如果你对其他 Python 技巧感兴趣，我为你整理了一份简短博客列表:

*   [Python 技巧:如何检查与熊猫的表格合并](/python-tricks-how-to-check-table-merging-with-pandas-cae6b9b1d540)
*   [Python 技巧:简化 If 语句&布尔求值](/python-tricks-simplifying-if-statements-boolean-evaluation-4e10cc7c1e71)
*   [Python 技巧:对照单个值检查多个变量](/python-tricks-check-multiple-variables-against-single-value-18a4d98d79f4)

如果你想了解更多关于 Python、数据科学或机器学习的知识，你可能想看看这些帖子:

*   [改进数据科学工作流程的 7 种简单方法](/7-easy-ways-for-improving-your-data-science-workflow-b2da81ea3b2)
*   [熊猫数据帧上的高效条件逻辑](/efficient-implementation-of-conditional-logic-on-pandas-dataframes-4afa61eb7fce)
*   [常见 Python 数据结构的内存效率](/memory-efficiency-of-common-python-data-structures-88f0f720421)
*   [与 Python 并行](/parallelism-with-python-part-1-196f0458ca14)
*   [数据科学的基本 Jupyter 扩展设置](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72)
*   [Python 中高效的根搜索算法](/mastering-root-searching-algorithms-in-python-7120c335a2a8)

如果你想了解更多关于如何将机器学习应用于交易和投资的信息，这里有一些你可能感兴趣的帖子:

*   [Python 中交易策略优化的遗传算法](https://pub.towardsai.net/genetic-algorithm-for-trading-strategy-optimization-in-python-614eb660990d)
*   [遗传算法——停止过度拟合交易策略](https://medium.com/towards-artificial-intelligence/genetic-algorithm-stop-overfitting-trading-strategies-5df671d5cde1)
*   [人工神经网络选股推荐系统](https://pub.towardsai.net/ann-recommendation-system-for-stock-selection-c9751a3a0520)

[](https://www.linkedin.com/in/louis-chan-b55b9287) [## Louis Chan—FTI Consulting | LinkedIn 数据科学总监

### 雄心勃勃的，好奇的和有创造力的个人，对分支知识和知识之间的相互联系有强烈的信念

www.linkedin.com](https://www.linkedin.com/in/louis-chan-b55b9287)