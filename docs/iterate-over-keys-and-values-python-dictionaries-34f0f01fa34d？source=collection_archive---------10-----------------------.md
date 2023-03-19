# 如何迭代 Python 字典中的键和值

> 原文：<https://towardsdatascience.com/iterate-over-keys-and-values-python-dictionaries-34f0f01fa34d?source=collection_archive---------10----------------------->

## 讨论如何在 Python 中迭代字典的键和值

![](img/cddb49cd8145621785bf7db43be4370b.png)

劳伦·曼克在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

字典是 Python 中最有用的数据结构之一。迭代字典的键和值是我们在处理这类对象时需要执行的最常用的操作之一。

在今天的简短指南中，我们将探讨如何在 Python 3 中迭代字典。具体来说，我们将讨论如何

*   仅迭代键
*   仅迭代值
*   一次性遍历键和值

首先，让我们创建一个示例字典，我们将在整篇文章中引用它来演示一些概念。

```
my_dict = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

## 遍历键

为了只迭代键，您需要调用`[keys()](https://docs.python.org/3/library/stdtypes.html#dict.keys)`方法，该方法返回字典键的新视图。

```
**for key in my_dict.keys():**
    print(key) *# Output
a
b
c
d*
```

## 迭代值

为了迭代字典的值，您只需要调用`[values()](https://docs.python.org/3/library/stdtypes.html#dict.values)`方法，该方法返回包含字典值的新视图。

```
**for value in my_dict.values():**
    print(value)# Output
*1
2
3
4*
```

## 对键和值进行迭代

现在，如果你需要一次遍历键和值，你可以调用`[items()](https://docs.python.org/3/library/stdtypes.html#dict.items)`。该方法将以`(key, value)`的形式返回一个包含键值对的元组。

```
**for key, value in my_dict.items():** print(f'Key: {key}, Value: {value}') # Output
*Key: a, Value: 1
Key: b, Value: 2
Key: c, Value: 3
Key: d, Value: 4*
```

请注意，上面探讨的三种方法返回的对象返回**视图对象**。换句话说，这些对象提供了字典中条目的动态视图，这意味着一旦条目被修改，视图对象也会反映这种变化。

## Python 2 中的字典迭代

Python 2 中的符号与 Python 3 略有不同。为了迭代键值，你需要调用如下所示的`iteritems()`方法:

```
for key, value in my_dict.iteritems():
print(f'Key: {key}, Value: {value}')# Output
*Key: a, Value: 1
Key: b, Value: 2
Key: c, Value: 3
Key: d, Value: 4*
```

## 最后的想法

在今天的文章中，我们讨论了如何使用`keys()`、`values()`和`items()`方法一次独立或同时迭代键和值。此外，我们讨论了如何使用 Python 2 符号和`iteritems()`方法迭代键和值。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒介上的每一个故事。你的会员费直接支持我和你看的其他作家。**

**你可能也会喜欢**

[](/how-to-upload-your-python-package-to-pypi-de1b363a1b3) [## 如何将 Python 包上传到 PyPI

towardsdatascience.com](/how-to-upload-your-python-package-to-pypi-de1b363a1b3) [](https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39) [## 用于日常编程的 11 个 Python 一行程序

### 令人惊叹的 Python 片段不会降低可读性

better 编程. pub](https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39) [](/6-tips-to-help-you-stand-out-as-a-python-developer-2294d15672e9) [## 帮助你脱颖而出成为 Python 开发者的 6 个技巧

### Python 开发人员和数据科学家的实用技巧

towardsdatascience.com](/6-tips-to-help-you-stand-out-as-a-python-developer-2294d15672e9)