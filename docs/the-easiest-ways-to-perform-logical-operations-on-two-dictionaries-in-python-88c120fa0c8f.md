# 在 Python 中对两个字典执行逻辑运算的最简单方法

> 原文：<https://towardsdatascience.com/the-easiest-ways-to-perform-logical-operations-on-two-dictionaries-in-python-88c120fa0c8f?source=collection_archive---------28----------------------->

## 交、并、差和对称差

![](img/8b8c2bc7e7df52029c236d8cf4a284bc.png)

[来自 Unsplash](https://unsplash.com/photos/nShLC-WruxQ)

在本文中，我们将看一看在 Python 中寻找两个字典的交集、并集、差集和对称差集的最简单的方法，都是在只有键和键值对的级别上。

让我们考虑 2 个字典，它们具有:

*   `int`类型的值(因此我们也可以使用数学运算)，
*   一些共同的钥匙，
*   一些共同的键值对，
*   其他键-值对特定于每个字典。

```
d1 = {'a':1, 'b':2, 'c':3, 'd':4, 'e':5, 'f':6}
d2 = {'c':3, 'd':4, 'e':50, 'f':60, 'g':7, 'h':8}
```

这里的目的是为以下每个逻辑操作创建一个新的*字典*:

**交集(“与”):**

*   两个字典中相同的键值对，
*   在两个字典中相同的关键字，在一些关键字的值在两个字典中不同的情况下，将一些规则应用于它们相应的值；

**Union ("or"):**

*   如果两个字典中的键值不同，一些规则应用于相同的键值；

**差异(“非”):**

*   取在一个字典中存在而在另一个字典中不存在的键-值对(即使相同的键可以以不同的值存在于另一个字典中)，
*   只取存在于一个字典中的关键字及其相应的值；

**对称差:**

*   只取在任何一个字典中存在而在另一个字典中不存在的键，以及它们相应的值。

# 交集

为了找到字典条目的交集(即共同的键-值对)，我们可以使用下面的代码:

```
inter = dict(d1.items() & d2.items())
inter**Output:** {'d': 4, 'c': 3}
```

如果我们有兴趣找到所有共同的键，即使其中一些键的值在我们的字典中是不同的？我们可以决定从一个字典中保留这些键的值，比如说从`d1`:

```
inter_d1 = {k:v for k,v in d1.items() if k in d2}
inter_d1**Output:** {'c': 3, 'd': 4, 'e': 5, 'f': 6}
```

或者来自`d2`:

```
inter_d2 = {k:v for k,v in d2.items() if k in d1}
inter_d2**Output:** {'c': 3, 'd': 4, 'e': 50, 'f': 60}
```

或者，我们可能希望从两个字典中共同获取每个键的两个值，对它们应用一个函数，并将结果用作新字典中该键的值:

```
inter_max = {k:max(d1[k],d2[k]) for k in d1.keys() if k in d2}
inter_min = {k:min(d1[k],d2[k]) for k in d1.keys() if k in d2}
inter_sum = {k:d1[k]+d2[k] for k in d1.keys() if k in d2}print(inter_max)
print(inter_min)
print(inter_sum)**Output:** {'c': 3, 'd': 4, 'e': 50, 'f': 60}
{'c': 3, 'd': 4, 'e': 5, 'f': 6}
{'c': 6, 'd': 8, 'e': 55, 'f': 66}
```

当然，我们可以使用任何用户定义的函数来实现这些目的:

```
def extraction_twice(a,b):
    return (a-b)*2inter_extraction_twice = {k:extraction_twice(d2[k],d1[k]) for k in d1.keys() if k in d2}
inter_extraction_twice**Output:** {'c': 0, 'd': 0, 'e': 90, 'f': 108}
```

同样，在值为字符串类型的情况下:

```
d1_new = {1:'my', 2:'our', 3:'your'}
d2_new = {1:'home', 2:'family', 4:'work'}def combine_strings(str1,str2):
    return str1+ ' '+ str2inter_string = {k:combine_strings(d1_new[k],d2_new[k]) for k in d1_new.keys() if k in d2_new}
inter_string**Output:** {1: 'my home', 2: 'our family'}
```

# 联盟

合并两个字典的代码类似于我们用来查找字典的键值对交集的代码，只是这里我们应该使用 merge 操作符:

```
union = dict(d1.items()|d2.items())
union**Output:** {'d': 4, 'c': 3, 'h': 8, 'f': 60, 'a': 1, 'e': 50, 'g': 7, 'b': 2}
```

*边注:*在 Python 3.9 中，这段代码简化为`d1|d2`。

这里的问题是键`c`的值取自`d2`，而`d`的值取自`d1`。为了确定这些值是从哪里获取的，有一个更好的选择——解包操作符`**`:

```
union_d1 = {**d2, **d1}
union_d1**Output:** {'c': 3, 'd': 4, 'e': 5, 'f': 6, 'g': 7, 'h': 8, 'a': 1, 'b': 2}
```

此处所有常用键的值均取自`d1`。要从`d2`中取出它们，我们必须镜像代码:

```
union_d2 = {**d1, **d2}
union_d2**Output:** {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 50, 'f': 60, 'g': 7, 'h': 8}
```

如前所述，我们可能希望从两个字典中为每个这样的键取两个值，对它们应用一个函数(标准的或用户定义的)，并将结果用作新字典中该键的值，而不是直接从其中一个字典中获取这些值。这里的方法包括:

*   创建其中一个字典的副本，
*   基于要应用的函数和来自另一个字典的值更新该副本，
*   将更新的副本与另一个字典合并。

```
d1_copy = d1.copy()
d1_copy.update({k:max(d1[k],d2[k]) for k in d2 if d1.get(k)})
union_max = {**d2, **d1_copy}
union_max**Output:** {'c': 3, 'd': 4, 'e': 50, 'f': 60, 'g': 7, 'h': 8, 'a': 1, 'b': 2}
```

在这种情况下，我们使用了`max()`函数。然而，我们可以使用任何其他标准的或用户定义的函数，就像我们之前在寻找交集时所做的那样。

# 差异

为了隔离某个字典所特有的条目(即使相同的键可能以不同的值出现在另一个字典中)，例如`d1`，我们可以使用下面两段代码中的一段:

```
dif_d1 = dict(d1.items() - d2.items())
dif_d1**Output:** {'f': 6, 'a': 1, 'e': 5, 'b': 2}
```

或者:

```
dif_d1 = {k:v for k,v in d1.items() if k not in d2 or d2[k]!=v}
dif_d1**Output:** {'a': 1, 'b': 2, 'e': 5, 'f': 6}
```

对于`d2`，代码必须被镜像:

```
dif_d2 = dict(d2.items() - d1.items())
dif_d2**Output:** {'h': 8, 'f': 60, 'e': 50, 'g': 7}
```

或者:

```
dif_d2 = {k:v for k,v in d2.items() if k not in d1 or d1[k]!=v}
dif_d2**Output:** {'e': 50, 'f': 60, 'g': 7, 'h': 8}
```

如果我们想将某个字典特有的键与它们相应的值隔离开来，例如`d1`，我们应该使用下面的字典理解:

```
dif_d1_keys={k:v for k,v in d1.items() if k not in d2}
dif_d1_keys**Output:** {'a': 1, 'b': 2}
```

对于`d2`:

```
dif_d2_keys={k:v for k,v in d2.items() if k not in d1}
dif_d2_keys**Output:** {'g': 7, 'h': 8}
```

# 对称差

最后，为了找到对称差异，即对于*和*字典唯一的关键字及其相应的值，我们需要:

*   为了根据最后一个公式找出两个字典的差异，
*   将得到的字典合并成一个。

```
sym_dif = {**{k:v for k,v in d1.items() if k not in d2}, **{k:v for k,v in d2.items() if k not in d1}}
sym_dif**Output:** {'a': 1, 'b': 2, 'g': 7, 'h': 8}
```

# 结论

总结一下，我们考虑了在 Python 中对两个字典执行主要逻辑操作以创建新字典的快速而全面的方法。我们发现我们的两个字典的交集、并集、差集和对称差集都是在只有键和键值对的层次上，有时对键使用不同的函数。当然，这些方法并不是唯一的，您可以提出其他想法，或者在 Python 的未来版本中，将会引入更明确的方法来完成这些任务。然而，建议的代码片段简洁、优雅，而且几乎都是一行程序。

感谢阅读！

如果你喜欢这篇文章，你也可以发现下面这些有趣的:

<https://betterprogramming.pub/read-your-horoscope-in-python-91ca561910e1>  </bar-plots-best-practices-and-issues-30f1ad32c68f>  </testing-birthday-paradox-in-faker-library-python-54907d724414> [## 在 Faker 库中测试生日悖论(Python)

towardsdatascience.com](/testing-birthday-paradox-in-faker-library-python-54907d724414)