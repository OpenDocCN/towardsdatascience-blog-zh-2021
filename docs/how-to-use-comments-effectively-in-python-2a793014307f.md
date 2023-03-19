# 如何在 Python 中有效地使用注释

> 原文：<https://towardsdatascience.com/how-to-use-comments-effectively-in-python-2a793014307f?source=collection_archive---------18----------------------->

## 如果使用得当，注释是一个很有价值的工具——这就是你应该如何最有效地使用它们。

![](img/e9e74673078fde737dfac9a4eac3f617.png)

(src =[https://unsplash.com/photos/5KKglNl852A](https://unsplash.com/photos/5KKglNl852A)

# 介绍

软件工程师或数据科学家可能会使用许多工具来为他们的代码提供更多的表达。有很多原因可以让你这么做。例如，在需要教授的循序渐进的过程中，注释可能是宝贵的资产。注释还可以提供有价值的信息，这些信息对于处理某些代码是必不可少的。

我回想起一个例子，我不得不挖掘一些 Julia 代码，因为没有它的文档。我不知道某个东西的输入应该是如何构造的，但是我发现一些奇怪的注释存储在一边，解释了我需要的数据结构是如何呈现的，然后我的代码开始了比赛。虽然这绝对不是注释的最佳应用，但它确实坚定地证明了注释对于阐明某些观点是多么重要。我们不能指望文档包含开发人员可能想从最初的程序员那里了解的所有内部信息，这正是注释派上用场的地方。

然而，我想谈一点关于使用注释的原因是，虽然它们很简单，但实际上使用它们是一门艺术。也就是说，我发现在较新的程序员代码中发现一些相当可怕的注释是很常见的。很多时候，有些评论要么是矛盾的，要么是直接解释的…例如，我不需要一个评论告诉我你在做这个算术:

```
# divide x by 5
new = x / 5
```

如果对此有任何评论，它应该高于所有的算术，而仅仅是一个公式，例如:

```
# z = 3 * 2 + x / 5
```

然而，在这种特殊情况下，这仍然没有太大意义——因为这个表达式中只有一个独立变量。有那么多正确的地方可以评论，但结果也有那么多错误的地方和事情可以评论。所以事不宜迟，我想提供一些有效注释代码的个人技巧，以及 Python 编程语言的 PEP8 代码注释惯例。

# PEP8 要说的话

首先，PEP8 指出代码需要与软件保持同步。随着给定注释集周围的代码发生变化，这些注释也应该重写。不适用于给定代码的注释比没有注释更糟糕。PEP8 还指出，注释应该用完整的句子。这当然是有适当的语法和标点的，比如一个句子以大写开头，以句号结尾。

```
# This would be a proper comment.
#this one isn't
```

当然，这有一个例外，如果我们有一个已存在的别名，我们应该在代码中遵守那个别名的大小写，比如这个例子:

```
# g is Jimmy's height
g = jimmys_height
```

不

```
# G is Jimmy's height
g = jimmys_height
```

在多句注释中，除了在最后一句之后，你应该在句末句号之后使用两个空格。当然，这些评论也应该具有可读性，并且易于理解。PEP8 的另一个奇怪的惯例是注释应该是英文的……“除非你 120%确定只有说你的语言的人会查看代码……”([直接引用](https://www.python.org/dev/peps/pep-0008/#comments))。)我发现这很有趣，因为我在一些非常流行的 Python 软件中看到过许多西班牙语或其他语言的评论。

还有一点，PEP8 说行内注释应该很少使用。明确地说，行内注释就是代码后面的注释。

# 我要说的是

首先，我想说的是，如果有一种负面的注释类型，我宁愿看到空白，那就是

> 多余的评论。

评论可以被过度使用，这绝对不是你想做的事情。因为你在解释你正在做的每件事而重复你的代码行不是一个好的编码实践。不要做这样的工程师:

```
# Multiply z and x to get b.
b = z * x
# Add five to b
b += 5
# Multiply slope by x and add b:
y = m * x + b
```

我们可以看到你在做什么！这不需要注释，只要看一下这个例子，您就能看出注释使代码更难阅读——而不是更容易？这一部分的一个更好的例子是用这个算法实际做了什么来标记它下面的整个代码，而不是用它所需要的操作来标记。

```
# Calculate the line.
b = z * x
b += 5
y = m * x + b
```

另外，在你的磅后面加一个空格也是程序员之间的约定。这绝对是一个可以遵循的好规则:

```
# Comment this.
#Not this.
```

我认为这类似于将操作符挤在数据旁边，这只是让东西难以阅读。另一件事是当描述一个短语时，要理解评论实际上属于哪里。想想之前的台词。我们不希望在任何地方添加内嵌评论，尤其是在这之后！

```
# This is correct.
b = z * x
b += 5    # This is not correct.
y = m * x + b
# Please do not do this.
```

您可以非常有效地使用注释来描述功能区域的目标，而不是描述单独的行。虽然有时这些确实是一个好主意，但请考虑我的 OddFrames.jl 包中的以下代码:

```
function OddFrame(file_path::String)# Create labels/columns from file.extensions = Dict("csv" => read_csv)extension = split(file_path, '.')[2]labels, columns = extensions[extension](file_path)length_check(columns)name_check(labels)types, columns = read_types(columns)# Get our coldata.coldata = generate_coldata(columns, types)# dead function."""dox"""head(x::Int64) = _head(labels, columns, coldata, x)head() = _head(labels, columns, coldata, 5)# drop function.drop(x) = _drop(x, columns)drop(x::Symbol) = _drop(x, labels, columns, coldata)drop(x::String) = _drop(Symbol(x), labels, columns, coldata)
# dropna function.dropna() = _dropna(columns)dtype(x::Symbol) = typeof(coldata[findall(x->x == x,labels)[1]][1])
# dtype function.dtype(x::Symbol, y::Type) = _dtype(columns[findall(x->x == x,labels)[1]], y)# Create the type.self = new(labels, columns, coldata, head, drop, dropna, dtype);select!(self)return(self);end
```

这些评论是针对 Julia 包的，但实际上这两个包在评论时应该没有太大区别。在这个例子中，我使用注释来解释我正在创建的数据——这些数据都包含在我们的类型中。我正在描述我想要的输出，您可以让代码解释我是如何到达那里的。我认为这是使用注释的一个很好的方式，因为它使得找出函数的特定区域变得不那么困难。

# 结论

老实说，评论是一个完整的主题，我认为从教育的角度来看，应该更加关注它。这是因为它们非常强大，但只有在正确使用的情况下。当然，这在很大程度上取决于程序员个人认为需要评论什么，这是非常主观的。关于什么该评论，什么不该评论，确实没有指南。

然而，使用这些建议来避免重复，从一个更广泛的角度来看问题，肯定会使你的评论比过去更有价值。我认为我们都可以不断改进这一点，我知道我肯定不会总是写评论，当我写评论时，它们可能非常模糊，也不是非常全面。在很多方面，我觉得好的代码不应该需要注释。如果你的代码很棒，那么它应该像读英语或其他什么一样可读。然而，话虽如此，我仍然发现如果使用得当，注释会有很大的好处，而且我肯定能看到它们在某些方面是如何有效的。我希望这个关于我们都可能犯过无数次的错误的小概述，特别是当涉及到 PEP8 方面的事情时，有助于使我们未来的代码看起来更好，并且通过准确、无冗余的注释更容易理解。感谢您阅读我的文章。