# 如何在 Python 中使用多重处理包

> 原文：<https://towardsdatascience.com/how-to-use-the-multiprocessing-package-in-python3-a1c808415ec2?source=collection_archive---------0----------------------->

## 用不到 6 分钟的时间了解多重处理

![](img/4ae2e44b5a841c4c8f135159eda67545.png)

由[朱莉安娜·罗蒙](https://unsplash.com/@roomajus)在 [Unsplash](https://unsplash.com/) 上拍摄的照片

当一个长时间运行的进程必须加速或者多个进程必须并行执行时，多重处理是最重要的。在单核上执行一个进程会限制它的能力，否则它会将触角伸向多个内核。如果耗时的任务有并行运行的范围，并且底层系统有多个处理器/内核，Python 提供了一个易于使用的接口来嵌入多处理。

本文将区分多处理和线程，引导您了解用于实现多处理的两种技术——进程和池，并探索进程的交互和共享内存的概念。

# 多重处理与多线程

多重处理利用全部 CPU 内核(多个进程)，而多线程将多个线程映射到每个进程。在多重处理中，每个进程都与自己的内存相关联，这不会导致数据损坏或死锁。线程利用共享内存，因此加强了线程锁定机制。对于与 CPU 相关的任务，多处理是更好的选择，然而，对于与 I/O 相关的任务( [IO 绑定与 CPU 绑定的任务](https://www.oreilly.com/library/view/hands-on-microservices-with/9781789342758/82531d54-039c-4c96-8fcb-58a53cee28e6.xhtml))，多线程执行得更好。

> 在 Python 中，全局解释器锁(GIL)是一种只允许单线程控制 Python 解释器的锁。在多线程的情况下，它主要用于 IO 绑定的作业，GIL 没有太大的影响，因为锁是在线程之间共享的，而它们正在等待 I/O
> 
> 另一方面，多处理为每个进程分配一个 Python 解释器和 GIL。

在下一节中，让我们看看多处理包的一个重要概念——Process 类。

# 使用过程

多处理中的`Process`类一次性分配内存中的所有任务。使用`Process`类创建的每个任务都必须分配一个单独的内存。

设想一个场景，其中要创建十个并行进程，每个进程都必须是一个单独的系统进程。

下面是一个例子(输出的顺序是不确定的):

[](https://replit.com/@allasamhita/Process-Class?lite=true) [## 流程类

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Process-Class?lite=true) 

`Process`类为从 0 到 10 的数字启动了一个进程。`target`指定要调用的函数，`args`确定要传递的参数。`start()`方法开始流程。所有的进程都循环等待，直到每个进程执行完毕，这是使用`join()`方法检测到的。`join()`有助于确保程序的其余部分仅在多重处理完成后运行。

`***sleep()***` *方法有助于理解进程的并发程度！*

在下一节中，让我们看看各种进程通信技术。

## 如何实现管道

如果两个进程需要通信，管道是最好的选择。一个管道可以有两个端点，每个端点都有`send()`和`recv()`方法。如果两个进程(线程)同时读取或写入同一个端点，管道中的数据可能会损坏。

 [## 管道通信

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pipe-Communication?lite=true) 

`cube_sender`和`cube_receiver`是使用管道相互通信的两个进程。

*   数字 19 的立方从管道的一端送到另一端(`x_conn`到`y_conn`)。`x_conn`是流程`p1`的输入。当在`x_conn`上调用`send()`时，输出被发送到`y_conn`。
*   `y_conn`是流程`p2`的输入，该流程接收输出并打印结果立方体。

## 如何实现队列

要在共享通信通道中存储多个进程的输出，可以使用队列。例如，假设任务是找出前十个自然数的立方，然后给每个数加 1。

定义两个功能`sum()`和`cube()`。然后定义一个队列(`q`)，调用`cube()`函数，然后调用`add()`函数。

[](https://replit.com/@allasamhita/Queue-Communication?lite=true) [## 队列通信

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Queue-Communication?lite=true) 

代码解释了两个进程之间对象的通信，在我们的例子中是`q`。方法`empty()`是确定队列是否为空，`get()`返回存储在队列中的值。

结果的顺序是不确定的。

## 如何实现共享内存

受队列的启发，共享内存无缝地存储进程间的共享数据。它可以有两种类型:**值**或**数组**。

## 价值

单个值可以在多个流程之间共享，如下所示:

[](https://replit.com/@allasamhita/Shared-Memory-Value?lite=true) [## 共享内存值

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Shared-Memory-Value?lite=true) 

数字 19 作为参数传递给函数`cube()`。`value`属性取`Value`、*、*、`num`的实际值。修改后的数字随后被发送到`cube()`功能。最终的双立方数反映在打印语句中。

## 排列

值列表可以在多个流程之间共享，如下所示:

[](https://replit.com/@allasamhita/Shared-Memory-Array?lite=true) [## 共享内存阵列

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Shared-Memory-Array?lite=true) 

`Array()`初始化拥有长度为 3 的`int`数据类型的空数组。通过给数组中的每个元素加 1 来循环数组。

你可以在不同的过程中使用`arr`，就像**值**一样。这本质上就是**共享内存**的概念。

**注**:‘d’表示双精度浮点数，‘I’(在数组中(" I "，3))表示有符号整数。

## 如何实现服务器进程

服务器进程是在 Python 程序开始时触发的主进程。其他进程可以利用它的对象进行操作。类别`Manager()`的管理器对象控制服务器进程。`Manager()` 支持多种数据类型，如 **list、dict、Lock、RLock、Semaphore、BoundedSemaphore、Namespace、Condition、Event、Queue、Value、Array** 。

为了更好地理解这个概念，请看这个例子:

[](https://replit.com/@allasamhita/Server-Process?lite=true) [## 服务器进程

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Server-Process?lite=true) 

这里使用 manager 对象初始化和操作字典和列表类型。

## 共享内存与服务器进程:

1.  `Manager()`与共享内存相比，支持多种数据类型
2.  通过网络，不同计算机上的进程可以共享一个管理器
3.  服务器进程比共享内存慢

# 使用游泳池

多重处理中的`Pool`类可以处理大量的进程。它允许您在每个进程中运行多个作业(因为它能够对作业进行排队)。内存只分配给正在执行的进程，不像`Process`类，它把内存分配给所有的进程。`Pool`类获取存在于池中的工作进程的数量，并生成进程。

使用`Process`类启动许多进程实际上是不可行的，因为它可能会破坏操作系统。因此，出现了一个池，它将在存在最少数量的工作进程的情况下，处理向所有产生的进程分配作业和从所有产生的进程收集结果(*最优选地，工作进程的数量等于 CPU 核心的数量*)。

`*Pool*` *在交互式解释器和 Python 类中不起作用。它要求 __main__ 模块可由子模块导入。*

`Pool`类自带**六个**有价值的方法:

## **敷**

`apply()`方法**阻塞**主进程，直到所有进程完成。它**接受多个参数，保持结果的顺序，并且** **不是并发的**。

[](https://replit.com/@allasamhita/Pool-apply?lite=true) [## 池应用()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-apply?lite=true) 

如果要计算前十个自然数的立方，必须循环这些数字，一次发送一个给`apply()`方法。这里的进程数设置为 4；然而，`cube()`只在一个池工作线程中执行。

`close()`方法终止池，`join()`等待工作进程终止。

## **地图**

`map()`方法**支持** **并发** — **不接受多个参数，阻塞主程序**，直到所有进程完成。它还保持结果的顺序或**(尽管计算顺序可能不同！).**

[](https://replit.com/@allasamhita/Pool-map?lite=true) [## 池地图()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-map?lite=true) 

与`apply()`不同，`map()`接受迭代器作为参数传递给函数`cube()`。

## **应用 _ 异步**

`apply_async()`中的回调函数可用于在其执行完成后立即返回值。这个方法**维护结果**的顺序，而**支持并发**。

[](https://replit.com/@allasamhita/Pool-applyasync?lite=true) [## 池应用 _ 异步()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-applyasync?lite=true) 

**注意**:您可以使用`wait()`来阻止异步调用。

## **map_async**

与`map()`不同，`map_async()`是**非阻塞(并维持结果的顺序)**。

[](https://replit.com/@allasamhita/Pool-mapasync?lite=true) [## 池映射 _ 异步()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-mapasync?lite=true) 

当`map_async()`运行时,“这里”和“再次这里”被写入控制台，展示了它的非阻塞特性。但是，您可以使用`wait()`来阻止异步调用。

## **星图**

与 map()不同，`starmap()`接受多个参数。它**维持结果的顺序，是并发的，阻塞主进程**。

[](https://replit.com/@allasamhita/Pool-starmap?lite=true) [## Pool star_map()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-starmap?lite=true) 

## **星图 _ 异步**

与`starmap()`不同，`starmap_async()`是**非阻塞的(并且维持结果的顺序)**。

[](https://replit.com/@allasamhita/Pool-starmapasync?lite=true) [## 池 starmap_async()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-starmapasync?lite=true) 

## 交互邮件访问协议

与`map()`不同的是，`imap()`不会等待所有的结果，而是返回一个迭代器(不是一个列表)。

[](https://replit.com/@allasamhita/Pool-imap?lite=true) [## 池 imap()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-imap?lite=true) 

## **imap_unordered**

与`imap()`不同，结果的**顺序并不总是保持**。

[](https://replit.com/@allasamhita/Pool-imapunordered?lite=true) [## 池 imap_unordered()

### 由 allasamhita 回复的 Python

replit.com](https://replit.com/@allasamhita/Pool-imapunordered?lite=true) 

# 结论

在本教程中，您学习了如何使用 Python 中的多重处理实用程序。您学习了**进程**与**池**的不同之处，并且您创建了一个`cube()`函数来理解所有的概念。您了解了进程通信、共享内存、服务器进程以及同步和异步池。

我希望你喜欢阅读这篇文章。

*参考:*[【https://docs.python.org/3/library/multiprocessing.html】T42](https://docs.python.org/3/library/multiprocessing.html)