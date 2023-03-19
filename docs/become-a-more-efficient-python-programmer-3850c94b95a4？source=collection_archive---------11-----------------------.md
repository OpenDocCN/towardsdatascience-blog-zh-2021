# 成为更高效的 Python 程序员

> 原文：<https://towardsdatascience.com/become-a-more-efficient-python-programmer-3850c94b95a4?source=collection_archive---------11----------------------->

## 了解用 Python 创建列表和完成其他任务的最佳方式

![](img/1ad073c116530f2f4b9c8e9be0f3f5b1.png)

照片由 [Gema Saputera](https://unsplash.com/@gemasaputera?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

任何 python 程序员的目标都是编写可读性最强、计算效率最高的代码。在之前的教程中，我们介绍了用 python 创建列表的几种方法，包括使用 for 循环、map 和 filter 函数以及列表理解。但是哪一个可读性最强并且需要的执行时间最少呢？

在本教程中，我们将使用 Python ***timeit*** 模块，首先通过测量它们的执行时间来比较在 Python 中创建列表的所有三种方法的性能，包括 for 循环、map 函数和列表理解。

之后，我们将比较 reduce 函数和一些完成相同任务的内置或专用 Python 函数，并比较它们的执行时间。

*要查看 lambda 函数、map 和/或 reduce 函数和/或 list comprehensions，请查看:*

[](/three-functions-to-know-in-python-4f2d27a4d05) [## Python 中需要了解的三个函数

### 了解如何使用 python 中的映射、过滤和归约函数

towardsdatascience.com](/three-functions-to-know-in-python-4f2d27a4d05) [](/list-comprehensions-in-python-28d54c9286ca) [## Python 中的列表理解

### 用 python 创建列表的更优雅、更简洁的方式

towardsdatascience.com](/list-comprehensions-in-python-28d54c9286ca) 

# 创建列表

我们将创建一个包含从 0 到 9 的平方值的数字列表。为此，我们将创建三个函数，一个使用 for 循环，一个使用 map 函数，一个使用 list comprehensions。我们将在这些函数中用来循环的 iterable(我们可以循环的东西)将是一个 range 对象，它是由 python 中内置的 range 函数创建的。

在我们创建这三个函数之后，我们将使用 ***timeit*** 模块来测量所有三个函数的执行时间，以便衡量所使用的每种方法的计算效率。

我们开始吧！

## For 循环

创建这个列表的一种方法是使用 for 循环遍历 range 对象，并将平方值追加到新列表中。

这是我们如何用代码实现的:

```
def for_loop():
    squared_list = []

    for num in range(10):
        squared_list.append(num**2) return squared_list
```

> 注意我们是如何在 range 对象上循环的，这个对象是由 range 函数创建的，名为 **range(10)** 。这是一个可迭代的对象，我们可以使用 for 循环来遍历它，这将为我们提供从 0 到 9 的整数。我们也将在其他函数中使用这个相同的对象。

## 地图功能

制作这个列表的另一种方法是使用名为 map 的内置 python 函数。 [map](https://docs.python.org/3/library/functions.html#map) 函数接受两个参数:我们想要应用的函数和我们想要应用它的可迭代对象或序列(比如本例中的 range 对象)。换句话说，map 函数将这个函数映射或应用到我们传入的 iterable 对象的每个元素。

> *地图(函数，可迭代)*

我们可以使用下面的代码利用 map 函数创建上面的列表:

```
def map_func():
    return list(map(lambda num: num**2, range(10)))
```

> 我们使用一个 **lambda 函数**(匿名函数)作为我们传递给 map 函数的函数，当它遍历 range 对象时，它接受 num 并返回 num 的平方。
> 
> 记住，map 函数将返回一个 map 对象，它是一个迭代器。如果我们想从这个 map 对象创建一个列表，我们需要将 map 对象传递给内置的 list 函数。

## 列出理解

我们将编写的最后一个函数将使用 list comprehensions 来创建我们想要的列表。列表理解允许我们从其他序列(或可迭代对象)中以一种非常简洁但可读性很高的方式创建列表。

列表理解由括号组成，括号包含一个表达式，后跟一个 for 循环，以及零个或多个 for 或 if 子句。

> <iterable>中<name>的</name></iterable>

我们将使用 list comprehensions 遍历另一个 iterable 对象，特别是 range 对象，并添加从 0 到 9 的数字的平方，如下所示:

```
def list_comprehension():
    return [num**2 for num in range(10)]
```

## 性能比较

为了测量每个函数花费的时间，我们将使用 python 内置的[***time it***](https://docs.python.org/3/library/timeit.html)*模块。这个模块将为我们提供一种简单的方法来测量运行这些函数所花费的时间。*

*首先，我们将导入 ***timeit*** 模块。然后从那个模块我们将使用 ***timeit*** 函数。 ***timeit*** 函数接受多个参数；但是，我们将只传入一个，其他的使用默认值。*

> *timeit . timeit(*stmt = ' pass '*， *setup='pass'* ， *timer= <默认定时器>* ， *number=1000000* ， *globals=None* )*

*我们将传入的参数是我们想要运行的函数( *stmt* 参数)。我们可以用 *number* 参数指定我们希望该函数运行的次数，而 ***timeit*** 函数将返回总的执行时间。*

*我们将每个函数运行 100 万次(这是默认值)。下面是我们的代码和结果:*

```
*import timeitprint(f'Time for the for loop: {timeit.timeit(for_loop)}')
print(f'Time for map: {timeit.timeit(map_func)}')
print(f'Time for the list comprehension: {timeit.timeit(list_comprehension)}')**Results:****Time for the for loop: 3.5020802999999887
Time for map: 4.0746793999999795
Time for the list comprehension: 3.3324222999999904***
```

> *正如我们在上面看到的，理解一个列表花费了最少的时间，只有 3.33 秒。for 循环略微落后，为 3.50 秒，map 函数的执行时间最长，为 4.07 秒。*

> ***基于这些结果，以及列表理解通常比映射和过滤函数以及 for 循环更容易阅读和理解的事实，使列表理解成为我创建列表的首选方法。***

*[](/three-concepts-to-become-a-better-python-programmer-b5808b7abedc) [## 成为更好的 Python 程序员的三个概念

### 了解 Python 中的*和**运算符，*args 和**kwargs 以及更多内容

towardsdatascience.com](/three-concepts-to-become-a-better-python-programmer-b5808b7abedc)* 

# *减少功能*

*我们现在将比较 reduce 和使用 for 循环的执行时间。然后，我们将 reduce 与一些相应的内置或专用 Python 函数进行比较。*

## *Reduce 与 For 循环*

*我们将编写两个取数字 1 到 9 的乘积的函数。一个函数将使用 for 循环，另一个将使用 reduce 函数。*

## *带 For 循环的函数*

```
*def for_loop_prod():
    product = 1

    for num in range(1,10):
        product*=num

    return product*
```

> **我们要将 1 和 9 之间的数相乘，得到它们的乘积，这将由* ***range(1，10)*** *提供。我们创建变量* ***产品*** *并将其设置为 1。然后，我们使用 for 循环遍历 range 对象，并将每个数字乘以上一次迭代的结果。在循环数字 1-9 之后，* ***乘积*** *或累加器将等于 362880，我们将返回该值。**

*在我们编写将使用 reduce 的函数之前，让我们简单回顾一下 reduce 函数。*

## *什么是 reduce？*

*简单地说，reduce 函数接受一个 iterable，比如一个 list，或者在本例中是 range 对象，并将其缩减为一个累积值。reduce 函数可以接受三个参数，其中两个是必需的。两个必需的参数是:一个函数(它本身接受两个参数)和一个 iterable。第三个参数是一个初始化器，是可选的。*

> *reduce(函数，可迭代[，初始值设定项])*

*要使用 reduce 函数，我们需要按如下方式导入它:*

```
*import functoolsorfrom functools import reduce*
```

*reduce 接受的第一个参数，函数本身必须接受两个参数。Reduce 然后将这个函数累积地应用到 iterable 的元素上(从左到右)，并将其减少到一个值。*

*因此，我们可以使用 reduce 函数来计算数字 1–9 的乘积，如下所示:*

```
*def reduce_prod(): 
    return reduce(lambda x,y:x*y, range(1,10))*
```

> *我们的 iterable 对象是 range 对象。我们的**λ函数**接受两个参数， **x** 和 **y** 。Reduce 将首先获取 range 对象的前两个元素 1 和 2，并将它们作为参数 **x** 和 **y** 传递给 **lambda 函数**。**λ****函数**返回它们的乘积，即 1 * 2，等于 2。Reduce 随后将使用这个累加值 2 作为新的或更新的 **x** 值，并使用 range 对象中的下一个元素 3 作为新的或更新的 **y** 值。然后，它将这两个值(2 和 3)作为 **x** 和 **y** 发送给我们的 **lambda 函数**，然后返回它们的乘积 2 * 3 或 6。然后，这个 6 将被用作我们新的或更新的 **x** 值，range 对象中的下一个元素将被用作我们新的或更新的 **y** 值，即 4。依此类推，直到到达 range 对象的末尾。**换句话说，x 参数用累积值更新，y 参数从 iterable 更新。***

## *性能比较*

*我们将再次使用 ***timeit*** 函数来测量上述 for 循环和 reduce 函数的执行时间。*

```
*import timeitprint(f'Time for a for loop: {timeit.timeit(for_loop)}')
print(f'Time for reduce: {timeit.timeit(reduce_function)}')**Results:****Time for a for loop: 1.0081427000004624
Time for reduce: 1.684817700000167***
```

> *正如我们所看到的，与 reduce 函数相比，for 循环的执行时间更少。然而，在我们决定使用 for 循环来完成类似于获取 iterable 的乘积的任务之前，让我们看看专用函数在计算效率方面能提供什么。*

*[](/linear-algebra-for-machine-learning-22f1d8aea83c) [## 用于机器学习的线性代数

### 用于机器学习的线性代数概念综述

towardsdatascience.com](/linear-algebra-for-machine-learning-22f1d8aea83c)* 

## *减少与专用功能*

*我们将 reduce 函数与四个专用的 Python 函数进行比较: **prod** 、 **sum** 、 **max** 和 **min** 。 **prod** 函数实际上在 **math** 模块中，所以这是我们需要首先导入的唯一函数。*

*以下是接受 range 对象的乘积、总和、最大值和最小值的函数:*

```
***Product:**def reduce_prod(): 
    return reduce(lambda x,y:x*y, range(1,10))def math_prod():
    return math.prod(range(1,10))**Sum:**def reduce_sum(): 
    return reduce(lambda x,y:x+y, range(1,10))def builtin_sum():
    return sum(range(1,10))**Max:**def reduce_max():
    return reduce(lambda x,y: x if x > y else y, range(1,10))def builtin_max():
    return max(range(1,10))**Min:**def reduce_min():
    return reduce(lambda x,y: x if x < y else y, range(1,10))def builtin_min():
    return min(range(1,10))*
```

*以下是他们的执行时间:*

```
*print(f'Time for reduce product: {timeit.timeit(reduce_prod)}')
print(f'Time for math prod: {timeit.timeit(math_prod)}')print(f'Time for reduce sum: {timeit.timeit(reduce_sum)}')
print(f'Time for builtin sum: {timeit.timeit(builtin_sum)}')print(f'Time for reduce max: {timeit.timeit(reduce_max)}')
print(f'Time for builtin max: {timeit.timeit(builtin_max)}')print(f'Time for reduce min: {timeit.timeit(reduce_min)}')
print(f'Time for builtin min: {timeit.timeit(builtin_min)}')**Results:****Time for reduce product: 1.7645603999999366
Time for math prod: 0.5869173999999475****Time for reduce sum: 1.4824804999998378
Time for builtin sum: 0.4908693999998377****Time for reduce max: 1.6678851000001487
Time for builtin max: 0.591082900000174****Time for reduce min: 1.5096722000000682
Time for builtin min: 0.6109481999999389***
```

> *在所有情况下，专用 Python 函数的性能明显优于 reduce 函数。如果我们将数学 prod 函数与上面的 for 循环进行比较，prod 函数的性能要比 for 循环好得多。*

## *结论*

*看起来，如果 Python 中有一个专门的函数来做我们需要的事情，我们应该使用它，因为它很可能会比用 for 循环或 reduce 函数编写自己的代码给我们带来更好的性能。*

***注意:**我们可以通过传入[操作符](https://docs.python.org/3/library/operator.html)函数而不是 lambda 函数作为参数来获得更好的性能。例如，在上面的 sum 函数中，我们可以传入*操作符。add* 作为 reduce 的函数参数，这将比使用 For 循环提供更好的性能。*

*因此，如果内置或专用的 Python 函数尚不存在，那么 reduce 函数可以与关联运算符函数一起使用，如果这是一个选项的话。否则，一个编写良好的 for 循环对于可读性和性能来说是最好的。*

*如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [***链接***](https://lmatalka90.medium.com/membership) *报名，我会赚一小笔佣金。**

*[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership)* 

# *摘要*

*在本文中，我们看到了用 Python 制作列表的不同方法的性能:使用 for 循环、map 函数和列表理解。我们看到，列表理解通过具有最低的执行时间，在这些方法中提供了最大的可读性和最好的性能。接下来，我们看到了使用 for 循环、reduce 函数和一些内置函数的性能。我们发现内置函数大大优于 reduce 函数，并且在较小的程度上，使用 for 循环。总之，为了创建列表，我建议尽可能使用列表理解，而不是使用 reduce 函数，使用专用函数来获得更好的可读性和性能。*