# Python 中的地图函数

> 原文：<https://towardsdatascience.com/the-map-function-in-python-eb9a90707d17?source=collection_archive---------8----------------------->

## 了解如何使用 Python 中的地图功能

![](img/3fde19ceaccf4479be2ba214c1482220.png)

照片由[福蒂斯·福托普洛斯](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

尽管 Python 是一种面向对象的语言，但它仍然提供了提供函数式编程风格的函数。其中一个函数是内置的 ***map*** 函数，知道这个函数会非常有用。

在本教程中，我们将学习 Python 中的 ***map*** 函数是什么以及如何使用。

## 地图功能

假设您想要使用我们已经拥有的列表创建一个列表。这意味着我们希望使用现有的列表，对每个元素应用某种操作或函数，并使用这些输出来创建一个新的列表。

例如，如果我们有一个数字列表，我们想创建一个包含它们的方块的新列表。一种方法是使用 for 循环遍历数字列表，并应用一个函数返回每个数字或元素的平方。当我们遍历列表时，我们可以在新列表中添加或附加平方值。

让我们看看如何在代码中做到这一点:

> 我们有一个数字列表， **num_list** ，我们想要创建一个新列表， **num_list_squared** ，它包含了 **num_list** 的平方。我们使用 for 循环遍历 **num_list** ，并将每个数字的平方或 **num** 添加到我们的 **num_list_squared** 列表中。

另一种实现方式是使用名为 ***map*** 的内置 Python 函数。

[map](https://docs.python.org/3/library/functions.html#map) 函数接受两个参数:**我们想要应用的函数和我们想要应用它的 iterable 对象或序列(比如本例中的 list)。**换句话说， ***映射*** 函数将这个函数映射或应用到我们传入的序列的每个元素。

> 映射(函数，可迭代)

这个 map 函数将返回一个`map object`，它是一个迭代器。如果我们想从这个 map 对象创建一个列表，我们需要将 map 对象传递给内置的 list 函数，如下所示:

> list(map(function，iterable))

因此，如果 iterable 是一个由元素 **x** 、 **y** 和 **z** 组成的列表，那么 ***map*** 函数将完成以下任务:

> list(map(f，[x，y，z])→[f(x)，f(y)，f(z)]

> 函数 **f** ，是作为参数传递给**映射**函数的函数。

让我们看看如何使用内置的 ***map*** 函数来完成上面的代码:

*记住，我们可以将* ***映射*** *函数应用于任何可迭代对象或序列中的每个元素，而不仅仅是一个列表。*

**我们来分解一下这行代码发生了什么:**

```
num_list_squared = list(map(squared, **num_list**))
```

> **map** 函数从 **num_list** 中获取第一个元素，即 1，并将其作为参数传递给**平方**函数(因为我们将该函数作为第一个参数传递给了 **map** 函数)。**平方**函数然后返回 1 的平方，也就是 1，它被添加到我们的**地图**对象中。然后 **map** 函数从 **num_list** 中取出第二个元素，即 2，并将其作为参数传递给**平方**函数。**平方**函数返回 2 的平方，也就是 4，然后被添加到我们的**地图**对象中。在它完成了对 **num_list** 元素的遍历，并将我们剩余的平方数添加到 **map** 对象中之后， **list** 函数将这个 **map** 对象投射到一个列表中，该列表被分配给变量 **num_list_squared** 。

## 使用 lambda 表达式

我们可以通过传入一个 ***lambda 表达式*** 作为我们的函数来进一步缩短我们的代码:

*关于 lambda 函数的更多信息:*

[](/lambda-expressions-in-python-9ad476c75438) [## Python 中的 Lambda 表达式

### 如何用 python 写匿名函数

towardsdatascience.com](/lambda-expressions-in-python-9ad476c75438) 

**我们传入 *map* 的函数也可以是 Python** 中的内置函数。例如，如果我们有一个字符串列表，并且我们想要创建一个包含这些字符串长度的新列表，我们只需传入内置的 ***len*** 函数，如下所示:

```
list(map(len, **list_of_strings**))
```

*学习更多关于可迭代、迭代器和迭代的知识:*

[](/iterables-and-iterators-in-python-849b1556ce27) [## Python 中的迭代器和迭代器

### Python 中的可迭代对象、迭代器和迭代

towardsdatascience.com](/iterables-and-iterators-in-python-849b1556ce27) 

## 密码学:凯撒密码

让我们尝试一个稍微有趣一点的涉及密码学的例子，特别是凯撒密码。caesar 密码通过将消息中的每个字母替换为移位的字母或字母表中相隔指定数量空格的字母来加密消息。因此，如果我们选择空格数为 1，那么字母表中的每个字母将被替换为相隔 1 个空格的字母。所以字母 a 会被字母 b 代替，字母 b 会被字母 c 代替，以此类推。如果我们选择空格数为 2，那么 a 就用 c 代替，b 就用 d 代替。

如果我们在计算空格的时候到达了字母表的末尾，那么我们就回到字母表的开头。换句话说，字母 z 将被替换为字母 a(如果我们移动 1 个空格)，或者被替换为字母 b(如果我们移动 2 个空格)。

例如，如果我们要加密的消息是“abc”，我们选择空格数为 1，那么加密的消息将是“bcd”。如果消息是“xyz”，那么加密的消息将是“yza”。

[](/two-cool-functions-to-know-in-python-7c36da49f884) [## Python 中需要了解的两个很酷的函数

### 了解如何使用 Python 中的 tqdm 制作表格和显示进度条

towardsdatascience.com](/two-cool-functions-to-know-in-python-7c36da49f884) 

## **使用地图功能**

嗯，我们对一个可迭代对象的每个元素都应用了一些东西。在这种情况下，iterable 对象是一个字符串，我们希望用不同的字母/元素替换字符串中的每个字母/元素。而 ***地图*** 功能恰恰可以做到这一点！

> 让我们假设所有的消息都是小写字母。空格数将是 0-26 之间的一个数。请记住，我们只想用其他字母替换字母。因此，任何非字母元素，如空格或符号，将保持不变。

我们首先需要接触小写字母。我们可以写出一个全是小写字母的字符串，也可以使用如下的字符串模块:

```
**abc** = 'abcdefghijklmnopqrstuvwxyz'orimport string**abc** = string.ascii_lowercaseprint(**abc**)
# 'abcdefghijklmnopqrstuvwxyz'
```

然后，我们可以编写我们的 ***加密*** 函数如下:

```
def encrypt(**msg**, n):
    return ''.join(map(lambda x:abc[(abc.index(x)+n)%26] if x in abc else x, **msg**))encrypt('how are you?',2)
# 'jqy ctg aqw?'
```

> 我们用两个参数创建函数 **encrypt** :我们想要加密的消息， **msg** ，以及我们想要移动字母的空格数 **n** 。我们传递给映射函数的 iterable 是消息 **msg** 。我们传递给 **map** 函数的函数将是一个 **lambda 函数**，它从 **msg** 字符串中获取每个元素，如果元素是字母表中的一个字母，它将根据我们传递的 **n** 值用移位的字母替换它。它通过获取字母表中该字母的当前索引或 abc.index(x ),加上 **n** 值，然后获取该和的模。如果我们到达了末尾(如果 abc.index(x)+n 是一个大于 25 的数)，模数运算符用于从字母表的开头开始返回。换句话说，如果原来的字母是 z(它在我们上面创建的 **abc** 字符串中的索引是 25)，并且 **n** 的值是 2，那么(abc.index(x)+n)%26 最后将是 27%26，这将产生余数 1。从而将字母 z 替换为字母表中索引 1 的字母，即 b。

```
map(lambda x:abc[(abc.index(x)+n)%26] if x in abc else x, **msg**)
```

记住 ***贴图*** 函数会返回一个 ***贴图*** 对象。因此，我们可以使用字符串方法 join，它接受一个 iterable(***map***对象就是这样，因为它是迭代器，并且所有迭代器都是 iterable)，然后将它连接成一个字符串。然后，该函数返回该字符串。

要解密消息，我们可以使用下面的 **decrypt** 函数(注意我们是如何从 abc.index(x)中减去 **n** 而不是将其相加的):

```
def decrypt(**coded**, n):
    return ''.join(map(lambda x:abc[(abc.index(x)-n)%26] if x in abc else x, **coded**))decrypt('jqy ctg aqw?',2)
# 'how are you?'
```

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。*

[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership) 

*希望你喜欢这个关于 Python 中****map****函数的教程。感谢您的阅读！*