# 如何计算 Python 中列表项的出现次数

> 原文：<https://towardsdatascience.com/count-occurrences-of-python-list-item-e87782d8954c?source=collection_archive---------21----------------------->

## 了解如何在 Python 中计算特定列表元素的出现次数

![](img/db46be3ac49aa8dcf451d07310312d4c.png)

[斯蒂夫·约翰森](https://unsplash.com/@steve_j?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/count-number?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

统计列表元素在 Python 中出现的次数绝对是一项相当常见的任务。在今天的简短指南中，我们将探索几种不同的方法来计算列表项目。更具体地说，我们将探索如何

*   统计单个列表项出现的次数
*   对多个列表项的出现次数进行计数

我们将展示如何使用特定的列表方法或借助`collections`模块来实现这两者。

首先，让我们创建一个示例列表，我们将在本指南中引用它来演示一些概念。

```
>>> my_lst = [1, 1, 2, 3, 1, 5, 6, 2, 3, 3]
>>> my_lst
[1, 1, 2, 3, 1, 5, 6, 2, 3, 3]
```

## 计算单个列表项出现的次数

当计算一个特定元素在列表中出现的次数时，你的第一个选择是`[list.count()](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)`方法。例如，为了计算`1`在列表`my_lst`中出现的次数，您只需运行以下命令

```
**>>> my_lst.count(1)**
3
```

或者，你甚至可以使用`collections`模块附带的`Counter`类。`collections.Counter` class 是`dict`的子类，用于统计可散列对象。本质上，它是一个集合，用于观察计数的元素是否存储为字典键，以及它们对应的计数是否存储为字典值。

```
>>> from collections import Counter
>>>
**>>> Counter(my_lst)[1]**
3
```

## 计算多个列表项出现的次数

现在，如果你想计算多个条目在一个给定列表中出现的次数，你有两个选择。

第一种选择是在 for 循环中调用`list.count()`。假设您想要计算列表元素`1`、`3`和`5`在我们的列表中出现的次数。您可以这样做，如下所示:

```
>>> lookup = [1, 3, 5]
>>> **[my_lst.count(e) for e in lookup]**
[3, 3, 1]
```

或者，您可以使用`collections.Counter`类，在我看来，它更适合计算多个项目的出现次数。

如果您想要**计算列表中每个元素出现**的次数 **，那么您可以简单地创建一个`Counter`的实例。**

```
>>> from collections import Counter
>>>
**>>> Counter(my_lst)
Counter({1: 3, 2: 2, 3: 3, 5: 1, 6: 1})**
```

如果您想要**统计** **元素子集**在列表中出现的次数，那么下面的命令将会完成这个任务:

```
>>> from collections import Counter
>>>
**>>> lookup = [1, 3, 5]
>>> all_counts = Counter(my_lst)
>>> counts = {k: all_counts[k] for k in lookup}**
>>> counts
{1: 3, 3: 3, 5: 1}
```

## 最后的想法

在今天的简短指南中，我们探索了几种不同的方法来计算 Python 中列表项的出现次数。我们展示了如何使用列表方法`count()`或`collections.Counter`类来计算一个或多个列表项的出现次数。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](https://medium.com/geekculture/use-performance-visualization-to-monitor-your-python-code-f6470592a1cb) [## 使用性能可视化来监控 Python 代码

### 如何持续测量、可视化和提高 Python 代码在生产中的性能

medium.com](https://medium.com/geekculture/use-performance-visualization-to-monitor-your-python-code-f6470592a1cb) [](/16-must-know-bash-commands-for-data-scientists-d8263e990e0e) [## 数据科学家必须知道的 16 个 Bash 命令

### 探索一些最常用的 bash 命令

towardsdatascience.com](/16-must-know-bash-commands-for-data-scientists-d8263e990e0e) [](/4-amazingly-useful-additions-in-python-3-9-732115c59c9d) [## Python 3.9 中 4 个非常有用的新增功能

towardsdatascience.com](/4-amazingly-useful-additions-in-python-3-9-732115c59c9d)