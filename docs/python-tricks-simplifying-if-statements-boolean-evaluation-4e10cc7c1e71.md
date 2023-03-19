# Python 技巧:简化 If 语句和布尔求值

> 原文：<https://towardsdatascience.com/python-tricks-simplifying-if-statements-boolean-evaluation-4e10cc7c1e71?source=collection_archive---------5----------------------->

![](img/72e97f91b0ed3292bec1ab4d8e6ae57b.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 布尔求值如何帮助缩短 if 语句？

欢迎阅读一系列短文，每篇短文都有方便的 Python 技巧，可以帮助你成为更好的 Python 程序员。在这篇博客中，我们将研究布尔运算。

## 情况

假设我们有一个变量`x`，如果满足某个条件，我们想做点什么。基本方法如下:

```
if x == condition:
    print("Conditions met!")
else:
    print("Conditions not met!")
```

使用运算符`==`，我们正在对`x`是否等于`condition`进行布尔运算。但是你知道吗，我们并不总是需要显式地调用一个布尔求值来使 if 语句工作。

## 数字类型的布尔计算

我们都知道 Python 中的`True`和`False`可以分别用整数表示为`1`和`0`。这意味着不用检查`x`是否是`1`，我们可以简单地做以下事情:

```
x = 1# This will return True
if x:
    print("True")
else:
    print("False")
```

事实上，除了`0`和`0.0`之外的所有整数和浮点数都将返回`True`作为布尔值。(是的，甚至`np.inf`和`np.nan`也被评估为`True`)

```
for x in range(-10, 11):
    print(f"{x}: {bool(x)}") # Only 0 will be Falseimport numpy as np
for x in np.arange(-10, 11, .5):
    print(f"{x}: {bool(x)}") # Only 0.0 will be Falseprint(bool(np.inf)) # True
print(bool(np.nan)) # True
```

## 无的布尔评估

从输入中提取信息的函数如果提取不出任何信息，返回`None`是很常见的。这方面的一个例子是`re.match`:

```
import rematch = re.match(r"Goodbye", "Hello, World!")
if match is None:
    print("It doesn't match.")
else:
    print("It matches.")
```

然而，当你有一堆`if x is
None`和`if x is not None`等的时候，这可能会令人困惑。这就是使用布尔评估和坚持一致的方法可以有所帮助的地方:

```
import rematch = re.match(r"Goodbye", "Hello, World!")
if match:
    # this block will run if match is **not** **None**
    print("It match.")
else:
    # this block will run if match is **None**
    print("It doesn't match.")
```

根据你的喜好，你可以把`if match`或者`if not match`作为你的 if 语句。这里的关键是在同一个代码库中坚持使用而不是混合使用这两种代码。此外，如果您使用的是 Python 3.9 或更高版本，还可以使用 Walrus 运算符来进一步简化上面的代码:

```
import reif match := re.match(r"Goodbye", "Hello, World!"):
    # this block will run if match is **not** **None**
    print("It match.")
else:
    # this block will run if match is **None**
    print("It doesn't match.")
```

## 可重复项的布尔求值

除了`int`、`float`、`None`之外，还可以将`dict`、`set`、`list`、`tuple`、`str`评价为布尔(注意`namedtuple`不在其中)。布尔求值在这些可迭代对象上工作的方式是使用它们内置的`__len__`方法。也就是说，iterable 的长度将被用作布尔值。对数字类型重新应用布尔求值逻辑，我们现在也知道了下面的特殊情况，其中 iterables 被求值为`False`:

```
print(bool(tuple())) # Falseprint(bool(dict()))  # Falseprint(bool(set()))   # Falseprint(bool(list()))  # Falseprint(bool(""))      # False
```

这意味着不做:

```
x = [0, 1, 2, 3]if len(x) > 0:
    print("The list is not empty")
else:
    print("The list is empty")
```

我们可以这样做:

```
x = [0, 1, 2, 3]if x:
    print("The list is not empty")
else:
    print("The list is empty")
```

## 类的布尔求值

现在我们知道了更复杂的数据结构的布尔求值的魔力依赖于内置方法`__len__`，我们可以将这一知识扩展到其他类。假设我们有一门课:

```
class Dummy:
    def __init__(self):
        self.__state = False

    def toggle(self):
        self.__state = not self.__state

    def __len__(self):
        return self.__state
```

如果我们运行下面的脚本，我们将看到类`Dummy`的布尔求值是如何变化的:

```
d = Dummy()bool(d) # Falsed.toggle()
bool(d) # Trued.toggle()
bool(d) # False
```

> 请注意，在更改`self.__state`后调用`__len__`将会抛出一个`TypeError`，因为`None`不能被转换为`int`类型，而`int`是`__len__`应该返回的类型。

## 结论

这篇博客讨论了布尔求值是如何工作的，以及它如何帮助简化 if 语句，并使代码库更加一致地可维护。概括一下，下面是 Python 中一些非布尔类型的布尔求值逻辑列表:

*   `int` : `False`如果`0`，否则`True`
*   `float` : `False`如果`0.0`，否则`True`
*   `dict`、`set`、`list`、`tuple`、`str`:如果长度为`0`，则为`False`，否则为`True`
*   自定义类:`False`如果`__len__`返回`0`，否则`True`

这篇博文就讲到这里吧！我希望你已经发现这是有用的。如果你对其他 Python 技巧感兴趣，我为你整理了一份简短博客列表:

*   [Python 技巧:扁平化列表](/python-tricks-flattening-lists-75aeb1102337)
*   [Python 技巧:对照单个值检查多个变量](/python-tricks-check-multiple-variables-against-single-value-18a4d98d79f4)
*   [Python 技巧:如何检查与熊猫合并的表格](/python-tricks-how-to-check-table-merging-with-pandas-cae6b9b1d540)

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

<https://www.linkedin.com/in/louis-chan-b55b9287> 