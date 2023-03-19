# 关于 Python 字典数据结构的一切:初学者指南

> 原文：<https://towardsdatascience.com/everything-about-python-dictionary-data-structure-beginners-guide-fca53357ac81?source=collection_archive---------12----------------------->

## 在本文中，我们将重点介绍 Python 字典数据结构的完整演示。

![](img/58b5096c2e46e8a51b0fed6130886c14.png)

[Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [AltumCode](https://unsplash.com/@altumcode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# **目录**

*   什么是 Python 字典？
*   如何创建 Python 字典
*   如何从 Python 字典中访问值
*   如何向 Python 字典添加元素
*   如何从 Python 字典中移除元素
*   如何更改 Python 字典中的元素
*   如何迭代 Python 字典
*   Python 中的嵌套字典

# 什么是 Python 字典？

Python 字典是用于存储对象组的数据结构。它由键-值对的映射组成，其中每个键都与一个值相关联。它可以包含相同或不同数据类型的数据，是无序的，并且是可变的。

# 如何创建 Python 字典

# 空字典

要在 Python 中初始化一个空字典，我们可以简单地运行下面的代码并打印其内容:

您应该得到:

```
{}
```

# 带值的字典

当我们想要创建一个包含一些想要填充的值的字典时，我们将这些值作为一系列逗号分隔的键-值对来添加。例如，假设我们想要创建一个以国家为键、以人口为值的字典:

您应该得到:

```
{'China': 1439323776, 'India': 1380004385, 'USA': 331002651, 'Indonesia': 273523615, 'Pakistan': 220892340, 'Brazil': 212559417}
```

# 如何从 Python 字典中访问值

为了访问存储在 Python 字典中的值，我们应该使用与值相关联的键。例如，假设我们想从上面的**国家**字典中得到美国的人口。我们知道人口值的关键字是“USA ”,我们用它来访问人口值:

您应该得到:

```
331002651
```

注意:与 [Python list](https://pyshark.com/python-list-data-structure/) 不同，您不能使用索引从字典中访问值。访问这些值的唯一方法是搜索字典中存在的关键字。

# 如何向 Python 字典添加元素

在本节中，我们将继续使用 **countries** 字典，并讨论向 Python 字典添加元素的方法。

# 添加单个元素

假设我们想要将另一个国家及其人口添加到我们的**国家**字典中。例如，我们要添加日本的人口 126，476，461。通过将它作为一个额外的键值对添加到字典中，我们可以很容易地做到这一点:

您应该得到:

```
{'China': 1439323776, 'India': 1380004385, 'USA': 331002651, 'Indonesia': 273523615, 'Pakistan': 220892340, 'Brazil': 212559417, 'Japan': 126476461}
```

您可以看到，我们已经成功地向字典中添加了一个新元素。

# 添加多个元素

现在，如果我们想要添加多个国家呢？假设我们现在想在字典中再添加两个国家及其人口。例如，俄罗斯和墨西哥的人口分别为 145，934，462 和 128，932，753。

相同的语法会起作用吗？不完全是。所以我们需要这个**。update()**Python 字典数据结构的方法。它允许向字典中添加多个逗号分隔的键值对。

逻辑是从新的键-值对创建一个新字典( **new_countries** )，然后将其合并到 **countries** 字典中:

您应该得到:

```
{'China': 1439323776, 'India': 1380004385, 'USA': 331002651, 'Indonesia': 273523615, 'Pakistan': 220892340, 'Brazil': 212559417, 'Japan': 126476461, 'Russia': 145934462, 'Mexico': 128932753}
```

您可以看到，我们已经成功地向词典中添加了新元素。

# 如何从 Python 字典中移除元素

在本节中，我们继续使用 **countries** 字典，并讨论从 Python 字典中移除元素的方法。

# 移除单个元素

假设我们需要做一些修改，从字典中删除一个针对中国及其人口的键-值对。我们可以使用**轻松移除它。pop()** 方法:

您应该得到:

```
{'India': 1380004385, 'USA': 331002651, 'Indonesia': 273523615, 'Pakistan': 220892340, 'Brazil': 212559417, 'Japan': 126476461, 'Russia': 145934462, 'Mexico': 128932753}
```

您可以看到，我们已经成功地从字典中删除了一个元素。

# 移除多个元素

下一步是探索如何从 Python 字典中移除多个元素。假设我们想从**国家**字典中删除日本和墨西哥及其各自的人口。

我们知道**。pop()** 方法允许在每个函数调用中移除单个元素，这给了我们一个想法，如果我们迭代一个包含我们想要移除的键的[列表](https://pyshark.com/python-list-data-structure/)，我们就可以成功地调用**。pop()** 对于每个条目:

您应该得到:

```
{'India': 1380004385, 'USA': 331002651, 'Indonesia': 273523615, 'Pakistan': 220892340, 'Brazil': 212559417, 'Russia': 145934462}
```

您可以看到，我们已经成功地从字典中删除了元素。

# 如何更改 Python 字典中的元素

另一个功能是改变 Python 字典中的元素。您将在以下章节中看到，更改元素的功能与[添加元素](https://pyshark.com/python-dictionary-data-structure/#how-to-add-elements-to-a-python-dictionary)的功能相同。

这是为什么呢？这是因为当我们试图向字典中添加新元素时，Python 会查找我们试图添加的特定键，如果字典中存在该键，它会覆盖数据；但是如果键不存在，它会向字典中添加一个新的键-值对。

# 更改单个元素

假设我们想在**国家**字典中将巴西的人口值更新为 212560000:

您应该得到:

```
{'India': 1380004385, 'USA': 331002651, 'Indonesia': 273523615, 'Pakistan': 220892340, 'Brazil': 212560000, 'Russia': 145934462}
```

# 更改多个元素

现在，假设我们想在 **countries** 字典中将印度尼西亚和巴基斯坦人口的值分别更新为 273530000 和 220900000。

逻辑是从新的键值对创建一个新的字典( **update_countries** )，然后更新 **countries** 字典中现有的键值对:

您应该得到:

```
{'India': 1380004385, 'USA': 331002651, 'Indonesia': 273530000, 'Pakistan': 220900000, 'Brazil': 212560000, 'Russia': 145934462}
```

# 如何迭代 Python 字典

在这一节中，我们将关注 Python 字典的不同迭代方式。

# 迭代字典键

假设我们想要迭代 **countries** 字典中的键，并在单独的行上打印每个键(在我们的例子中是每个国家)。

我们将简单地使用一个**和**一起作为**循环。**keys()【字典法:

您应该得到:

```
India
USA
Indonesia
Pakistan
Brazil
Russia
```

# 迭代字典值

另一个用例可能是我们想要找到存储在 **countries** 字典中的所有国家的人口总数。

正如你所想象的，我们将需要再次使用**作为**循环，现在我们也将使用**。**字典法取值():

您应该得到:

```
2563931498
```

# 迭代字典条目

Python 字典的一项是它的键值对。这允许我们一起对键和值进行迭代。

我们如何使用它？假设您想从 **countries** 字典中找到人口最多的国家。迭代字典的每一项允许我们同时跟踪键和值:

您应该得到:

```
India 1380004385
```

# Python 中的嵌套字典

嵌套字典是由其他字典组成的字典。

你可以用类似于[创建字典](https://pyshark.com/python-dictionary-data-structure/#how-to-create-a-python-dictionary)的方式来创建嵌套字典。

例如，假设我们想要创建一个包含每个国家首都及其人口信息的字典:

您应该得到:

```
{'China': {'capital': 'Beijing', 'population': 1439323776}, 'India': {'capital': 'New Delhi', 'population': 1380004385}, 'USA': {'capital': 'Washington, D.C.', 'population': 331002651}, 'Indonesia': {'capital': 'Jakarta', 'population': 273523615}, 'Pakistan': {'capital': 'Islamabad', 'population': 220892340}, 'Brazil': {'capital': 'Brasilia', 'population': 212559417}}
```

# 结论

这篇文章是关于 [Python](https://www.python.org/) 字典及其方法的介绍性演练，学习这些方法很重要，因为它们被用于[编程](https://pyshark.com/category/python-programming/)和[机器学习](https://pyshark.com/category/machine-learning/)的许多领域。

如果你有任何问题或者对编辑有任何建议，请在下面留下你的评论。

*原载于 2021 年 11 月 21 日*[*【https://pyshark.com】*](https://pyshark.com/python-dictionary-data-structure/)*。*