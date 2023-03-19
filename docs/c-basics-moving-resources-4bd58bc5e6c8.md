# C++基础:移动资源

> 原文：<https://towardsdatascience.com/c-basics-moving-resources-4bd58bc5e6c8?source=collection_archive---------26----------------------->

## 什么时候应该写自己的 move 构造函数和 move 赋值操作符？

![](img/370af33488cf3eca008f8b5f29457cd1.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 拍摄

# 简介—为什么要移动资源

在编写程序时，你会遇到需要将(大量)资源从一个对象转移到另一个对象的情况。

在 C++中，我们有移动语义，这是一种移动资源的方式，以避免在内存中进行复制，这不仅会使我们的程序变慢，而且会使用更多不必要的空间。

我们在这里不讨论移动语义，因为你可以在互联网上找到很多解释右值和移动语义的资源。

不太明显的是，什么时候我们可以依靠编译器来帮助我们移动资源，什么时候我们必须编写自己的移动构造函数和移动赋值操作符。

我们将在下面的章节中看到一些例子。当然，要求你使用现代 c++，至少是 c++11。

# 隐式声明和定义的特殊成员函数

如果我们创建一个像下面这样的简单结构，编译器会隐式地为我们生成一些特殊的函数，这样我们就不用写冗长的代码了。我们将由编译器生成以下函数:

*   默认构造函数
*   复制 ctor
*   移动构造函数
*   复制赋值运算符
*   移动赋值运算符
*   破坏者

了解这一点对于我们理解在管理(大量)资源时是否需要编写它们是很重要的。

# 移动资源

我们可以用多种方式管理资源，最常见的方式是使用 std::vector，但在其他情况下，我们可能希望使用原始指针或智能指针。

我们可能还需要在创建包装器时管理操作系统的资源，例如像 Linux 中的套接字句柄。

## 用 std::vector 管理资源

在编写管理向量的类时，我们不必专门编写 move 构造函数和 move 赋值操作符，因为 std::vector 已经为我们实现了。请看下面的例子:

在我们的 ***数据*** 类中，我们只实现了一个默认的构造函数，仅此而已。但是正如你看到的，我们可以复制和移动我们的资源。

```
Data data2 = data;
```

上面一行调用复制构造函数，下面一行调用移动构造函数。

```
Data data3 = std::move(data);
```

如果我们看到程序的输出，我们会看到类似这样的内容:

```
Data's internalData is at: 0x558d72e74eb0
Data2's internalData is at: 0x558d72e75460
Data3's internalData is at: 0x558d72e74eb0
data is now empty
```

我们可以看到 ***数据 2*** 具有不同的地址，这是因为资源被复制到内存中的新空间，而 ***数据 3*** 与 ***数据*** 具有相同的地址，因为资源刚刚被移动。结果， ***数据*** 变空，因为它的资源已经从中释放。

## 智能指针呢？

有共享指针和唯一指针，但我们在这里将重点放在共享指针上，因为唯一指针不允许你复制它，因为它必须是唯一的:)，它只能移动。

这个程序的输出是:

```
Data's internalData is at: 0x5599c3db8ec0
Number of owners: 1
Data2's internalData is at: 0x5599c3db8ec0
Number of owners: 2
Data3's internalData is at: 0x5599c3db8ec0
Number of owners: 2
data is now null
```

在这种情况下，我们的地址都是一样的，这是因为 ***shared_ptr*** 是为了共享资源，所以当你通过调用这行代码进行复制时:

```
Data data2 = data;
```

资源没有被复制，但是它们现在是共享的，这可以从所有者的计数中看出，在那一行之后变为 2。

现在如果我们调用下面的行:

```
Data data3 = std::move(data);
```

调用 move 构造函数， ***data3 的*** internalData 指向与 ***data2 的*** internalData 相同的地址，但是现在 ***data*** 已经无法访问这些资源，因为它们已经被转移到 ***data3*** 。

在这种情况下，我们也可以依靠编译器来完成它的工作，为我们实现 move 构造函数(以及所有其他特殊的成员函数)。

## 原始指针呢？

在某些情况下，我们可能想要管理原始指针，让我们看一个例子。

就像前面的例子一样，我们试图依靠编译器来完成它的工作。该程序的输出如下:

```
Data's internalData is at: 0x5565b0edaeb0
Data2's internalData is at: 0x5565b0edaeb0
Data3's internalData is at: 0x5565b0edaeb0
```

它们都指向同一个地址，这里有点不对劲。至少我们期望 ***Data2 的 internalData*** 指向不同的地址，因为它应该复制。

这显然行不通。原因是隐式生成的复制构造函数对成员进行了*成员式复制，所以复制的是地址，而不是数据。*

代码中缺少的另一件重要的事情是，当对象被销毁时，我们没有释放内存，这将导致 ***内存泄漏*** 。所以我们需要写自己的析构函数。

在我们添加了析构函数之后，会发生什么呢，当我们执行它的时候，这个程序会崩溃。

```
Data's internalData is at: 0x5632d3066eb0
Data2's internalData is at: 0x5632d3066eb0
Data3's internalData is at: 0x5632d3066eb0
double free or corruption (!prev)
Aborted (core dumped)
```

这是因为我们没有正确实现下面的特殊成员函数:

*   复制构造函数
*   移动构造函数
*   复制赋值运算符
*   移动赋值运算符

现在让我们将完整的实现编写如下:

输出将是正确的，如下所示:

```
Data's internalData is at: 0x5638e02c2eb0
Data2's internalData is at: 0x5638e02c4270
Data3's internalData is at: 0x5638e02c2eb0
Data is now empty
```

***Data2*** 的 ***internalData*** 现在指向不同的地址，有自己的数据副本，而 ***Data*** 在其 ***internalData*** 被移动到 ***Data3*** 后变为空。

# 结论

在试验了上面的不同场景后，我们现在可以得出结论，当我们的类管理原始资源(如原始指针)和操作系统句柄(如套接字)时，我们只需要编写自己的移动构造函数和移动赋值操作符。否则，我们可以依靠编译器为我们生成它们。

## 三/五法则

要记住的一件重要事情是三法则，它说:

> 如果您需要显式声明析构函数、复制构造函数或复制赋值操作符，您可能需要显式声明这三者。

对于现代 C++来说，我们需要增加两个函数，即移动构造函数和移动赋值操作符，这就是五的规则。

在上面的例子中，我们需要编写析构函数，所以我们需要所有五个特殊的成员函数。