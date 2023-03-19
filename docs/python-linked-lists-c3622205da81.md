# 如何在 Python 中实现链表

> 原文：<https://towardsdatascience.com/python-linked-lists-c3622205da81?source=collection_archive---------2----------------------->

## 探索如何使用 Python 从头开始编写链表和节点对象

![](img/15a2bb24202cddc0f7688065bb8cf25f.png)

照片由 [Mael BALLAND](https://unsplash.com/@mael_balland?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/chain?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

## 介绍

链表是最基本的数据结构之一，**表示一系列节点**。序列的第一个元素称为链表的头**和尾**，最后一个元素对应于尾**和尾**。

序列中的每个节点都有一个指向下一个元素的指针，也可以有一个指向上一个元素的指针。在**单链表**中，每个节点只指向下一个节点。

![](img/ae3d48bbe57e93b3bfd6745e4bcc992b.png)

单链表—来源:[作者](https://gmyrianthous.medium.com/)

另一方面，在**双向链表**中，每个节点既指向前一个节点，也指向下一个节点。

![](img/0d81d91e19345cc26013cf6b5ff76117.png)

双向链表——来源:[作者](https://gmyrianthous.medium.com/)

链表在各种场景中都非常有用。在以下情况下，它们通常优于标准阵列

*   在序列中添加或删除元素时，您需要一个恒定的时间
*   更有效地管理内存，尤其是当元素的数量未知时(如果是数组，您可能必须不断地缩小或增大它们。注意，填充数组通常比链表占用更少的内存。
*   您希望更有效地在中间点插入项目

与其他通用语言不同，Python 的标准库中没有内置的链表实现。在今天的文章中，我们将探索如何使用 Python 实现一个用户定义的链表类。

## 在 Python 中实现用户定义的链接类

首先，让我们为链表中的单个节点**创建一个用户定义的类。这个类既适用于单链表，也适用于双向链表。因此，这个类的实例应该能够存储节点的值，下一个和上一个节点的值。**

```
class Node:
    def __init__(self, value, next_node=None, prev_node=None):
        self.value = value
        self.next = next_node
        self.prev = prev_node

    def __str__(self):
        return str(self.value)
```

请注意，当一个`Node`的实例将`next`设置为`None`时，这意味着它本质上是链表的尾部(单个或两个)。类似地，在双向链表中，当一个节点的`prev`被设置为`None`时，这表明该节点是链表的头。

既然我们已经为节点创建了一个类，现在我们可以为链表本身创建类了。如前所述，链表有一个`head`、一个`tail`和指向彼此的节点。

```
class LinkedList:
    def __init__(self, values=None):
        self.head = None
        self.tail = None if values is not None:
            self.add_multiple_nodes(values)
```

现在，为了将构造函数中提供的值作为节点添加到链表中，我们需要定义两个额外的方法。

第一个方法叫做`**add_node**`,用于向链表中添加一个节点。

```
def add_node(self, value):
    if self.head is None:
        self.tail = self.head = Node(value)
    else:
        self.tail.next = Node(value)
        self.tail = self.tail.next
    return self.tail
```

现在让我们快速浏览一下这个方法的逻辑。如果链表没有头，那么这意味着它是空的，因此要添加的节点将是链表的头和尾。如果头部不为空，那么我们添加新创建的`Node`作为当前`tail`的`next`元素，最后移动尾部指向新创建的`Node`。

第二个方法叫做`**add_multiple_nodes**`，它在构造函数中被调用，并且简单地调用我们之前定义的`add_node`方法，以便在链表实例中添加多个值作为节点。

```
def add_multiple_nodes(self, values):
    for value in values:
        self.add_node(value)
```

到目前为止，我们的链表类如下所示:

```
class LinkedList:
    def __init__(self, values=None):
        self.head = None
        self.tail = None if values is not None:
            self.add_multiple_nodes(values) def add_node(self, value):
        if self.head is None:
            self.tail = self.head = Node(value)
        else:
            self.tail.next = Node(value)
            self.tail = self.tail.next
        return self.tail def add_multiple_nodes(self, values):
        for value in values:
            self.add_node(value)
```

现在让我们创建一个额外的方法，它能够插入一个新元素，但是这次是在链表的开始，也就是作为一个头。

```
def add_node_as_head(self, value):
    if self.head is None:
        self.tail = self.head = Node(value)
    else:
        self.head = Node(value, self.head)
    return self.head
```

现在让我们在类中重写一些可能有用的特殊方法。首先，让我们实现`__str__`方法，以便链表对象的字符串表示是人类可读的。例如，当打印出一个带有节点`a, b, c, d`的链表时，输出将是`a -> b -> c -> d`。

```
def __str__(self):
    return ' -> '.join([str(node) for node in self])
```

其次，让我们也实现`__len__`方法，它将返回我们的用户定义类的长度，本质上是序列中包含的节点数。我们需要做的就是遍历序列的每个节点，直到到达链表的尾部。

```
def __len__(self):
    count = 0
    node = self.head
    while node:
        count += 1
        node = node.next
    return count
```

最后，让我们通过实现`__iter__`方法来确保`LinkedList`类是可迭代的。

```
def __iter__(self):
    current = self.head
    while current:
        yield current
        current = current.next
```

此外，我们还可以创建一个名为`values`的属性，这样我们就可以访问序列中所有节点的值。

```
@property
def values(self):
    return [node.value for node in self]
```

最终的类如下所示:

```
class LinkedList:
    def __init__(self, values=None):
        self.head = None
        self.tail = None if values is not None:
            self.add_multiple_nodes(values) def __str__(self):
        return ' -> '.join([str(node) for node in self]) def __len__(self):
        count = 0
        node = self.head
        while node:
            count += 1
            node = node.next
        return count def __iter__(self):
        current = self.head
        while current:
            yield current
            current = current.next @property
    def values(self):
        return [node.value for node in self] def add_node(self, value):
        if self.head is None:
            self.tail = self.head = Node(value)
        else:
            self.tail.next = Node(value)
            self.tail = self.tail.next
        return self.tail def add_multiple_nodes(self, values):
        for value in values:
            self.add_node(value) def add_node_as_head(self, value):
        if self.head is None:
            self.tail = self.head = Node(value)
        else:
            self.head = Node(value, self.head)
        return self.head
```

现在，即使我们的`Node`类可以表示包含在单向或双向链表中的节点，我们定义的`LinkedList`类只能支持单向链表。这是因为在添加节点时，我们没有指定前一个节点。

为了处理双向链表，我们可以简单地创建一个额外的类，它继承自`LinkedList`类并覆盖`add_node`和`add_node_as_head`方法:

```
class DoublyLinkedList(LinkedList):
    def add_node(self, value):
        if self.head is None:
            self.tail = self.head = Node(value)
        else:
            self.tail.next = Node(value, None, self.tail)
            self.tail = self.tail.next
        return self def add_node_as_head(self, value):
        if self.head is None:
            self.tail = self.head = Node(value)
        else:
            current_head = self.head
            self.head = Node(value, current_head)
            current_head.prev = self.head
        return self.head
```

## Python 中用户自定义链表的完整代码

包含我们在今天的教程中创建的三个类的完整代码作为 GitHub 要点在下面给出。

包含 Node、LinkedList 和 DoublyLinkedList Python 类的完整代码——来源:[作者](https://gmyrianthous.medium.com/)

## 最后的想法

在今天的指南中，我们讨论了最基本的数据结构之一，即链表。鉴于 Python 的标准库不包含这种特定数据结构的任何实现，我们探索了如何从头实现用户定义的链表类。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership>  

**你可能也会喜欢**

</leetcode-problem-2-python-1c59efdf3367>  </augmented-assignments-python-caa4990811a0> 