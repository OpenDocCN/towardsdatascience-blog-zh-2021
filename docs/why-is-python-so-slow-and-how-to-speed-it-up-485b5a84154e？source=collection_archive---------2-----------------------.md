# Python 为什么这么慢，如何加速

> 原文：<https://towardsdatascience.com/why-is-python-so-slow-and-how-to-speed-it-up-485b5a84154e?source=collection_archive---------2----------------------->

## 看看 Python 的瓶颈在哪里

![](img/53afb5d1ac61ae32ec3bfeb580ba9ed8.png)

让我们看看 Python 引擎是如何工作的，这样我们就可以走得更快(图片由 [Unsplash](https://unsplash.com/photos/B12lJCBoBIE) 上的[凯文·布茨](https://unsplash.com/@kevin_butz)提供)

在这篇文章中，我们会发现 Python 并不是一门糟糕的语言，只是非常慢。它针对其构建目的进行了优化:简单的语法、可读的代码和开发人员的大量自由。然而，这些设计选择确实使 Python 代码比其他语言如 C 和 Java 慢。

理解 Python 在幕后是如何工作的将向我们展示它为什么慢的原因。一旦原因清楚了，我们就能解决它。读完这篇文章后，你会清楚地了解:

*   Python 是如何设计和工作的
*   为什么这些设计选择会影响执行速度
*   我们如何解决这些瓶颈，从而显著提高代码的速度

这篇文章分为三个部分。在**第一部分**中，我们来看看 Python 是如何设计的。然后，在**B 部分**中，看看这些设计选择如何以及为什么会影响速度。最后，在**C 部分**中，我们将学习如何解决 Python 设计中产生的瓶颈，以及如何显著提高代码速度。
走吧！

# A 部分——Python 的设计

让我们从定义开始。维基百科将 Python 描述为:

> Python 是一种解释型高级通用编程语言。它是动态类型化和垃圾回收的。

信不信由你，读完这篇文章后，你就会明白上面的两句话了。这个定义很好地展示了 Python 的设计。高级的、解释的、通用的、动态类型和垃圾收集的方式为开发人员减少了很多麻烦。

在接下来的部分中，我们将讨论这些设计元素，解释它对 Python 性能的意义，并以一个实际例子结束。

![](img/d044a67bccb4d861048bf6935037e2b7.png)

Python 就像一只风筝；容易使用并且不是超级快。c 就像战斗机；速度超快，但不太容易操作(图片由 [Shyam](https://unsplash.com/@thezenoeffect) 在 [Unsplash](https://unsplash.com/photos/G6TsQ-KayBw) 上提供)

## 缓慢与等待

首先，让我们谈谈当我们说“慢”时，我们试图测量什么。你的代码可能会因为很多原因而变慢，但并不是所有原因都是 Python 的错。假设有两种类型的任务:

1.  **输入/输出任务**
2.  **CPU-任务**

输入输出任务的例子是写一个文件，从一个 API 请求一些数据，打印一个页面；它们涉及*等待*。尽管它们会导致你的程序花费更多的时间来执行，但这不是 Python 的错。它只是在等待回应；更快的语言不能等待更快。这种缓慢是*而不是*我们在本文中试图解决的问题。正如我们稍后将看到的，我们可以线程化这些类型的任务(在本文 的 [**中也有描述)。**](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e)

在本文中，我们解释了为什么 Python 执行 CPU 任务**比其他语言慢。**

## 动态类型与静态类型

Python 是动态类型的。在像 C、Java 或 C++这样的语言中，所有的变量都是静态类型的，这意味着你要写下像`int my_var = 1;`这样的变量的具体类型。在 Python 中，我们只需输入`my_var = 1`。然后我们甚至可以分配一个完全不同类型的新值，比如`my_var = “a string"`。我们将在下一章看到它是如何工作的。

尽管动态类型对于开发人员来说非常容易，但是它有一些主要的缺点，我们将在接下来的部分中看到。

## 编译与解释

编译代码意味着将一种语言的程序转换成另一种语言，通常是比源代码更低级的语言。当你编译一个用 C 语言写的程序时，你把源代码转换成机器码(CPU 的实际指令)，然后你就可以运行你的程序了。

Python 的工作方式略有不同:

1.  源代码不是编译成机器码，而是编译成平台无关的**字节码**。像机器码一样，字节码也是指令，但是它们不是由 CPU 执行，而是由解释器执行。
2.  源代码在运行时被编译**。Python 根据需要编译文件，而不是在运行程序前编译所有东西。**
3.  **解释器**分析字节码并将其翻译成机器码。

Python 必须编译成字节码，因为它是动态类型的。因为我们没有预先指定变量的类型，所以我们必须等待实际的值，以便在转换为机器代码之前确定我们试图做的事情实际上是否合法(比如将两个整数相加)。这就是**解释器**所做的事情。在静态类型的编译语言中，编译和解释发生在运行代码之前。

总之:代码会因为运行时发生的编译和解释而变慢。相比之下，静态类型的编译语言编译后只运行 CPU 指令。

用 C 语言编写的编译模块来扩展 Python 实际上是可能的。 [**这篇文章**](https://mikehuls.medium.com/getting-started-with-cython-how-to-perform-1-7-billion-calculations-per-second-in-python-b83374cfcf77) **和** [**这篇文章**](https://mikehuls.medium.com/write-your-own-c-extension-to-speed-up-python-x100-626bb9d166e7) 演示了如何用 C 语言编写自己的扩展来加速代码 **x100** 。

## 垃圾收集和内存管理

当你在 Python 中创建一个变量时，解释器会自动在内存中挑选一个足够大的地方来存放变量值，并将它存储在那里。然后，当不再需要该变量时，内存槽再次被释放，以便其他进程可以再次使用它。

在编写 Python 的语言 C 中，这个过程根本不是自动化的。当你声明一个变量时，你需要指定它的类型，这样才能分配正确的内存量。垃圾收集也是手动的。

那么 Python 如何跟踪哪个变量要进行垃圾收集呢？对于每个对象，Python 会跟踪有多少对象引用了该对象。如果一个变量的引用计数是 0，那么我们可以断定这个变量没有被使用，它可以在内存中被释放。我们将在下一章看到这一点。

## 单线程与多线程

一些语言，比如 Java，允许你在多个 CPU 上并行运行代码。然而，Python 被设计成在单个 CPU 上是单线程的。确保这一点的机制被称为 GIL:全局解释器锁。GIL 确保解释器在任何给定时间只执行一个线程。

GIL 解决的问题是 Python 使用**引用计数**进行内存管理的方式。需要保护变量的引用计数，防止两个线程同时增加或减少计数。这可能会导致各种奇怪的内存泄漏错误(当一个对象不再需要但没有被删除时)，或者更糟糕的是，错误地释放内存。在最后一种情况下，一个变量被从内存中删除，而其他变量仍然需要它。

简而言之:由于垃圾收集的设计方式，Python *不得不*实现一个 GIL 来确保它在单线程上运行。尽管有办法避开 GIL，阅读 [**这篇文章**](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e) ，线程化或多重处理你的代码并显著加速*。*

# *B 部分——引擎盖下的一瞥:实践中的 Pythons 设计*

*理论讲够了，让我们来看看实际行动吧！现在我们知道了 Python 是如何设计的，让我们看看它是如何工作的。我们将比较 C 和 Python 中变量的简单声明。通过这种方式，我们可以看到 Python 如何管理它的内存，以及为什么它的设计选择会导致执行时间比 c 慢。*

*![](img/26da4f9e05c976bd38a8e2092b4ff23a.png)*

*现在我们已经完全解构了 Python，让我们把它放回一起，看看它是如何运行的(图片由 [Jordan Bebek](https://unsplash.com/@freshgreenfreedom) 在 [Unsplash](https://unsplash.com/photos/2u4o1of4ONA) 上提供)*

## *在 C 中声明变量*

*让我们从在 C 中声明一个名为 *c_num* 的整数开始。*

```
*int c_num = 42;*
```

*当我们执行这行代码时，我们的机器会执行以下操作:*

1.  *在某个地址(内存中的位置)为整数分配足够的内存*
2.  *将值 *42* 分配给上一步分配的内存位置*
3.  *将 *c_num* 指向该值*

*现在内存中有一个对象，看起来像这样:*

*![](img/322f237236488f0b86ca1f8ae152687e.png)*

*一个名为 c_num 的整型变量的表示，值为 42(图片由作者提供)*

*如果我们给 *c_num* 分配一个新号码，我们就把这个新号码写到同一个地址； ***覆盖*** ，以前的值。这意味着变量是可变的。*

*![](img/6c60891517518717c00c5016b2c96677.png)*

*我们给 c_num(作者图片)赋值 404*

*请注意，地址(或内存中的位置)没有改变。想象一下，c_num 拥有一块足够容纳一个整数的内存。您将在下一部分看到这与 Python 的工作方式不同。*

## *在 Python 中声明变量*

*我们将做与前一部分完全相同的事情；声明一个整数。*

```
*py_num = 42*
```

*这一行代码在执行过程中触发以下步骤:*

1.  *创建一个对象；给一个地址分配足够的内存*
2.  *将 PyObject 的 typecode 设置为整数(由解释器决定)*
3.  *将 PyObject 的值设置为 42*
4.  *创建一个名字叫做 *py_num**
5.  *将 *py_num* 指向对象*
6.  *将 PyObject 的 refcount 增加 1*

*在引擎盖下，首先要做的是创建一个对象。这就是“Python 中的一切都是对象”这句话的含义。Python 可能有`int`、`str`和`float`类型，但是[在幕后，每个 Python 变量只是一个对象](https://mikehuls.medium.com/write-your-own-c-extension-to-speed-up-python-x100-626bb9d166e7)。这就是为什么动态类型是可能的。*

*注意 PyObject 是*而不是*Python 中的对象。它是 C 语言中的一个结构，表示所有 Python 对象。如果你对这个 PyObject 在 C 中的工作方式感兴趣，可以看看这篇文章**，在这篇文章中，我们用 Python 编写了自己的 C 扩展，提高了执行速度 x100！***

***上述步骤在下面的内存中创建(简化的)对象:***

***![](img/c86b69058c5608af2a0beabf6931038b.png)***

***内存中的 Python 整数(简化版)(图片由作者提供)***

***您会立即注意到，我们执行了更多的步骤，需要更多的内存来存储一个整数。除了类型和值之外，我们还存储 refcount 用于垃圾收集。您还会注意到，我们创建的变量 *py_num* 没有内存块。该内存由新创建的 *py_num* 指向的对象所拥有。***

> ***从技术上讲，Python 没有像 C 语言那样的变量；Python 有名字。变量拥有内存并可以被覆盖，名字是变量的指针。***

***那么当我们想给 *py_num* 赋一个不同的值时会发生什么呢？***

1.  ***在某个地址创建一个新的对象，分配足够的内存***
2.  ***将对象的类型码设置为整数***
3.  ***将 PyObject 的值设置为 404(新值)***
4.  ***指向对象的指针***
5.  ***将新的 PyObject 的 refcount 增加 1***
6.  ***将旧对象的引用计数减少 1***

***这些步骤会改变记忆，如下图所示:***

***![](img/5337138493a23a05ab562ea2fc11f843.png)***

***给 py_num(作者图片)赋值后的内存***

***上图将展示我们没有给 *py_num* 赋值，而是将名称 *py_num* 绑定到一个新对象。这样我们也可以分配一个不同类型的值，因为每次都会创建一个新的对象。Py_num 只是指向一个不同的 PyObject。我们不像在 C 中那样覆盖，我们只是指向另一个对象。***

***还要注意旧对象上的 refcount 是 0；这将确保它被垃圾收集器清理掉。***

# ***C 部分——如何加快速度***

***在前面的部分中，我们已经深入挖掘了 Pythons 的设计，并且已经看到了实际效果。我们可以得出结论，执行速度的主要问题是:***

*   *****解释**:由于变量的动态类型化，编译和解释发生在运行时。出于同样的原因，我们必须创建一个新的 PyObject，在内存中选择一个地址，并分配足够的内存。每次我们创建或“覆盖”一个“变量”时，我们都会创建一个新的 PyObject，并为其分配内存。***
*   *****单线程**:垃圾收集的设计方式强制 GIL:将所有执行限制在单个 CPU 上的单线程上***

***![](img/b62b92125dd8409695fc3ec6e9d34eab.png)***

***是时候用这款喷气发动机加速我们的热腐了(图片由 [Kaspars Eglitis](https://unsplash.com/@kasparseglitis) 在 [Unsplash](https://unsplash.com/photos/fkcjWXPRAZU) 上拍摄)***

***那么，有了这篇文章的所有知识，我们如何补救这些问题呢？以下是一些提示:***

1.  ***像`range()`一样使用 Python 中的内置 C 模块***
2.  ***I/O 任务释放 GIL，因此它们可以被线程化；您可以等待许多任务同时完成(**更多信息** [**这里**](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e) **和** [**这里**](https://mikehuls.medium.com/advanced-multi-tasking-in-python-applying-and-benchmarking-threadpools-and-processpools-90452e0f7d40) )***
3.  ***通过多重处理并行运行 CPU 任务( [**更多信息**](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e) )***
4.  ***创建自己的 C 模块并导入 Python 你可以用比 Python 快 100 倍的编译 C 代码来扩展 Python。( [**信息**](https://mikehuls.medium.com/write-your-own-c-extension-to-speed-up-python-x100-626bb9d166e7) )***
5.  ***不是有经验的 C 程序员？编写类似 Python 的代码，由 Cython 编译成 C，然后整齐地打包成 Python 包。它以 C 语言的速度提供了 Python 的可读性和简单语法( [**更多信息**](https://mikehuls.medium.com/getting-started-with-cython-how-to-perform-1-7-billion-calculations-per-second-in-python-b83374cfcf77) )***

***![](img/bd65a2d59992b2c7e9953f4d59269679.png)***

***这才叫快！(图片由 [SpaceX](https://unsplash.com/@spacex) 在 [Unsplash](https://unsplash.com/photos/-p-KCm6xB9I) 上拍摄)***

# ***结论***

***如果你还在读这篇文章，那么这篇文章的复杂性和篇幅并没有吓退你。向你致敬！我希望已经阐明了 Python 如何在幕后工作以及如何解决其瓶颈。***

***如果你有建议/澄清，请评论，以便我可以改进这篇文章。与此同时，请查看我的其他关于各种编程相关主题的文章，比如:***

*   ***[自己写 C 扩展加速 Python x100](https://mikehuls.medium.com/write-your-own-c-extension-to-speed-up-python-x100-626bb9d166e7)***
*   ***[cyt hon 入门:如何用 Python 执行>每秒 17 亿次计算](https://mikehuls.medium.com/getting-started-with-cython-how-to-perform-1-7-billion-calculations-per-second-in-python-b83374cfcf77)***
*   ***[Python 中的多任务处理:通过同时执行，将程序速度提高 10 倍](https://mikehuls.medium.com/multi-tasking-in-python-speed-up-your-program-10x-by-executing-things-simultaneously-4b4fc7ee71e)***
*   ***[Python 中的高级多任务处理:应用线程池和进程池并进行基准测试](https://mikehuls.medium.com/advanced-multi-tasking-in-python-applying-and-benchmarking-threadpools-and-processpools-90452e0f7d40)***
*   ***[用 FastAPI 用 5 行代码创建一个快速自动归档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)***
*   ***[创建并发布自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)***
*   ***[创建您的定制私有 Python 包，您可以从您的 Git 库 PIP 安装该包](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893)***
*   ***[绝对初学者的虚拟环境——什么是虚拟环境，如何创建虚拟环境(+示例)](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)***
*   ***[通过简单的升级大大提高您的数据库插入速度](https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424)***

***编码快乐！***

***—迈克***

***页（page 的缩写）学生:比如我正在做的事情？跟我来！***