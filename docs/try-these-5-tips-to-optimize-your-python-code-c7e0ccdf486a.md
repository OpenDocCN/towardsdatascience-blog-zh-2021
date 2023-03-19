# 尝试以下 5 个技巧来优化你的 Python 代码

> 原文：<https://towardsdatascience.com/try-these-5-tips-to-optimize-your-python-code-c7e0ccdf486a?source=collection_archive---------21----------------------->

## 编写更好的 python 代码的快速技巧

![](img/f6501c805e10a41843d6e8cfdab9a6a7.png)

照片由[布鲁诺布埃诺 发自](https://www.pexels.com/@brunogobofoto?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)[佩克斯 ](https://www.pexels.com/photo/person-writing-on-pink-sticky-notes-3854816/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

Python 确实是最通用、最流行的语言之一，几乎可以用于任何领域。但是 python 最突出的缺点之一就是速度。

由于 python 是一种解释型语言，执行指令所需的时间比编译型语言(如 C、C++甚至 Java)要长得多。因此，我们需要注意优化我们编写的 Python 代码，以使代码执行得更好更快。

虽然在较简单的程序中无法体验到性能的提高，但是在一些真实项目中，我们肯定可以注意到这种变化。

在本文中，我将介绍 5 个这样的技巧，它们将使您的 python 代码更加优化，并将提高其性能。

# **1。** **使用内置函数，而不是从头开始编码**

Python 以其惊人的库而闻名，这些库几乎可用于计算机科学的任何领域，你应该好好利用它们。Python 中的一些内置函数如`map()`、`sum()`、`max()`等已经在 C 语言中实现了，所以在执行过程中不会对它们进行解释，节省了大量时间。

例如，如果您想将一个字符串转换成一个列表，您可以使用`map()`函数，而不是手动将字符串的内容追加到一个列表中。代码更加优化，大小也更小。

```
string = ‘Australia’U = map(str, s)print(list(string))Output: [‘A’, ‘u’, ‘s’, ‘t’, ‘r’, ‘a’, ‘l’, ‘i’, ‘a’]
```

您可以在 python 上找到更多类似的很酷的功能，它们可以使您的任务变得简单和优化。

# **2。** **关注代码执行过程中的内存消耗**

减少代码中的内存占用肯定会使代码更加优化。我们需要记住不必要的内存消耗是否正在发生。

让我们观察下面的代码片段。这里我们使用+操作符添加一个较小的字符串来生成一个较大的字符串。

```
myString = ‘United ‘myString += ‘states ‘myString += ‘of ‘myString +=’America’print(myString)
```

这个方法会在我们每次写 myString 的时候生成一个新的字符串，并且会导致不必要的内存消耗。我们可以在获取一个列表中的所有字符串后，使用函数 join()来代替使用这个方法来连接字符串。这将是更快的，并为内存优化。

```
string2 = [‘United ‘, ‘states ‘, ‘of ‘,’America’]string3 = ‘’.join(string2)print(string3)
```

> **输出**

```
United states of America
```

此外，在打印字符串中的变量时使用`f-strings`代替传统的'+'操作符在这种情况下也非常有用。

> *想* ***敬请期待*** *与更多相似* ***精彩*** *文章上* ***Python &数据科学*** *—做会员考虑使用我的推荐链接:*[*https://pranjalai.medium.com/membership*](https://pranjalai.medium.com/membership)*。*

# **3。****Python 中的记忆化**

那些知道动态编程概念的人非常熟悉记忆的概念。在这个过程中，通过在存储器中存储函数值来避免重复计算。尽管使用了更多的内存，但性能提升是显著的。Python 附带了一个名为`functools`的库，它附带了一个 LRU 缓存装饰器，可以让你访问用来存储某些值的缓存。

让我们以斐波那契数列为例。对于普通的 python 代码，在本地机器上计算 fib(35)的执行时间是 4.19147315400005 秒。

而当我们添加以下代码行时:

```
from functools import lru_cache@lru_cache(maxsize=100)
```

在代码块的开头，执行时间减少到 3.440899990891921e-05。

在这里，本地机器的执行时间呈指数级减少。时间差只会随着 fib(n)中 n 值的增加而增加。

# **4。** **使用 C 库/PyPy 获得性能增益**

如果有一个 C 库可以做你的工作，那么最好使用它来节省解释代码的时间。最好的方法是使用 python 中的`ctype`库。还有一个叫做 CFFI 的库，它提供了一个优雅的 c 接口

如果你不想使用 C 语言，那么另一种方法是使用 PyPy 包，因为 JIT(即时)编译器的存在极大地提高了你的 Python 代码。

# **5。** **正确使用数据结构和算法**

这更像是一个通用的技巧，但却是最重要的一个，因为它可以通过改进代码的时间复杂度，为您带来可观的性能提升。

例如，在 python 中使用字典而不是列表总是一个好主意，以防没有任何重复的元素，并且要多次访问这些元素。

这是因为字典使用散列表来存储元素，当在最坏的情况下搜索列表时，时间复杂度为 O(1 ),而不是 O(n)。因此，它将为您带来可观的性能增益。

# **结论**

本文到此为止。这是用 python 编码时能显著提高性能的五个技巧。但是您应该记住的另一件事是，性能的提高不应该影响代码的可读性，这也是现实项目中的一个重要方面。

敬请期待！！了解更多与 Python 和数据科学相关的技巧。

> *在你走之前……*

如果你喜欢这篇文章，并希望**继续关注更多关于 **Python &数据科学**的**精彩文章**——请考虑使用我的推荐链接[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)成为一名中级会员。**

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。