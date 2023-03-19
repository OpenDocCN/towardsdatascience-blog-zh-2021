# 如何用 Python 中迭代器的组合追踪危险的小行星

> 原文：<https://towardsdatascience.com/how-to-track-hazardous-asteroids-with-composition-of-iterators-in-python-3945cf8e8f84?source=collection_archive---------31----------------------->

## 去锈蟒

## 应用函数组合创建迭代器管道

![](img/7b45f4ce102928906ee268e331f87078.png)

由 [SpaceX](https://unsplash.com/@spacex?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**功能性**和**面向对象的**编程经常被认为是相反的。将 Python 的迭代器对象组合成一个管道就是它们完美互补的例子。我将使用 NASA 的一个开放 API 来展示这种模式。

# 显示优点的东西

API 小行星——NeoWs 是一个 RESTful 的近地小行星信息网络服务。它允许搜索离地球最近的小行星。

你可以在 [NASA API 门户](https://api.nasa.gov/)上找到。

它包含一些奇怪的数据——最大物体直径、最近接近距离、接近日期等等。在这些数据中，每个物体都有一个字段显示该物体对地球有潜在的危险。

让我们考虑这样一种情况，当我们想要获取小行星数据，然后按日期将其写入文件，并找到一个潜在危险的小行星。

通常，我从编写代码开始，我希望如何与不存在的高级类或函数进行交互。

这段代码混合了两种范式**功能性**和**面向对象**编程。

**函数式编程**擅长数据处理任务，例如对数据进行顺序操作和改变。**对象**允许将相关的行为耦合到一个实体中，并存储一个状态。结合两种范例使代码更具声明性。

# 配置

我们将从隔离每个步骤开始，以分离对象。我们将创建存储每个步骤配置的基本类，稍后将添加所需的实现。

我们需要一个从 **URL 加载数据的步骤。**

接下来，我们添加一个步骤，它将按日期将加载的小行星数据写入文件。

最后一步将负责寻找危险的小行星。我们将存储一个负责该属性的字段。

# 功能组成

**函数组合**是函数式编程的主要模式之一。

直观地说，组合函数是一个链接过程，函数`*f*`的输出馈入函数`*g*`的输入。

在 Python 中，我们可以如下实现它。

```
def compose2(f, g):
    return lambda *args: g(f(*args))
```

注意，我们希望在从左到右读取函数时执行它们，我们使用了`g(f(x))`。

例如，考虑两个函数。

```
def square(x):
    return x*xdef format(x):
    return f"x*x = {x}"
```

我们可以应用`compose2()`函数制作一个新的。

```
>>> square_and_format = compose2(square, format)
>>> print(square_and_format(2))
x*x = 4
```

# 减少

另一种常用的功能模式是**折叠**或**减少**功能。`reduce()`方法将数组累加成一个值。`reduce()`方法为数组的每个值执行一个提供的函数(从左到右)。

顺便说一句，吉多·范·罗苏姆不喜欢它。

> 所以现在减少()。这实际上是我最讨厌的一个，因为除了几个涉及+或*的例子之外，几乎每次我看到带有重要函数参数的 reduce()调用时，我都需要拿起笔和纸，在我理解 reduce()应该做什么之前，画出实际输入到该函数中的内容。因此，在我看来，reduce()的适用性仅限于关联操作符，在所有其他情况下，最好显式写出累加循环。
> **——吉多·范·罗苏姆**， [**命运的减少(Python 3000 中的**](https://www.artima.com/weblogs/viewpost.jsp?thread=98196)

幸运的是，它被藏在了`functools`模块里。所以我们可以将我们的`compose2`函数应用到函数列表中。

```
from functools import reducedef compose(*functions):
    return reduce(compose2, functions)
```

现在，我们能够为函数列表应用**组合**。

# 请求即付的

为了能够对我们的对象应用函数组合和归约，我们需要使我们的类可调用。

在 Python 中，如果我们想让一个对象可调用，我们需要实现神奇的方法`__call__`。

对于`Load`类，这将是请求数据的简单返回。

```
class Load:
    ... def __call__(self):
        return requests.get(self.url)
```

对于`Write`对象来说，就有点棘手了。我们将迭代器作为参数，保存它，并返回对象本身。稍后我们将实现迭代接口，它将允许我们组合这样的对象。

```
class Write:
    ... def __call__(self, iterator):
        self.iterator = iterator
        return self
```

对于`Find`，它看起来与`Write`一样。

```
class Find:
    ... def __call__(self, iterator):
        self.iterator = iterator
        return self
```

# 迭代程序

最后，我们必须为对象实现一个迭代器接口。从技术上讲，一个 Python **对象**必须实现`__iter__`方法来提供迭代支持。

在我们的例子中，迭代也将保持数据处理的主要逻辑。

我们不需要为`Load`类实现迭代。它以列表的形式返回数据，默认情况下可以迭代。

对于类`Write`，处理和保存数据到文件的逻辑将在这个方法中实现。

对于`Find`类，在迭代过程中，我们将过滤满足我们条件的对象。

# 决赛成绩

这就是了。我们实现了几个类，这些类可以通过配置启动，并组成一个管道，如下所示。

```
pipe = pipeline.compose(
    pipeline.Load(
        url=API_URL, api_key="DEMO_KEY", start_date="2021-05-09"
    ),
    pipeline.Write(to_path="/tmp"),
    pipeline.Find(field="is_potentially_hazardous_asteroid"),
)for each in pipe():
    print(each)
```

完整的示例源代码请看我的 GitHub repo。

<https://github.com/pavel-fokin/iterators-composition>  

看起来不错！你怎么想呢?

***与我分享你对***[***LinkedIn***](https://www.linkedin.com/in/pavel-fokin/)***和***[***Twitter***](https://twitter.com/pavelfokin_)***。***

# 结论

这种模式有几个优点，我们举几个例子。

**优点**

*   关注点分离。每一步都有其独立的作用。
*   算法的更多声明性符号。
*   每一步都可以单独测试。

但是和其他模式一样，它不应该被过度使用。如果逻辑变得太复杂或者步骤太多，最好重新考虑这个方法。

# 更多阅读

如果你喜欢这篇文章，你可以对以下内容感兴趣。

不用 OOP 用 Python 解释**依赖倒置原理**。

<https://levelup.gitconnected.com/tenet-of-inversion-with-python-9759ef73dbcf>  

仔细观察 Python 中**多态性**的类型。

<https://levelup.gitconnected.com/hidden-power-of-polymorphism-in-python-c9e2539c1633>  

***感谢阅读！与我分享你对***[***LinkedIn***](https://www.linkedin.com/in/pavel-fokin/)***和***[***Twitter***](https://twitter.com/pavelfokin_)***的想法。如果你喜欢这个故事，请关注***[***me***](https://medium.com/@pavelfokin)***获取更多关于编程的精彩文章。***