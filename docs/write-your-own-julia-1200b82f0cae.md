# 写你自己的朱莉娅

> 原文：<https://towardsdatascience.com/write-your-own-julia-1200b82f0cae?source=collection_archive---------27----------------------->

## Julia 是一门很棒的语言，但是如果你根据自己的喜好对它进行定制，它会变得更好！

![](img/602bd367549b5e5b4a09cf142aac2c4e.png)

(src =[https://pixabay.com/images/id-918449/](https://pixabay.com/images/id-918449/)

# 介绍

J ulia 是一种编程语言，它带来了各种令人敬畏的可能性。真正让 Julia 语言令人敬畏的一点是，它主要是自己编写的。这意味着大多数 Julia 用户可以解释和重写 Julia 的几乎任何部分，而无需学习任何其他编程语言。既然如此，这意味着我们实际上可以改变朱莉娅做完全不同的事情，并按照我们的想法编程，而不必做任何出格的事情。没有多少其他编程语言可以完成这样的目标，所以 Julia 在这方面确实很酷。今天我想通过一些方法来定制我们的 Julia 安装。这将是新功能的增加，以及改变一些方法来处理不同的类型。

# Startup.jl

每次实例化 Julia 时，它都会运行一系列文件，这些文件被加载到新的 Julia 环境中。其中一个很好的例子就是 Base.jl 文件。Julia 库中所有可用的方法和类型都存储在这里，无论何时启动 Julia，它们都会自动存在以供使用。为了改变或增加 Julia 语言已经拥有的功能，您可能有很多原因想要这样做。

Julia 记住，用户可能希望在启动时对他们的环境进行一些更改，并为我们提供了使用 startup.jl 进行更改的能力。在 Linux 和其他类似 Unix 的系统上，这将是~/.julia。julia/config 路径不存在，那么您可以简单地创建它:

```
shell> cd ~/.julia
/home/emmett/.juliashell> ls
artifacts  compiled  environments  makiegallery  pluto_notebooks  registries
clones    conda     logs    packages  prefs    scratchspacesshell> mkdir configshell> cd config
/home/emmett/.julia/configshell> nano startup.jlshell>
```

# 改变朱莉娅

现在我们有了 startup.jl，我们可以使用 dispatch 修改 Julia 中的任何代码。我写了一篇文章，详细介绍了我们将要对其他一些类型做些什么，我认为这肯定值得一读。它更详细地介绍了在 Julia 中使用类型和分派，我认为代码很好地展示了 Julia 的全部内容:

</extending-julias-operators-with-amazing-results-96c042369349>  

我将在其中添加一些代码，根据我的喜好稍微改变一下 Julia。对于这个定制，我决定对加法运算符+进行修改。每当我们通过这个操作符传递两个数组时，我们都会得到一个元素相加的返回。这很奇怪，因为还有另一个操作符，元素相加操作符。+.很容易理解为什么这是他们附带的功能，我想代码应该是这样的:

```
+(x::Array, y::Array) = .+(x::Array, y::Array)
```

但这不是我想要它做的。当然，加法运算符不仅用作数字加法的符号，也用于组合。我认为看到这个操作符的另一个用途，组合数组，会很酷。为此，我们只需显式导入操作符，然后添加一行 dispatch。

```
import Base: +
+(x::Array, y::Array) = [x;y]
```

现在，这将连接我们的两个数组，而不是元素相加。我将这段代码添加到我的 startup.jl 中，现在在我的本地系统上总是这样！

# 结论

Julia 的一个非常酷的特性是，不管你知道什么，只要你知道语言本身，你就能在最底层使用它。它本身就是写出来的，真的很酷。我们可以通过简单地使用 dispatch 非常快速地改变不同基础组件的功能，这真的很酷。虽然我的例子非常简单，但是可以用其他类型和方法来做得更好。我认为这甚至对&&位操作符也有意义，我认为它可以产生一些非常棒的结果。感谢你的阅读，我希望这有助于使朱莉娅成为你自己！