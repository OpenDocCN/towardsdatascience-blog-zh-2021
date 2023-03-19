# UCL 数据科学协会:Python 序列

> 原文：<https://towardsdatascience.com/ucl-data-science-society-python-sequences-e3ffa67604a0?source=collection_archive---------20----------------------->

## 工作坊 2:列表、元组、集合和字典！

![](img/f685baaa1831700cba62e57c3ff4a655.png)

由[大卫·普帕扎](https://unsplash.com/@dav420?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今年，作为 UCL 数据科学协会的科学负责人，该协会将在整个学年举办一系列 20 场研讨会，主题包括数据科学家工具包 Python 简介和机器学习方法。对于我介绍和交付的每一个，我的目标是创建一系列小型博客帖子，这些帖子将概述主要观点，并提供完整研讨会的链接，供任何希望跟进的人使用。所有这些都可以在我们的 [GitHub](https://github.com/UCL-DSS) 上找到，并将在全年更新新的研讨会和挑战。

本系列的第二个研讨会介绍了 Python 中的序列。这是 Python 入门系列的前一个研讨会的继续，涵盖了 Python 中的列表、元组、集合和字典的基础知识。一如既往，这是研讨会的总结，完整的 Jupyter 笔记本可以在我们的 [GitHub](https://github.com/UCL-DSS/python-sequence-workshop) 上找到。

如果您错过了，请参加上周的 Python 基础知识研讨会:

</ucl-data-science-society-python-fundamentals-3fb30ec020fa>  

第二个工作坊是 Python 序列的介绍，包括 Python 代码中可以找到的四个主要数据结构`list`、`tuple`、`set`和`dictionary`。在我们开始之前，有一些关键短语需要理解:

*   **可变**:这意味着项目一旦被创建就可以被改变。与此相反的是不可变的，这意味着一旦对象被创建，它就不能被改变。这将影响数据结构在代码中的使用情况。
*   **Ordered** :这意味着项目的存储方式不会改变(除非明确说明)，并且可以通过了解项目的存储顺序来访问。与此相反的是一个无序对象，其中存储的项目不能按照它们被放置的顺序来访问。
*   **可索引**:这意味着可以根据使用索引将项目添加到数据结构中的顺序来访问项目。这仅适用于有序对象，因为无论何时调用或使用它，顺序都保持不变。

## 目录

经常遇到的第一种数据结构是列表结构。它们是有益的，因为它们可以在一个变量中存储多个项目，就像您所期望的书面列表一样。列表的主要特征是:

*   **可变**:一旦创建就可以更改。
*   **有序:**它们在创建时保持它们的顺序。
*   **可索引**:我们可以根据数据在列表中的位置来访问数据。
*   **可以包含重复记录**:相同值的数据可以存储在一个列表中。

这会影响列表在程序中的使用方式。

我们可以使用`=`操作符在 Python 中创建一个`list`，就像我们在[第一次研讨会](/ucl-data-science-society-python-fundamentals-3fb30ec020fa)中对变量赋值所做的那样，但是这里我们使用`[]`来包含列表或者使用`list()`方法，如下所示:

```
*#create a list of Fruit using the [] notation*
fruit_list = ["Apple", "Banana", "Peach"]

*#creating a list of vegetables using the list() nottation*
vegetable_list = list(["Pepper", "Courgette", "Aubergine"])
```

这意味着我们在这些附件中包含了我们希望包含在列表中的所有项目，用逗号分隔每个项目。

虽然我们在上面的列表中使用了字符串，但实际上我们可以在列表中使用任何我们想要的数据类型，如下所示(甚至是列表中的列表！):

```
*#different list*
random_list = ["Hello", 3, "Cheese", 6.2, [1,2,3]]

*#print the result*
print(type(random_list))
print(random_list)# out:<class "list">
["Hello", 3, "Cheese", 6.2, [1,2,3]]
```

列表的一个重要特征是它们是有序的数据集合。这意味着如果我们知道索引是什么，我们就可以根据它的位置(索引)从列表中访问信息！但是需要注意的是，Python 从 0 开始计数，这意味着列表中的第一项的索引为 0。例如，我们可以使用`1`的索引值从上面的`fruit_list`中提取`Banana`。这是使用方括号符号来完成的，在这里，我们在调用列表名称之后，在`[index]`中传递索引，如下所示:

```
fruit = friut_list[1]print(fruit)#out:`Banana`
```

我们还可以使用`list[first_index, last_index_not_inclusive]`形式的列表片和负索引来访问列表中的多个条目，其中我们从`-1`开始从列表的末尾往回计数。看看您能否理解下面的列表片段是如何工作的:

```
*#create a list of scores*
scores = [12,42,62,65,73,84,89,91,94]*#second lowest to fifth lowest*
print(scores[1:5])

*#print second lowest*
print(scores[1:2])

*#print the fifth lowest to the highest*
print(scores[5:])

*#print the third highest to the highest*
print(scores[-3:])#out:42, 62, 65, 73
42
84, 89, 91, 94
89, 91, 94
```

使用列表可以做的其他事情包括:如果我们知道值，则查找项目的索引，添加项目，删除项目，按升序或降序对列表进行排序，将列表添加在一起，以及查找列表的长度。如果你想知道如何做到这一切，你可以去我们 GitHub 上的练习册:[练习册](https://github.com/UCL-DSS/python-sequence-workshop/blob/main/workshop.ipynb)。

## **元组**

Python 中我们经常遇到的第二种数据结构是元组。这些类似于列表，但具有以下特征:

*   **不可改变的**:一旦它们被创建，就不能被改变。
*   **有序:**项目添加的顺序保持不变。
*   **可索引:**我们可以根据索引来访问项目。
*   **可以包含重复:**该数据结构中可以有相同值的项目。

这改变了它们相对于列表的用途，因为主要区别在于它们是不可变的。这意味着，当您希望确保数据不会在程序中被更改(即使是意外更改)时，可以使用它们，而不是使用列表，同时仍然保留相同的功能。

相对于列表，我们可以通过三种主要方式创建元组:

*   我们可以使用`()`来包含用逗号分隔的项目。
*   我们可以在由逗号分隔的`()`或`[]`包含的项目周围调用`tuple()`。

我们可以看到这一点:

```
*#create the tuple* 
cars = ("Ford", "Hyundai", "Toyata", "Kia")*#create a second tuple* 
fruits_tuple = tuple(("Strawberry", "peach", "tomato")) *#create the third tuple* 
vegetable_tuple = tuple(["potato", "onion", "celery"])#print the result
print(cars)
print(type(cars))print(fruits_tuple)
print(type(fruits_tuple))print(vegetable_tuple)
print(type(vegetable_tuple))#out:
('Ford', 'Hyundai', 'Toyata', 'Kia')
<class 'tuple'>
('Strawberry', 'peach', 'tomato')
<class 'tuple'>
('potato', 'onion', 'celery')
<class 'tuple'>
```

事实上，它们是有序的和可索引的，这意味着我们可以像访问列表一样使用索引来访问元组中的信息，所以我们在这里不再重复。

虽然元组是不可变的，但有一种方法可以实际改变其中的信息。这是通过将两个元组相加或相乘来实现的(好吧，这样不会真正改变其中的信息)，如下所示:

```
*#create new tuples*
tuple1 = ("a", "b", "c")
tuple2 = (1,2,3)

*#add together using the +*
tuple3 = tuple1 + tuple2
print(tuple3)

*#multiply an existing tuple together* 
tuple4 = tuple1*2
print(tuple4)#out:
('a', 'b', 'c', 1, 2, 3)
('a', 'b', 'c', 'a', 'b', 'c')
```

除此之外，我们还有内置的功能和方法，包括:查找长度、元组中特定值的实例数、最小值和最大值。如果你想知道如何做到这一切，你可以去我们的 GitHub 上的练习册:[练习册](https://github.com/UCL-DSS/python-sequence-workshop/blob/main/workshop.ipynb)。

## **设置**

紧接着，另一个类似于列表的数据结构是集合。这一点的主要特点是:

*   **可变**:一旦创建，就可以改变。
*   **无序:**当你稍后调用它们时，顺序不再保持不变。
*   **不可索引:**你不能用给定的索引访问某个项目。
*   **不允许重复**:集合不能包含重复值。

这再次改变了它们在程序中相对于列表的使用方式。最重要的是，它们不能包含重复的信息，所以如果您不关心单个项目的实例数量，只需要比使用集合更唯一的值。然而，在这种情况下，如果您关心一个项被添加到数据结构中的顺序，您就不会使用集合。

我们可以通过两种主要方式来创建它们:

*   我们可以使用`{}`来包含由逗号分隔的项目列表
*   我们可以使用`set()`符号来包含由逗号分隔的条目列表或元组

这方面的一个例子如下:

```
*#create a set using curly brackets*
fruits = {"apple", "banana", "cherry"}

*#create a set using the set constructor*
vegetables = set(("courgette", "potato", "aubergine"))#print the results
print(fruits)
print(vegetables)#out:
{'cherry', 'apple', 'banana'}
{'potato', 'aubergine', 'courgette'}
```

事实上，集合中的条目是未索引的，也就是说，它们不能像在列表中那样被访问，因为我们不能保证它们在我们输入它们时的位置。因此，有两种主要方法来检查一个项目是否在器械包中:

```
#use a loop to iteratre over the set
for x in fruits:
    print(x)

#or check whether the fruit you want is in the set
print("apple" in fruits)
#which acts the same way as if it were in a list#out:
cherry
apple
banana
True
```

这也意味着在集合中添加和删除项目与我们处理元组或列表的方式略有不同。然而，我们也有类似的列表功能，因为我们可以找到长度或找到一个项目是否在一个集合中。有关这方面的更多信息可在[工作簿](https://github.com/UCL-DSS/python-sequence-workshop/blob/main/workshop.ipynb)中找到。

## 词典

最后，在 Python 编程中经常遇到的另一种数据结构是字典。这些具有以下特点:

*   **可变:**创建后可以更改。
*   **Ordered** :条目添加到字典的顺序保持不变。
*   **可索引**:我们可以根据它们的索引(在本例中是它们的键)来访问项目
*   **不能包含重复值:**至少就它们的键而言

与以前的数据结构相比，这种结构的主要区别在于数据存储在`key:value`对中，这意味着我们将通过指定`key`来访问字典中的条目，而不是编号索引。这意味着我们可以将数据与特定的键相关联，这在我们想要维护数据集中不同字典之间的关系时是有意义的。

创建这个数据结构的主要方法是在`{}`中指定`key:value`对。虽然这类似于一个集合，但是主要的区别是`key`和`value`被`:`分开，这确保了一个字典被创建。我们也可以在两个列表上使用`dict()`来创建它。这可以如下进行:

```
new_dict = {"Name":"Peter Jones",
           "Age":28,
           "Occupation":"Data Scientist"}print(new_dict)#out:
{'Name': 'Peter Jones', 'Age': 28, 'Occupation': 'Data Scientist'}
```

这种结构本质上意味着我们可以使用`key`来访问`values`而不是数字索引。例如，如果我们想知道这个人的名字，我们可以用:

```
#the first way is as we would with a list
print(new_dict["Name"])#however we can also use .get()
print(new_dict.get("Name"))#the difference between the two is that for get if the key
#does not exist an error will not be triggered, while for 
#the first method an error will be
#try for yourself:
print(new_dict.get("colour"))
#print(new_dict["colour"])#out:
Peter Jones
Peter Jones
None
```

后一种方法更有用，因为如果键不存在，它将停止你的程序，除非被处理，而不是一个错误，而是通过一个 None 值。

以这种方式访问信息意味着我们不能在数据集中有重复项，因为我们不知道我们将访问什么:

```
second_dict = {"Name":"William",
              "Name":"Jessica"}print(second_dict["Name"])#out:Jessica
```

正如我们在这里看到的，我们有两个“name”键，当试图访问信息时，它只打印第二个值，而不是第一个值。这是因为第二个密钥会覆盖第一个密钥值。

由于这也是可变的，我们可以在字典创建后改变、添加或删除条目，与列表的方式类似，但我们使用键而不是使用`[]`中的索引:

```
#create the dictionary
car1 = {"Make":"Ford",
       "Model":"Focus",
       "year":2012}#print the original year
print(car1["year"])#change the year
car1["year"] = 2013#print the new car year
print(car1["year"])#add new information key
car1["Owner"] = "Jake Hargreave"#print updated car ifnormation
print(car1)#or we can add another dictionary to the existing dictionary using the update function
#this will be added to the end of the existing dictionary
car1.update({"color":"yellow"})
#this can also be used to update an existing key:value pair#print updated versino
print(car1)#out:
2012
2013
{'Make': 'Ford', 'Model': 'Focus', 'year': 2013, 'Owner': 'Jake Hargreave'}
{'Make': 'Ford', 'Model': 'Focus', 'year': 2013, 'Owner': 'Jake Hargreave', 'color': 'yellow'}
```

字典的好处是键，这意味着我们可以将特定的值与用于访问信息的给定键相关联。这可以包括使用记录，例如使用特定的 id 与其他信息相关联。

其他方法包括访问所有的键、访问所有的值、访问作为元组的`key:value`对以及打印字典的长度。

完整的研讨会笔记，以及进一步的例子和挑战，可以在这里找到 [**。**](https://github.com/UCL-DSS/python-sequence-workshop)

如果您想了解我们协会的更多信息，请随时关注我们的社交网站:

https://www.facebook.com/ucldata:[脸书](https://www.facebook.com/ucldata)

insta gram:[https://www.instagram.com/ucl.datasci/](https://www.instagram.com/ucl.datasci/)

领英:[https://www.linkedin.com/company/ucldata/](https://www.linkedin.com/company/ucldata/)

如果你想了解 UCL 数据科学协会和其他优秀作者的最新信息，请使用我下面的推荐代码注册 medium。

<https://philip-wilkinson.medium.com/membership>  

或者，如果您想阅读我的其他作品，请访问:

</a-complete-data-science-curriculum-for-beginners-825a39915b54>  </ucl-data-science-society-object-oriented-programming-d69cb7a7b0be>  <https://python.plainenglish.io/abstract-data-types-and-data-structures-in-programming-570d40cb4b44> 