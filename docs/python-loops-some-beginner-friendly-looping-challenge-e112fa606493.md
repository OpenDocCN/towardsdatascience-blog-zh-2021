# Python 循环:一些初学者友好的循环挑战

> 原文：<https://towardsdatascience.com/python-loops-some-beginner-friendly-looping-challenge-e112fa606493?source=collection_archive---------3----------------------->

## 遍历列表、元组、字典和字符串

![](img/43b05a83647c8929ff15d1e1f690b7cb.png)

由[博尼瓦尔·塞巴斯蒂安](https://unsplash.com/@sebastien_bonneval?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在编程中，循环是一种逻辑结构，它重复一系列指令，直到满足某些条件。循环允许对 iterable 对象中的每一项重复相同的任务集，直到所有项都用完或达到循环条件。

循环应用于 iterables，iterables 是以特定数据格式(如字典)存储一系列值的对象。循环的美妙之处在于，你只需编写一次程序，就可以根据需要在任意多的元素上使用它。

本文的目的是实现一些应用于四种 Python 数据类型的中间循环挑战:**列表、元组、字典和字符串**。选择这些挑战是为了给各种可能的操作提供一个通用的循环结构。

# 遍历列表

列表是 Python 中的内置数据类型。它们是可变的、无序的元素序列——也称为可迭代对象。它是 Python 循环中使用的最通用的数据类型。所以我们的第一组循环挑战是基于列表的。

**挑战#1:** **遍历列表中的每个数字，分离出偶数和奇数。还要确定可能的异常值。**

```
my_list = [1, 2, 3, 4, 5, 6, 7, 100, 110, 21, 33, 32, 2, 4]even = []
not_even = []
outlier = []for i in my_list:
    if i > 90:
        outlier.append(i)    
    elif i%2 == 0:
        even.append(i)
    else:
        not_even.append(i)print('Even numbers', even)
print('Odd numbers', not_even)
print('outliers', outlier)Out:
Even numbers [2, 4, 6, 32, 2, 4]
Odd numbers [1, 3, 5, 7, 21, 33]
outliers [100, 110]
```

**挑战 2:找出列表中所有数字的总和**

```
num_sum = 0for i in my_list:
    num_sum += iprint('Sum of the elements in the list', num_sum)Out:
Sum of the elements in the list 330
```

**挑战 3:计算列表中偶数的和**

```
# step 1: find the even numbers
even = []for i in my_list:
    if i%2 == 0:
        even.append(i)# step 2: add all items in even list
sum_num = 0for i in even:
    sum_num +=iprint('The sum of even numbers', sum_num)Out:
The sum of even numbers 260
```

**挑战 4:计算一个列表中偶数的个数**

```
count = 0for i in range(len(my_list)):
    if my_list[i]%2==0:
        count +=1print('The count of even numbers in the list', count)Out:
The count of even numbers in the list 8
```

**挑战 5:计算列表中所有元素的累积和**

```
initial_val = 0
cumsum = []for i in my_list:
    initial_val += i
    cumsum.append(initial_val)print('The cummulative sums of the list', cumsum)Out:
The cummulative sum of the list [1, 3, 6, 10, 15, 21, 28, 128, 238, 259, 292, 324, 326, 330]
```

**挑战#6:遍历两个不同的列表，用** `**zip**` **函数**将它们聚合

```
state_id = [1,2,3]
state = ['Alabama', 'Virginia', 'Maryland']for i, j in zip(state_id, state):
    print(f'{i} {j}')Out:
1  Alabama
2  Virginia
3  Maryland
```

# 遍历元组

元组类似于列表，但关键的区别是，元组是不可变的。与列表不同，没有与元组相关联的方法来添加或移除元组中的项目。由于元组是不可变的，所以它们被广泛用于不变的数据(例如，社会保险号、出生日期等。)

**挑战 7:遍历混合数据类型的元组，只提取整数值**

```
my_tup = (1, 2, 'apple', 3, 4, 'orange')for i in my_tup:
    if type(i) == int:
        print(i)Out:
1
2
3
4
```

**挑战 8:元组解包:提取存储在列表中的元组**

```
list_of_tutple = [(1,2), (3, 4), (5, 6)]list1 = []
list2 = []for i,j in list_of_tutple:
    list1.append(i)
    list2.append(j)

print('List of first tuple items', list1)
print('List of second tuple items', list2)Out:
List of first tuple items [1, 3, 5]
List of second tuple items [2, 4, 6]
```

**挑战 9:应用** `**enumerate()**` **函数提取元组**中的元素

```
my_tup = ('Apple', 'Orange', 'Banana')for i,j in enumerate(my_tup, start=1):
    print(i,j)Out:
1 Apple
2 Orange
3 Banana
```

# 循环浏览字典

Python 中的字典是键值对的集合，这意味着字典中的每个条目都有一个键和一个关联值。下面是一个 Python 字典的例子，其中水果是“键”，价格是“值”。[注意:接下来的循环挑战将使用该字典作为输入。]

```
# Python dictionary
my_dict = {"apple": 2.50, "orange": 4.99, "banana": 0.59}
```

**挑战 10:访问字典中的所有键**

```
for i in my_dict.keys():
    print(i)Out:
apple
orange
banana
```

**挑战 11:访问字典中的所有值**

```
for i in my_dict.values():
    print(i)Out:
2.5
4.99
0.59
```

**挑战 12:从字典中同时访问键和值**

```
for i,j in my_dict.items():
    print('Key: ', i)
    print(f'Value: {j}')Out: 
Key:  apple
Value: 2.5
Key:  orange
Value: 4.99
Key:  banana
Value: 0.59
```

**挑战 13:找出字典中所有值的平均值**

```
for i in my_dict.values():
    values.append(i)

average = sum(values)/len(values)print(f'Average of values: {average}')Out:
Average of values: 2.6933333333333334
```

**挑战 14:条件循环:根据特定条件过滤字典条目**

```
fruits = []for i in my_dict.keys():
    if i in ["apple", "orange", "pear", "watermelon"]:
        fruits.append(i)print(fruits)Out:

['apple', 'orange']
```

**挑战 15:更新值:将所有值减少 25%(在实际应用中，这就像给所有水果打八五折)**

```
for i, j in my_dict.items():
    my_dict.update({i: j*.75})print(my_dict)Out:
{'apple': 1.875, 'orange': 3.7425, 'banana': 0.4425}
```

# 在字符串中循环

Python 没有单独的“字符”数据类型，而是由字符串对象表示一系列字符。字符串是不可变的——一旦创建就不能更改——但是每个元素都可以通过循环访问。

**挑战 16:通过字符串访问所有元素的简单迭代**

```
my_str = 'Matplotlib'for i in my_str:
    print(i, end=' ')Out:
M a t p l o t l i b
```

**挑战 17:切片:遍历字符串中的替换字符**

```
my_str = 'Matplotlib'for i in my_str[0: len(my_str): 2]:
    print(i, end=' ')Out:

M t l t i
```

**挑战 18:迭代一个句子并打印每个单词**

```
sent = 'It is a dark night'# splitting the sentence into words
sent_split = sent.split()# extract each word with a loop
for i in sent_split:
    print(i, end = ' / ')Out:
It / is / a / dark / night /
```

# 一锤定音

本文的目的是提供一些初学者友好的中级循环挑战，这些挑战经常出现在数据科学项目中。当然，有数百种可能的组合可以应用于各种情况和各种数据类型，但是这里给出的例子是更高级循环的基础。

希望这些挑战是有用的，如果你有意见，请随意写在下面，或者通过[媒体](https://mab-datasc.medium.com/)、[推特](https://twitter.com/DataEnthus)或 [LinkedIn](https://www.linkedin.com/in/mab-alam/) 与我联系。