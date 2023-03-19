# 与 Julia 一起阅读 CSV 文件—第 2 部分

> 原文：<https://towardsdatascience.com/reading-csv-files-with-julia-part-2-51d74434358f?source=collection_archive---------45----------------------->

## 货币、布尔值等的自定义格式

![](img/4bbb69a79ca11622ad73a2437007d627.png)

让·奥诺雷·弗拉戈纳，公共领域，通过维基共享

之前，我已经展示了如何读取基本的分隔文件——即文件中的值由常用字符(如逗号、分号或制表符)分隔。现在是时候升级我们的游戏，使用`CSV.jl`处理一些更奇特的边缘情况了。

> 我们将重点了解如何正确解析数据类型，以便我们的数据帧从一开始就尽可能干净。

这是与 Julia 一起阅读 CSV 文章的第 2 部分，所以如果您是新来的，请查看第 1 部分:

</reading-csv-files-with-julia-e2623fb62938>  

和以前一样，我们从导入包和模拟一些虚拟数据开始。我将重用上一篇文章中的`write_string`函数。

# 读取布尔值

我们的第一个挑战将是处理布尔值。**布尔值为真或假，通常用于从数据框中选择行的子集。**为了增加难度，我们将在 csv 文件中使用`Y/N`作为布尔值:

这给出了以下内容:

```
julia> animals_table = CSV.read("animals_like.csv", DataFrame)
5×4 DataFrame
│ Row │ Animal  │ Colour │ Cover        │ Liked  │
│     │ String  │ String │ String       │ String │
├─────┼─────────┼────────┼──────────────┼────────┤
│ 1   │ bunny   │ white  │ fur          │ Y      │
│ 2   │ dragon  │ green  │ scales       │ Y      │
│ 3   │ cow     │ brown  │ fur          │ N      │
│ 4   │ pigeon  │ grey   │ feathers     │ N      │
│ 5   │ pegasus │ white  │ feathers,fur │ Y      │
```

正如我们所看到的，我们的`Liked`列的数据类型为`String`。我们也可以通过致电`eltypes(animals_table)`来确认这一点。

因为我们希望将`Y/N`的值转换成布尔值以便于索引，所以让`CSV.jl`通过使用`truestrings`和`falsestrings`参数将它们解析为布尔值——注意**这些必须是数组**:

现在，我们的最后一篇专栏文章将把`Bool`读为数据类型，我们可以直接做一些有趣的索引:

```
julia> animals_table2[animals_table2[:Liked], :]
3×4 DataFrame
│ Row │ Animal  │ Colour │ Cover        │ Liked │
│     │ String  │ String │ String       │ Bool  │
├─────┼─────────┼────────┼──────────────┼───────┤
│ 1   │ bunny   │ white  │ fur          │ 1     │
│ 2   │ dragon  │ green  │ scales       │ 1     │
│ 3   │ pegasus │ white  │ feathers,fur │ 1     │
```

# 货币和小数

我已经受够了我们心爱的动物，所以让我们看看我们的储蓄账户:

现在，如果你注意的话，你会注意到`Amount`列使用的是`,`而不是小数点。这在世界的某些地方很常见，所以如果你正在处理来自东欧的数据，你很可能会看到类似上面的东西。多么快乐！🙌
如果我们试着正常读取，得到的`Amount`列将会变成`String`:

一个简单的解决方法是指定用于表示小数的字符:

现在我们可以随心所欲地加减了。例如，我们可以看到我们每个月存了多少钱:

另外，请注意，我们的解析器成功地判断出第一列是一个日期。如果您的日期列有其他格式，您可以通过设置(surprise，surprise…) `dateformat`参数:`CSV.read("my_file.csv", DataFrame, dateformat="Y-m-d")`来手动指定日期格式**。**

# 更多的钱，更多的问题

![](img/65c58a20c3f77c1b88d1b6ce9b18c062.png)

照片由[达里奥·马丁内斯-巴特列](https://unsplash.com/@dariomartinezb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/money-problems?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

纯粹出于运气，我们刚刚中了彩票💸。让我们将新的总数添加到电子表格中:

现在我们的十进制字符串不再能帮助我们:

```
4×3 DataFrame
│ Row │ DateTime   │ Amount       │ Currency │
│     │ Dates.Date │ String       │ String   │
├─────┼────────────┼──────────────┼──────────┤
│ 1   │ 2001-01-01 │ 10,56        │ €        │
│ 2   │ 2001-02-01 │ 12,40        │ €        │
│ 3   │ 2001-03-01 │ 6,50         │ €        │
│ 4   │ 2001-04-01 │ 1.000.006,57 │ €        │
```

这是因为用作千位分隔符的`.`混淆了`CSV.jl`，使其认为这是一个字符串列。

在早期版本的`CSV.jl`中，您可以在读取时通过指定一个`transforms`参数来直接修复这个问题，但是这个参数似乎已经被移除了，所以我们必须在数据被读入后改变它。

为了解决这个问题，我们需要一个函数:

1.  接受字符串作为输入。
2.  将字符串中的`.`替换为空。
3.  将`,`替换为`.`。
4.  然后将字符串解析为浮点数。

让我们为此创建一个函数，并将其映射(按元素方式应用)到该特定列:

这将为我们提供预期的正确数据类型:

```
4×3 DataFrame
│ Row │ DateTime   │ Amount    │ Currency │
│     │ Dates.Date │ Float64   │ String   │
├─────┼────────────┼───────────┼──────────┤
│ 1   │ 2001-01-01 │ 10.56     │ €        │
│ 2   │ 2001-02-01 │ 12.4      │ €        │
│ 3   │ 2001-03-01 │ 6.5       │ €        │
│ 4   │ 2001-04-01 │ 1.00001e6 │ €        │
```

# 💰终于，我们发财了…

好吧，也许不是，但至少我们可以很容易地阅读各种疯狂，这都要感谢朱莉娅和`CSV.jl`。现在，您知道如何将字符串解释为布尔值，读取以`,`为小数的数字，甚至可以按列应用任意函数来整理您喜欢的任何内容。

> 最后一个提示:如果你不确定数据的结构，你可以设置`CSV.read()`的`limit`参数，只读取文件的前`n`行。👀

如果你正在寻找更多的朱莉娅文章，看看这些:

</vectorize-everything-with-julia-ad04a1696944>  </control-flow-basics-with-julia-c4c10abf4dc2> 