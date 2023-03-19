# 7 个方便的内置 Python 函数

> 原文：<https://towardsdatascience.com/7-handy-built-in-python-functions-3a32d2aa0301?source=collection_archive---------25----------------------->

## 你知道如何正确使用这些已经内置在 Python 中的函数吗？

![](img/b890d06b0afb44c2eaed8cfc33eada76.png)

苏珊·艾米丽·奥康纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 内置 Python 函数

对内置 Python 函数的认识是防止人们使用它们和错过解决编码问题的简单方法的最大障碍。我意识到，花时间复习这些功能来提高我自己的意识对我有好处。因此，这里有一些我最喜欢的快速解释和例子。

# 1.帮助()

这并不完全是您对内置编码函数的想法，但它可能仍然是最方便的。如果您不带任何参数运行`help()`,那么您将启动一个交互式帮助系统并得到以下响应:

```
Welcome to Python 3.6's help utility!

If this is your first time using Python, you should definitely check out the tutorial on the Internet at h[ttps://docs.python.org/3.6/tutorial/.](https://docs.python.org/3.6/tutorial/)

Enter the name of any module, keyword, or topic to get help on writing Python programs and using Python modules.  To quit this help utility and return to the interpreter, just type "quit".

To get a list of available modules, keywords, symbols, or topics, type "modules", "keywords", "symbols", or "topics".  Each module also comes with a one-line summary of what it does; to list the modules whose name or summary contain a given string such as "spam", type "modules spam".help>
```

底部还会出现一个框，供您键入需要帮助的内容。

如果你用一个参数运行帮助函数，那么你将得到关于那个东西的信息，可能是一个模块，函数，类，等等。例如，你可以输入`help(int)`，一个很长的 int 类可用方法列表就会出现。

我想大多数人都知道`help()`的存在，但是如果你像我一样，那么你会忘记使用它。每当我记得使用它时，我总能学到一些新的有用的东西或功能。

# 2.abs()

`abs()`函数返回一个数字的绝对值。如果我在一列数字中搜索，并且寻找那些满足一定数量阈值的数字，无论是正数还是负数，我会经常使用这个方法。这里有一个例子:

```
>>> A = [-3, -2, -1, 0, 1, 2, 3]
>>> [i for i in A if abs(i)>2][-3, 3]
```

# 3.圆形()

舍入确实如您所料，但是能够指定您想要舍入到的位置确实很方便。你知道你可以绕到消极的地方吗？有一个可选参数，用于在要舍入的小数之前或之后添加位数。如果该参数不包含任何内容，则默认值为 0，即四舍五入到最接近的整数。这里有一些例子来感受一下`round()`函数是如何工作的:

```
>>> round(12.345)
12>>> round(12.345, 1)
12.3>>> round(123, -1)
120
```

# 4.zip()

`zip()`函数可以将两个 iterables 中的项目配对成元组。例如，如果您有两个列表，您希望将它们组合起来以形成列表中的项目对。这是一个从我一直使用的列表中创建字典的便利技巧。如果你有一个包含你想要的键的列表和一个你想要的值的列表，那么你可以把它们压缩在一起，然后用`dict()`函数把它们转换成一个字典。这里有一个例子:

```
>>> a = ['hello', 'ketchup', 'macaroni']
>>> b = ['world', 'mustard', 'cheese']
>>> dict(zip(a, b)){'hello': 'world', 'ketchup': 'mustard', 'macaroni': 'cheese'}
```

# 5.枚举()

这一个类似于`zip()`和`dict()`的把戏，但是这一次也许你有一个条目列表，你想把它变成一个以索引号为关键字的字典。枚举使用项和索引号创建 iterable 对象的元组。然后你可以用`dict()`包装它，这样你就有了另一种简单的方法从列表中创建字典。下面是一个实际例子:

```
>>> weather = ['Clouds', 'Sun', 'Rain', 'Snow']
>>> dict(enumerate(weather)){0: 'Clouds', 1: 'Sun', 2: 'Rain', 3: 'Snow'}
```

# 6.地图()

`map()`函数有两个参数，一个函数和一个 iterable。它会将该函数应用于 iterable 中的每一项。我经常将它与 lambda 函数一起使用，作为对列表中的所有项目执行简单的小函数的简单方法。这里有一个例子:

```
>>> a = [1, 2, 3]
>>> list(map(lambda x: x**2, a))
[1, 4, 9]
```

我将整个事情包装在一个`list()`中，因为没有它，只会返回一个迭代器对象，而不是实际的值。

# 7.过滤器()

与上面的`map()`类似，`filter()`函数接受两个参数，一个函数和一个 iterable，但是你也可以将函数设置为 None，它将对项目进行简单的 truthy 或 falsy 检查。如果你传入一个函数，它需要返回布尔值真或假。无论哪种方式，唯一被归还的物品将是真实的。我有时会用这个从列表中过滤掉虚假的条目，只保留真实的条目。这里有一个例子:

```
>>> a = [0, 1, False, True, '', 'hello']
>>> list(filter(None, a))
[1, True, 'hello']
```

就像使用上面的`map()`函数一样，我将整个东西包装在一个`list()`中，因为如果没有它，只会返回一个迭代器对象，而不是实际的值。

# 结论

这些只是 Python 内置函数中的一部分，仔细阅读它们也让我对我经常使用的其他库利用这些函数的方式有了更深刻的认识。我在这里只列出了一些对我的编码工作有用的东西，但是你可以在 [Python 文档](https://docs.python.org/3/library/functions.html)中找到完整的列表。有太多需要探索和寻找的用例。保持冷静，继续编码！