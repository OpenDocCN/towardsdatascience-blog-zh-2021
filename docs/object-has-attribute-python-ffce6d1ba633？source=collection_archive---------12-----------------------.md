# 在 Python 中检查对象是否具有属性

> 原文：<https://towardsdatascience.com/object-has-attribute-python-ffce6d1ba633?source=collection_archive---------12----------------------->

## Python 编程

## 了解如何确定 Python 对象是否具有特定属性

![](img/4e010e245f851d8add86afb178f10e86.png)

丹尼尔·施鲁迪在 [Unsplash](https://unsplash.com/s/photos/object?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

Python 是一种通用编程语言，作为标准库的一部分，它具有相当丰富的内置功能。此外，由于它的流行，它也有一个活跃的社区，这意味着有无数的第三方库和模块建立在内置功能之上。

因此，如果不查看相应的文档，很难记住每个模块、类甚至方法是如何工作的。在今天的文章中，我们将讨论如何通过编程来确定一个特定的对象是否具有特定的属性。

具体来说，我们将探索如何

*   确定对象是否具有特定属性
*   列出对象的所有属性
*   列出实例化对象的属性值

首先，让我们创建几个伪 Python 类，我们将在本文中引用它们，以演示一些概念并提供一些更实际的例子。

```
class Vehicle: def __init__(self, no_of_wheels, color):
        self.no_of_wheels = no_of_wheels
        self.color = color

    def print_characteristics(self):  
        print(
            f'No. of wheels: {self.no_of_wheels}\n'
            f'Color: {self.color}'
        ) class Car(Vehicle): def __init__(self, no_of_wheels, color, is_electrical):
        super().__init__(no_of_wheels, color)
        self.is_electrical = is_electrical def print_characteristics(self):  
        print(
            f'No. of wheels: {self.no_of_wheels}\n'
            f'Color: {self.color}\n'
            f'Is Electrical: {self.is_electrical}\n'
        ) def paint_car(self, new_color):
       self.color = new_color class Bicycle(Vehicle): def  __init__(self, no_of_wheels, color, is_mountain):
        super().__init__(no_of_wheels, color)
        self.is_mountain = is_mountain def print_characteristics(self):  
        print(
            f'No. of wheels: {self.no_of_wheels}\n'
            f'Color: {self.color}\n'
            f'Is Mountain: {self.is_mountain}\n'
       )
```

## 检查对象是否具有特定属性

如果你想确定一个给定的对象是否有特定的属性，那么`[**hasattr()**](https://docs.python.org/3/library/functions.html#hasattr)`方法就是你要找的。该方法接受两个参数，字符串格式的对象和属性。

```
hasattr(object, name)
```

如果提供的字符串对应于某个对象属性的名称，该方法将返回`True`，否则将返回`False`。

例如，考虑下面的检查

```
>>> car = Car(4, 'white', True)
>>> bike = Bicycle(2, 'blue', False)
>>> **hasattr(car,  'paint_car')**
True
>>> **hasattr(bike, 'paint_car')**
False
```

或者，您甚至可以调用`[getattr()](https://docs.python.org/3/library/functions.html#getattr)`方法并捕捉当对象没有指定属性时引发的`AttributeError`。本质上，这是我们之前讨论的`hasattr()`方法的实际实现。

```
car = Car(4, 'white', True)**try:
    getattr(car, 'is_mountain')
except AttributeError:
    # Do something
    pass**
```

## 列出对象的所有属性

另一个有用的命令是`[**dir()**](https://docs.python.org/3/library/functions.html#dir)`，它返回一个包含指定对象属性的列表。本质上，这个方法将返回包含在`__dict__`属性中的键。请注意，如果您覆盖了`__getattr__`属性，这种行为可能会被修改，因此`dir()`结果可能不准确。

```
>>> from pprint import prrint
>>>
>>> bike = Bicycle(2, 'blue', False)
>>> **pprint(dir(bike))**
['__class__',
'__delattr__',
'__dict__',
'__dir__',
'__doc__',
'__eq__',
'__format__',
'__ge__',
'__getattribute__',
'__gt__',
'__hash__',
'__init__',
'__init_subclass__',
'__le__',
'__lt__',
'__module__',
'__ne__',
'__new__',
'__reduce__',
'__reduce_ex__',
'__repr__',
'__setattr__',
'__sizeof__',
'__str__',
'__subclasshook__',
'__weakref__',
'color',
'is_mountain',
'no_of_wheels',
'print_characteristics']
```

## 列出实例化对象的属性值

最后，如果你想列出一个实例化对象的属性值，那么你可以调用`[vars()](https://docs.python.org/3/library/functions.html#vars)`方法，该方法将返回指定对象的`__dit__`属性。

```
>>> from pprint import prrint
>>>
>>> bike = Bicycle(2, 'blue', False)
**>>> pprint(bike.vars())
{'color': 'blue', 'is_mountain': False, 'no_of_wheels': 2}**
```

## 最后的想法

在今天的简短指南中，我们探讨了如何找出某个特定属性是否属于某个类。此外，我们讨论了如何列出指定对象的所有属性，以及如何使用`vars()`推断实例化对象的实际属性值。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](/16-must-know-bash-commands-for-data-scientists-d8263e990e0e) [## 数据科学家必须知道的 16 个 Bash 命令

### 探索一些最常用的 bash 命令

towardsdatascience.com](/16-must-know-bash-commands-for-data-scientists-d8263e990e0e) [](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) [## 使用预提交挂钩自动化 Python 工作流

### 什么是预提交钩子，它们如何给你的 Python 项目带来好处

towardsdatascience.com](/automating-python-workflows-with-pre-commit-hooks-e5ef8e8d50bb) [](https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39) [## 用于日常编程的 11 个 Python 一行程序

### 令人惊叹的 Python 片段不会降低可读性

better 编程. pub](https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39)