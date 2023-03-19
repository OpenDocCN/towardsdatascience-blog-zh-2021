# 列表理解 vs. For 循环:这不是你想的那样

> 原文：<https://towardsdatascience.com/list-comprehensions-vs-for-loops-it-is-not-what-you-think-34071d4d8207?source=collection_archive---------1----------------------->

## 许多关于堆栈溢出的文章、帖子或问题都强调列表理解比 Python 中的循环更快。它比这更复杂。

![](img/5bda92b81963d8ef47fbf31682bad611.png)

[信用](https://www.piqsels.com/en/public-domain-photo-oegux)

# 列表理解与 For 循环

通常的文章将执行以下情况:使用 for 循环创建列表，而不是列表理解。所以我们开始吧，计时。

```
import time
iterations = 100000000start = time.time()
mylist = []
for i in range(iterations):
    mylist.append(i+1)
end = time.time()
print(end - start)
**>> 9.90 seconds**start = time.time()
mylist = [i+1 for i in range(iterations)]
end = time.time()
print(end - start)
**>> 8.20 seconds**
```

我们可以看到，for 循环比 list 理解慢(9.9 秒对 8.2 秒)。

> 列表理解比循环创建列表更快。

但是，这是因为我们在每次迭代中通过**添加**新元素来创建一个列表。这太慢了。

旁注:如果它是一个 Numpy 数组而不是一个列表，那就更糟了。for 循环需要几分钟才能运行。但是你不应该为此责怪 for 循环:我们只是没有使用正确的工具。

```
myarray = np.array([])
for i in range(iterations):
    myarray = np.append(myarray, i+1)
```

> 不要通过在循环中追加值来创建 numpy 数组。

# For 循环比列表理解更快

假设我们只想执行一些计算(或者多次调用一个独立的函数)，而不想创建一个列表。

```
start = time.time()
for i in range(iterations):
    i+1
end = time.time()
print(end - start)
**>> 6.16 seconds**start = time.time()
[i+1 for i in range(iterations)]
end = time.time()
print(end - start)
**>> 7.80 seconds**
```

在这种情况下，我们看到列表理解比 for 循环慢 25%。

> For 循环比 list comprehensions 运行函数更快。

# 数组计算比循环快

这里缺少一个元素:什么比 for 循环或列表理解更快？数组计算！实际上，在 Python 中使用 for 循环、列表理解或。适用于熊猫。相反，你应该总是喜欢数组计算。

如您所见，我们可以使用 **list(range())** 更快地创建我们的列表

```
start = time.time()
mylist = list(range(iterations))
end = time.time()
print(end - start)
**>> 4.84 seconds**
```

只用了 4.84 秒！这比我们之前的列表理解(8.2 秒)节省了 40%的时间。此外，使用 list(range(iterations))创建一个列表比执行简单的计算(使用 for 循环需要 6.16 秒)还要快。

如果你想在一个数字列表上执行一些计算，最佳实践是**而不是**使用列表理解或 for 循环**来执行数组计算。**

> 数组计算比循环快。

# 结论

## 列表理解比循环理解快吗？

列表理解是创建列表的正确工具——然而使用 *list(range())* 更好。For 循环是执行计算或运行函数的合适工具。在任何情况下，都要避免使用 for 循环和 list comprehensions，而是使用数组计算。

# 奖金:Numpy Arange

用 **np.arange()** (6.32 秒)创建列表比用 **list(range())** (4.84 秒)慢 48%。

```
import numpy as np
start = time.time()
mylist = np.arange(0, iterations).tolist()
end = time.time()
print(end - start)
**>> 6.32 seconds**
```

但是，使用 np.arange()创建一个 numpy 数组比**先**创建一个列表并**然后**将其保存为 numpy 数组快一倍。

```
start = time.time()
mylist = np.array(list(range(iterations)))
end = time.time()
print(end - start)
**>> 10.14 seconds**
```

一旦你有了 numpy 数组，你就能进行闪电般的数组计算。