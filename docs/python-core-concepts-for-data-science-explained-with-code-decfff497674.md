# 解释数据科学的 Python 核心概念(带代码)

> 原文：<https://towardsdatascience.com/python-core-concepts-for-data-science-explained-with-code-decfff497674?source=collection_archive---------12----------------------->

## 每个数据科学家都应该知道的 Python 概念

![](img/9a1f8f78aeba05df052ca616abe6c99a.png)

照片由[米米·蒂安](https://unsplash.com/@mimithian?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Python 提供了大量用于数据科学的库，如 Pandas、Numpy 和 Scikit-learn。但是，立即学习这些库并跳过基础知识并不好。

如果你想学习数据科学的 Python，你应该先掌握 Python 的核心概念。拥有坚实的 Python 基础将有助于您避免常见错误和不良实践。这样一来，学习数据科学中使用的 Python 库就容易多了。

在本指南中，我们将看到每个数据科学家都应该知道的一些必备的 Python 概念。在本文的最后，您会发现一个 PDF 版本的 Python for Data Science 备忘单(见下面目录中的第 9 节)

```
**Table of Contents** 1\. [Python Attributes vs Methods](#57b7)
2\. [Python Data Types](#8f2c)
3\. [Python Variables](#fe7a)
4\. [Python Lists](#bfea)
5\. [Python Dictionary](#8a21)
6\. [Python If Statement](#11c7)
7\. [Python For Loop](#06da)
8\. [Python Function](#f108)
9\. [Python for Data Science Cheat Sheet](#d384) (Free PDF)
```

# 1.Python 属性与方法

我说不清我初学 Python 的时候，交替使用“属性”和“方法”这两个词有多久了。

当你学习像 Pandas 这样的库时，你会频繁地调用属性和方法，所以知道它们之间的区别是有好处的。

*   **属性**:属性是存储在类中的变量。即与对象相关联的值。使用带点的表达式按名称引用它。对于一个对象`Foo`，你称一个属性`bar`为`Foo.bar`
*   **方法**:方法是定义在类体内的函数。对于一个对象`Foo`，你调用一个方法`baz`作为`Foo.baz()`

当谈到数据科学时，我们不经常使用类，所以如果您不太理解定义，请不要担心。现在，你可以记住这些符号。

在本指南中，我们将会看到很多 Python 方法的实际例子。

*不想看书？改看我的视频:*

# 2.Python 数据类型

Python 中的每个值都是一个对象。一个对象有不同的数据类型。以下是 Python 中最常见的数据类型。

1.  String (str):一个字符串代表一系列字符。在 Python 中，引号内的任何内容(单引号或双引号)都是字符串。
2.  布尔值:真/假值
3.  Integer (int):可以不用小数部分书写的数字。
4.  Float (float):包含浮点小数点的数字。

在接下来的章节中，我们将会看到列表和字典等其他数据类型的细节。

要检查对象的数据类型，使用`type`功能。

```
IN [0]: **type**(1)
OUT[0]: intIN [1]: **type**(2.3)
OUT[1]: floatIN [2]: **type**("Hello World")
OUT[2]: str
```

## 字符串方法

我们可以像在 Microsoft Excel 中一样对字符串应用不同的函数。然而，在 Python 中我们使用方法。方法是“属于”一个对象的函数。为了调用一个方法，我们在对象后面使用`.`符号。

让我们来看一些改变文本大小写的字符串方法。

```
IN [3]: "Hello World".upper()
OUT[3]: HELLO WORLDIN [4]: "Hello World".lower()
OUT[4]: hello worldIN [5]: "hello world".title()
OUT[5]: Hello World
```

# 3.Python 变量

变量帮助我们存储数据值。在 Python 中，我们经常处理数据，因此变量对于正确管理这些数据非常有用。为了给变量赋值，我们使用了`=`符号。

让我们创建一条“我正在学习 Python”的消息，并将其存储在一个名为`message_1`的变量中。

```
message_1 = "I'm learning Python"
```

我们可以创造尽可能多的变量。只要确保给新变量赋予不同的名字。让我们创建一个新的信息，说“这很有趣！”并将其存储在一个名为`message_2`的变量中。

```
message_2 = "and it's fun!"
```

## **字符串串联**

我们可以通过使用`+`操作符将`message_1`和`message_2`放在一起。这称为字符串串联。

```
IN [6]: message_1 + message_2
OUT[6]: "I'm learning Pythonand it's fun!"
```

我们需要添加一个空格`“ ”`使文本可读。这一次，让我们将这个新消息存储在一个名为`message`的新变量中

```
message = message_1 + ' ' + message_2IN [7]: message
OUT[7]: "I'm learning Python and it's fun!"
```

现在你可以改变写在`message_1`或`message_2`中的信息，这也将改变我们新的`message`变量。

# 4.Python 列表

在 Python 中，列表用于在单个变量中存储多个项目。列表是有序且可变的容器。在 Python 中，我们把可以改变值的对象称为“可变的”。也就是说，列表中的元素可以改变它们的值。

为了创建一个列表，我们必须引入方括号`[]`中的元素，用逗号分隔。

考虑以下名为“countries”的列表，其中包含了世界上人口最多的一些国家。

```
countries = ['United States', 'India', 'China', 'Brazil']
```

请记住，列表可以有不同类型的元素和重复的元素。

## 列表索引

通过索引，我们可以根据列表项或元素的位置来获取它们。列表中的每个项目都有一个索引(在列表中的位置)。Python 使用从零开始的索引。也就是说，第一个元素(“美国”)的索引为 0，第二个元素(“印度”)的索引为 1，依此类推。

为了通过索引访问一个元素，我们需要使用方括号`[]`。

```
IN [8]: countries[0]
OUT[8]: United StatesIN [9]: countries[1]
OUT[9]: IndiaIN [10]: countries[3]
OUT[10]: Brazil
```

也有负索引帮助我们从列表的最后一个位置开始获取元素，所以我们不使用从 0 及以上的索引，而是使用从-1 及以下的索引。

让我们得到列表的最后一个元素，但是现在使用负索引。

```
IN [11]: countries[-1]
OUT[11]: Brazil
```

## 限幅

切片意味着访问列表的一部分。切片是列表元素的子集。切片符号具有以下形式:

```
list_name[start:stop]
```

其中“start”表示第一个元素的索引，stop 表示要停止的元素(不包括在切片中)。

让我们看一些例子:

```
IN [12]: countries[0:3]
OUT[12]: ['United States', 'India', 'China']IN [13]: countries[1:]
OUT[13]: ['India', 'China', 'Brazil']IN [14]: countries[:2]
OUT[14]: ['United States', 'India']
```

## 列出操作和方法

你可以用列表做很多事情。您可以添加新元素、删除任何元素、更新值等等。在这。部分，我们将看到最常见的列表操作和方法。

**向列表添加元素** 有不同的方法可以帮助我们向列表添加新元素。让我们使用`countries`列表测试它们(我们将在每个测试中定义`countries`列表，以相同数量的元素开始)

```
# append(): Adds a new element at the end of a list
countries = ['United States', 'India', 'China', 'Brazil']
IN [15]: countries.append('Canada')
IN [16]: countries
OUT[16]: ['United States', 'India', 'China']# insert(): Adds a new element at a specific position
countries = ['United States', 'India', 'China', 'Brazil']
IN [17]: countries.insert(0,'Canada')
IN [18]: countries
OUT[18]: ['Canada', 'United States', 'India', 'China', 'Brazil']
```

我们甚至可以使用`+`来连接列表。

```
countries_2 = ['UK', 'Germany', 'Austria']IN [19]: countries + countries_2
OUT[19]: 
['Canada',
 'United States',
 'India',
 'China',
 'Brazil',
 'UK',
 'Germany',
 'Austria']
```

我们可以把列表`countries`和`countries_2`放在另一个列表中，这叫做嵌套列表。

```
nested_list = [countries, countries_2]IN [20]: nested_list
OUT[20]: 
[['Canada', 'United States', 'India', 'China', 'Brazil'],
 ['UK', 'Germany', 'Austria']]
```

**移除元素** 有不同的方法可以帮助我们从列表中移除元素。让我们用我们创建的第一个`countries`列表来测试它们。

```
# remove(): Removes the first matching value.
IN [21]: countries.remove('United States')
IN [22]: countries
OUT[22]: ['India', 'China', 'Brazil']# pop(): Removes an item at a specific index and then returns it.
IN [23]: countries.pop(0)
IN [24]: countries
OUT[24]: ['China', 'Brazil']# del: Removes an item at a specific index
IN [25]: **del** countries[0]
IN [26]: countries
OUT[26]: ['Brazil']
```

**排序列表**
我们可以很容易地使用`.sort()`方法对列表进行排序。让我们创建一个名为`numbers` 的新列表，然后从最小到最大的数字对其进行排序。

```
numbers = [4, 3, 10, 7, 1, 2]
IN [27]: numbers.sort()
IN [28]: numbers
OUT[28]: [1, 2, 3, 4, 7, 10]
```

我们可以在`.sort()`方法中添加`reverse`参数来控制顺序。如果我们希望它是后代，我们设置`reverse=True`。

```
numbers.sort(reverse=True)
```

**更新列表中的值**
为了更新列表中的值，我们使用索引来定位我们想要更新的元素，然后使用`=`符号将其设置为新值。

```
numbers[0] = 1000
IN [29]: numbers
OUT[29]: [1000, 2, 3, 4, 7, 10]
```

**复制列表**
每当我们需要复制一个已有的列表时，我们可以使用以下选项:

```
# option 1: [:]
new_list = countries[:]# option 2: .copy()
new_list_2 = countries.copy()
```

`[:]`来源于我们之前学过的刀法。在这种情况下，我们既没有设置开始值，也没有设置停止值，因此选择了整个列表。另一方面，`.copy()`方法显式地复制了一个列表。

请注意，使用下面的代码复制一个对象不会像我们使用前面的选项那样创建一个独立的副本。

```
new_list_3 = countries
```

# 5.Python 词典

在 Python 中，字典帮助我们将数据存储在`key:value` 对中(一对称为一个项目)。和列表一样，字典也是可变的，但是和列表不同，字典是通过键访问的，不允许使用重复的键。

字典有以下形式:

```
my_dict = {'key1':'value1', 'key2':'value2'}
```

让我们用一些关于我的信息来创建我们的第一本字典。

```
my_data = {'name':'Frank', 'age':26}
```

让我们看看如何在字典中访问一个值以及一些常用的方法。

```
my_data['name'] # returns value that corresponds to "name" key
my_data.keys() # returns all keys of the dictionary
my_data.values() # returns all values of the dictionary
my_data.items() # returns the pairs key-value of a dictionary
```

## 添加和更新元素

类似于列表，我们可以在字典中添加和更新值。让我们从添加一个新的表示我的身高的对键值开始。

```
my_data['height']=1.7
```

运行上面的代码后，字典获得了一个新条目。

```
IN [30]: my_data
OUT[30]: {'name': 'Frank', 'age': 26, 'height': 1.7}
```

如果我添加的数据不正确，我们可以用`.update()`方法更新它，甚至添加更多的数据。

让我们更新我的身高，并添加一个新的关键。

```
my_data.update({'height':1.8,'languages':['English', 'Spanish']})
```

现在我的字典有以下条目:

```
{'name': 'Frank',
 'age': 26,
 'height': 1.8,
 'languages': ['English', 'Spanish']}
```

## 复制字典并删除元素

我们可以使用。方法来制作字典的副本。

```
new_dict = my_data.copy()
```

`new_dict`字典将拥有与原来的`my_data` 字典相同的条目。

我们可以使用列表中使用的相同方法来删除字典条目。

```
# pop(): Removes an item by specifying its key
IN [35]: my_data.pop('height')
IN [36]: my_data
OUT[36]: {'name': 'Frank', 'age': 26, 'languages': ['English', 'Spanish']}# del: Removes an item with a specific key
IN [37]: **del** my_data['languages']
IN [38]: my_data
OUT[38]: {'name': 'Frank', 'age': 26}# clear(): Removes all items in a dictionary
IN [39]: my_data.clear()
IN [40]: my_data
OUT[40]: []
```

# 6.如果语句

if 语句是 Python 中最常用的语句之一。这是一个条件语句，用来决定一个语句(或语句块)是否被执行。

下面是 if 语句的语法:

```
**if** <condition>:
    <code>
**elif** <condition>:
    <code>
...
**else**:
    <code>
```

让我们通过一个简单的例子来看看 if 语句的实际应用。下面的代码将根据我们在一个`age`变量中引入的值输出一条消息。

```
age = 18**if** age>=18:
    **print**("You're an adult!")
**elif** age>=13:
    **print**("You're a teenager!")
**else**:
    **print**("You're a kid")
```

下面的代码是说“如果年龄等于或大于 18 岁，你是成年人；如果年龄在 13 到 17 岁之间，你是青少年；如果年龄小于 13 岁，你就是个孩子”

但这还不是全部！我们可以使用带有关键字`in`的`if`语句来验证一个元素是否在列表中。

让我们核实一下中国是否在我们的“国家”列表中

```
countries = ['United States', 'India',
             'China', 'Brazil']**if** 'China' **in** countries:
    **print**("Country is in list")
**else**:
    **print**("Not in list")
```

在本例中，输出将是“国家在列表中”

# 7.For 循环

for 循环是 Python 中经常使用的循环。它允许我们遍历一个 iterable 对象(例如，列表、字典)并对每个元素执行相同的操作。

下面是 for 循环的语法:

```
**for** <variable> **in** <list>:
 <code>
```

让我们遍历我们的`countries`列表并打印所有的元素。

```
**for** country **in** countries:
 **print**(country)United States
India
China
Brazil
```

我们可以一起使用 for 循环和 if 语句来只对某些元素执行操作。例如，让我们遍历同一个列表，但现在只打印元素“美国”

```
**for** country **in** countries:
    **if** country == "United States":
        **print**(country)United States
```

## 枚举循环中的元素

有时候我们需要在一个循环中枚举元素。我们可以用 for 循环来实现。

```
**for** i, element **in** **enumerate**(<iterable>):
    <code>
```

让我们循环并枚举`countries`列表中的元素。

```
**for** i, country **in** **enumerate**(countries):
    **print**(i)
    **print**(country)0
United States
1
India
2
China
3
Brazil
```

## 遍历字典元素

我们可以在遍历字典时分别获得键和项。我们只需要使用`.items()`方法。

```
**for** key, value **in** my_dict.items():
    <code>
```

让我们遍历`my_data`字典并获取键和值。

```
**for** key, value **in** my_data.items():
    **print**(key)
    **print**(value)
```

# 8.Python 函数

Python 有很多内置函数，可以帮助我们执行特定的任务。我们来看看其中最常见的。

```
**len**() -> calculates the length of any iterable object
**max**() -> returns the item with the highest value in an iterable
**min**() -> returns the item with the lowest value in an iterable
**type**() -> returns the type of an object
**range**() -> returns a sequence of numbers
```

我们也可以创建自己的函数。下面是用于创建函数的语法:

```
**def** function(<params>):
    <code>
    **return** <data>
```

看起来很简单，对吗？让我们创建一个对两个值`a`和`b`求和的函数。

```
**def** sum_values(a,b):
    x = a+b
    **return** x
```

每次我们调用函数`sum_values`时，我们都会对`a`和`b`的值求和。

```
IN []: sum_values(2,3)
OUT[]: 5
```

就是这样！在学习任何数据科学库之前，这些都是您需要了解的 Python 核心概念。

# 用于数据科学的 Python 备忘单

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。每月 5 美元，让您可以无限制地访问数以千计的 Python 指南和数据科学文章。如果你使用[我的链接](https://frank-andrade.medium.com/membership)注册，我会赚一小笔佣金，不需要你额外付费。

[](https://frank-andrade.medium.com/membership) [## 通过我的推荐链接加入媒体——弗兰克·安德拉德

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

frank-andrade.medium.com](https://frank-andrade.medium.com/membership)