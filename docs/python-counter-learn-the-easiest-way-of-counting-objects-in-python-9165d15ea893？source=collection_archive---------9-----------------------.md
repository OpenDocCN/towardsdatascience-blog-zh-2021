# Python 计数器:学习用 Python 计算对象的最简单方法

> 原文：<https://towardsdatascience.com/python-counter-learn-the-easiest-way-of-counting-objects-in-python-9165d15ea893?source=collection_archive---------9----------------------->

## 为初学者提供易于理解的示例

![](img/d33c4e447ed8c3fbdf0218a2d5481089.png)

[斯蒂夫·约翰森](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

开发应用程序时，您可能需要完成一些任务，包括对容器对象中的元素进行计数。您可能需要计算文本中的词频，或者您可能想知道列表对象中最常用的数字。对于这种情况，Python 在其**集合模块**中提供了一个方便的工具，一个名为 **Counter()** 的内置类。

在这篇短文中，我将尝试用简单易懂的例子向初学者解释如何使用**计数器**类。

# **Python 中的计数器类**

你可以在下面的[**docs.python.org**](https://docs.python.org/3/library/collections.html#collections.Counter)找到计数器类的解释

> 一个`[Counter](https://docs.python.org/3/library/collections.html#collections.Counter)`是一个`[dict](https://docs.python.org/3/library/stdtypes.html#dict)`子类，用于计数可散列对象。它是一个集合，其中的元素存储为字典键，它们的计数存储为字典值。计数可以是任何整数值，包括零或负计数。`[Counter](https://docs.python.org/3/library/collections.html#collections.Counter)`类类似于其他语言中的包或多集。

简而言之， **Counter** 接受一个 iterable 对象(比如一个字符串、一个列表或一个 dict 对象),并给出对象计数，只要这些对象是可散列的。

在下面的代码片段中，我创建了一个列表、一个字符串和一个 dictionary 对象。当它们被传入**计数器类**时，它返回一个字典，其中键是元素，它们的计数是返回的字典的值。

```
**from collections import Counter**a_list = [1,2,2,3,4,4,4]
a_string = 'data'
a_dict = {'a': 5, 'b':3, 'c':5, 'd':5, 'e':1 }**#counting objects in a list**
c_list = Counter(a_list)**#counting characters in a string**
c_string = Counter(a_string)**#counting values in a dictionary**
c_dict = Counter(a_dict.values())print (c_list)
print (c_string)
print (c_dict)***Output:***
*Counter({4: 3, 2: 2, 1: 1, 3: 1}) 
Counter({'a': 2, 'd': 1, 't': 1}) 
Counter({5: 3, 3: 1, 1: 1})*
```

# **对不可清洗的对象使用计数器类**

请注意，如果容器对象中的元素是不可哈希的类型，比如 list 对象，就会出现错误。

```
**from collections import Counter**a_list  = [1,2,[5,6,7,7],2,3,4,4,4]**#counting objects in a list**
c_list = Counter(a_list)print (c_list)***Output:*** TypeError: unhashable type: 'list'
```

解决方法是在将对象传递给**计数器类之前，将任何列表元素转换成**元组**。请看下面你如何用一行代码实现它；**

```
**from collections import Counter**a_list  = [1,2,[5,6,7,7],2,3,4,4,4]**#counting objects in a list which has unhashable list object** c_list = **Counter**(**tuple**(item) **if** type(item) **is** list **else** item **for** item **in** a_list)print (c_list)***Output:*** Counter({4: 3, 2: 2, 1: 1, (5, 6, 7, 7): 1, 3: 1})
```

由于列表元素被转换为元组对象，现在 Counter 类工作时没有错误消息。

# 计数器类中的方法

**Counter()** 是**字典类**的子类，因此**计数器对象**支持字典中可用的方法。除此之外，还有另外三种方法是计数器类独有的。

## 寻找最常见的元素

为了找到集合对象中最常见的元素，可以使用`**most_common**` ([ *n* ])方法。

`**elements**`()方法返回一个遍历元素的迭代器，每个元素重复它的计数。

使用`**subtract**`([*iterable-or-mapping*])方法，您可以从 iterable 或 dictionary 对象中减去元素计数。

```
**from collections import Counter**a_text = **'When developing your application, you may need to overcome some tasks involving counting the elements in a container object. You may need to count word frequencies in a text or you may want to know the most frequently used number in a list object.'**a_text_splited = a_text.**split()**
a_counter = **Counter**(a_text_splited)
most_occur = a_counter.**most_common(5)**print(most_occur)
print(**sorted**(a_counter.**elements()**))***Output:*** [('may', 3), ('to', 3), ('in', 3), ('a', 3), ('you', 2)]['When', 'You', 'a', 'a', 'a', 'application,', 'container', 'count', 'counting', 'developing', 'elements', 'frequencies', 'frequently', 'in', 'in', 'in', 'involving', 'know', 'list', 'may', 'may', 'may', 'most', 'need', 'need', 'number', 'object.', 'object.', 'or', 'overcome', 'some', 'tasks', 'text', 'the', 'the', 'to', 'to', 'to', 'used', 'want', 'word', 'you', 'you', 'your']
```

# 关键要点和结论

在这篇短文中，我解释了 Python **Counter() class** 以及它如何帮助您对 iterable 中的对象进行计数。关键要点是:

*   计数器接受一个可迭代对象(比如一个字符串，一个列表，或者一个字典对象),只要对象是可哈希的，就给出对象的计数。
*   使用**计数器类**时，如果容器对象中的元素是不可销毁的类型，比如 list 对象，就会出现错误。解决方法是在将对象传递给**计数器类之前，将任何列表元素转换成**元组**。**
*   为了找到一个集合对象中最常见的元素，可以使用`**most_common**` ([ *n* ])方法。

我希望你已经发现这篇文章很有用，并且**你将开始在你自己的代码中使用 Python 计数器类**。