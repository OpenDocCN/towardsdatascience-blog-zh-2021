# 与 Julia 一起阅读 CSV 文件

> 原文：<https://towardsdatascience.com/reading-csv-files-with-julia-e2623fb62938?source=collection_archive---------20----------------------->

## 了解如何使用 CSV.jl 读取各种逗号分隔的文件

![](img/de392d30d9ef357d5d41fc0cb8f88fcd.png)

见作者页面，CC 由 4.0<[https://creativecommons.org/licenses/by/4.0](https://creativecommons.org/licenses/by/4.0)通过维基共享

你曾经收到过用分号(`;`)作为分隔符的`.csv`文件吗？还是一个没有头文件的文件？或者也许你在欧洲有一些同事用`,`而不是`.`来表示小数？哦，使用 CSV 文件的乐趣…

继续阅读，学习如何使用`CSV.jl`在 Julia 中阅读各种分隔符分隔的文件格式

> 要获得所有媒体文章的完整访问权限，包括我的文章，请考虑在此订阅。

## 生成数据

我们将自己生成所有示例，因此您可以轻松下载代码，并在自己的环境中试验结果。我们开始吧！首先，我们需要加载将要使用的包。即`CSV.jl`和`DataFrames.jl`:

下一步是生成一些我们可以实际读取的数据。在现实世界中，您可能不需要这样做，因为您已经有了想要读取的数据，但是通过提供一个易于复制的示例，您可以自己处理代码！

我将使用`"""`将文件的内容设置为多行字符串。然后，我们可以用传统的方式写入文件，打开一个连接，将字符串的所有内容转储到该连接中。

> 您可以通过从 REPL 使用`;`跳转到 shell 模式并键入`head animals.csv`来检查文件的内容

## 使用默认参数读取文件

结果是一个“漂亮的”`.csv`文件，因为它是由逗号分隔的**，有标题，并且没有奇怪的引用**或其他不寻常的事情发生...默认的`CSV.read()`函数应该没有问题。

```
4×3 DataFrame
│ Row │ Animal │ Colour │ Cover    │
│     │ String │ String │ String   │
├─────┼────────┼────────┼──────────┤
│ 1   │ bunny  │ white  │ fur      │
│ 2   │ dragon │ green  │ scales   │
│ 3   │ cow    │ brown  │ fur      │
│ 4   │ pigeon │ grey   │ feathers │
```

因为我们将多次执行这一步骤(将字符串写入文件),所以我准备了一个方便的函数供以后使用:

## 不同的分隔符

解决了这个问题，让我们尝试一些更复杂、不那么标准的东西…自定义分隔符！您仍然可以使用`CSV.read()`，您只需要指定`delim`关键字参数就可以了！

> 更好的是，`CSV.jl`足够聪明，可以为我们猜出分隔符。尝试在没有`delim`参数的情况下读取管道分隔文件！

**对于** `**tab**` **分隔的文件，只需使用** `**delim=`\t`**` **即可。**

## 无头文件

另一个常见的场景是我们要读入的文件没有头文件。让我们准备这样一个例子:

在这种情况下，标题看起来与正文的其余部分没有太大的不同，所以`CSV.jl`在这里不能做任何巧妙的事情，会认为第一行是标题。做`CSV.read("no_headers.csv", DataFrame)`会给你:

```
3×3 DataFrame
│ Row │ bunny  │ white  │ fur      │
│     │ String │ String │ String   │
├─────┼────────┼────────┼──────────┤
│ 1   │ dragon │ green  │ scales   │
│ 2   │ cow    │ brown  │ fur      │
│ 3   │ pigeon │ grey   │ feathers │
```

我们不希望我们的列被称为`bunny`、`white`和`fur`...那是不明智的！你有**两个选择来解决这个问题。第一，您可以只设置`header = false`，在这种情况下，您的结果将以通用列名结束。或者，您也可以在同一参数下手动设置列名:**

您将得到以下结果:

```
julia> CSV.read("no_headers.csv", DataFrame, header=false)
4×3 DataFrame
│ Row │ Column1 │ Column2 │ Column3  │
│     │ String  │ String  │ String   │
├─────┼─────────┼─────────┼──────────┤
│ 1   │ bunny   │ white   │ fur      │
│ 2   │ dragon  │ green   │ scales   │
│ 3   │ cow     │ brown   │ fur      │
│ 4   │ pigeon  │ grey    │ feathers │julia> CSV.read("no_headers.csv", DataFrame, header=["Animal", "Colour", "Cover"])
4×3 DataFrame
│ Row │ Animal │ Colour │ Cover    │
│     │ String │ String │ String   │
├─────┼────────┼────────┼──────────┤
│ 1   │ bunny  │ white  │ fur      │
│ 2   │ dragon │ green  │ scales   │
│ 3   │ cow    │ brown  │ fur      │
│ 4   │ pigeon │ grey   │ feathers │
```

请对同事好一点，用选项 2！😉

## 引用字符

分隔文件的另一个典型的奇怪之处是**字段本身可以包含分隔符**。在这种情况下，通常**引用字段**，这样解析器可以很容易地忽略分隔符。

![](img/a73b4fc61793db2baaf2f873fc4d6546.png)

该死的不合格的帕伽索斯！|信用: [pxfuel](https://www.pxfuel.com/en/free-photo-xlxub)

让我们在列表中添加一个飞马来演示这一点:

事实上，我们对`"`没有任何问题，因为这些是默认的报价，所以`CSV.read("animals_quoted.csv", DataFrame)`会工作得很好:

```
julia> CSV.read("animals_quoted.csv", DataFrame)
5×3 DataFrame
│ Row │ Animal  │ Colour │ Cover        │
│     │ String  │ String │ String       │
├─────┼─────────┼────────┼──────────────┤
│ 1   │ bunny   │ white  │ fur          │
│ 2   │ dragon  │ green  │ scales       │
│ 3   │ cow     │ brown  │ fur          │
│ 4   │ pigeon  │ grey   │ feathers     │
│ 5   │ pegasus │ white  │ feathers,fur │
```

好吧，那么引用默认工作，但如果我们有一些疯狂的工作呢🤪为我们准备文件，他们决定使用`&`作为引用字符？？疯狂降临在我们身上...

运行这个会给我们一个警告！

```
julia> CSV.read("animals_&.csv", DataFrame)
┌ Warning: thread = 1 warning: parsed expected 3 columns, but didn't reach end of line around data row: 6\. Ignoring any extra columns on this row
└ @ CSV ~/.julia/packages/CSV/CJfFO/src/file.jl:606
5×3 DataFrame
│ Row │ Animal  │ Colour │ Cover     │
│     │ String  │ String │ String    │
├─────┼─────────┼────────┼───────────┤
│ 1   │ bunny   │ white  │ fur       │
│ 2   │ dragon  │ green  │ scales    │
│ 3   │ cow     │ brown  │ fur       │
│ 4   │ pigeon  │ grey   │ feathers  │
│ 5   │ pegasus │ white  │ &feathers │
```

看看最后一行变得多么奇怪，第三个逗号之后的所有内容都被忽略了。你应该总是注意你得到的警告！

> 你应该总是注意你得到的警告！它们可能包含一些关于您正在处理的数据的重要信息。

解决这个问题的正确方法是用`quotechar='&'`指定一个**自定义引用字符**:

那么在动物王国一切都很好:

```
julia> CSV.read("animals_&.csv", DataFrame, quotechar='&')
5×3 DataFrame
│ Row │ Animal  │ Colour │ Cover        │
│     │ String  │ String │ String       │
├─────┼─────────┼────────┼──────────────┤
│ 1   │ bunny   │ white  │ fur          │
│ 2   │ dragon  │ green  │ scales       │
│ 3   │ cow     │ brown  │ fur          │
│ 4   │ pigeon  │ grey   │ feathers     │
│ 5   │ pegasus │ white  │ feathers,fur │
```

## 笔记

注意，对于文件的编写，我们也可以使用`CSV.jl`,但是我觉得那样会违背本教程的目的。为了完整起见，下面是编写文件的片段:

![](img/b8f393b44446b724c65cfe69a8c284ed.png)

大卫·坦尼尔斯《更年轻的公共领域》，维基共享

## 结论

在这篇文章中，你学会了如何:

*   将简单的 csv 文件读入数据帧
*   指定分隔符
*   读取无头文件
*   指定引用字符

你没有学到的:

*   如何处理不同的列类型，例如解析布尔值、日期类型。
*   处理货币格式，如$1，320.23。

如果您想了解这些，请继续阅读第 2 部分:

</reading-csv-files-with-julia-part-2-51d74434358f>  

关于 Julia 的其他帖子，您可能会感兴趣:

</vectorize-everything-with-julia-ad04a1696944>  </control-flow-basics-with-julia-c4c10abf4dc2> 