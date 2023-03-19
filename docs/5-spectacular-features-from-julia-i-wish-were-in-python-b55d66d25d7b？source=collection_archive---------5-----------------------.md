# 我希望 Julia 的 5 个引人入胜的特性是用 Python 编写的

> 原文：<https://towardsdatascience.com/5-spectacular-features-from-julia-i-wish-were-in-python-b55d66d25d7b?source=collection_archive---------5----------------------->

## 意见

## 我非常喜欢 Julia 编程语言的一些特性，我希望它们是 Python 语言。

![](img/68259292b1a8ecba5ca47dbd035154bb.png)

(src =[https://pixabay.com/images/id-3461405/](https://pixabay.com/images/id-3461405/)

# 介绍

每当程序员使用多种语言时，他们开始看到某些语言相对于其他语言的优势和权衡。我最喜欢用的编程语言之一是 Julia 编程语言。编程语言中有太多的事情做得非常好，以至于每当我对 Julia 使用不同的语言时，我都会错过。我有一篇关于我为什么如此爱朱莉娅的文章，以及我个人与这篇文章中所写语言的关系:

[](/how-i-came-to-love-the-julia-language-948c32e2f9b0) [## 我是如何爱上朱莉娅语言的

### 为什么 Julia 迅速成为我最喜欢的数据科学编程语言

towardsdatascience.com](/how-i-came-to-love-the-julia-language-948c32e2f9b0) 

当然，鉴于我主要从事数据科学工作，这个应用程序的另一个流行选择是 Python 编程语言。我也喜欢这种语言，我也认为它有很多很棒的特性。这些特性中的一些肯定会激发出一些伟大的 Julia 代码，但是今天我想把重点放在相反的方面。今天我想展示一些我最喜欢的 Julia 语言的特性，我认为 Python 可以从中受益。鉴于 Python 语言，我认为其中的一些功能很有意义，然而其他功能只是我喜欢的功能，显然不是 Python 方向的一部分。本文还有一个笔记本，以防您希望看到使用的 Julia 代码(真的不多):

[](https://github.com/emmettgb/Emmetts-DS-NoteBooks/blob/master/Julia/These%20features%20would%20be%20cool%20in%20Python.ipynb) [## emmetts-DS-NoteBooks/这些特性在 master 的 Python.ipynb 中会很酷…

### 各种项目的随机笔记本。通过创建帐户，为 emmettgb/Emmetts-DS 笔记本电脑的开发做出贡献…

github.com](https://github.com/emmettgb/Emmetts-DS-NoteBooks/blob/master/Julia/These%20features%20would%20be%20cool%20in%20Python.ipynb) 

# №1:健壮的类型系统

所有程序员都喜欢 Julia 的一点是它健壮的类型系统。就我个人的喜好而言，谈到打字的力量，朱莉娅非常适合。也就是说，就类型的强度而言，Python 处于相同的领域，尽管在类型改变方面可能更含蓄一些。也就是说，我认为 Julia 的类型层次和基本数据类型远远优于 Python。

这并不是说 Python 的类型系统不健壮，但是确实有改进的余地。当涉及到数值类型和可迭代对象的继承时，尤其如此。例如，字符串应该是数组的子类型，因为最终字符串本质上只是一个字符数组。当然，这只是一个具体的例子，但是我认为类型系统可以使用许多小的调整。在 Julia 中，我们有一个很棒的东西叫做抽象类型，它有助于创建类型层次，而抽象类型在 Python 中很少使用——这方面的部分问题也意味着每个子类都有一个支持的子初始化。

# 2 号:方法错误

我不喜欢 Python 的一点是缺少方法错误。为了传达为什么我认为 Python 需要方法错误，以及为什么在当前的迭代中没有方法错误是令人困惑的，我将给出一个 Julian 的例子。假设我们用 Python 和 Julia 编写了相同的函数:

```
function double(x::Int64) x * 2enddef double(x : int):
    return(x * 2)
```

在 Julian 的例子中，我们不能通过这个方法传递除了整数以外的任何值。这意味着，例如，一个字符串将返回一个方法错误。

```
double("Hello")MethodError: no method matching double(::String)
Closest candidates are:
  double(::Int64) at In[1]:1

Stacktrace:
 [1] top-level scope
   @ In[3]:1
 [2] eval
   @ ./boot.jl:360 [inlined]
 [3] include_string(mapexpr::typeof(REPL.softscope), mod::Module, code::String, filename::String)
   @ Base ./loading.jl:1116
```

现在让我们在我的 Python REPL 中尝试同样的事情:

```
>>> double("Hello")'HelloHello'
```

这是双函数的预期行为吗？当然，没有人会认为这是一个字符串——但是让我们考虑一下，我们使用了一个不能和*操作符一起使用的类型。

```
d = {"H" : [5, 10, 15]}>>> double(d)Traceback (most recent call last):File "<stdin>", line 1, in <module>File "<stdin>", line 2, in doubleTypeError: unsupported operand type(s) for *: 'dict' and 'int'>>>
```

让我们假设我没有编写这个函数——我导入了一些软件，现在我看到了这个错误。尽管我指定了这个函数只能将这种类型作为参数使用，但是现在我们遇到了一个问题，有些人需要参考文档，或者 google this TypeError，因为他们甚至不知道这是在哪里发生的。即使在编写了一行函数之后，我们也知道没有办法找出这个错误发生在函数的什么地方。将此与 Julia 方法的错误输出进行比较:

```
MethodError: no method matching double(::String)
Closest candidates are:
  double(::Int64) at In[1]:1

Stacktrace:
 [1] top-level scope
   @ In[3]:1
 [2] eval
   @ ./boot.jl:360 [inlined]
 [3] include_string(mapexpr::typeof(REPL.softscope), mod::Module, code::String, filename::String)
   @ Base ./loading.jl:1116
```

我们得到的第一件事是 double 函数不接受字符串作为参数，然后这后面是一个修正—double 应该与我们指定的整数一起使用。我只是觉得指定参数的类型应该比文档元数据更有用。我认为诊断为方法提供错误类型的问题是很难理解的。即使在 Julia 中使用了错误的方法，输出也可以为您提供足够的提示，让您在许多情况下正确使用该方法。比如，插页！()方法:

```
insert!(5)MethodError: no method matching insert!(::Int64)
Closest candidates are:insert!(::Vector{T}, ::Integer, ::Any) where T at array.jl:1317
insert!(::BitVector, ::Integer, ::Any) at bitarray.jl:887
```

最接近的候选列表的第一行告诉我们所有我们需要知道的关于这个方法的信息。在函数式风格中，变异类型总是先出现，我们的抽象向量，然后是整数，然后是 any。既然我们可以假设向量中的值的时间基本上可以是任何值，那么很可能是::Any。考虑到数组索引总是整数，这可能是整数。

# №3:可迭代运算符

在 Python 中，对可迭代对象进行基本的数学运算要困难得多。在大多数情况下，人们会希望为此导入 NumPy。然而，像 R 语言一样，Julia 支持元素操作符。这些操作符做两件事:

*   提供一种简单的方法来执行一维数组和多维数组的运算。
*   允许人们从运算符中快速区分出正在对两个可迭代对象执行运算。

这两件事都很重要，而且每次想做这个算术的时候就跳进一个循环肯定不是最酷的事情。虽然我完全理解元素级乘法有一些可用的特性，但我认为操作符是解决这个问题的更充分的方法。

# №4:(当然)多重调度

我们都看到了这一天的到来…

> 我喜欢多重派遣。

需要澄清的是，我从来没有说过地球上的每一种编程语言都需要多重调度。然而，对于 Julia 编程来说，我更喜欢在给定的编程语言中使用多重分派。这只是一种不碍事的流畅的编程方式，这也是我真正喜欢它的地方。它用参数多态在类型和方法之间建立了更强的关系。

最终，使编程语言独一无二的是它各自的范例和方法与语言交互的方式。Pythonic 式的做事方式当然有好处。类很牛逼，它们的子函数也很牛逼。我认为面向对象编程是在类型和它们各自的功能之间建立关系的一个很好的方法。也就是说，Python 根本不是纯粹的面向对象编程语言。如果是这样的话，我认为这种语言不太可能在数据科学中如此流行。

也就是说，Python 更具声明性的“方法到类型”方面(方法是在类的范围之外定义的)与类型和它们的函数产生了更大的分离。这是因为正如我之前详细讨论的，没有方法错误。也就是说，将一个类型与一个函数联系起来的唯一东西是该函数中的算法，也许还有参数类型的转换。然而，如前所述，参数的强制转换除了作为代码的文档之外，实际上没有任何作用。多重分派实际上是一个过度强大的编程泛型，如果你想了解更多，我有一篇关于它的文章，你可以在这里阅读:

[](/how-julia-perfected-multiple-dispatch-16675db772c2) [## 朱莉娅如何完善多重派遣

### 让我们看看 Julia 语言是如何让多重调度变得完美的

towardsdatascience.com](/how-julia-perfected-multiple-dispatch-16675db772c2) 

# №5:扩展底座

我真正喜欢 Julia 的最后一个特性是从语言基础上扩展方法的能力。在 Julia 中，语言基础中的许多函数在整个生态系统中都有使用。当涉及到编写软件文档时，这确实会产生很多问题，因为有时人们必须编写一个基本函数调用的每个例子——通常有很多，我也认为这使得很多事情更容易假设。

例如，我想过滤一个数据框。朱利安基地附带一个过滤器！()函数，用于数组和其他基本数据结构，以删除某些值。为了在数据帧中实现这一点，我们进行了本质上完全相同的调用——这都是因为 Julia 具有扩展函数的能力。我认为这很棒，因为它在 Julia 包之间创造了很多一致性。

也就是说，这类事情肯定至少部分依赖于多重调度。然而，让我们考虑一下 Python 操作符的使用。我们可以把两个整数加在一起，当然没问题:

```
5 + 5
```

但是我们也可以添加其他数字，比如布尔和浮点数:

```
5.5 + False
```

不仅如此，我们还可以将这些运算符用于字符串。这意味着这种方法适用于所有这些类型——就像多重分派一样。假设这些操作与方法的行为方式相同可能有点像 Julian，但不管怎样，我们在这里看到了一个实现，它揭示了这种多态性在某种程度上已经存在于 Python 中。

# 结论

我真的很喜欢这两种语言，尽管我更喜欢 Julia 中的许多特性。也就是说，我认为两种语言都可以从对方那里学到一些东西，在某些方面都有所提高。这并不是说这些会发生或不会发生，但我认为大多数人不太可能会看到 Python 语言。这里我认为唯一相对紧迫的问题是方法错误，因为我真的认为这是 Python 中所缺少的。

也就是说，我认为这显示了不同的语言有不同的方面，一些人可能比其他人更喜欢。Julia 处理类型和多重分派的方式当然有很多缺点，Python 处理类型的方式也是如此。非常感谢您的阅读，它对我来说真的意味着整个世界，我希望这是一个有趣的视角，让我们看看 Python 可以从 Julia 身上学到的一些潜在的变化和教训。