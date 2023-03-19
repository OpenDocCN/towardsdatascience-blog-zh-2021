# 解决 Python 编码问题的最佳方式

> 原文：<https://towardsdatascience.com/best-way-to-solve-python-coding-questions-376539450dd2?source=collection_archive---------5----------------------->

## 了解如何有效解决 Python 编码问题

![](img/ef2aa3950be4c8e5bfd04e24abe96f66.png)

韦斯利·廷吉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

关于 Python 编码网站(如 codewars 或 leetcode)的好处，以及使用它们是否真的能让我们成为更好的程序员，肯定存在一些争议。尽管如此，许多人仍然使用它们来准备 Python 面试问题，保持他们的 Python 编程技能，或者只是为了好玩。然而，对于任何 Python 程序员或数据科学家来说，这些资源肯定都有一席之地。

在本教程中，我们将看看从这些 python 编码问题中提取最大效用的最佳方式。我们将研究一个相当简单的 Python 编码问题，并通过适当的步骤来解决它。这包括首先用伪代码想出一个计划或大纲，然后从最简单的解决方案开始用不同的方法解决它。

# Python 编码问题

> 我们需要编写一个函数，它将单个整数值作为输入，并返回从零到该输入的整数之和。如果传入了非整数值，该函数应该返回 0。

因此，如果我们将数字 5 传递给函数，那么它将返回整数 0 到 5 的和，或 *(0+1+2+3+4+5)* ，等于 15。如果我们传入除了整数之外的任何其他数据类型，比如字符串或浮点数等，函数应该返回 0。

</a-skill-to-master-in-python-d6054394e073>  

# 制定计划

我们应该做的第一件事是用伪代码解决这个问题。伪代码只是一种不用担心编码语法就能规划出我们的步骤的方法。

我们可以试着这样做:

```
def add(num):
    # if num is an integer then
        # add the integers 0 through num and return sum

    # if num is not an integer then return 0**Sample inputs/outputs:****# input: 5, output: 15
# input: 'and', output: 0**
```

> 我们定义了一个函数 **add** ，它接受一个输入 **num** 。在 **add** 函数中，我们使用注释编写了一个步骤大纲。如果传递给函数的值是一个整数，那么我们将把整数 0 和该值相加，然后返回和。如果传递给函数的值不是一个整数，那么我们简单地返回 0。在那之后，我们写一些例子，我们希望我们的输出被给定一个特定的输入。

让我们在上面的伪代码中展开这一步:

```
# add the integers 0 through num and return sum
```

这一步可以用很多方法来完成。如果我们使用 for 循环尝试这一步，伪代码会是什么样子？

```
def add(num):
    # if num is an integer then**# create sum variable and assign it to 0****# using a for loop, loop over integers 0 to num****# update sum value by adding num to it****# return sum**# if num is not an integer then return 0
```

让我们根据上面创建的蓝图来尝试解决它！

# 使用 For 循环

我们可以使用 for 循环来解决这个提示，如下所示:

```
def add(num):
    **# if num is an integer then** if type(num) == int:**# create sum variable and assign it to 0** sum = 0**# using a for loop, loop over integers 0 to num** for x in range(num+1):**# update sum value** sum += x**# return sum** return sum**# if num is not an integer then return 0** else:
        return 0
```

## 下面我们来剖析一下代码。

我们首先使用**类型的**函数检查传入的值 **num** 是否是一个整数。

```
if type(num) == int:
```

如果类型是整数，我们创建一个 **sum** 变量，并给它赋值 0。

```
sum = 0
```

然后，我们使用 for 循环和 **range** 函数对从 0 开始的整数进行循环，直到传递给我们的函数的整数。请记住，range 函数创建了一个 **range 对象**，它是一个可迭代的对象，从 0 开始(如果我们没有指定起始值)，到小于停止值的整数(因为停止值是唯一的)。这就是为什么我们需要在停止值( **num+1** )上加 1，因为我们想把从 0 到这个数字的所有整数加起来。

> `range`(开始、停止[、步进])

range 函数将创建一个 range 对象，这是一个可迭代的对象，因此我们可以使用 for 循环来遍历它。当我们循环遍历这个 iterable 对象时，我们将每个数字，或 **x** ，添加到 **sum** 变量中。

```
for x in range(num+1):
sum += x
```

然后，在 for 循环迭代完成后，该函数返回总和。

```
return sum
```

最后，如果传入的数字不是整数，我们返回 0。

```
else:
    return 0
```

**这是没有注释的代码:**

```
def add(num):
if type(num) == int:
sum = 0
for x in range(num+1):
sum += x
return sum
else:
        return 0
```

如果我们测试我们的**加法**函数，我们得到正确的输出:

```
add(5) 
# 15add('and')
# 0
```

这是解决编码问题的一个好方法，它完成了工作。它易于阅读，工作正常。但是，在我看来，通过尝试以其他方式解决这个问题，我们也许可以通过利用我们的其他 python 知识和解决问题的技能从中提取更多的价值。这些其他的方法可能更 pythonicic 化，也可能不更 python 化，但是思考不同的方法来解决同一个问题是非常有趣和有用的。

让我们试着用不同的方法解决这个编码问题。

*关于可迭代对象的更多信息:*

</three-concepts-to-become-a-better-python-programmer-b5808b7abedc>  

# 使用 Reduce 函数

我们最近在之前的教程中学习了 reduce 函数的功能。reduce 函数接受一个 iterable 对象，并将其缩减为一个累积值。reduce 函数可以接受三个参数，其中两个是必需的。两个必需的参数是:一个函数(它本身接受两个参数)和一个 iterable 对象。

我们可以使用 reduce 函数来计算一个可迭代对象的和。

因此，代替 for 循环，我们可以使用 reduce 来解决我们的 Python 问题:

```
from functools import reducedef add(num):
if type(num) == int:
return reduce(lambda x,y: x+y, range(num+1))else:
        return 0
```

> 就是这样！我们使用 lambda 函数作为函数参数，使用 range 对象作为我们的 iterable 对象。然后， **reduce 函数**将我们的 range 对象减少到一个值，即 sum。然后我们返回总和。

*关于 reduce 功能的更多信息:*

</three-functions-to-know-in-python-4f2d27a4d05>  

# 使用三元运算符

通过使用三元运算符，我们可以进一步缩短代码。使用三元运算符，我们可以使用以下格式将上面的 if/else 语句缩短为一行:

> x if C else y

> ***C*** *是我们的条件，先评估哪个。如果评估为真，则评估* ***x*** *并返回其值。否则，对* ***y*** *求值并返回其值。*

我们可以在代码中实现这一点，如下所示:

```
def add(num):
return reduce(lambda x,y: x+y, range(num+1)) if type(num) == int else 0
```

通过这些改变，我们设法将函数中的代码减少到一行。这可能不是解决这个问题的最具可读性或 pythonic 式的方法，但在我看来，它通过迫使我们找出解决同一问题的不同方法来帮助我们提高编码和解决问题的技能。

让我们看看能否用另一种方法解决这个编码问题。

*关于三元运算符的更多信息:*

</ternary-operators-in-python-49c685183c50>  

# sum()函数

我们可以使用 Python 内置的 sum 函数以不同的方式解决这个编码问题。sum()函数可以接受一个 iterable 对象，并返回其元素的总和。如果我们想先将起始值添加到元素中，我们也可以传入一个起始值。

> sum(iterable，start)

让我们使用 sum 函数来解决编码问题:

```
def add(num):
return sum(range(num+1)) if type(num) == int else 0
```

就是这样！这可能是解决这个编码问题的最好方法，因为它是最简洁和易读的解决方案。此外，它还可能具有最佳性能。

*有关代码性能的更多信息:*

</become-a-more-efficient-python-programmer-3850c94b95a4>  

如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [***链接***](https://lmatalka90.medium.com/membership) *报名，我就赚点小提成。*

<https://lmatalka90.medium.com/membership>  

# 结论

在本教程中，我们了解到使用不同的方法解决 Python 问题可以通过拓宽我们的知识库来增强我们的编码和解决问题的技能。我们看了一个示例 python 编码问题，并经历了解决它的步骤。我们首先计划如何用伪代码解决这个问题。然后，我们通过使用 for 循环来解决提示问题，从而实现了这些步骤。后来，我们用 reduce 函数解决了同样的问题。我们还实现了三元运算符来进一步缩短代码，但仍然保持可读性。最后，我们使用 Python 内置的 sum 函数和三元运算符，得出了最短但仍然最 Python 化的解决方案。