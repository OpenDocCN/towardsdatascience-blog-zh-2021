# Python 的隐藏宝石

> 原文：<https://towardsdatascience.com/hidden-gems-of-python-76020b14e42f?source=collection_archive---------1----------------------->

## 我甚至不知道 Python 有哪些特性

![](img/b55591e3c00249fed8656ccd4a6c77bf.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Ksenia Makagonova](https://unsplash.com/@dearseymour?utm_source=medium&utm_medium=referral) 拍摄的照片

这些天我有了一个新的消遣——阅读 Python 文档只是为了好玩！当你在闲暇时阅读一些东西，你会注意到一些有趣的花絮，否则你会错过的。所以，这是让我“哦！你可以用 Python 来做吗？”

# 1.功能属性

类似于设置类和对象的属性，也可以设置函数的属性。

我们在第 10 行设置了属性‘optional _ return ’,在第 11 行设置了属性‘is _ awesome’。在后面的第 19 & 20 行中，我们已经在函数之外访问了这些属性。代码的输出将是:

```
Final answer is 2197
Show calculations --> 13
Is my function awesome? --> Yes, my function is awesome.
```

当您希望选择检索某个中间变量，而不是在每次调用函数时用 return 语句显式返回它时，这就很方便了。还要注意，属性可以从函数定义内部或从函数定义外部设置。

# 2.For-else 循环

在 Python 中，可以为循环的*添加一个 *else* 子句。只有在执行过程中在循环体中没有遇到 *break* 语句，才会触发 *else* 子句。*

```
All elements at least 3 letters long
```

注意 *else* 对于缩进在*的层次上，而对于*不在*的层次上。这里，没有元素的长度短于 3。所以，永远不会遇到 *break* 语句。因此， *else* 子句将被触发(在*循环的*被执行后)并打印如上所示的输出。*

有人可能会说，这可以通过使用一个单独的变量来实现，该变量跟踪是否遇到了 *break* 语句。也许它也能减少下一个阅读代码的人的困惑。以下是实现相同结果的等效方法:

不过，我想知道还是好的。

# 3.“int”的分隔符

很难在视觉上区分 10000000 和 10000000 这样的整数(它们甚至是不同的数字吗？).我们不能像在英语中一样在 Python 中使用逗号，因为 Python 会将其解释为多个整数的元组。

Python 有一个非常方便的处理方法:我们可以使用下划线来提高可读性。因此，1_000_000 将被解释为单个 *int* 。

```
<class 'int'>
<class 'int'>
True
```

# 4.eval()和 exec()

Python 能够动态读取一个字符串，并像处理一段 Python 代码一样处理它。这是通过使用 *eval* ()和 *exec* ()函数实现的(‘eval’用于评估*表达式*，‘exec’用于执行*语句*)。

```
b = 5
c is 9
```

在第 3 行， *eval* ()函数将输入字符串作为 Python 表达式读取，对其求值，并将结果赋给变量‘b’。在第 6 行， *exec* ()函数将输入字符串作为 Python 语句读取并执行。

您甚至可以将动态创建的字符串传递给这些函数。例如，您可以创建 1000 个名为 x_0，x_1，…，x_999 的变量，而不必在代码中手动编写每个变量声明。这可能看起来像是一个完全没有意义的特性，但事实并非如此。

> 在更广泛的编程环境中，不仅仅是 Python，eval/exec 的使用非常强大，因为它允许您编写动态代码，使用运行时可用的信息来解决甚至无法在编译时表达的问题。[……]exec 实际上是嵌入在 Python 中的 Python 解释器，因此如果您有一个特别难解决的问题，您可以解决它的方法之一是编写一个程序来*编写一个程序来解决它*，然后使用 exec 运行第二个程序。

你可以阅读更多史蒂文·达普拉诺的美丽解释。

# 5.省略

*省略号*或“…”是 Python 的内置常量，类似于*无*、*真*、*假*等内置常量。它可以以不同的方式使用，包括但不限于:

## 5.1 未成文代码的占位符

与 *pass 类似，*省略号可以在代码没有写完整时用作占位符，但需要一些占位符来保证语法准确性

## 5.2 替代'*无'*

当想要表示一个空的输入或返回时，通常选择 None。但有时 *None* 可以是函数的预期输入或返回之一。在这种情况下，省略号可以作为占位符。

给定 n，函数 *nth_odd()* 计算第 n 个奇数。给定第 n 个奇数，函数 *original_num* ()计算原始数 n。这里， *None* 是函数 *original_num* ()的预期输入之一，因此我们不能将其用作参数 *m 的默认占位符。*代码的输出将是:

```
This function needs some input
Non integer input provided to nth_odd() function
9 is 5th odd number
16 is not an odd number
```

## 5.3 NumPy 中的数组切片

NumPy 使用*省略号*对数组进行切片。下面的代码显示了分割 NumPy 数组的两种等效方法:

```
[ 0  2  4  6  8 10 12 14]
[ 0  2  4  6  8 10 12 14]
```

因此，'…'表示需要多少':'就有多少。

## 省略的布尔值

与*无(*的布尔值为*假)相反，*的*省略号*的布尔值被视为*真。*

# TL；速度三角形定位法(dead reckoning)

总之，到目前为止我发现的有趣的特性是:

1.  函数属性:像对象一样给函数分配属性
2.  For-else 循环:跟踪循环是否在没有 *break* 语句的情况下执行
3.  *int* 的分隔符:32_534_478 是一个 *int*
4.  *eval* ()和 *exec* ():将字符串作为 Python 代码读取并运行
5.  *省略号*:通用内置常量。

# 离别赠言

Python 不仅是一种有用的语言，而且是一种非常有趣的语言。我们在生活中都很忙，但是为了更好地了解这门语言并没有坏处。我很想知道一些你可能发现的复活节彩蛋。

![](img/15ac4010e2fc9a728dabd32dae438f78.png)

图片由 [u/ANewMuleSkinner](https://www.reddit.com/user/ANewMuleSkinner/) 发布在 [reddit](https://www.reddit.com/r/seinfeld/comments/2g94iv/are_you_reading_my_vcr_manual/) 上

# 资源

您可以在下面的 Python 文档页面上阅读更多关于我上面提到的内容:

[1] [功能属性](https://docs.python.org/3/reference/datamodel.html#:~:text=Special%20read%2Donly%20attributes%3A%20__,in%20or%20None%20if%20unavailable.)

[For-else](https://docs.python.org/3/tutorial/controlflow.html#break-and-continue-statements-and-else-clauses-on-loops)

[3] [评估/执行](https://docs.python.org/3/library/functions.html#eval)

[省略号](https://docs.python.org/3/library/constants.html#Ellipsis)