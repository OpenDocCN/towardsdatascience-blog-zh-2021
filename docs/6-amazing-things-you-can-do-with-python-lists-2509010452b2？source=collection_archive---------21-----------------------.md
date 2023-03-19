# 使用 Python 列表可以做 6 件惊人的事情

> 原文：<https://towardsdatascience.com/6-amazing-things-you-can-do-with-python-lists-2509010452b2?source=collection_archive---------21----------------------->

## 对您的脚本任务非常有用

![](img/77e31f95c10b21a405842c1c37b9a711.png)

照片由 [**扎米**](https://www.pexels.com/@zamisphere?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 发自 [**Pexels**](https://www.pexels.com/photo/man-in-mid-air-with-skateboard-2387325/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

我们在 Python 中使用的最常见的数据结构之一是**列表**,我们在 Python 中做的几乎每个项目中都使用这种数据结构。因此，了解一些与列表相关的技巧不仅会使您的工作更容易，而且有时它们比我们经常编写的简单代码更有效。

在这篇文章中，我将讨论 6 个使用 python 列表的技巧和诀窍，它们肯定会让你的生活变得更轻松。这些技巧包括 Python 中的一些内置函数和一些列表理解。

# **1。检索列表的某一部分**

假设我们想获取一个列表的某个部分，并从中创建一个新的列表。我们可以使用 Python 中的 *slice()* 方法来实现。该方法将起始索引值、结束索引值和增量顺序作为参数，并按该顺序检索元素。

```
list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]elements = slice(4 ,10 ,1)list2 = list1[elements]print(list2)
```

> **输出:**

```
[5, 6, 7, 8, 9, 10]
```

这个方法也可以用来反转列表，但是我们有另一个很酷的方法，我们将在本文的第三点看到。

# **2。对列表中的每个元素执行类似的操作**

如果您想对列表中的每个元素执行一组操作，可以使用 Python 中的 *map()* 函数。

您只需要编写包含这些操作的函数，然后使用 *map()* 函数，该函数将一个函数和一个 iterable(本例中为 list)作为参数，并对列表的所有元素执行操作。

> **例子**

```
mylist = [1,2,3,4,5]def multi(x):
    return 10*xlist(map(multi, mylist))
```

在这种情况下， *map()* 函数将用户定义函数 *multi()* 应用于`mylist[]`的所有元素并返回值。

> **输出**

```
[10, 20, 30, 40, 50]
```

# **3。倒序列表**

这是一个几乎所有 python 程序员都知道的相当常见的技巧，但是值得一试。在这种情况下，我们使用[]运算符，通过将第三个参数设置为-1 来反向遍历列表。

> **示例**

```
str=”strings are cool”print(str[::-1])
```

> **输出**

```
looc era sgnirts
```

> *想* ***敬请期待*** *同更多类似* ***精彩*** *文章上* ***Python &数据科学*** *—做会员考虑使用我的推荐链接:*[*【https://pranjalai.medium.com/membership*](https://pranjalai.medium.com/membership)*。*

# **4。同时迭代多个列表**

假设您有两个想要同时访问其元素的列表。Python 中的 zip()函数允许您使用一行代码来完成这项工作。该函数将多个列表作为参数，并同时遍历它们。当较小的列表用尽时，迭代停止。

zip 函数最棒的地方在于它可以用来同时遍历多个列表。

> **例子**

```
colour = [“red”, “yellow”, “green”]fruits = [‘apple’, ‘banana’, ‘mango’]price = [60,20,80]**for colour, fruits, price in zip(colour, fruits, price):
    print(colour, fruits, price)**
```

在这里，我们遍历了三个列表来打印其中的值。

> **输出**

```
red apple 60yellow banana 20green mango 80
```

# **5。连接字典中的列表**

当字典中的键值对的值是一个列表时，这个技巧会很有用。如果我们想要合并所有在字典中作为值出现的列表，我们可以借助内置的 sum()函数来完成。

> **示例**

```
dictionary = {“a”: [1,2,3], “b”:[4,5,6]}numbers = sum(dictionary.values(), [])print(numbers)
```

> **输出**

```
[1, 2, 3, 4, 5, 6]
```

# **6。将两个列表合并成一个列表**

假设您有两个列表，并且您想要合并这两个列表以形成一个字典，即一个列表中的元素将是键，而另一个列表中的元素将是值。使用 python 中的 zip()函数，我们只用一行代码就可以完成这项任务。

> **示例**

```
items = [“footballs”, “bats”, “gloves”]price = [100, 40, 80]dictionary = dict(zip(items, price))print(dictionary)
```

> **输出**

```
{‘footballs’: 100, ‘bats’: 40, ‘gloves’: 80}
```

# **结论**

这些是一些提示和技巧，你可以在处理列表时应用，使你的工作更容易。这些技巧不仅需要较少的代码行，其中一些对提高性能也有帮助。

除了这些技巧，您还可以使用列表理解来使您的代码更加紧凑和高效。在我们接下来的文章中，我将讨论更多与 python 相关的技巧和诀窍。

敬请期待！！了解更多与 Python 和数据科学相关的技巧。

> *走之前……*

如果你喜欢这篇文章，并希望**关注更多关于 **Python &数据科学**的**精彩文章**——请考虑使用我的推荐链接[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)成为中级会员。**

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。