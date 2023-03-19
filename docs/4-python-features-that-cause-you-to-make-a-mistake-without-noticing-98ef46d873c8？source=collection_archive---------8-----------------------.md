# 4 个 Python 特性让你在没有注意到的情况下犯了错误

> 原文：<https://towardsdatascience.com/4-python-features-that-cause-you-to-make-a-mistake-without-noticing-98ef46d873c8?source=collection_archive---------8----------------------->

## 不要让他们拖累你

![](img/5ee44f8a2d3247f9754b87cdd7c8bb5f.png)

Brando Louhivaara 在 [Unsplash](https://unsplash.com/s/photos/hidden?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

自从我踏入数据科学的第一步，我就一直在使用 Python。虽然我过分关注第三方库，如 Pandas 和 Scikit-learn，但学习基本 Python 也是至关重要的。

出于各种原因，Python 是一门伟大的语言。很容易学习和理解。你可以像阅读普通英语一样阅读 Python 代码。Python 也带来了快速简单的开发过程，我认为这是一个动力助推器。

另一方面，Python 有一些我们需要额外注意的特性。否则，我们可能会在不知不觉中犯一些错误。我在学习 Python 的旅途中其实也犯过这样的错误。

在本文中，我们将讨论 4 个 Python 特性，如果使用不当，它们可能会给你带来麻烦。

## 1.有效的方法

Python 提供了几种修改内置数据结构的方法。例如，sort 方法可用于对列表中的项目进行排序。

然而，这些方法中的一些在适当的位置工作，这意味着它们不返回任何东西。这些方法的一个潜在错误是用它们进行变量赋值。

假设我们有下面的列表，并且需要对它进行排序。

```
mylist = [1, 4, 2, 10, 5]mysortedlist = mylist.sort()
```

让我们检查排序列表中项目的顺序。

```
print(mysortedlist)
None
```

它实际上没有任何值，因为 sort 方法对原始列表中的项目进行排序，并且不返回任何内容。

```
print(mylist)
[1, 2, 4, 5, 10]
```

当你使用合适的方法时要小心。否则，您将最终拥有没有值的变量。

如果你不想修改原来的列表，你可以使用 sorted 函数，它不会改变原来的列表，而是返回一个排序后的列表。

```
mylist = ["d", "a", "c"]
mysortedlist = sorted(mylist)print(mysortedlist)
['a', 'c', 'd']
```

## 2.创建空集

Set 是 Python 中内置的数据结构。它类似于一个列表，但是它只包含唯一的值。

```
mylist = [1, 1, 2, 3, 3, 4]myset = set(mylist)print(myset)
{1, 2, 3, 4}
```

当我们将列表转换为集合时，重复项将被删除。集合表示为用逗号分隔的大括号中的值。因此，我们应该能够创建如下空集:

```
myset = {}
```

让我们检查一下 myset 变量的类型。

```
type(myset)
dict
```

这是一本字典，不是一套。Dictionary 是另一种内置的数据结构，用花括号中的键值对表示。中间没有项目的花括号会产生字典。

我们可以使用 set 函数创建一个空集。

```
myset = set()type(myset)
set
```

## 3.创建包含单个项目的元组

Tuple 是 Python 中的另一种内置数据结构。它用括号中的值表示，并用逗号分隔。

```
mytuple = ("Jane","John","Matt")type(mytuple)
tuple
```

元组类似于列表，但它们是不可变的。因此，我们不能更新元组中的值。

append 方法也不适用于元组。然而，我们可以将元组加在一起以创建另一个元组。

```
mytuple = ("Jane","John","Matt")mytuple + ("Ashley", "Max")
('Jane', 'John', 'Matt', 'Ashley', 'Max')
```

用一个条目创建一个元组有点棘手。虽然只有一项，但你应该在它后面加一个逗号。否则，你将得到一个字符串而不是一个元组。

```
mytuple = ("Jane")print(mytuple)
Janetype(mytuple)
str
```

让我们用逗号做同样的操作。

```
mytuple = ("Jane",)print(mytuple)
('Jane',)type(mytuple)
tuple
```

## 4.列表理解中的多个可重复项

列表理解是 Python 最好的特性之一。它提供了一种基于其他可迭代对象(如集合、元组、其他列表等)创建列表的方法。

列表理解也可以描述为用更简单、更吸引人的语法来表示 for 和 if 循环。不过，它们也比循环快。

这里有一个简单的例子。

```
a = [1, 2, 3]mylist = [x**2 for x in a]mylist
[1, 4, 9]
```

新列表包含列表 a 中每个项目的方块。列表理解允许在不同的可重复项中使用多个项目。例如，我们可以基于另外两个列表创建一个列表。

考虑以下列表 a 和 b。

```
a = [1,2,3]b = [6,7,8]
```

我想创建一个包含列表 a 和 b 中具有相同索引的产品的列表。因此，新列表将包含项目 6、14 和 24。下面的列表理解似乎是这个任务的一个很好的解决方案。

```
newlist = [x*y for x in a for y in b]
```

让我们检查结果。

```
newlist
[6, 7, 8, 12, 14, 16, 18, 21, 24]
```

这不是我们所需要的。不是将具有相同索引(1*6、2*7 和 3*8)的项目相乘，而是将第一个列表中的所有项目与第二个列表中的所有项目相乘。它实际上是一个叉积。

Python 为我们的问题提供了一个很好的解决方案:zip 函数。

```
newlist = [x*y for x, y in zip(a,b)]newlist
[6, 14, 24]
```

zip 函数返回一个 zip 对象，该对象包含两个列表中匹配项的元组。

```
list(zip(a, b))[(1, 6), (2, 7), (3, 8)]
```

我们已经讨论了 4 个不太明显的 Python 特性。它们可能会让你犯一些乍一看很难注意到的错误。然而，这样的错误可能会有严重的后果，尤其是当你在一个长脚本中使用它们的时候。

感谢您的阅读。如果您有任何反馈，请告诉我。