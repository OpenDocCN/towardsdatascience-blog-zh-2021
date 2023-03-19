# Python 的 4 个必须知道的特殊方法

> 原文：<https://towardsdatascience.com/4-must-know-special-methods-for-python-2ffa62b59326?source=collection_archive---------4----------------------->

## 您需要在用户定义的类中实现的方法

![](img/0b03cf4a82e48011fc72b241388733f4.png)

多萝西娅·奥德尼在 [Unsplash](https://unsplash.com/s/photos/special?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Python 中的一切都是对象，我们通过类来定义对象。当我们定义一个对象时，我们实际上创建了一个类的实例。因此，类是 Python 中最基本的部分。

课程有:

*   数据属性:定义创建一个类的实例需要什么
*   方法(即过程属性):定义如何与类的实例交互

我们可以使用数据和过程属性创建自己的类。这是我们的游乐场，所以我们可以实现各种功能来定制一个类。

除了用户定义的函数，还可以在用户定义的类中使用内置的 Python 函数。这就是特殊(或神奇)方法的用途。

特殊的方法允许使用内置的 Python 函数来丰富用户定义的类。考虑打印函数，这是最常用的 Python 函数之一。如果您使用它来打印您的类的实例，它将打印出如下内容:

```
<__main__.Book object at 0x7f9ed5b1d590>
```

它显示类(书)的名称和对象的存储位置，这不是打印功能的正确使用。我们可以通过在类中实现 __str__ special 方法来自定义它的行为。

在这篇文章中，我们将讨论 4 种你可能会在课堂上用到的特殊方法。

## 1.__init__

创建类的实例时，会自动执行 __init__ special 方法。它也被称为类构造函数。__init__ 的参数表示一个类的数据属性。

让我们创建一个名为 Book 的类。

```
class Book(): def __init__(self, name, writer, pages):
       self.name = name
       self.writer = writer
       self.pages = pages
```

自我指的是实例本身。Book 类有 3 个数据属性，需要在创建 Book 实例时指定。

```
b = Book("Moby Dick", "Herman Melville", "378")type(b)
__main__.Book
```

变量 b 是 Book 类的一个实例。

## 2.__str__

我们使用 __str__ 特殊方法在我们的类中实现内置的打印函数。如果没有 __str__，下面是 print 函数的作用。

```
print(b)
<__main__.Book object at 0x7f9ed5b1d590>
```

让我们在类定义中定义 __str__ 方法。

```
def __str__(self):
   return f"The title of the book is {self.name}"
```

现在打印功能将返回名称的标题。它通过 name 属性访问图书的名称。你可以用你喜欢的任何方式定制它。

```
print(b)
The title of the book is Moby Dick
```

## 3.__len__

len 函数返回对象的长度。对于字符串，它返回字符数。对于熊猫数据框，它返回行数。

我们可以通过在类定义中实现 __len__ special 方法来自定义它的行为。让我们用它来返回一个 book 对象的页数。

如果 __len__ 没有在类定义中实现，当您试图在类的对象上使用它时，将会出现错误。它没有像打印功能那样的默认行为。

```
def __len__(self):
   return int(self.pages)len(b)
378
```

请注意，每次向类中添加新函数时，都需要重新创建对象才能使用新函数。

## 4.__eq__

__eq__ 特殊方法允许比较一个类的两个实例。如果它是在我们的类中定义的，我们可以检查一个实例是否等于另一个实例。使用 __eq__ 方法指定相等条件。

在我们的例子中，如果两本书的名字和作者相同，我们可以声明这两本书是相等的。它们可能有不同的页数。

```
def __eq__(self, other):
   return (self.name == other.name) & (self.writer == other.writer)
```

如果两个实例的名称和写入都相同，则“==”运算符将返回 True。让我们创建两本书并检查它们是否相等。

```
b = Book("Moby Dick", "Herman Melville", "378")a = Book("Moby Dick", "Herman Melville", "410")b == a
True
```

如果 names 或 writes 不同，相等运算符将返回 False。

```
b = Book("Moby Dick", "Herman Melville", "378")a = Book("Moby Dick", "Melville", "410")b == a
False
```

## 结论

对象是 Python 的核心，因此创建健壮的、功能强大的、设计良好的类至关重要。我们已经介绍了 4 种特殊的方法，您需要在自己的类中实现它们。

感谢您的阅读。如果您有任何反馈，请告诉我。