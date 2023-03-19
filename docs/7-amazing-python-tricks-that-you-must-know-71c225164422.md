# 你必须知道的 7 个惊人的 Python 技巧

> 原文：<https://towardsdatascience.com/7-amazing-python-tricks-that-you-must-know-71c225164422?source=collection_archive---------23----------------------->

## 对 python 程序员来说更容易但有用的技巧

![](img/4069ff9a6e3e115a76e2ee9e1b49d769.png)

信用:[像素](https://www.pexels.com/photo/trees-in-forest-327178/)

我们都知道 Python 代码比其他语言更简洁，可靠性更高。但是 Python 比你想象的更强大。Python 中有许多快捷方式和技巧可以使用，这肯定会使代码库更加精确，也会优化代码的执行。

在今天的文章中，我们将讨论 7 个可以在 Python 项目中使用的技巧和诀窍。大多数技巧并不总是被初学程序员使用，所以它也将帮助你脱颖而出！

# **1。** **计算你的代码的运行时间**

如果您正在比较不同的代码集以获得最佳解决方案，例如为您的问题选择最快的算法，那么计算代码的运行时间就变得非常重要。

这个小技巧一定会帮助你实现这个目标。这里我们将借助 python 自带的时间模块

```
import time
startTime = time.time()#Our main code starts herex = 10
y = 20
z = x+y
print(f”the sum is: “,z)#Our main code ends hereendTime = time.time()net = endTime — startTimeprint(f”time taken: “,net)
```

> **输出**

```
the sum is: 30time taken: 0.0008382797241210938
```

# **2。** **整体列表排版**

有时我们需要改变列表中所有条目的数据类型。关于这一点，通常会使用一个循环来完成。但是这个方法根本没有优化，我们可以使用 python 中的 map 函数使这个过程更快更简单。

```
list1 = [1,2,3,4,5]print(f”list in integer form: “, list1)list2 = list(map(float,list1))print(f”list in float form: “, list2)
```

> **输出**

```
list in integer form: [1, 2, 3, 4, 5]list in float form: [1.0, 2.0, 3.0, 4.0, 5.0]
```

# **3。** **求一个矩阵的转置**

在处理与数据科学相关的问题时，有时我们需要找到矩阵的转置。

虽然 NumPy 中有一个单独的方法来获得矩阵的转置，但是我们可以使用简单的 Python 和 zip()函数来实现。

```
matrix = [[1, 2, 3], [4, 5, 6]]transpose =zip(*matrix)for row in transpose:print(row)
```

> **输出**

```
(1, 4)(2, 5)(3, 6)
```

# **4。** **翻字典**

假设您面临一个必须反转字典的情况，即前一个字典中的键将是当前字典的值，反之亦然。

只需一行 python 代码，您就可以使用 for 循环实现这一点。但是在这里你不能在同一个字典中进行修改。您必须创建一个新的字典来存储键值对。

```
dict1={“key1”: 1, “key2”: 2, “key3”: 3, “key4”: 4}dict2={v: k for k, v in dict1.items()}print(dict2)
```

> **输出**

```
{1: ‘key1’, 2: ‘key2’, 3: ‘key3’, 4: ‘key4’}
```

**5。** **统计一个单词在句子中出现的次数:**

如果你想知道一个特定的单词在一组句子中出现的次数，最常用的方法是将一个句子中的单词拆分成一个列表，然后在列表中查找这个特定的单词。

但是 Python 中的`re`模块使得使用`findall()`方法变得更加容易。即使有多个句子，这种方法也非常有效。

```
import resentence = “I have 3 cats, my friend has 3 cats. In total we have 6 cats”len(re.findall(‘cats’, sentence))
```

> **输出**

```
3
```

# **6。** **枚举功能**

如果您想保留列表项的索引以及列表中的列表项，可以使用 Python 中的 enumerate()函数。这个函数在处理算法问题时特别有用。

```
names = [“Ram”,”Shyam”,”Vivek”,”Samay”]for index, element in enumerate(names):print(“Name at Index”, index, “is”, element)
```

> **输出**

```
Name at Index 0 is RamName at Index 1 is ShyamName at Index 2 is VivekName at Index 3 is Samay
```

# **7。** **合并两个列表形成一个字典**

假设您有一个商品列表和一个价格列表，您想将它们存储为一个字典，其中的`key:value`将是`items:price`。

一般的方法是使用 for 循环遍历这两个列表，并将它们合并到一个字典中。但是在 Python 中 zip 函数的帮助下，您可以用一行代码来完成。

```
items = [“jackets”, “shirts”, “shoes”]price = [100, 40, 80]dictionary = dict(zip(items, price))print(dictionary)
```

> **输出**

```
{‘jackets’: 100, ‘shirts’: 40, ‘shoes’: 80}
```

# **结论**

这些是一些提示和技巧，您可以在代码中实现它们来提高代码质量和优化。这些技巧也将帮助你减少代码的行数。

为了更好地理解，您可以添加注释来描述代码片段的功能，因为大多数代码都过于紧凑，初学者难以理解。我们将在下一篇文章中尝试介绍更多的 Python 技巧。

> *在你走之前……*

如果你喜欢这篇文章，并且想继续关注我即将发表的关于 **Python &数据科学**的**激动人心的**文章**——请点击这里[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)考虑成为一名中级会员。**

通过这种方式，会员费的一部分归我，这激励我写更多关于 Python 和数据科学的令人兴奋的东西。

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。