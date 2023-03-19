# Python 技巧:对照单个值检查多个变量

> 原文：<https://towardsdatascience.com/python-tricks-check-multiple-variables-against-single-value-18a4d98d79f4?source=collection_archive---------13----------------------->

![](img/92a61198f5ab33077a27fec0a05f3106.png)

照片由[克里斯里德](https://unsplash.com/@cdr6934?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 如何一次用一个值比较多个变量？

欢迎阅读一系列短文，每篇短文都有方便的 Python 技巧，可以帮助你成为更好的 Python 程序员。在这篇博客中，我们将研究变量比较。

## 情况

你有变量`x, y, z`，你也有一个常量`c`，你想检查它是否存在于三个变量中。我们可以按照下面的逻辑用蛮力的方法来做，但是有更好的解决方案吗？

```
if (x == c) or (y == c) or (z == c):
    print("It exists!")
else:
    print("It does not exist!")
```

## 可能的解决方案:迭代器

```
if any(i == c for i in (x, y, z)):
    print("It exists!")
else:
    print("It does not exist!")
```

如果您想检查是否所有的`x, y, z`都有价值`c`，您也可以将`any`改为`all`。这两个函数检查所提供的迭代器中是否有一个/全部被评估为`True`。这意味着如果我们有一个元组`t = (0, 1, 2)`，那么`all(t)`将返回`False`，因为第一个元素`0`将被计算为`False`。这也提供了很大的灵活性，因为像`0`、`None`、`[]`、`()`、`""`、`{}`这样的公共值都将被评估为`False`。

> 注意`x, y, z`已经被合并到一个元组中，而不是一个列表中，以获得更好的内存性能。如果您想了解更多关于数据结构的内存使用情况，我以前写过一篇文章:

[](/memory-efficiency-of-common-python-data-structures-88f0f720421) [## 常见 Python 数据结构的内存效率

### 你听说过内存过度分配吗？

towardsdatascience.com](/memory-efficiency-of-common-python-data-structures-88f0f720421) 

## 更好的解决方案:使用 Iterable 的成员测试

除了创建一个 tuple 并遍历它，我们还可以通过成员测试来完成它，而不需要使用`any`。

```
# Membership test with list
if c in [x, y, z]:
    print("It exists!")
else:
    print("It does not exist!")# Membership test with tuple
if c in (x, y, z):
    print("It exists!")
else:
    print("It does not exist!")# Membership test with set
if c in {x, y, z}:
    print("It exists!")
else:
    print("It does not exist!")
```

虽然`list`、`tuple`和`set`都支持成员测试，因为它们都是 Python 中的可迭代对象，但是在选择使用哪一个时还是有细微的差别。例如，假设元组中值的唯一性很高，使用`tuple`将占用三者中最少的内存。另一方面，`set`的实现允许常数成本成员测试，这意味着它在三者中具有最小的计算复杂度，尽管构建一个集合可能会超过它的好处。而对于`list`，在这种情况下使用变长数组没有任何优势，所以忘了它吧。

这篇博文就讲到这里吧！我希望你已经发现这是有用的。如果你对其他 Python 技巧感兴趣，我为你整理了一份简短博客列表:

*   [Python 技巧:拉平列表](/python-tricks-flattening-lists-75aeb1102337)
*   [Python 技巧:简化 If 语句&布尔求值](/python-tricks-simplifying-if-statements-boolean-evaluation-4e10cc7c1e71)
*   [Python 技巧:如何检查与熊猫的表格合并](/python-tricks-how-to-check-table-merging-with-pandas-cae6b9b1d540)

如果你想了解更多关于 Python、数据科学或机器学习的知识，你可能想看看这些帖子:

*   [改进数据科学工作流程的 7 种简单方法](/7-easy-ways-for-improving-your-data-science-workflow-b2da81ea3b2)
*   [熊猫数据帧上的高效条件逻辑](/efficient-implementation-of-conditional-logic-on-pandas-dataframes-4afa61eb7fce)
*   [常见 Python 数据结构的内存效率](/memory-efficiency-of-common-python-data-structures-88f0f720421)
*   [Python 的并行性](/parallelism-with-python-part-1-196f0458ca14)
*   [为数据科学建立必要的 Jupyter 扩展](/cookiecutter-plugin-for-jupyter-easily-organise-your-data-science-environment-a56f83140f72)
*   [Python 中高效的根搜索算法](/mastering-root-searching-algorithms-in-python-7120c335a2a8)

如果你想了解更多关于如何将机器学习应用于交易和投资的信息，这里有一些你可能感兴趣的帖子:

*   [Python 中用于交易策略优化的遗传算法](https://pub.towardsai.net/genetic-algorithm-for-trading-strategy-optimization-in-python-614eb660990d)
*   [遗传算法——停止过度拟合交易策略](https://medium.com/towards-artificial-intelligence/genetic-algorithm-stop-overfitting-trading-strategies-5df671d5cde1)
*   [人工神经网络选股推荐系统](https://pub.towardsai.net/ann-recommendation-system-for-stock-selection-c9751a3a0520)

[](https://www.linkedin.com/in/louis-chan-b55b9287) [## Louis Chan-FTI Consulting | LinkedIn 数据科学总监

### 雄心勃勃的，好奇的和有创造力的个人，对分支知识和知识之间的相互联系有强烈的信念

www.linkedin.com](https://www.linkedin.com/in/louis-chan-b55b9287) 

> 特别感谢 [Sander Koelstra](https://medium.com/u/9b0823a6e3a3?source=post_page-----18a4d98d79f4--------------------------------) 指出了一些误导性的陈述。