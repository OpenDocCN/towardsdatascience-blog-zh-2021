# Python 中的 defaultdict 对象

> 原文：<https://towardsdatascience.com/a-better-alternative-to-python-dictionaries-6eaf8b6c48d0?source=collection_archive---------31----------------------->

## 了解 Python 字典对象的更好替代方案

![](img/643cc3cc9ad056112b29e7a55f2e2c88.png)

戴维·舒尔茨在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

每个 Python 开发人员都知道 Python 中的字典对象。然而，还有另一个类似字典的对象，称为 ***defaultdict*** 对象，与常规字典对象相比，它提供了额外的功能。

下面我们通过一个例子来介绍一下这个 ***defaultdict*** 对象。

## 计数示例

假设我们想统计一个单词中每个字母出现的次数。我们将创建一个字典，用键作为字母，用它们的值作为出现的次数。

我们将使用大多数英语词典中最长的单词(根据谷歌):

```
word = 'pneumonoultramicroscopicsilicovolcanoconiosis'
```

我们可以使用我们都熟悉的 dictionary 对象来实现这一点，如下所示:

```
letter_count = dict()for letter in word:
    if letter not in letter_count:
        letter_count[letter] = 1
    else:
        letter_count[letter] += 1print(letter_count)**Output:**{'p': 2,
 'n': 4,
 'e': 1,
 'u': 2,
 'm': 2,
 'o': 9,
 'l': 3,
 't': 1,
 'r': 2,
 'a': 2,
 'i': 6,
 'c': 6,
 's': 4,
 'v': 1}
```

> 我们首先使用 **dict()构造函数**创建一个空字典对象。然后我们使用 for 循环遍历单词中的每个字母。当我们循环单词中的每个字母时，我们检查该字母是否已经是字典中的一个关键字。如果没有，我们将键设置为字母，并将其值设置为 1(因为这是该字母在我们的循环中第一次出现)。如果这个字母已经作为一个键存在，我们就在它的值上加 1。

尽管一个常规的 dictionary 对象可以工作，但是这里有两个问题。

首先，注意我们首先必须检查字典中是否已经存在这个键。否则，我们就会有一个 **KeyError** 。

```
for letter in word:
    letter_count[letter] += 1# KeyError
```

其次，如果我们试图检查字典中不存在的字母数，我们也会得到一个 **KeyError** 。

```
letter_count['p']
# 2letter_count['z']
# KeyError
```

当然，我们可以用字典 ***get()*** 的方法来解决第一个问题，然而第二个问题仍然存在:

```
for letter in word:
    letter_count_2[letter] = letter_count_2.get(letter, 0) + 1letter_count_2['p']
# 2letter_count_2['z']
# KeyError
```

> **get** 方法接受两个参数:我们想要检索的键的值，以及如果键不存在，我们想要分配给该键的值。

[](/sorting-a-dictionary-in-python-4280451e1637) [## 用 Python 对字典进行排序

### 如何用 python 对字典进行排序

towardsdatascience.com](/sorting-a-dictionary-in-python-4280451e1637) 

## defaultdict 对象

这是一个很好的例子，当使用一个[***default dict***](https://docs.python.org/3/library/collections.html#collections.defaultdict)*对象会很有帮助。 ***defaultdict*** 类是内置 ***dict*** 类的子类，也就是说它继承了它。因此，它具有与 ***dict*** 类相同的功能，但是，它接受一个额外的参数(作为第一个参数)。这个附加参数***default _ factory***将被调用来为一个尚不存在的键提供一个值。*

> **类*`collections.**defaultdict**`(*default _ factory = None*， */* ，*...* ])*

*我们来看看如何使用 ***defaultdict*** 来完成上述任务。*

*我们首先必须从集合模块导入 ***默认字典*** :*

```
*from collections import defaultdict*
```

*然后我们使用***default dict()***构造函数创建一个 ***defaultdict*** 对象:*

```
*letter_count_3 = defaultdict(int)*
```

*注意我们是如何将 ***int*** 函数作为***default _ factory***参数的参数传入的。如果某个键不存在，这个 ***int*** 函数将被不带任何参数地调用，以提供该键的默认值。*

```
*for letter in word:
    letter_count_3[letter] += 1*
```

> *因此，当我们在单词上循环时，如果那个键不存在(意味着我们第一次遇到一个字母)，则调用 **int** 函数，而不向它传递任何参数，它返回的将是那个键的值。在不传递任何参数的情况下调用 **int** 函数会返回整数 0。然后，我们在返回的 0 上加 1。*

*这解决了我们上面遇到的两个问题，因为检索任何不存在的键的值都将返回零:*

```
*letter_count_3['p']
# 2letter_count_3['z']
# 0*
```

*我们还可以传入一个 **lambda 函数**作为***default _ factory***参数，该参数不接受任何参数并返回值 0，如下所示:*

```
*letter_count_3 = defaultdict(lambda: 0)*
```

*如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [*链接*](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。**

*[](https://lmatalka90.medium.com/membership) [## 通过我的推荐链接加入媒体——卢艾·马塔尔卡

### 阅读卢艾·马塔尔卡的每一个故事(以及媒体上成千上万的其他作家)。您的会员费直接支持…

lmatalka90.medium.com](https://lmatalka90.medium.com/membership)* 

**我希望你喜欢这篇关于 Python 中****default dict****对象的文章。感谢您的阅读！**