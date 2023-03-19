# 使用 Python 列表:备忘单

> 原文：<https://towardsdatascience.com/working-with-python-lists-a-cheatsheet-4e8c2858c324?source=collection_archive---------17----------------------->

![](img/651e27bfbb455859a7f786e795fd7ee4.png)

照片由[伯纳德·赫尔曼](https://unsplash.com/@bernardhermant?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## Python 列表的方法、功能和用例

写了几篇关于[计量经济学](/econometrics-techniques-for-data-science-ef4a880415b4)、[逻辑回归](/logistic-regression-from-statistical-concept-to-machine-learning-78691a4d20e1)和[正规化](/avoid-overfitting-with-regularization-6d459c13a61f)的文章之后——我又回到了基础！

许多复杂的数据科学算法都是用简单的构件构建的。你提升技能的速度很大程度上取决于你的基础有多强。在接下来的几篇文章中，我将触及一些这样的基础话题。希望学习这些话题能让你的旅程变得愉快有趣。

今天的话题是 **Python 列表**。

大多数人会先学习工具，然后用几个例子来实践它们。我走的是相反的路线——首先关注问题，在这个过程中学习解决问题的工具、方法和功能。因此，今天的文章分为以下几个小部分:

*   什么是列表以及如何创建列表
*   如何添加/删除元素
*   如何访问列表项，最后
*   对它们执行什么操作

## 什么是列表？

让我们从一个熟悉的例子开始——购物清单。

苹果、香蕉、牛奶、糖、盐——这是我可能会写在一张纸上或手机记事本上的购物清单。

现在你有了一个购物清单，你能用它做什么呢？事实上有几件事:

*   *加上*“鸡蛋”因为你忘了
*   购买时去掉“苹果”
*   如果你在水果区，进入列表的下一个项目

不管出于什么原因，如果您想将这个列表存储/导入到 Python 程序中，您需要以特定的方式对其进行格式化。以下是 Python 存储杂货的方式:

```
grocery_list = ["apple", "banana", "milk", "sugar", "salt"]
```

因此，列表只不过是以特定格式存储的信息，程序可以理解并处理这些信息。

有一些内置的方法和函数可以完成这些任务，我们将在本文中看到如何使用其中的一些方法和函数:

**列表方法:** `append(), extend(), insert(), remove(), pop(), clear(), index(), count(), sort(), reverse(), copy()`

**内置函数:** `sum(), min(), max(), len(), enumerate(), map(), filter(), lambda(), set(), sorted(), zip()`

## 让我们创建一个列表

当你在做一个项目时，你很可能会使用别人已经创建的列表。但是，由于我们是从零开始，所以让我们用任意数字构建一个新的:

```
# manually creating a list
mylist = [10, 12, 8, 6, 14]
```

有时候人们用 Python 内置的`range()`函数生成人工列表。

```
# generating a list 
list(range(0, 5)) #start 0, stop 5>> [0,1,2,3,4]
```

现在我们有了一个列表，让我们来玩它。

## 添加到列表中

有几种方法可以将新项目添加到列表中。用`append()`方法，你可以在列表末尾添加一个条目，99:

```
mylist = [10, 12, 8, 6, 14]
mylist.append(99)>> [10, 12, 8, 6, 14, 99]
```

或者使用`insert()`方法，您可以在您想要的位置添加 99——比如说在位置 0。

```
mylist = [10, 12, 8, 6, 14]
mylist.insert(0, 99)>> [99, 10, 12, 8, 6, 14]
```

如果您想将另一个列表[99，100]而不是单个元素添加到现有列表中，该怎么办？

```
mylist = [10, 12, 8, 6, 14]
mylist.extend([99, 100])>> [10, 12, 8, 6, 14, 99, 100]
```

总结一下，我们刚刚学会使用三种方法:`append(), insert(), extend()`以便在现有列表的不同位置添加新元素。

## 从列表中删除

学会了如何添加新物品，自然接下来的话题就是如何移除物品了。我们将使用两种方法— `pop()`和`remove()`。

`pop()`方法根据位置移除项目。例如，如果我们要移除位置为 0 的项目:

```
mylist = [10, 12, 8, 6, 14]
mylist.pop(0)>> [12, 8, 6, 14]
```

类似地，要移除位置 2 处的项目:

```
mylist = [10, 12, 8, 6, 14]
mylist.pop(2)>>  [10, 12, 6, 14]
```

也可以从对面选择位置。比方说，要删除倒数第二个位置的元素:

```
mylist = [10, 12, 8, 6, 14]
mylist.pop(-2)>> [10, 12, 8, 6, 14]
```

另一种移除物品的方法叫做`remove()`。它不是根据位置而是根据值来移除项目。它是这样工作的:

```
mylist = [10, 12, 8, 6, 14]
mylist.remove(8)>> [10, 12, 6, 14]
```

还有一件事，你也可以用一个新的值替换(即改变)列表中现有项的值。假设用 99 代替 10:

```
mylist = [10, 12, 8, 6, 14]
mylist[0] = 99>> [99, 12, 8, 6, 14]
```

最后，不管出于什么原因，如果你决定删除列表，也有一个方法可以实现这个目的——`del()`:

```
del mylist
```

## 访问列表元素

既然您已经知道了如何创建和编辑列表，下一步自然是如何处理它们。但是在执行任何操作之前，您需要先访问这些项目，对吗？到目前为止，我们使用的 5 个项目的列表很容易可视化，但是如果有数千甚至数百万个项目呢？如果你不知道你到底在找什么呢？

下面我们将看到一些如何使用索引位置来访问列表项的例子。

```
# access the first item
mylist[1]# grab the last item
mylist[-1]
```

如果它是一个嵌套列表(列表中的列表)，并且您需要从内部访问一个项目，该怎么办？

```
# to access number 33 from the list above
mylist = [0,1,[33,34],9,10]
mylist[2][0]>> 33
```

这里发生的事情很简单，你正在采取一种逐步的方法——首先访问恰好在位置 2 的[33，34],然后从这个列表中的位置 0 访问 33。

***切片:*** 除了访问单个元素之外，您可能还希望访问一系列项目——这个过程称为切片。切片的一般格式如下:

```
# start index : stop index : steps 
mylist[::]
```

让我们使用这种格式以各种方式分割列表:

```
mylist = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]# slice items from index 0 to index 10 with 2 stepsmylist[0:10:2]>> [0, 2, 4, 6, 8]
```

类似地:

```
# slice items from index 1 to end
mylist[1:]# items from the beginning to index 10
mylist[:10]# items from index 5 to 10
mylist[5:10]
```

# 方法和功能

到目前为止，我们已经完成了一些基础工作——创建、编辑和访问列表元素。但是这并没有解决任何问题，也没有为用户提供多少信息。

为了生成信息，我们需要对列表应用方法和函数(顺便问一下:你知道方法和函数的区别吗？如果没有，请查看这个[堆栈溢出帖子](https://stackoverflow.com/questions/155609/whats-the-difference-between-a-method-and-a-function)。

在开始时已经提到过，但这里列出了列表对象的所有方法:

**列表方法:** `append(), extend(), insert(), remove(), pop(), clear(), index(), count(), sort(), reverse(), copy()`

我们已经应用了上面的一些方法，剩下的你可以自己尝试看看它们是如何工作的。

让我们来看看 Python 的内置函数，它们在您的数据科学和分析之旅的每一步都非常有用。

**内置函数:** `sum(), min(), max(), len(), set(), sorted(), enumerate(), map(), filter(), lambda(), zip()`

上面的前 4 个函数是不言自明的，可以通过以下方式应用:

```
# get the sum of all elements
sum(mylist)# minimum/maximum value
min(mylist)
max(mylist)# total number of elements
len(mylist)
```

接下来的两种方法——`set(), sorted()`——也很有用。

`set()`只返回列表中唯一的值，所以如果有重复的值，它将只返回其中的一个。

```
mylist = [1,1,2,2,3,3,4,4]
set(mylist)>> [1,2,3]
```

`sorted()`另一方面将返回一个排序列表(从低到高或反之亦然等)。).

```
mylist = [2,1,3]
sorted(mylist)>> [1,2,3]
```

# **特殊功能**

Python 标准库还附带了一些特殊的函数。我们将检查其中的 4 个— `enumerate(), map(), filter(), lambda()`。

## 枚举()

给定一个列表`mylist = [‘hello’, ‘world’]`,`enumerate()`函数将找出每个项目的索引位置:

```
mylist = ['hello', 'world']
list(enumerate(mylist))>> [(0, 'hello'), (1, 'world')]
```

## 地图()

`map()` function 接受两个输入——一个函数和一个可迭代对象——然后对对象执行函数，并将输出作为可迭代对象返回。这是它的一般公式:

```
map(function, iterable_object)
```

让我们来看看如何一步一步地使用它:

```
# define a function
def squared(num):
    return num**2# input data
mylist = [1,2,3,4]# execute map() function
mymap = map(squared, mylist)# get the output
list(mymap)>> [1, 4, 9, 16]
```

## 过滤器()

类似于`map()`函数，`filter()`也接受一个函数和一个 iterable 对象。然而，它接受的函数只返回布尔值—真或假。然后，该函数检查 iterable 中的每一项，看它们是真还是假，最后返回一个过滤后的对象。

因此，例如，如果我们想要过滤一个列表的奇数[1，2，3，4，5]，那么`filter()`函数将返回[1，3，5]。这是它如何在引擎盖下一步一步地工作:

```
# create a boolean function
def is_odd(num):
    return num%2 != 0# input data
mylist = [1,2,3,4,5]# filter odd numbers
my_filter = filter(is_odd, mylist)# get the output
list(my_filter)>> [1, 3, 5]
```

## 希腊字母的第 11 个

您在编程中经常遇到的最后一个函数是`lambda`函数。通常，当我们编写一个函数时，我们给它一个名字，并在各种场合重复使用它。一个返回数字平方的普通函数如下所示:

```
def my_squar_func(num):
    return num ** 2
```

相比之下，lambda 函数没有名称，是在操作中使用的一次性函数。下面是我们如何将上面的`my_square_func()`转换成λ表达式:

```
lambda num: num**2
```

它有哪些用例？

在上面的`map()`和`filter()`函数中，我们必须遵循许多步骤才能得到最终的输出，因为我们必须首先创建函数。`lambda()`在这两种情况下都会派上用场。这里有两个使用案例:

```
# map() + lambda
mylist = [1,2,3,4]
mymap = map(lambda num: num**2, mylist)
list(mymap)>> [1, 4, 9, 16]# filter() + lambda
mylist = [1,2,3,4,5]
my_filter = filter(lambda n: n%2 != 0, mylist)
list(my_filter)>> [1, 3, 5]
```

# 摘要

总而言之，在本文中，我们已经讨论了相当多的内容:

*   定义和创建列表；
*   用`append(), insert(), extent()`方法添加新元素；
*   使用`pop(), remove()`方法移除元素；
*   通过索引和切片访问列表项；
*   应用 Python 的内置函数`enumerate(), map(), filter()`和`lambda`函数对列表进行操作。

在本文中，我遗漏了许多其他方法和函数，但是我们在这里使用的是经常使用的方法和函数。我希望这篇文章是有用的。如果你有意见，请随意写在下面。也可以在 [Medium](https://mab-datasc.medium.com/) 、 [Twitter](https://twitter.com/DataEnthus) 或 [LinkedIn](https://www.linkedin.com/in/mab-alam/) 上与我联系。