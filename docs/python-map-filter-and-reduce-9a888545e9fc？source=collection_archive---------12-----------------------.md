# Python 映射、过滤和简化

> 原文：<https://towardsdatascience.com/python-map-filter-and-reduce-9a888545e9fc?source=collection_archive---------12----------------------->

## 使用 Map、Filter、Reduce 和 lambda 函数编写优雅而高效的代码

![](img/a3c6c1fc74f888170ce3439588c8e029.png)

雅各布·安德烈森在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在 Python 中，可以使用`def`来定义函数。另一种编写小功能的方法是使用 lambda。Lambda 函数是内联匿名函数。它只包含一个没有显式 return 语句的表达式。让我们看一些例子。

```
from time import time_nsdef cubed(x):
        return x**3lambda_cubed = lambda x:x**3start = time_ns()print(f"Cube of 9 is {cubed(9)}")print(f"Time taken for previous method : {(time_ns()-start)//1_000} ms")start = time_ns()print(f"Cube of 9 is {lambda_cubed(9)}")print(f"Time taken for previous lambda : {(time_ns()-start)//1_000} ms")start = time_ns()
print(f"Cube of 9 is {(lambda x:x**3)(9)}")
print(f"Time taken for inline lambda : {(time_ns()-start)//1_000} ms")
```

在前面的片段中，我们将比较一个函数和一个 lambda 函数计算 9 的立方所需的时间。我们可以给一个 lambda 函数命名。我们可以在片段中看到它们。如果我们运行它，我们会得到这样的结果。

```
Cube of 9 is 729
Time taken for previous method : 18 ms
Cube of 9 is 729
Time taken for previous lambda : 3 ms
Cube of 9 is 729
Time taken for inline lambda : 2 ms
```

每次执行代码片段时，这些数字可能会稍有不同。但是很明显，lambda 函数比它们的替代方法要快得多。

我们现在将看到如何使用 lambda 函数来进一步优化我们的代码。要了解关于 Python lambda 函数的更多信息，可以访问这个[链接](https://www.w3schools.com/python/python_lambda.asp)。

## 地图

我们可以使用 Python map 函数从另一个列表生成一个新列表。我们将比较循环，并映射生成一个包含 1–10 个方块的列表。

```
start = time_ns()
squares=[]
for i in range(1,11):
        squares.append(i**2)
print(squares)print(f"Time taken for a for loop to generate squares: {(time_ns()-start)//1_000} ms")start = time_ns()squares=list(map(lambda i:i**2,list(range(1,11))))print(squares)
print(f"Time taken for map and lambda function to generate squares : {(time_ns()-start)//1_000} ms")
```

上面代码片段的输出是…

```
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
Time taken for a for loop to generate squares: 12 ms
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
Time taken for map and lambda function to generate squares : 9 ms
```

## 过滤器

类似地，我们可以结合使用 filter 方法和 lambda 函数来从列表中过滤出条目。在下一个例子中，我们将使用循环和过滤方法创建一个从 1 到 10 的偶数列表。

```
start = time_ns()
evens=[]
for i in range(1,11):
        if i%2==0:
                evens.append(i)
print(evens)
print(f"Time taken for a for loop : {(time_ns()-start)//1_000} ms")start = time_ns()squares=filter(lambda i:i%2==0,list(range(1,11)))print(evens)
print(f"Time taken for filter : {(time_ns()-start)//1_000} ms")
```

如果我们比较每种方法的时间，我们会看到…

```
[2, 4, 6, 8, 10]
Time taken for a for loop : 6 ms
[2, 4, 6, 8, 10]
Time taken for filter : 4 ms
```

**减少**

Reduce 函数的工作方式类似于聚合函数。前面的方法`map`和`filter`是 python 内置的，但是我们需要导入`functools`来使用 reduce 函数。

```
from functools import reduceprint(reduce(lambda x,y:x+y, list(range(1,11))))
```

输出将是`55`。reduce 的工作方式是首先获取列表的前两个元素，并存储在`x`、`y`中。对 x 执行 lambda 函数后，y(本例中为 sum) reduce 函数将新值存储在 x 中。然后将下一个元素分配给 y，并将相同的函数应用于 x，y &结果存储在 x 中，依此类推。

没有必要将 lambda 函数与 map、filter 和 reduce 函数一起使用。这里有一个将一列数字作为输入的例子。

```
a=list(map(int, input().split()))
print(a)
```

如您所见，它们与预定义的 Python 函数配合得很好。

一旦您熟悉了这些功能，很容易发现这些功能会导致更短和优化的代码。虽然有时使用 for 循环可以使代码更具可读性。

但是这些功能的知识并没有被浪费。下次你准备在 Pandas 中预处理一个数据集时，了解 lambda 函数的概念将会使你受益。

点击 [**此处**](https://www.learnpython.org/en/Map,_Filter,_Reduce) 了解更多关于这些功能的信息。

希望你喜欢。敬请关注更多内容。