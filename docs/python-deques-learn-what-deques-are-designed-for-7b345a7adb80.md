# Python Deques:了解 Deques 的设计目的

> 原文：<https://towardsdatascience.com/python-deques-learn-what-deques-are-designed-for-7b345a7adb80?source=collection_archive---------44----------------------->

## 为初学者提供易于理解的示例

![](img/3097384293dae3328a07fbdc48b0070b.png)

利瓦伊·琼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Python 中有很多**内置的数据类型**。根据不同的用例，每种数据类型都有优缺点。因此，了解每种数据类型的优缺点是开发高效应用程序的关键。

在本文中，我将尝试解释 Python 中的 **deque** 数据类型。

# 什么是德奎？它们是为什么而设计的？

**Deque** 是**双端队列**的简称，这意味着它有效地支持从**两端**添加和移除项目。它们旨在高效地执行这些特定任务。

**队列**与**列表**、**堆栈**和**队列没有太大的不同。**在某种程度上， **deques** 结合了**list**、**stack**和 **queues** 的特点。

那么这些数据类型之间有什么区别；

*   **列表**数据类型使用连续的内存块，其中的元素彼此相邻存储。因此，随机访问列表**中的元素**很快，因为 Python 知道在哪里查看。
*   同时，当内存块不足以存储附加项时，**列出需要分配附加内存块的对象**，在这种情况下会降低**追加操作**的性能。
*   另一方面， **deques** 中的元素存储在它们自己的内存块中。因此，我们可以更有效地删除或添加元素。
*   对包含在一个**队列**中的元素的随机访问需要 Python 循环整个对象，这与相同操作在**列表对象**中的 O(1)常数时间复杂度相比效率不是很高。
*   deques 中的元素是双重链接的，因此每个元素都知道相邻项的位置。这带来了增量内存需求，因为元素包含两个以上的整数来存储相邻项的位置。

让我们看看 **deques** 的运行情况；

```
**import sys, time
from collections import deque**a_deque = **deque**([3,4,5,6])
a_list = [3,4,5,6]print(sys.getsizeof(a_deque))
print(sys.getsizeof(a_list))start_time = time.time()
for item in range(100000000):
 a_list.append(item)
end_time = time.time()
print("**Append time for LIST:** ", end_time - start_time)start_time = time.time()
for item in range(100000000):
 a_deque.append(item)
end_time = time.time()
print("**Append time for DEQUE:** ", end_time - start_time)start_time = time.time()
for item in a_list:
 aa = a_list[200]
end_time = time.time()
print("**Lookup time for LIST:** ", end_time - start_time)start_time = time.time()
for item in a_deque:
 aa = a_deque[200]
end_time = time.time()
print("**Lookup time for DEQUE:** ", end_time - start_time)**Output:**632 (size of deque object in bytes)
88 (size of deque object in bytes)Append time for LIST:  13.975143671035767 
Append time for DEQUE:  11.940298795700073 
Lookup time for LIST:  8.094613075256348 
Lookup time for DEQUE:  8.974030017852783
```

# **什么时候用德克？**

何时使用 deques

*   如果需要在线性序列的末尾删除或添加元素，那么使用 **deques** 。它们在这些操作上提供了 **O(1)** 恒定的时间效率。

何时不使用 deques

*   如果你经常需要随机访问时间复杂度为 O(n) 的元素，不要使用**队列**(执行操作所需的时间与对象的大小成正比)。**列表**在这种情况下是更好的选择，因为它们提供了 **O(1)** 的时间复杂度。
*   如果你有内存限制，不要使用**队列**。

# **关键要点和结论**

在这篇短文中，我解释了在 Python 中何时使用**dequee**数据类型。关键要点是:

*   **Deque** 是**双端队列**的简称，这意味着它有效地支持从**两端**添加和移除项目。它们旨在高效地执行这些特定任务。
*   如果需要在线性序列的末尾删除或添加元素，那么使用 **deques** 。它们为这些操作提供了恒定的时间效率。
*   如果经常需要随机访问容器对象中的元素，不要使用 **deques** 。

我希望你已经发现这篇文章很有用，并且**你将开始在你自己的代码**中使用 deque 数据类型。