# 布尔既有趣又神秘

> 原文：<https://towardsdatascience.com/booleans-are-fun-and-mysterious-9013e5f837c2?source=collection_archive---------62----------------------->

## 数据科学

## 一些有趣的关于布尔数据类型的谜题和答案。

![](img/b0b687fd3521c3995a1a97c1ddca8aad.png)

由[阿莱克·戈麦斯](https://unsplash.com/@allecgomes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/white?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在数据科学和编程中，布尔(真和假)数据被认为是简单而枯燥的。我相信布尔是有趣的，如果你不知道他们的逻辑，有时是不可预测的。我将说服你们中的许多人，一些有趣的特性与布尔对象相关联，而你们可能忽略了这些特性。

# 简单的难题

让我从一个简单的开始。

这段代码应该能够识别 int 和 boolean 对象。如果输入对象不是 boolean 或 integer，它会告诉您该对象是“其他东西！”。最后，我用四个例子测试了这个功能。您对这段代码的输出有什么猜测？看起来很简单，对吧？

```
An int object.
Something else!
A bool object.
A bool object.
```

如果你的猜测和上面的输出一样，那你就和我第一次一样错了。实际上，正确的答案是:

```
An int object.
Something else!
An int object.
An int object.
```

为什么布尔对象(true 和 False)被识别为整数对象？

事实上，在 Python 中，布尔类是从整数类继承而来的。长话短说，在 Python 的初始版本中没有布尔对象，他们用 1 和 0 代替 True 和 False。当他们决定在 Python 中加入布尔对象时，他们从 integer 类继承了布尔类(在这里阅读这个故事)。

当您运行代码时，因为代码将 True 和 False 识别为 int 类型，所以它打印“一个 int 对象”让你感到困惑和惊讶。

# 难题

好吧，这个很简单。让我向您展示另一段代码，并让您猜测输出。

第一次面对这个挑战，好像超级容易。我想，首先，两个函数几乎相同，应该给我相同的结果。0 是假的，1 是真的，2 应该是假的。简单地说，我期望类似于:

```
False
True
FalseFalse
True
False
```

但是，正如你所猜测的，我的答案是不正确的。正确答案是:

```
False
True
True

False
True
False
```

什么？！意思是`if 2`和`if 2 == True`在 Python 里不等于，说真的？第一个表达式为真，第二个为假。怎么可能呢？！

让我告诉你为什么会这样。我们有两种表达方式。先说简单的，就是`if 2 == True`。大家知道，在 Python 中，`True`等于`1`。因此`2 == True`是假的，这就是为什么与`if 2 == True`相关的代码块没有被执行。这很容易理解。

神秘的是为什么对应于`if 2`(在`func1()`中)的代码块被执行并返回 True？是不是说`2`等于`True`？如果`2`等于`True`，为什么`2 == True`不成立。很困惑，对吧？

如果你好奇想知道答案，这里就是。答案就在面向对象编程中，面向对象编程是 Python 的基础。如你所知，***Python 中的一切都是对象*** 。对象有属性和方法。有些属性和方法被称为神奇的方法或属性。

> 虽然这对于本文的其余部分不是必需的，但我鼓励您阅读我的 Python 面向对象编程三部曲。
> 
> [第一部分](https://tamimi-naser.medium.com/object-oriented-programming-for-python-beginners-part-1-8568c5d479ce) | [第二部分](https://tamimi-naser.medium.com/object-oriented-programming-for-python-beginners-part-2-dc93a8612729) | [第三部分](https://tamimi-naser.medium.com/object-oriented-programming-for-python-beginners-part-3-15ce9fd5d739)

# 魔法 _ 布尔 __

您可以为每个类和对象定义的神奇方法之一是`__bool__`。这个神奇的方法可以返回`False`或者`True`。当使用 if 和一个对象(如`if obj:`)时，`if`调用`__bool__`方法并根据其返回值，执行或不执行相应的块。让我们用 Python 做一个快速测试，看看`__bool__`方法为整数对象`2`和`0`返回了什么。

```
>>> n = 2
>>> n.__bool__()
True
>>> n = 0
>>> n.__bool__()
False
```

你也可以使用一个名为`bool()`的内置派系来检查一个对象的布尔值。

```
>>> n = 2
>>> bool(n)
True
```

你可以利用`__bool__`来改进面向对象编程。想想你的对象最重要的布尔方面是什么，基于此，通过`__bool__`方法返回一个合适的布尔值。例如，假设您正在进行一个强化学习项目，并且为您的机器人或代理准备了一个对象。在我看来，机器人/智能体最重要的方面是它是否活着。因此，我定义了`__bool__`方法，以在其返回值中反映该属性。下面是我将如何定义这类对象的一个示例:

在这个例子中，有一个名为`score`的属性，它存储了机器人对象的分数。当分数高于 0 时，你的机器人是活的，并且`bool()`返回 True。我们将机器人分数降低 10，然后降低 90(初始分数为 100)。一旦分数变为 0，对象将返回 False 以响应任何调用其布尔值的函数。用户可以在他/她的代码中利用这个属性，例如`while r: …`或`if r: …`。

如果对象没有`__bool__`方法，`if`，`bool()`和类似的函数会寻找另一个神奇的方法叫做`__len__`。该方法通常返回对象的长度。如果`__len__`返回 0， `bool()`返回 False 否则，它返回 True。在前面的例子中，我们可以简单地通过`__len__`返回分数，我们不再需要定义`__bool__`。下面是我们如何改变代码并得到相同的结果。

以你所学的所有知识，你一定能回答我的最后一个难题。以下代码的输出是什么？

```
bool("")
```

# 摘要

要理解 Python 中布尔数据类型和对象的行为，必须知道这种数据类型是从 integer 类继承的。另外，像`__bool__`和`__len__`这样的神奇方法定义了一个对象如何响应像`bool(obj)`这样的函数。

在推特上关注我的最新报道:[https://twitter.com/TamimiNas](https://twitter.com/TamimiNas)