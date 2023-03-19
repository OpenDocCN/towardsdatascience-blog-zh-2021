# Python 初学者突破(顶级 Python 内置函数)

> 原文：<https://towardsdatascience.com/python-beginner-breakthroughs-top-python-built-in-functions-19a6d978035b?source=collection_archive---------24----------------------->

## 为什么要重新发明轮子？Python 有许多内置函数，可以做一些你目前正在思考的事情

![](img/943f1322469972075cca2b9c0d519339.png)

serjan midili 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Python 本身有许多内置函数，每个想熟悉这种编程语言的人都应该知道。在本文中，我将重点介绍最重要的几个，您可以用它们来开发将为您的机器学习或数据科学项目提供动力的代码。你是否经常发现自己在编码，想做一些看似简单的事情，但当你开始尝试并把它打出来时，你会想‘肯定有更好的方法’。如果您遇到了这种情况，而且这个概念实际上非常简单，那么可能有一种更简单的方法，Python 内置函数。下面的列表没有特定的重要性顺序，但是我想强调 Python 必须提供的一些最常用的内置函数。

Python 的一个优点是，正如函数的命名一样，它们的名字很大程度上暗示了它的功能。下面我将函数分成三组:基本、数据类型相关和高级

![](img/930c959948076b38d614afcbcd6add11.png)

照片由[猎人哈利](https://unsplash.com/@hnhmarketing?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 基本功能:

## abs()

顾名思义，abs()函数返回一个数字的绝对值。输入可以是各种数字输入。

## 任何()

any()函数的工作原理是，如果项目是 iterable，则返回 True/False。这可以用来确定数据是否来自序列/可迭代类型。

## len()

这可能是 python 中最常用的函数之一，len()函数返回对象中元素的数量。因此，如果你的列表中有 5 个条目，len()函数将返回 5。

```
# Examples of Basic Functions
# abs() function
x = -24
abs(x)
24

# any() function
y = [0, 1, 2, 3, 4]
any(y)
True

# len() function
len(y)
5
```

## 最大()

另一个真正常用的函数，顾名思义，返回 iterable 的 max 元素(如果你只提供一个参数)。您还可以提供另外两个仅包含关键字的参数:key 和 default。使用像 max(list，key=len)这样的 max()函数将返回列表中最大长度(或最长)的元素。

## 最小值()

min 函数执行的功能与 max()函数相同，但方向相反。类似地，还有一些附加的仅关键字参数可以用来使函数更加可定制。

## 打印()

每个人都在 Hello World 的第一天练习中学习了这个函数，它允许用户在文本流中显示项目。

```
# max() function
l = [1, 4, 7, 21, 35]
m = [[1, 2, 3], [1, 2], [0, 3, 2, 1]]
max(l)
35
max(m, key=len)
[0, 3, 2, 1]

# min() function
l = [1, 4, 7, 21, 35]
m = [[1, 2, 3], [1, 2], [0, 3, 2, 1]]
min(l)
1
min(m, key=len)
[1, 2]

# print() function
x = 'hello world'
print(x)
hello world
```

## 圆形()

根据您希望如何表示数字，round()函数可以将数字舍入到指定的小数点(带有附加参数)

## 总和()

最后一个基本函数是 sum()，顾名思义，它将接受一个 iterable 并将元素相加。您还可以添加一个额外的参数来指定起点。

```
#round() function
x = 24.3452
round(x)
24
round(x, ndigits=2)
24.35

# sum() function
z = [1, 3, 5, 10, 51]
sum(z)
70
```

![](img/b89c2d50ef2fb88f7bf0a9e7db1af6b1.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 高级功能:

## 枚举()

enumerate()函数是一个非常有用的函数，它有助于获取一个序列(比如一个列表)并获取所有相应索引和元素的元组对。您可以使用附加的 start 参数定义第一个元组元素(编号)的起始编号。注意 enumerate()的输出是一个 enumerate 对象，您需要将它转换为列表或其他类型才能看到它。

## 过滤器()

顾名思义，filter()函数将根据函数参数是否为 true 来筛选 iterable。参数是 function(将应用于迭代器的函数)和 iterable(可以是任何 iterable 或序列类型)。注意，如果过滤器参数很简单并且在整个代码中不可重用，那么处理它的一个常见方法是使用 lambda 函数。

```
#enumerate() function
k = ['first', 'second', 'third', 'fourth']
z = enumerate(k)
print(list(z))
[(0, 'first'), (1, 'second'), (2, 'third'), (3, 'fourth')]

# filter() function
p = [1, 2, 3, 4, 5]
q = filter(lambda x: x % 2 == 0, p)
print(list(q))
[2, 4]
```

## 地图()

map()函数为 iterable 中的所有项计算给定函数，并输出相应的迭代器。例如，本质上，一个列表整数将用一个函数来计算，该函数将对该项求平方，并产生一个包含所有平方整数值的新列表。

## 已排序()

与许多更高级的函数一样，这个函数是帮助以所需格式表示序列类型数据的关键。sorted()函数正如其名所示，对数据进行排序。还有一些额外的参数可以用来构建一个更高级的排序逻辑。您提供一个键作为排序依据，并定义 reverse = True 或 False，以反转排序的方向。请注意，该函数适用于数字和字符串类型的数据

## zip()

使用 zip()函数可以处理多种序列类型并希望将它们组合在一起。就像你想象的衣服上有拉链一样，它有两个可重复项，并以拉链的方式将它们结合起来。它的方法是提供两个可迭代对象作为输入，然后返回一个元组迭代器。需要注意的一点是，你应该小心不同长度的可重复项，因为较长的项可能会产生问题。一件有趣的事情是，您也可以通过使用函数调用 zip(*zipped_item)来解压缩。

```
# map() function
z = [0 , 2, 5, 21]
y = map(lambda x: x ** 2, z)
print(list(z))
[0, 4, 25, 441]

# sorted function
a = ['r', 'd', 'a', 'b', 'z']
print(sorted(a))
['a', 'b', 'd', 'r', 'z']
print(sorted(a, reverse=True))
['z', 'r', 'd', 'b', 'a']

# zip() function
number_list = [1, 2, 3]
str_list = ['one', 'two', 'three']
zipped = list(zip(number_list, str_list))
print(zipped)
[(1, 'one'), (2, 'two'), (3, 'three')]
unzipped = list(zip(*zipped))
print(unzipped)
[(1, 2, 3), ('one', 'two', 'three')]
```

![](img/ffa3b2bc298eaa2ce7bfc688b489347f.png)

弗兰基·查马基在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 相关数据类型:

## 类型()

type()函数最简单的形式是返回提供给它的对象的类型。使用类型()的 3 个参数创建一个新的类型对象。参数是 name(一个类名)、bases(一个逐项列出基类的元组)和 dict(一个包含类主体定义的名称空间的字典)。

## int()

int()函数返回所提供的数字或字符串的整数对象。请注意，对于浮点数，它将向零截断。

## str()

与 int()类似，当给定一个对象作为输入时，str()函数返回一个字符串对象。

## 字典()

dict()函数根据提供的输入创建一个新的字典。

```
# type() function
x = 'string'
type(x)
str
x = 2.14
type(x)
float

#int() function
p = 2.21
int(p)
2

# str() function
z = 21
str(z)
'21'

# dict() function
z = dict()
print(z)
{}
# note there are many ways to build dictionaries
```

**总结**

Python 中有大量已经构建的函数，可以执行一些基本的数据操作。通过理解和了解它们，你可以简单地调用这些函数中的一个，而不是编写必要的程序。一如既往，如果您有任何问题或意见，请随时回复或评论！谢谢