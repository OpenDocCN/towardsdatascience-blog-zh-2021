# 在 Julia 中索引、排序和聚集你的数据帧

> 原文：<https://towardsdatascience.com/index-sort-and-aggregate-your-dataframes-in-julia-38646daf6214?source=collection_archive---------18----------------------->

## 使用 DataFrames.jl 进行常见数据分析的教程

![](img/6fc41e8dc208800c11c8a31ae82d304e.png)

由[杰斯温·托马斯](https://unsplash.com/@jeswinthomas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/banana-split?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

深入到 DataFrames.jl，我们将探索如何在 DataFrames 上做**布尔索引**，学习**如何按列值对数据进行排序**和**聚合表**以满足我们的需求。在最后一节，我们还将介绍一种超级强大的分析方法，称为:**分割-应用-组合**。

*如果你需要复习 DataFrames.jl，先看看这些文章:*

[](/julia-dataframes-jl-basics-95dba5146ef4) [## Julia DataFrames.jl 基础

### 用 DataFrames.jl 戳戳你的数据

towardsdatascience.com](/julia-dataframes-jl-basics-95dba5146ef4) [](/reading-csv-files-with-julia-e2623fb62938) [## 与 Julia 一起阅读 CSV 文件

### 了解如何使用 CSV.jl 读取各种逗号分隔的文件

towardsdatascience.com](/reading-csv-files-with-julia-e2623fb62938) [](/reading-csv-files-with-julia-part-2-51d74434358f) [## 与 Julia 一起阅读 CSV 文件—第 2 部分

### 货币、布尔值等的自定义格式。

towardsdatascience.com](/reading-csv-files-with-julia-part-2-51d74434358f) 

> 要获得所有媒体文章的完整信息，包括我的文章，请点击这里订阅。

# 获取一些数据

首先，我们需要从`RDatasets`包中挑选一个数据集。这样就省去了我们下载读入一个文件的麻烦。如果你想知道如何阅读 CSV，请查看我之前在 CSV.jl 和数据导入上的帖子——上面的链接。让我们**导入包并设置我们的数据集**。

![](img/bf15c4aa85025a2c05fb6f118f6e448f.png)

维多利亚博物馆在 [Unsplash](https://unsplash.com/s/photos/factory-workers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

这个数据集追踪了超过 500 个人的时薪、工作经验、教育和其他可能与薪水相关的因素。你可以在这里找到更多关于数据集[的信息。在我们的练习中，我们将只使用以下各列:](https://cran.r-project.org/web/packages/plm/plm.pdf)

1.  `NR`:唯一的工人标识符。
2.  `Year`:观测年份。
3.  `Wage`:计时工资日志。
4.  `Ethn`:工人的种族等级:黑人、hisp、其他
5.  多年的经验。

```
julia> first(males, 5)
5×5 DataFrame
 Row │ NR     Year   Wage     Ethn   Exper
     │ Int32  Int32  Float64  Cat…   Int32
─────┼─────────────────────────────────────
   1 │    13   1980  1.19754  other      1
   2 │    13   1981  1.85306  other      2
   3 │    13   1982  1.34446  other      3
   4 │    13   1983  1.43321  other      4
   5 │    13   1984  1.56813  other      5
```

# 索引

对数据帧的典型操作是**根据值** s 上的一些标准对数据进行子集化。我们可以通过首先构建一个**布尔索引**(真/假值的向量)来实现这一点，该索引对于期望值为真，否则为假。然后，我们可以将它作为数据帧的第一个参数放入括号中，以选择所需的行。

> 为了节省空间，我将只打印前 5 行。我用`first(dataframe, 5)`来做这个。

```
5×5 DataFrame
 Row │ NR     Year   Wage     Ethn   Exper
     │ Int32  Int32  Float64  Cat…   Int32
─────┼─────────────────────────────────────
   1 │    13   1980  1.19754  other      1
   2 │    17   1980  1.67596  other      4
   3 │    18   1980  1.51596  other      4
   4 │    45   1980  1.89411  other      2
   5 │   110   1980  1.94877  other      5
```

![](img/de37d47f70f02e92259fe26b7a6f3193.png)

[粘土堤](https://unsplash.com/@claybanks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/selecting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

我们使用冒号`:`来表示我们想要所有可用的列。您应该这样阅读上面的内容:给我所有的男性行，其中`my_index`为真，并返回所有可用的列。通常，你会**在一个步骤**中完成，因为真的没有必要分配一个单独的布尔索引向量:

这给出了与上面相同的结果。

如果有多个标准呢？假设我们需要 1980 年的观察数据，但只针对西班牙裔工人:

```
5×5 DataFrame
 Row │ NR     Year   Wage      Ethn  Exper
     │ Int32  Int32  Float64   Cat…  Int32
─────┼─────────────────────────────────────
   1 │  1142   1980  1.41311   hisp      2
   2 │  1641   1980  2.11169   hisp      4
   3 │  1644   1980  0.560979  hisp      1
   4 │  1721   1980  1.78447   hisp      5
   5 │  1763   1980  0.6435    hisp      1
```

如你所见，解决方案是使用 AND `&`逻辑运算符**对两个布尔向量**进行逐元素比较(因此我们用`.&`进行广播)。所以我们产生第三个向量，当且仅当年份是 T6，Ethn 是 T7 时，这个向量为真。

熟能生巧，所以让我们选择在 1980 年有 3 年以上工作经验的人:

```
males[(males.Year .== 1980) .& (males.Exper .> 3), :]
```

如果你需要其他的逻辑运算符，请查阅 Julia 手册中的本页。在那里你可以学习如何用`!`求**值**的反，并与**或运算符** : `|`交朋友。

为了完整起见，下面是如何用`filter`函数做同样的事情:

```
filter(df -> (df.Year == 1980) & (df.Exper > 3), males)
```

这在我之前的文章中有更详细的描述。

# 排序数据帧

![](img/9967ce0c4209d2f8d8d2e3d70e1d5a08.png)

照片由[德鲁·比默](https://unsplash.com/@drew_beamer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/collections/5844710/sorting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

让我们学习如何对数据集进行排序。正如你所想象的，这又是一个很常见的操作，幸运的是，它确实非常简单。首先，让我们按照`Year`列对数据进行排序。

```
5×5 DataFrame
 Row │ NR     Year   Wage     Ethn   Exper
     │ Int32  Int32  Float64  Cat…   Int32
─────┼─────────────────────────────────────
   1 │    13   1980  1.19754  other      1
   2 │    17   1980  1.67596  other      4
   3 │    18   1980  1.51596  other      4
   4 │    45   1980  1.89411  other      2
   5 │   110   1980  1.94877  other      5
```

简单对吗？只需将一个数据帧传递给**排序函数，并告诉它根据**对哪一列(或哪些列)进行排序。你希望它减少吗？对此有一个关键词:

您可能想知道为什么我们必须传入一个列名数组来进行排序。这样我们可以同时指定多个列进行排序。这里有一个例子:

```
5×5 DataFrame
 Row │ NR     Year   Wage     Ethn   Exper
     │ Int32  Int32  Float64  Cat…   Int32
─────┼─────────────────────────────────────
   1 │    13   1980  1.19754  other      1
   2 │    17   1980  1.67596  other      4
   3 │    18   1980  1.51596  other      4
   4 │    45   1980  1.89411  other      2
   5 │   110   1980  1.94877  other      5
```

请注意，我们首先获得了 1980 年的所有观测数据，而`NR`数字正在增加。

> `sort()`函数需要一个数组作为第二个参数，这样就可以同时对多列进行排序。

数据科学中一个重要的考虑因素是您的内存占用。以上所有操作**都返回了 DataFrame** 的副本，只留下我们的原始数据集。如果你不想改变你的原始数据集，这是很有用的，但是如果你想减少你的内存占用，你可以使用一个特殊版本的`sort`，结尾有一个刘海:`sort!`。这是 Julia 中的一个约定，函数通常是不可变的，如果一个函数改变了它的参数，那么它的名字应该以一个“砰”结束。下面是它在实践中的工作原理:

这将修改原始数据帧。

```
julia> first(males, 5)
5×5 DataFrame
 Row │ NR     Year   Wage     Ethn   Exper
     │ Int32  Int32  Float64  Cat…   Int32
─────┼─────────────────────────────────────
   1 │    13   1980  1.19754  other      1
   2 │    17   1980  1.67596  other      4
   3 │    18   1980  1.51596  other      4
   4 │    45   1980  1.89411  other      2
   5 │   110   1980  1.94877  other      5
```

> 以`!`结尾的函数表示该函数正在改变其输入参数。

# 拆分-应用-组合范例

现在我们知道如何索引和排序我们的数据帧。这很有趣，但更有趣的是，如果我们不需要用眼睛来观察所有这些观察结果。如果我们能够以某种方式聚合我们的数据并收集一些有趣的统计数据会怎么样？嗯，这在 SQL 中称为 GROUP BY 操作，我们在 DataFrames.jl 中也有类似的东西。对于初学者，让我们做一些非常简单的事情。让我们算出我们每年有多少次观察(行):

```
8×2 DataFrame
 Row │ Year   nrow
     │ Int32  Int64
─────┼──────────────
   1 │  1980    545
   2 │  1981    545
   3 │  1982    545
   4 │  1983    545
   5 │  1984    545
   6 │  1985    545
   7 │  1986    545
   8 │  1987    545
```

这是如何工作的？涉及两个功能:

*   `groupby`:获取数据帧和我们想要对其进行分组的列。
*   `combine`:获取`groupby`的输出，并对每个组应用函数`nrow`，汇总结果。

> `nrow`函数只计算数据帧中的行数。

最终的结果是一个 DataFrame，每个`Year`——我们分组的列——都有一行。柱子也用`Year`和`nrow`来命名。

你也可以**使用匿名(lambda)函数**进行上述操作。如果您有一些快速功能要运行，这是很有用的，但要让我们开始，这里有与上面相当的功能:

```
combine(groupby(males, ["Year"]), df -> nrow(df))
```

我认为这告诉了我们更多关于它是如何工作的。我是这样理解的:

**按列`Year`分割**名为 males 的数据集，然后对每个更小的数据集`df` **应用**函数`nrow`。最后，**将**结果组合成一个数据帧，这样我们最终得到一个包含两列的表:一列表示拆分(年份)，另一列表示我们的函数结果。

你在这里看到的实际上是**拆分-应用-组合范式**。了解如何快速有效地完成这项工作，您将能够从大多数数据集中提取您需要的所有信息！

让我们多做一些练习。工人每年的平均工资是多少？

```
8×2 DataFrame
 Row │ Year   x1
     │ Int32  Float64
─────┼────────────────
   1 │  1980  1.39348
   2 │  1981  1.51287
   3 │  1982  1.57167
   4 │  1983  1.61926
   5 │  1984  1.69029
   6 │  1985  1.73941
   7 │  1986  1.79972
   8 │  1987  1.86648
```

我知道你在想什么。那个栏目为什么叫`x1`？这很难看，而且当我不得不记住这些数字代表什么的时候，我的生活会变得更加艰难。不要担心，我们可以**将几乎任何我们喜欢的函数**传递到`combine`中，这样我们实际上可以创建一个具有新列名的数据框架:

```
8×2 DataFrame
 Row │ Year   wage_avg
     │ Int32  Float64
─────┼─────────────────
   1 │  1980   1.39348
   2 │  1981   1.51287
   3 │  1982   1.57167
   4 │  1983   1.61926
   5 │  1984   1.69029
   6 │  1985   1.73941
   7 │  1986   1.79972
   8 │  1987   1.86648
```

整洁多了。让我们看看我们是否可以将这个扩展到**在每次分割时收集多个统计数据**——记住我们可以在`combine`中做任何我们喜欢的事情:

```
8×3 DataFrame
 Row │ Year   wage_avg  people
     │ Int32  Float64   Int64
─────┼─────────────────────────
   1 │  1980   1.39348     545
   2 │  1981   1.51287     545
   3 │  1982   1.57167     545
   4 │  1983   1.61926     545
   5 │  1984   1.69029     545
   6 │  1985   1.73941     545
   7 │  1986   1.79972     545
   8 │  1987   1.86648     545
```

在这里，我们不仅收集每年的平均工资，还收集每年的样本量——独特工人的数量。以上真的很厉害，在调查数据集的时候可以帮到你很多。

# 迂回到方法链接，又名管道

![](img/3386bf362fa91b290912d667c661223a.png)

是的，这不是烟斗。乔纳森·卡罗尔在 [Unsplash](https://unsplash.com/s/photos/this-is-not-a-pipe?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

正如我们在上面看到的，使用**匿名函数可以非常有效地从数据集**中快速获得一些统计数据。然而，当它们变得复杂时，可能有点难以阅读。我喜欢通过使用管道或方法链来解决这个问题。

这里我将展示一个管道的例子。假设你有一个包含 3 个值的向量:`[1,2,3]`你想对所有的值求平方，求和，然后求和的平方根。您可以使用标准函数来实现这一点:

```
julia> sqrt(sum(([1,2,3] .^ 2)))
3.7416573867739413
```

请注意，我们必须像写数学函数一样，把上面的内容写出来。我们首先写`sqrt`，然后写`sum`等等，而不是像我们描述的那样从平方开始。这就是方法链发挥作用的地方。通过使用 Julia 中内置的管道操作符`|>`，我们可以将操作的结果传递给一个新函数。下面是与上面相同的操作，但是使用了管道:

```
julia> [1,2,3] .^ 2 |> sum |> sqrt
3.7416573867739413
```

我们为什么要用这个？嗯，这允许我们**编写链接在一起的快速而肮脏的匿名函数**来做一些奇特的聚合。

# 把所有的放在一起

![](img/2e56a270888a9a86322abf8e7f8cf0e7.png)

约翰·施诺布里奇在 [Unsplash](https://unsplash.com/s/photos/together?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

现在我们知道了如何使用管道，让我们看看如何在数据帧中使用它们。作为一个例子，假设我们想要**获得每个人的第一个和最后一个记录的工资**。因为我们知道如何排序和分割数据帧，所以我们可以这样做:

```
julia> first(firstlast, 5)
5×3 DataFrame
 Row │ NR     first    last
     │ Int32  Float64  Float64
─────┼─────────────────────────
   1 │    13  1.19754  1.66919
   2 │    17  1.67596  1.82033
   3 │    18  1.51596  2.87316
   4 │    45  1.89411  2.13569
   5 │   110  1.94877  2.11239
```

在第一步中，我们对分割的数据帧进行排序，然后我们使用这个`sorted_df`和**应用另一个函数**来提取第一个和最后一个工资值。让我们做一个快速的感觉检查，看看这是否确实是我们想要的:

```
julia> males[(males.NR .== 13) .& (map(year->year ∈ [1980, 1987], males.Year)), [:NR, :Year, :Wage]]
2×3 DataFrame
 Row │ NR     Year   Wage
     │ Int32  Int32  Float64
─────┼───────────────────────
   1 │    13   1980  1.19754
   2 │    13   1987  1.66919
```

是的，这行得通，所以让我们简化一下:

很漂亮，对吧？想象一下你可以用这些做什么样的疯狂分析！💪

让我们多练习一些。让我们创建一个名为`is_lower`的列，如果员工的最终工资**低于他的最佳工资**，则该列为真。我们需要像以前一样找出他们的第一份和最后一份工资，但是现在我们还需要存储最大工资值:

```
julia> first(finishers, 5)
5×5 DataFrame
 Row │ NR     first    max      last     is_lower
     │ Int32  Float64  Float64  Float64  Bool
─────┼────────────────────────────────────────────
   1 │    13  1.19754  1.85306  1.66919      true
   2 │    17  1.67596  1.82033  1.82033     false
   3 │    18  1.51596  2.87316  2.87316     false
   4 │    45  1.89411  2.13569  2.13569     false
   5 │   110  1.94877  2.20252  2.11239      true
```

这看起来是可行的，所以让我们稍微清理一下我们的代码，因为我们实际上不需要存储第一个/最后一个值——我只是添加它们来做一个快速的感觉检查。

```
julia> first(finishers, 5)
5×2 DataFrame
 Row │ NR     is_lower
     │ Int32  Bool
─────┼─────────────────
   1 │    13      true
   2 │    17     false
   3 │    18     false
   4 │    45     false
   5 │   110      true
```

最后，我们可以将上述内容合并回原始数据集:

```
julia> first(new_males, 5)
5×6 DataFrame
 Row │ NR     Year   Wage     Ethn   Exper  is_lower
     │ Int32  Int32  Float64  Cat…   Int32  Bool
─────┼───────────────────────────────────────────────
   1 │    13   1980  1.19754  other      1      true
   2 │    13   1981  1.85306  other      2      true
   3 │    13   1982  1.34446  other      3      true
   4 │    13   1983  1.43321  other      4      true
   5 │    13   1984  1.56813  other      5      true
```

# 结论

你现在已经掌握了可能是最强大的分析技术，并且知道如何在 Julia 中**拆分-应用-组合你的数据框架**。

您还学习了如何:

*   **使用布尔向量索引**你的数据帧
*   **根据列值对数据帧进行排序**——甚至可以使用`sort!`就地排序
*   **根据**对数据帧进行分组，并对其应用函数
*   使用**管道**操作符`|>`让你的 lambda 函数更加强大

谢谢你一路看完！👏

如果你想获得更多朱莉娅的乐趣，请访问 DataFrames.jl 和 CSV.jl:

[](/julia-dataframes-jl-basics-95dba5146ef4) [## Julia DataFrames.jl 基础

### 用 DataFrames.jl 戳戳你的数据

towardsdatascience.com](/julia-dataframes-jl-basics-95dba5146ef4) [](/reading-csv-files-with-julia-e2623fb62938) [## 与 Julia 一起阅读 CSV 文件

### 了解如何使用 CSV.jl 读取各种逗号分隔的文件

towardsdatascience.com](/reading-csv-files-with-julia-e2623fb62938) [](/reading-csv-files-with-julia-part-2-51d74434358f) [## 与 Julia 一起阅读 CSV 文件—第 2 部分

### 货币、布尔值等的自定义格式。

towardsdatascience.com](/reading-csv-files-with-julia-part-2-51d74434358f)