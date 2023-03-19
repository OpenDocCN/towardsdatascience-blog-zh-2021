# Python:知道和理解它的区别

> 原文：<https://towardsdatascience.com/python-the-difference-between-knowing-and-understanding-it-3b2ebd4e2317?source=collection_archive---------33----------------------->

## 你知道 Python 中列表的调整因子是什么吗？

![](img/89e89e6067f3fa0a40ff9b5cfffc5e53.png)

Artem Sapegin 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我只是在开玩笑。你不需要知道 Python 的列表大小调整因子就能很好地理解它。但是如果你想了解更多，请阅读这里的！

我来自一个非计算机科学(CS)和非技术背景的人，当我刚开始机器学习(ML)的旅程和职业生涯时，我遭受了 CS 基础知识的缺乏。这包括理解渐近运行时间、内存使用、数据结构、算法等等。更糟糕的是，我在没有工程基础的情况下直接进入了 ML——我甚至不知道类中的 *self* 是什么意思(在 Python 上下文中)。我是*。*

*足够幸运的是，有了足够的 ML 概念但缺乏工程知识，我在 ML 中获得了立足点，并开始了我的[旅程](/i-had-no-idea-how-to-write-code-two-years-ago-now-im-an-ai-engineer-13c530ab8227)学习和实践 ML。在这段旅程中，我很快意识到工程在 ML 工程中扮演着重要的角色。然而，事实上，知道如何简单地使用一种编程语言和很好地理解它之间有着明显的区别。*

*在你说你真正理解 Python 之前，你应该知道以下一些事情。本文分为几个部分:*

1.  *[**Python 中的一切都是对象**](https://medium.com/p/1ce01378a92#27cd)*
2.  *[**内置 Python 对象不包含 *__dict__* 属性**](https://medium.com/p/1ce01378a92#0005)*
3.  *[**指针在 Python**](https://medium.com/p/1ce01378a92#c61d) 中本来就不存在*
4.  *[**Python 的可变默认值是一切邪恶的根源**](https://medium.com/p/1ce01378a92#35e0)*
5.  *[**复制 Python objec**](https://medium.com/p/3b2ebd4e2317/)**ts***

## *1.Python 中的一切都是对象*

*好吧，我们从简单的开始。我相信大多数读者都知道 Python 是一种面向对象的编程语言。但是这到底意味着什么呢？*

*当我说*一切*时，我指的是*一切*。不仅用户定义的函数和类是对象，内置类型也是对象！本质上——***一切*** 。*

*正如您将在后面的第(3)点中看到的，Python 中的对象包含元数据(也称为*属性*)以及一些功能(也称为*方法*)。要查看与对象相关的所有关联元数据和功能，可以使用内置的`dir()`函数。*

```
*# Look at the metadata of an integer object
x = 1
dir(x)
>>> **['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'as_integer_ratio', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']***
```

*我们可以观察到，即使整数类型本身也是具有内置属性和方法的对象。例如，我们可以通过`real`和`imag`属性获得一个复数的实部和虚部。*

```
*real_x, imag_x = x.real, x.imag
print(f"Complex number of {x} is: {real_x} + {imag_x}i")
>>> **Complex number of 1 is: 1 + 0i***
```

*还有可以调用的方法包括`as_integer_ratio()`、`bit_length()`等等。*

```
*x.as_integer_ratio()
>>> **(1, 1)**x.bit_length()
>>> **1***
```

*此外，这些对象具有这些内置的 *dunder* 方法的事实允许它们执行某些操作，如加法(`__add__`)、转换为浮点(`__float__`)，甚至获得对象的可打印表示(`__repr__`)。*

> *随意探索其他内置数据结构的内置属性和方法，如列表、元组、字典等等！*

## *2.内置 Python 对象不包含 __dict__ 属性*

*如果你还不知道，我们可以使用内置的`vars`函数返回一个对象的`__dict__`属性，如果它存在的话。让我们看看如果在内置 Python 对象上使用这个函数会发生什么。*

```
*x, y, z = 1, "hello", {"hello": 1}vars(x)
>>> **TypeError**: **vars() argument must have __dict__ attribute**vars(y)
>>> **TypeError**: **vars() argument must have __dict__ attribute**vars(z)
>>> **TypeError**: **vars() argument must have __dict__ attribute***
```

*因此，我们无法通过`setattr()`函数为**内置 Python 对象**设置新的属性和方法，也无法覆盖现有的属性和方法。之所以如此，是因为使用 *C* 进行了优化，防止添加额外的属性，从而允许更多的“固定”内存分配。*

```
*setattr(x, "some_new_attr", 123)
>>> **AttributeError: 'int' object has no attribute 'some_new_attr'***
```

*然而，对于定制对象，我们可以自由地创建新的属性，因为这些对象通常有一个`__dict__`属性。这是因为`__dict__`属性是一个映射对象，用于存储对象的*可写*属性。*

> *注意:在类上调用`vars`时要小心，因为在类本身上调用`vars`和在该类的实例上调用`vars`是有区别的。*

```
*vars(Animal)
>>> **mappingproxy({{'__module__': '__main__',
                   '__annotations__': {'has_eyes': bool},
                   'mammal': True, ... }})***
```

## *3.指针在 Python 中并不存在*

*你可能会好奇——*列表*、*集合*、*字典*甚至*数据帧*都可以修改，它们的内存地址保持不变。什么叫指针不存在？！*

> ***注** : `*id()*`用于获取对象的内存地址*

```
*# Example using a list (or any other mutable data structure)
x = [1, 5, 10]
id(x)
>>> **140298465662784**x.pop()
id(x)
>>> **140298465662784***
```

*上述数据结构作为**可变**类型*存在的事实模仿了*Python 中指针的功能。实际上，你已经为其分配了变量的可变对象*并不*真正拥有那个内存地址。*

*那么 Python 是如何跟踪对象的呢？*

*在 Python 中，对象被存储为 *PyObject* s，这实质上是所有具有 *CPython* 结构的 Python 对象的基础结构。当我们检查两个对象是否相等时，即`a is b`，我们实际上是在检查这两个对象的内存地址是否相同。*

*创建任何对象时，都会创建一个 PyObject，它跟踪数据类型、值以及最重要的*引用计数*(即指向它的名称/变量的数量)。因此，更恰当的说法是 Python 中的每个名称/变量都是一个对 PyObject 的 ***引用*** 。*

```
*# As seen below, x and y point to the same PyObject
# Each of them do not own the memory addressx = "hello"
y = "hello"id(x) == id(y)
>>> **True**# The PyObject looks something like this
# Type: str; Value: "hello"; Reference Count: 2*
```

*如上所述，像 *int* 、 *str* 和*元组*这样的不可变数据结构并不拥有自己的内存地址，而是将名称/变量绑定到引用(*p objects*)。*

> ***注**:上面这个用字符串(“hello”)的例子是**字符串实习**的一个特例。通常情况下，不可变对象将具有不同的内存地址，其他例外情况有**整数缓存**或**空不可变对象**发生或发生。为了更清楚地理解 Python 中可变和不可变数据结构的内存管理，我强烈推荐[这篇文章](https://medium.com/@tyastropheus/tricky-python-i-memory-management-for-mutable-immutable-objects-21507d1e5b95)！*

*那么，可变数据结构的行为如何像指针呢？*

*可变数据结构是不同的，因为我们能够修改它的值*而不需要*修改它的内存地址。这样，包含相同值的相同列表(或任何其他可变数据结构)将**而不是**具有相同的内存地址。*

```
*x = [1, 5, 10]
y = [1, 5, 10]
id(x) == id(y)
>>> **False***
```

*RealPython 的这篇[文章](https://realpython.com/pointers-in-python/)对于理解 Python 中的“指针”非常有用。*

*你可能会问，这有什么关系？嗯，这就引出了我的下一个观点。*

## *4.Python 可变缺省值是所有罪恶的根源*

*因为可变对象模拟了指针的功能，所以当用作函数或方法中的默认参数时，它也可能是万恶之源。*

***常见用例:**列表(或其他可变数据结构)作为默认参数*

*正如你在下面的例子中看到的，这个在`arr`中使用`List`作为默认参数的`increment_list`函数可能会导致很多问题，因为在这个函数的`return`之后赋值的任何变量都拥有相同的内存地址(或*对象*)。这实质上意味着这个可变对象的值甚至可以在函数之外修改。*

*那么，可变数据结构应该如何被用作默认参数呢？*

*使用`None`作为默认值，并在函数中检查它。这确保了在创建新列表时，总是为这个`List`对象创建一个新的内存地址。*

*如何创建可变对象的副本？*

*我们可以在这些数据结构中使用`.copy()`方法来获得对象的副本。但是请注意，复制一个对象的结果是复制它，因此会消耗额外的内存。在进行复制时请记住这一点，尤其是在复制较大的对象(在内存中)时。这就把我带到了文章的最后一点— **复制 Python 对象**。*

```
*a = {1: 'hello'}
b = a.copy()
id(a) == id(b)
>>> **False***
```

## *5.复制 Python 对象*

*正如我们在上面看到的，我们使用了字典数据结构的内置`copy`方法。这实际上是创建了对象的**浅拷贝**，而不是**深拷贝**。有什么区别？*

*   *一个**浅拷贝**意味着构造一个新的集合对象，然后用在原始对象中找到的子对象的引用填充它。本质上，浅抄只是*深一级*。复制过程不会递归，因此不会创建子对象本身的副本。[4]*
*   *一个**深度复制**使复制过程递归。它意味着首先构造一个新的集合对象，然后用原始集合中找到的子对象的副本递归地填充它。以这种方式复制对象会遍历整个对象树，从而创建原始对象及其所有子对象的完全独立的克隆。[4]*

*以上到底是什么意思？让我们来看一些例子。*

*好吧，这里刚刚发生了什么？在上面的例子中，我们看到`list()`和`.copy()`都创建了原始列表的浅层副本，而来自*副本*模块的`deepcopy()`创建了深层副本。*

*最初检查时，我们确实希望所有副本都有自己的内存地址，因为它们是可变对象。然而，再深入*一层*(对象中的对象)，我们观察到第一个索引中的第一个列表与*浅拷贝*的原始列表具有**相同的**内存地址！这也适用于原始列表中的其他子对象(列表)。对于深层副本，这是另外一种情况，因为我们已经递归地创建了包含这个副本的列表。*

***如果我们修改原始或复制的对象会发生什么？***

*从上面可以看出，修改原始或浅复制对象的子对象将导致原始和浅复制对象的改变。深层复制的对象不受影响。*

*如果我们修改一个自定义类会发生什么？*

*我们不仅可以从内置对象中看到这种行为，还可以从自定义对象(如自定义类)中看到这种行为。下面，我们创建一个包含三个像素值作为属性的`RGB`类和一个接受两种颜色的复合`TwoColors`类，特别是`RGB`类的两个不同实例。*

***问题:**如果我们修改`pixel`的属性，你认为会发生什么？*

```
*pixel.red, pixel.blue, pixel.green = 100, 100, 100pixel
>>> **RGB(100,100,100)**pixel1
>>> **RGB(255, 255, 255)**pixel2
>>> **RGB(255, 255, 255)***
```

*希望你没弄错——是的，复制的对象没有**变化，因为这是一个单独的*层*对象。***

***问题**:如果我们修改复合对象`colours`中的子属性的属性会怎么样？*

```
*colours.colour_one.red, colours.colour_two.blue = 111, 77colours
>>> **TwoColours(RGB(111, 0, 0), RGB(255, 255, 77)**colours1
>>> **TwoColours(RGB(111, 0, 0), RGB(255, 255, 77)**colours2
>>> **TwoColours(RGB(0, 0, 0), RGB(255, 255, 255)***
```

*如您所料(希望如此)，深度复制的对象不会修改其值，因为它是原始对象的完整副本。*

# *结束语*

*至此，我希望您从这篇文章中学到了一些东西，并巩固了您的 Python 知识。感谢您的阅读！*

*如果你喜欢我的内容，并且*没有*订阅 Medium，请考虑支持我并通过我的推荐链接[订阅这里](https://davidcjw.medium.com/membership) ( *注:你的一部分会员费将作为推荐费分摊给我*)。*

# *参考*

*[1]:[Python 中的指针:有什么意义？](https://realpython.com/pointers-in-python/)*

*[2]: [基本的 Python 语义](https://jakevdp.github.io/WhirlwindTourOfPython/03-semantics-variables.html#:~:text=Python%20is%20an%20object%2Doriented,Python%20everything%20is%20an%20object.&text=In%20Python%20everything%20is%20an%20object%2C%20which%20means%20every%20entity,associated%20functionality%20(called%20methods).)*

*[3]: [设置 WithCopyWarning:视图与副本](https://realpython.com/pandas-settingwithcopywarning/)*

*[4]: [RealPython:复制 Python 对象](https://realpython.com/copying-python-objects/)*