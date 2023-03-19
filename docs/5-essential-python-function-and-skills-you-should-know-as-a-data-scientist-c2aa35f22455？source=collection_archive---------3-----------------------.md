# 作为数据科学家，您应该知道的 5 个基本 Python 函数和技能

> 原文：<https://towardsdatascience.com/5-essential-python-function-and-skills-you-should-know-as-a-data-scientist-c2aa35f22455?source=collection_archive---------3----------------------->

## 可以使你的程序更加简洁易读的函数和技巧

![](img/1b65bef724df644ec727d6e81954e445.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

在过去的几年里，我一直从事数据科学方面的工作，这涉及到大量的 python 工作。因此，随着时间的推移，我学到了一些技能，这使得使用 python 工作变得更加有趣和容易。这些技巧提高了代码的可读性，使代码紧凑，并为您节省了大量时间。因此，让我们深入了解每个此类功能的用例。

由[吉菲](https://giphy.com/)

# *是实例*

*isinstance* 是 python 的内置函数，用来判断一个对象是否属于某个特定类型。检查下面的例子；如果 *a* 是一个*整数*，则打印“a 是一个整数”，否则打印“a 不是一个整数”

```
if isinstance(a, int):
    print("a is an integer")
else:
    print("a is not an integer")if isinstance(c, list):
    print("c is a list")
else:
    print("c is not a list")
```

例如，我假设数据将是一个浮点数列表，并相应地编写了代码。然而，当通过我的代码时，数据变成了导致错误计算的字符列表。这时我们可以使用 *isinstance* 来检查数据类型是否确实是代码所期望的。

# 希腊字母的第 11 个

它是 python 中最重要和最强大的函数之一。这也是一个广泛使用的功能，所以我们有必要了解它的工作原理。 *lamba* 允许你定义一个函数，而不用使用 python *def 实际定义一个函数。*基本上，它帮助我们定义匿名函数，这些函数主要是初等的单行函数。虽然要定义一个复杂的函数，还是用 *def 来定义一个函数比较好。*

```
alpha = lambda x: x*3
print(alpha(3))
## output will be 9
print(alpha(4.5))
## output will be 13.5beta = lambda x: x>0
print(beta(4))
## output will be True
print(beta(-3))
## output will be Falsestudent_tuples = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
]
sorted(student_tuples, key=**lambda** student: student[2])
##output will be [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

当您需要处理整个数据的每个数据点时，例如，如上所示对数组进行排序，此函数非常有用。

# 活力

这个函数帮助你同时遍历两个或更多的列表。在下面的例子中，我们在一个循环中遍历三个列表，对元素进行索引求和，并将求和结果追加到一个新列表中。

```
alpha = [1,2,3,4,5,6]
beta = [7,8,9,10,11,12]
gamma = [13,14,15,16,17,18]
output = []for a, b, c in zip(alpha, beta, gamma):
    output.append(a+b+c)
```

作为一名数据科学家，这是很有用的，例如，在训练神经网络时。我们通常将输入存储为一个列表，并将相应的基本事实存储为一个单独的列表。为了训练模型，我们必须同时迭代这两个列表，以向网络提供输入，并使用来自模型的相应基础事实和预测来计算损耗。

# 过滤器

这个函数帮助你从列表中过滤数据。在下面的例子中，我们正在过滤奇数。代替 lambda，你也可以定义一个常规函数，并在这里作为参数给出。我有意使用 lambda 来展示 lambda 的另一个用例。

```
alpha = [1,2,3,4,5,6]filtered = list(filter(lambda x:x%2, alpha))
print(filtered)
## output will be [1, 3, 5]
```

不管怎么说，在数据科学中，我们必须做大量的数据过滤，所以过滤器是我们新的最好的朋友。

# 一行用于循环

这用于使代码更加紧凑。在下面的例子中，我比较了编写循环的传统方法和单行方法。

```
## conventional method
for i in range(10):
    print(i)## one line method
for i in range(10): print(i)## conventional method
output = []
for i in range(10): 
    output.append(i**2)## one line method
output=[]
for i in range(10): output.append(i**2)
```

上面的例子很简单，不会经常使用。现在，让我们来看一个在一行中创建列表的综合方法。

```
values = ['alpha', 'beta', 'gamma', 'phi']
# regular function
def ends_with_a(values):
    valid = []
    for word in values:
        if word[-1].lower() == 'a':
            valid.append(word)

    return valid
print(ends_with_a(values))
## output will be ['alpha', 'beta', 'gamma'] # list comprehension
filtered_list = [word for word in values if word[-1].lower() == 'a']
# results
print(filtered_list)
## output will be ['alpha', 'beta', 'gamma']
```

这里的第一部分展示了如何使用一个常规函数获得以' a '结尾的单词。第二部分展示了一种叫做列表理解的强大方法。我们基本上得到了相同的结果，但是第二部分的代码更加简洁。因此，一行循环可以广泛地用于编写干净的代码。

# 总结

上面提到的 python 技巧非常强大，作为一名数据科学家，应该充分了解它们并经常使用它们。从程序员的角度来看，使用这些函数可以使代码更好看，写起来更快。一定要练习这些技巧，并把它们应用到日常工作中去，以适应它们。

如果你知道任何这样的功能，我错过了，但发挥了重要作用，请让我知道。

*成为* [*介质会员*](https://medium.com/@AnveeNaik/membership) *解锁并阅读介质上的许多其他故事。关注我们的*[*Medium*](https://medium.com/@AnveeNaik)*，阅读更多此类博文*。