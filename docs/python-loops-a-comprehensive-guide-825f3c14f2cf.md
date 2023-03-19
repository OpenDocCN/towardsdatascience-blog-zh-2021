# Python 循环:综合指南

> 原文：<https://towardsdatascience.com/python-loops-a-comprehensive-guide-825f3c14f2cf?source=collection_archive---------13----------------------->

## 遍历列表、元组、字典和字符串以及循环控制—中断、继续、传递

![](img/ff169c269fc0fbcbf33022ed7575ab1f.png)

普里西拉·杜·普里兹在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# **什么是循环**

在编程中，循环意味着以相同的顺序多次重复相同的计算。

想想现实生活中的情况。你是一名野外生物学家，正在测量森林中的树木。你选择一棵树，测量它的直径和高度，记在你的笔记本上，估计它的总体积。

接下来，你选择另一棵树，测量它的直径和高度，记在你的笔记本上，估计它的总体积。

然后，你再选一棵树，测量它的直径和高度，记在你的笔记本上，估计它的总体积。

您不断重复相同的过程，直到样本中的所有树都用完为止。在编程行话中，你在每棵树上循环，以相同的顺序执行相同的任务。

回到编程上来，如果给你一个整数值列表，要求你对每一项求平方，然后加 5，最后报告结果——这就是循环的一个例子。

## 我们可以循环什么？

那么我们能循环什么呢？基本上，任何可迭代的数据类型。Python 中的可迭代对象是以不同数据格式存储的值序列，例如:

*   列表(例如[10，12，13，15])
*   元组(例如(10，12，13，15)
*   字典(例如{ '姓名':'艾伦'，'年龄':25})
*   字符串(例如“数据科学”)

## **什么是不同种类的循环？**

你主要会遇到的两种循环:for 循环和 while 循环。其中，for 循环是数据科学问题中最常见的一种。主要区别在于:

*   for 循环对 iterable 对象中的每个元素迭代有限次
*   而循环继续进行，直到满足某个条件

# 在列表上循环

遍历列表非常简单。给你一个值列表，要求你对每一项做些什么。假设您有:

```
my_list = [1,2,3,4]
```

并且要求您计算列表中每个值的平方:

```
for each_value in my_list:
    print(each_value * each_value)Out:
1
4
9
16
```

类似地，你可以做一些更复杂的循环(例如“嵌套循环”)。例如，给你两个列表，要求你(I)将一个列表的值与另一个相乘，(ii)将它们追加到一个空列表中，以及(iii)打印出新的列表。让我们按顺序来做:

```
new_list = []list1 = [2, 3, 4]
list2 = [4, 5, 6]for i in list1:
    for j in list2:
        new_list.append(i*j)

print(new_list)Out:
[8, 10, 12, 12, 15, 18, 16, 20, 24]
```

# 在元组上循环

根据元组的结构和要完成的任务，在元组上循环可能有点复杂。

让我们将一些元组存储在一个列表中，每个元组代表一个班级中学生的姓名和年龄:

```
students = [('Allie', 22), ('Monty', 18), ('Rebecca', 19)]
```

现在的任务是(I)提取所有年龄,( ii)将它们存储在一个列表中,( iii)计算平均年龄:

```
ages = []for i,j in students:
    ages.append(j)avg = sum(ages)/len(ages)
print(avg)Out: 
19.666666666666668
```

这里的每个元组包含两项(姓名和年龄)。即使你对名字不感兴趣，通过 *i* 和 *j* 你也指定了这两个项目，并要求将项目 *j* (年龄)添加到新列表中。叫做“元组解包”。

# 在字典上循环

Python 中的字典是一个[键-值对](/working-with-python-dictionaries-a-cheat-sheet-706c14d29da5)的集合——这意味着字典中的每一项都有一个键和一个关联值。字典的一个例子:

```
# fruit price dictionary
fruit_prices = {"apple": 2.50, "orange": 4.99, "banana": 0.59}
```

您可以循环遍历这些字典元素，并执行各种操作。这里有几个例子:

提取字典中的所有键:

```
for i in fruit_prices.keys():
    print(i)Out:
apple
orange
banana
```

将所有值存储在列表中:

```
values = []
for i in fruit_prices.values():
    values.append(i)
print(values)Out:
[2.5, 4.99, 0.59]
```

挑战:你能在字典中找到所有价格的平均值吗？

# 在字符串上循环

让我们考虑字符串—“Hello”。它看起来像一个可迭代的对象吗？事实上的确如此。

```
for i in 'Hello':
    print(i)Out:
H
e
l
l
o
```

你可以用 for 循环解开字符串中的每个字符，并对它们进行各种操作。

同样，也可以迭代一个句子中的每个单词。然而，在这种情况下，需要额外的步骤来拆分句子。

```
sent = 'the sky is blue'# splitting the sentence into words
sent_split = sent.split()# extract each word with a loop
for i in sent_split:
    print(i)Out:
the
sky
is
blue
```

# While 循环

与 for 循环一样，while 循环重复执行一段代码——只要条件为真。只有当循环条件为假时，循环才会中断。

while 循环的一般结构如下:

```
i = 0while i <=5:
    print(i)
    i = i+1 # option to break out of the loop
Out:
0
1
2
3
4
5
```

在上面的例子中，在每次迭代中，打印出 *i* 的值，直到它达到 5。此后，while 循环条件变为假(即当 *i = 6* 时 *i ≤ 5* 变为假)。

while 循环的一个实际用例是在网站上使用您的登录凭证。当您没有提供正确的用户名或密码时，您无法登录*。*

```
user_id = 'user101'while True:
    user = input('Enter your user ID: ')

    if user == user_id:
        print("You have entered ", user) 
        break
    else:
        print("Enter a valid user ID ")
```

# 循环控制:继续、中断、通过

所谓的循环控制关键词有三种:break、continue、pass。这些语句改变了循环的流程，并允许程序在某个外部条件被触发时退出或跳过部分循环。

## 破裂

如果循环中存在一个`break` 语句，当满足一个条件时，它将终止循环。

```
string = 'hello, there'for i in string:
    if i == ',':
        break
    print(i)Out:
h
e
l
l
o
```

在上面的代码片段中，我们要求程序在找到字符串中的逗号并执行下一条语句(打印 I)后立即存在。想想开头给出的现实生活中的例子。作为一名野外生物学家，你在反复测量树木。但是你会打破这个循环——**如果下雨**！

## 继续

`continue`语句没有跳出循环，而是简单地跳过一次迭代，继续下一次迭代。

让我们执行上面相同的代码，但是使用 continue 关键字。

```
string = 'hello, there'for i in string:
    if i == ',':
        continue
    print(i)Out:
h
e
l
l
o

t
h
e
r
e
```

所以在这种情况下，如果循环遇到一个逗号，循环就跳过这个逗号继续。

因此，在树木测量示例中，如果您遇到一棵枯树/断树，您只需跳过它并继续下一个。

## 及格

不做任何事情，它只是一个还没写完的语句的占位符。

```
string = 'hello, there'for i in string:
    pass
```

如果我们没有放一个`pass`在那里，它将抛出一个错误消息，其余的代码将不会执行。

## 摘要

本文的目的是给出 Python 中的`for` 循环和`while` 循环的直观感受。给出了如何循环遍历列表、元组、字典和字符串等可迭代对象的例子。到最后，循环控制语句的概念——break、continue 和 pass——都包含了例子。

本文旨在给出循环如何工作的初步概述。在以后的文章中，我将介绍一些数据科学家在项目中经常遇到的高级循环挑战。

希望这篇文章是有用的，如果你有意见，请写在下面，或者通过[媒体](https://mab-datasc.medium.com/)、 [Twitter](https://twitter.com/DataEnthus) 或 [LinkedIn](https://www.linkedin.com/in/mab-alam/) 与我联系。