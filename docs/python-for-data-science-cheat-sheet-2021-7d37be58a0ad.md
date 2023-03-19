# Python 数据科学备忘单(2021)

> 原文：<https://towardsdatascience.com/python-for-data-science-cheat-sheet-2021-7d37be58a0ad?source=collection_archive---------8----------------------->

## 2021 年初学 Python 的绝对基础

![](img/5b73b3a4a307582c8d4856f5b97d6444.png)

阿什·爱德华兹拍摄于 Unsplash 的照片

Python 已经成为 2021 年执行数据科学最流行的计算语言。但在你能够制作令人震惊的深度学习和机器学习模型之前，你需要先了解 Python 的基础知识和不同类型的对象。

查看下面的不同部分，了解各种类型的对象及其功能。

> **章节:**
> 1。[变量和数据类型](#6695)
> 2。[列出了](#55b2)和
> 3。[琴弦](#6594)琴弦
> 4。 [NumPy 数组](#f090)
> 5。[图书馆](#f4f5)

# 变量和数据类型

Python 中的变量用于存储值，在这里您将学习如何将变量赋给特定的值，如何更改该值，以及在 Python 中转换为不同的数据类型。

## 变量赋值

```
>>> x=5
>>> x
 5
```

## 变量计算

变量可以加、减、乘、除等。

```
x = 5 for all functions below
```

*   两个变量的和

```
>>> x+2
 7
```

*   两个变量相减

```
>>> x-2
 3
```

*   两个变量的乘法

```
>>> x*2
 10
```

*   变量的指数运算

```
>>> x**2
 25
```

*   变量的余数

```
>>> x%2
 1
```

*   变量的除法

```
>>> x/2
 2.5
```

## 数据类型的转换

Python 提供了将变量从一种数据类型转换成另一种数据类型的能力。

*   字符串变量

```
>>> str(x)
 '5'
```

*   变量到整数

```
>>> int(x)
 5
```

*   浮动变量

```
>>> float(x)
 5.0
```

*   布尔型变量

```
>>> bool()
 True
```

# 列表

list 是 Python 中的一种数据结构，它是可变元素的有序序列。列表中的每个元素或值称为一个项目。

以下代码中的**我的列表**和**我的列表 2** 在本节中用作列表示例。

```
>>> a = 'is'
>>> b = 'nice'
>>> my_list = ['my','list',a,b]['my', 'list', 'is', 'nice']>>> my_list2 = [[4,5,6],[3,4,5,6]][[4, 5, 6],[3, 4, 5, 6]]
```

## 选择列表元素

从列表或项目中选择特定项目的不同方式。

*   选择索引 1 处的项目

```
>>> my_list[1]
 'list'
```

*   选择倒数第三项

```
>>> my_list[-3]
 'list'
```

*   选择索引 1 和 2 处的项目

```
>>> my_list[1:3]
 ['list', 'is']
```

*   选择索引 0 之后的项目

```
>>> my_list[1:]
 ['list', 'is', 'nice']
```

*   选择索引 3 之前的项目

```
>>> my_list[:3]
 ['my', 'list', 'is']
```

*   复制我的列表(_ u)

```
>>> my_list[:]
 ['my', 'list', 'is', 'nice']
```

*   my_list[list][itemsOfList]

```
>>> my_list2[1][0]
 3
>>> my_list2[1][:2]
 [3, 4]
```

## 列表操作

因为列表是可变的，所以你可以将列表相加或者相乘。

*   添加列表

```
>>> my_list + my_list
 ['my', 'list', 'is', 'nice', 'my', 'list', 'is', 'nice']
```

*   乘法列表

```
>>> my_list*2
 ['my', 'list', 'is', 'nice', 'my', 'list', 'is', 'nice']
```

## 列出方法

向列表中添加项目、删除项目、计数项目等各种功能。

*   获取项目的索引

```
#Gets the index at of item 'a' in the list
>>> my_list.index(a)
 2
```

*   清点物品

```
#Counts how mnay times 'a' occurs in the list
>>> my_list.count(a)
 1
```

*   将项目添加到列表中

```
#Adds 'T' to the end of the list
>>> my_list.append('T')
 ['my', 'list', 'is', 'nice', 'T']
```

*   移除项目

```
#Removes T from the list
>>> my_list.remove('T')
 ['my', 'list', 'is', 'nice']
```

*   删除项目。v2

```
#Removes the first value from the list
>>> del(my_list[0:1])
 ['list', 'is', 'nice']
```

*   删除项目. v3

```
#Removes the last item from the list
>>> my_list.pop(-1)
 ['my', 'list', 'is']
```

*   颠倒列表

```
#Reverses the entire list
>>> my_list.reverse()
 ['nice', 'is', 'list', 'my']
```

*   插入一个项目

```
#Inserts item 'T' at index 0 in the list
>>> my_list.insert(0,'T')
 ['T', 'my', 'list', 'is', 'nice']
```

*   对项目进行排序

```
#Sorts the list alphabetically
>>> my_list.sort()
 ['is', 'list', 'my', 'nice']
```

# 用线串

Python 中的字符串是一系列字符。字符串是不可变的。这意味着一旦定义，就不能更改。

在本节中，下面代码中的 my_string 被用作字符串的示例。

```
>>> my_string = 'thisStringIsAwesome'
>>> my_string
 'thisStringIsAwesome'
```

## 选择字符串元素

从字符串或值中选择特定值的不同方法。

*   选择索引 3 处的项目

```
>>> my_string[3]
 's'
```

*   选择索引 4 到 8 处的项目

```
>>> my_string[4:9]
 'Strin'
```

## 字符串操作

你可以用不同的方式把字符串加在一起。

*   添加字符串

```
>>> my_string + 'Innit'
 'thisStringIsAwesomeInnit'
```

*   乘法字符串

```
>>> my_string*2
 'thisStringIsAwesomethisStringIsAwesome'
```

*   成串

```
>>> 'm' in my_string
 True
```

## 字符串方法

改变字符串外观或从字符串中提取统计数据的各种函数。

*   字符串转换为大写

```
#Converts all values from the string to uppercase
>>> my_string.upper()
 'THISSTRINGISAWESOME'
```

*   字符串转换为小写

```
##Converts all values from the string to lowercase
>>> my_string.lower()
 'thisstringisawesome'
```

*   计数字符串元素

```
#Counts how mnay times 'w' appears in the string
>>> my_string.count('w')
 1
```

*   替换字符串元素

```
#Replaces all values of 'e' in the string with 'i'
>>> my_string.replace('e','i')
 'thisStringIsAwisomi'
```

*   去除空白

```
#Removes any unecessary spaces from the string
>>> my_string.strip()
 'thisStringIsAwesome'
```

# NumPy 数组

NumPy 数组是由相同类型的值组成的网格。维数是数组的秩；数组的形状是一组整数，给出了数组在每个维度上的大小。

下面代码中的 **my_array** 和 **my_2darray** 在本节中被用作 NumPy 数组的例子。

```
>>> my_list = [1,2,3,4]
>>> my_array = np.array(my_list)
>>> my_2darray = np.array([[1,2,3],[4,5,6]])
```

## 选择 NumPy 数组元素

从数组中选择特定值的不同方法。

*   选择索引 1 处的项目

```
>>> my_array[1]
 2
```

*   选择索引 0 和 1 处的项目

```
>>> my_array[0:2]
 array([1, 2])
```

*   my _ 2d array[行，列]

```
>>> my_2darray[:,0]
 array([1, 4])
```

## NumPy 数组操作

这些操作向您展示了如何将两个数组相加，将一个值乘以一个数组，并对该数组进行比较。

*   添加列表

```
>>> my_array + np.array([5,6,7,8])
 array([ 6,  8, 10, 12])
```

*   乘法数组

```
>>> my_array*2
 array([2, 4, 6, 8])
```

*   大于或小于

```
>>> my_array > 3
 array([False, False, False,  True])
```

## NumPy 数组方法

各种函数，增加一个数组的值，删除一个值，计算一个数组的平均值和中值，等等。

*   获取数组的维数

```
>>> my_array.shape
 (4,)
```

*   将项目追加到数组

```
#Adds an arrasy to the end of another array
>>> np.append(other_array)
```

*   在数组中插入项目

```
#Inserts the value of 5 into index 1 of the array
>>> np.insert(my_array,1,5)
 array([1, 5, 2, 3, 4])
```

*   删除数组中的项目

```
#Removes the value in index 1 from the array
>>> np.delete(my_array,[1])
 array([1, 3, 4])
```

*   阵列的平均值

```
>>> np.mean(my_array)
 2.5
```

*   数组的中值

```
>>> np.median(my_array)
 2.5
```

*   标准偏差

```
>>> np.std(my_array)
 1.118033988749895
```

# 图书馆

像 NumPy 这样的各种库需要先导入 Python，然后才能使用该库的任何函数。

*   导入库

```
>>> import nump as np
```

*   选择性进口

```
>>> from math import pi
```

现在和可预见的将来，Python 都是编程和数据科学的霸主。但是在接受创建复杂模型和项目的挑战之前，您需要先学习 Python 的基础知识。

开始的时候使用这个备忘单作为指南，需要的时候再回头看，你将会很快成为一名专业的 Python 程序员。

[**与 1k+人一起加入我的电子邮件列表，免费获得完整的 Python for Data Science 备忘单小册子。**](http://pages.christopherzita.com/pythoncheatsheets)