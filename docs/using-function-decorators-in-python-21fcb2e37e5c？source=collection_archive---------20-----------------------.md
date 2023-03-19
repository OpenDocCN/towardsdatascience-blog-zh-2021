# 在 Python 中使用函数装饰器

> 原文：<https://towardsdatascience.com/using-function-decorators-in-python-21fcb2e37e5c?source=collection_archive---------20----------------------->

## 了解如何通过使用函数装饰器来扩展函数的功能

![](img/8951a72de6000dadfcc9df0c6affd565.png)

照片由[阿里·马哈茂迪](https://unsplash.com/@sam_ihmn?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在这篇文章中，我将谈谈 Python 中的 ***函数装饰器*** ，一个不容易把握的话题，却是 Python 编程中极其有用的设计模式。

在 Python 中，*函数装饰器*实际上是*函数包装器*。

> 函数装饰器通过包装函数来扩展函数的功能，而不修改其最初的预期行为。

像往常一样，让我们从基础开始，慢慢地理解什么是函数装饰器。

# 在函数中定义函数

在 Python 中，函数体可以包含一个或多个函数:

```
def do_something():
    def internal_function():
        return "In internal_function() function"

    return internal_function
```

在上面的代码片段中，在`do_something()`中我有另一个名为`internal_function`的函数。当`do_something()`被调用时，它会将`internal_function`返回给调用者。让我们验证一下:

```
f = do_something()
type(f)                # *function*
```

在上面的语句中，当`internal_function`返回给调用者时，我将它赋给了一个名为`f`的变量。因为可以将函数赋给变量，所以现在可以使用变量调用函数，如下所示:

```
f()                    # 'In internal_function() function'
```

# 将函数传递给函数(也称为包装函数)

让我们修改`do_something()`，使它现在有一个名为`f`的参数。该参数现在将接受一个可以在`internal_function()`中调用的函数参数:

```
def do_something(**f**):    
    def internal_function(): **return f()** return internal_function
```

假设现在我有了另一个名为`function1`的函数:

```
def function1():
    return 'function1'
```

我现在可以将这个函数传递给`do_something()`:

```
f = do_something(**function1**)
```

`f`现在将包含对`internal_function()`的引用，该引用返回执行`function1`的结果(传递到`do_something()`)。作为函数调用`f`和直接调用`function1()`是一样的:

```
f()            # 'function1'
# same as:
function1()    # 'function1'
```

> 本质上，我们在这里做的是用另一个函数包装`*function1*`，在本例中是`*do_something*`。

# 函数装饰器

在 Python 中，`do_something()`被称为**函数装饰器**。而不是将`function1`传入`do_something()`(就像我们在上一节所做的那样):

```
f = do_something(**function1**)  # think of this as wrapping function1 
                             # with do_something
```

Python 允许您给函数装饰器加上前缀“ **@”符号**，并将其放在您想要换行的函数之前，如下所示:

```
**@do_something**
def function1():
    return 'function1'
```

> 他被称为函数装饰者。

您现在可以正常调用`function1()`:

```
function1()    # 'function1'
```

# 带参数的函数修饰符

在前面的例子中，您的函数装饰器(`do_something()`)包装了一个不接受任何参数的函数(`function1()`不接受任何参数)。如果您现在想将它包装在一个接受参数的函数中，比如下面的函数，该怎么办呢？

```
def add(n,m):
    return n + m
```

在这种情况下，需要给`internal_function()`添加参数:

```
def do_something(f):    
    def internal_function(**n,m**):
        return f(**n,m**) return internal_function
```

您现在可以像这样使用函数装饰器:

```
@do_something
def add(n,m):
    return n+madd(**4,5**)    # 9
```

> 如果你不使用@符号，你的代码将如下所示:
> 
> `f = do_something(add)`
> `f(4,5) # 9`

这个函数装饰器现在可以应用于任何接受两个参数的函数。但是，如果您想将它应用于接受不同数量参数的函数，该怎么办呢？在这种情况下，您可以使用 ***args** 和 ****kwargs** 类型定义一个通用函数装饰器。

> 如果你不熟悉它们的工作方式，请参考我的文章“**理解 Python 中的*args 和* * kwargs**”([https://towards data science . com/Understanding-args-and-kwargs-in-Python-321937 f 49 C5 b](/understanding-args-and-kwargs-in-python-321937f49c5b))。

我们修改后的`do_something()`函数装饰器现在看起来像这样:

```
def do_something(f):    
    def internal_function(***args, **kwargs**):
        return f(***args, **kwargs**) return internal_function
```

您现在可以将`do_something()`函数装饰器应用于具有不同参数的函数:

```
@do_something
def add(n,m):
    return n+m**@do_something
def square(n):
    return n**2**print(add(4,5))     # 9
**print(square(2))    # 4**
```

# 函数装饰符的使用

我知道你脑子里的下一个问题是:那么函数装饰器的用例是什么？上面的函数 decorators 似乎没有做任何有用的事情。

我给你举个例子。假设您想创建一个日志文件，在每次调用函数时记录一个条目，详细信息如下:

*   被调用函数的名称
*   函数接收的参数
*   调用了时间函数

如果没有函数装饰器，您需要将代码插入到您希望记录的所有函数中。但是这样做，你实际上是在修改函数的功能。您希望在不修改函数的情况下做到这一点。

你可以用一个函数装饰器轻松解决这个问题。

要创建一个函数装饰器，首先要创建一个函数，比如说，`function_logger()`，其中有一个内部函数叫做`wrapper()`(你可以使用任何你想要的名字):

```
from datetime import datetimedef function_logger(f):    
    def wrapper(*args, **kwargs):
        # store the current date and time, and function details 
        # into a log file:
        date_time = datetime.now().strftime("%y-%m-%d %H:%M:%S")
        with open("log.txt", "a") as logfile:
            logfile.write(f'{f.__name__}: {args}, {date_time}\n')           
        return f(*args, **kwargs) return wrapper
```

现在，您可以对任何函数应用函数装饰器:

```
**@function_logger**
def add(n,m):
 return n+m**@function_logger**
def square(n):
 return n**2print(add(4,5))
print(square(2))
```

调用这些函数将导致在名为 **log.txt** 的日志文件中添加一个条目:

```
add: (4, 5), 21-05-29 14:16:56
square: (2,), 21-05-29 14:16:56
```

如果您不再需要记录函数，只需删除函数装饰器！

# 带参数的函数修饰符

上一节中的例子显示了`function_logger`函数装饰器在每次调用被记录的函数时为日志文件创建一个条目。如果您希望根据函数的类型将条目记录到不同的文件中，该怎么办？嗯，很简单——你只需用另一个函数包装`function_logger()`,把文件名作为参数传入，然后返回`function_logger`函数，就像这样:

```
from datetime import datetime**def logger(filename):** 
    def function_logger(f):    
        def wrapper(*args, **kwargs):
            date_time = datetime.now().strftime("%y-%m-%d %H:%M:%S")
            with open(**filename**, "a") as logfile:
                logfile.write(
                    f'{f.__name__}: {args}, {date_time}\n')
            return f(*args, **kwargs)
        return wrapper
 **return function_logger**
```

现在，您可以指定要使用的日志文件:

```
**@logger('log1.txt')**
def add(n,m):
    return n+m**@logger('log2.txt')**
def square(n):
    return n**2print(add(4,5))
print(square(2))
```

> 如果不使用@符号，您的代码将如下所示:
> 
> `f = logger(‘log1.txt’)(add)`
> `f(4,5)`

简单？我希望这篇文章能揭开 Python 中函数装饰器背后的奥秘。下次您遇到函数装饰器时，您不必再想了——只要把它们想象成函数包装器就行了。