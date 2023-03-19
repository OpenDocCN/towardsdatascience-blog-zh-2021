# 超越 value_counts():只用 3 行代码创建视觉上引人入胜的频率表(使用 OkCupid 数据)

> 原文：<https://towardsdatascience.com/going-beyond-value-counts-creating-visually-engaging-frequency-tables-with-only-3-lines-of-code-3021c7756991?source=collection_archive---------41----------------------->

## 一些你可能不知道的简单而有用的生活窍门

![](img/76db0bf53350d5bd63e050f036f0564d.png)

图片由作者创作，灵感来自 [Loveascii](http://loveascii.com/hearts.html)

虽然在 Python pandas 库中为一个系列对象创建频率表最简单的方法是应用`value_counts()`方法，但是这个操作的结果看起来相当简单。我们可以通过调整方法的布尔参数`normalize`、`sort`、`ascending`和`dropna`，或者将值(如果它们是数字的话)分组到 bin 中，来使它提供更多的信息。然而，这里的选项非常有限，所以为了在视觉上增强结果频率表，我们可以考虑一些简单而有用的技巧，比如方法链接、文本定制、为每个频率值添加`%`符号，以及使用漂亮打印的功能。

在本文中，我们将使用包含年龄、性别、位置、教育等信息的 [Kaggle 数据集](https://www.kaggle.com/andrewmvd/okcupid-profiles)进行实验。，针对 *OkCupid 交友 app* 的 6 万用户💞。然而，出于我们的目的，我们将只使用用户状态的数据。

# 1.方法链接

首先，让我们为用户状态创建一个基本频率表:

```
import pandas as pd
profiles = pd.read_csv("okcupid_profiles.csv", na_values="unknown")
s = profiles["status"]
s.value_counts()**Output:**single            55697
seeing someone     2064
available          1865
married             310
Name: status, dtype: int64
```

尽管此表清楚地显示了总体趋势，但以相对频率查看此信息会更方便。让我们将`True`分配给`normalize`参数，并将结果频率表(它本身是一个序列)分配给一个名为`s_norm`的变量。

(*旁注:*在下文中，我们将默认保留`value_counts()`方法的所有其他参数，这意味着我们将只考虑按降序排序的频率表，并排除缺失值。对于我们的目的，使用或不使用这些参数并不重要。)

```
s_norm = s.value_counts(normalize=True)
s_norm**Output:**single            0.929275
seeing someone    0.034437
available         0.031117
married           0.005172
Name: status, dtype: float64
```

现在所有的频率都被转换成分数，但是我们更喜欢看到百分比，所以让我们把这个系列乘以 100:

```
s_pct = s_norm.mul(100)
s_pct**Output:**single            92.927456
seeing someone     3.443673
available          3.111652
married            0.517218
Name: status, dtype: float64
```

我们真的不需要如此精确的百分比。此外，让我们想象一下，我们根本不需要小数点:

```
s_pct_rounded = s_pct.round()
s_pct_rounded**Output:**single            93.0
seeing someone     3.0
available          3.0
married            1.0
Name: status, dtype: float64
```

不幸的是，舍入到整个部分给我们留下了所谓的[可空整数](https://pandas.pydata.org/pandas-docs/stable/user_guide/integer_na.html)(即小数部分等于 0 的浮点数)。要修复它，我们可以使用`convert_dtypes()`或`astype(int)`:

```
s_pct_int = s_pct_rounded.convert_dtypes()
s_pct_int**Output:**single            93
seeing someone     3
available          3
married            1
Name: status, dtype: Int64
```

现在让我们展开`s_pct_int`的整个表达式，显示我们链接的所有方法:

```
s_pct_int = profiles['status'].value_counts(normalize=True).mul(100).round().convert_dtypes()
```

# 2.添加表格标题

在不了解上下文的情况下，从上表中无法清楚地看出这些数字代表的是百分比而不是绝对频率。让我们使用 f 字符串格式为表格添加一个标题:

```
print(f"OkCupid user statuses, %\n{s_pct_int}")**Output:**OkCupid user statuses, %
single            93
seeing someone     3
available          3
married            1
Name: status, dtype: Int64
```

一个好主意是，特别是如果我们一次打印出几个频率表，从视觉上突出表的其余部分的标题，例如使其加粗。为此，我们可以使用 [ANSI 转义码](https://en.wikipedia.org/wiki/ANSI_escape_code)序列。特别是，要在 Python 中以粗体显示一个字符串，我们应该在该字符串之前添加序列`\033[1m`,在它之后添加`\033[0m` —:

```
print(f"\033[1mOkCupid user statuses, %\033[0m\n{s_pct_int}")**Output:****OkCupid user statuses, %** 
single            93 
seeing someone     3 
available          3 
married            1 
Name: status, dtype: Int64
```

# 3.将`%`符号加到每个频率值上

在上面的例子中，我们给表格标题添加了`%`符号。如果我们想把它加到每个频率值上呢？这里的一个解决方法是制作一个频率值列表，在每个列表中添加`%`符号，然后从这个列表中创建一个序列。为了制作列表，我们可以使用下面的 For 循环:

```
lst_with_pct_symb = []
for value in s_pct_int.values:
    lst_with_pct_symb.append(f"{value}%")
lst_with_pct_symb**Output:**['93%', '3%', '3%', '1%']
```

或者更简洁地说，一个列表理解:

```
lst_with_pct_symb = [f"{value}%" for value in s_pct_int.values]
```

现在，让我们创建并打印一个更新的频率表。这一次，我们将从标题中删除`%`符号:

```
s_with_pct_symb = pd.Series(lst_with_pct_symb, index=s_pct_int.index)
print(f"\033[1mOkCupid user statuses\033[0m\n{s_with_pct_symb}")**Output:****OkCupid user statuses**
single            93%
seeing someone     3%
available          3%
married            1%
dtype: object
```

# 4.漂亮的印刷桌子

最后，我们可以把频率表打印出来。为此，我们将使用`to_markdown()` pandas 方法，该方法需要安装(不一定是导入)[制表](https://pypi.org/project/tabulate/)模块(`pip install tabulate`)。

**重要提示:**为了正确显示结果，只能在`print()`命令中使用`to_markdown()`方法。

让我们再次展示名为`s_with_pct_symb`的用户状态频率表，这次是一个真实的表，使用的是`to_markdown()`的基本语法。如前所述，我们将添加一个粗体标题并应用 f 字符串格式。为了一致性，在下文中，我们将要显示的频率表分配给一个名为`S`的变量:

```
S = s_with_pct_symb
print(f"\033[1mOkCupid user statuses\033[0m\n{S.to_markdown()}")**Output:****OkCupid user statuses**
|                | 0   |
|:---------------|:----|
| single         | 93% |
| seeing someone | 3%  |
| available      | 3%  |
| married        | 1%  |
```

我们可能要做的第一件事是删除一个自动创建的表头(在我们的例子中是多余的)。为此，我们必须为一个名为`headers`的可选参数分配一个空列表或字符串:

```
print(f"\033[1mOkCupid user statuses\033[0m\n{S.to_markdown(headers=[])}")**Output:****OkCupid user statuses**
|:---------------|:----|
| single         | 93% |
| seeing someone | 3%  |
| available      | 3%  |
| married        | 1%  |
```

在[表格文档](https://pypi.org/project/tabulate/)中，我们可以找到一些其他参数进行调整。但是，它们大多与 DataFrame 对象相关。在我们的例子中，假设我们有一个序列，并且记住在添加了`%`符号之后，频率值实际上变成了字符串，我们有一个小得多的选择。

我们来玩一下参数`tablefmt`和`stralign`。它们中的第一个定义了表格格式，可以有下列值之一:`plain`、`simple`、`github`、`grid`、`fancy_grid`、`pipe`、`orgtbl`、`jira`、`presto`、`pretty`、`psql`、`rst`等。例如，我们之前看到的表格格式叫做`pipe`，这是`to_markdown()` pandas 方法的默认格式。奇怪的是，对于制表软件包本身，默认的表格格式是`simple`。至于第二个参数`stralign`，它用于覆盖默认的字符串数据对齐方式(即`left`)。这里可能的选项是`right`和`center`。

```
print(f"\033[1mOkCupid user statuses\033[0m\n"
      f"{S.to_markdown(headers=[], tablefmt='fancy_grid', stralign='right')}")**Output:****OkCupid user statuses**
╒════════════════╤═════╕
│         single │ 93% │
├────────────────┼─────┤
│ seeing someone │  3% │
├────────────────┼─────┤
│      available │  3% │
├────────────────┼─────┤
│        married │  1% │
╘════════════════╧═════╛
```

现在让我们把注意力转向我们之前创建的名为`s_pct_int`的频率表。提醒其语法和外观:

```
s_pct_int = profiles['status'].value_counts(normalize=True).mul(100).round().convert_dtypes()
s_pct_int**Output:**single            93
seeing someone     3
available          3
married            1
Name: status, dtype: Int64
```

要为`s_pct_int`创建降价表，添加相应的表头是有意义的。同样，让我们为`tablefmt`参数尝试一个新值:

```
S = s_pct_int
print(f"\033[1mOkCupid user statuses\033[0m\n"
      f"{S.to_markdown(headers=['STATUS', '%'], tablefmt='github')}")**Output:****OkCupid user statuses**
| STATUS         |   % |
|----------------|-----|
| single         |  93 |
| seeing someone |   3 |
| available      |   3 |
| married        |   1 |
```

然而，这里我们有一个好消息:对于数字频率值，我们可以使用`floatfmt`参数来定制浮点数的格式。这意味着上面方法链中的最后两个方法(`round()`和`convert_dtypes()`)是多余的，可以删除。它留给我们之前创建的频率表`s_pct`:

```
s_pct = profiles['status'].value_counts(normalize=True).mul(100)
s_pct**Output:**single            92.927456
seeing someone     3.443673
available          3.111652
married            0.517218
Name: status, dtype: float64
```

让我们展示一下它的降价表示:

```
S = s_pct
print(f"\033[1mOkCupid user statuses\033[0m\n"
      f"{S.to_markdown(headers=['STATUS', '%'], tablefmt='github', floatfmt='.0f')}")**Output:****OkCupid user statuses**
| STATUS         |   % |
|----------------|-----|
| single         |  93 |
| seeing someone |   3 |
| available      |   3 |
| married        |   1 |
```

由于正确的数字格式，我们获得了与前一个相同的表。

**注意:**`floatfmt`参数不配合表格格式`pretty`使用。

# 实用的外卖

尽管上面的整个演练花费了大量的迭代和描述，下面我们将找到 4 个不同版本的`profiles['status']`频率表的最终代码解决方案，全部以%表示:

*   2 个带有/不带有`%`符号的简单表格，
*   2 个印刷精美的表格，带/不带`%`符号，带/不带表头。

每个解决方案最多只需 3 行简单代码，就能生成感兴趣的频率表的直观有效的表示。

```
S = profiles['status'].value_counts(normalize=True).mul(100).round().convert_dtypes()
print(f"\033[1mOkCupid user statuses, %\033[0m\n{S}")**Output:****OkCupid user statuses, %**
single            93
seeing someone     3
available          3
married            1
Name: status, dtype: Int64
```

```
s_pct_int = profiles['status'].value_counts(normalize=True).mul(100).round().convert_dtypes()
S = pd.Series([f"{value}%" for value in s_pct_int.values], index=s_pct_int.index)
print(f"\033[1mOkCupid user statuses\033[0m\n{S}")**Output:****OkCupid user statuses**
single            93%
seeing someone     3%
available          3%
married            1%
dtype: object
```

```
S = profiles['status'].value_counts(normalize=True).mul(100)
print(f"\033[1mOkCupid user statuses\033[0m\n"
      f"{S.to_markdown(headers=['STATUS', '%'], tablefmt='github', floatfmt='.0f')}")**Output:****OkCupid user statuses**
| STATUS         |   % |
|----------------|-----|
| single         |  93 |
| seeing someone |   3 |
| available      |   3 |
| married        |   1 |
```

```
s_pct_int = profiles['status'].value_counts(normalize=True).mul(100).round().convert_dtypes()
S = pd.Series([f"{value}%" for value in s_pct_int.values], index=s_pct_int.index)
print(f"\033[1mOkCupid user statuses\033[0m\n{S.to_markdown(headers=[], tablefmt='fancy_grid')}")**Output:****OkCupid user statuses**
╒════════════════╤═════╕
│ single         │ 93% │
├────────────────┼─────┤
│ seeing someone │ 3%  │
├────────────────┼─────┤
│ available      │ 3%  │
├────────────────┼─────┤
│ married        │ 1%  │
╘════════════════╧═════╛
```

# 结论

在本文中，我们讨论了一些简单而强大的方法来改进频率表的布局和整体可读性。他们都有`value_counts()`熊猫方法作为核心部分，但都超越了它，产生了更有影响力的表现。更重要的是，每个建议的解决方案，在其最终形式下，最多需要 3 行代码。

我希望你喜欢阅读我的文章，并发现它很有帮助。感谢大家的阅读，祝使用 OkCupid 交友 app 的人好运😉💘

**你会发现这些文章也很有趣:**

[](/5-pandas-methods-youve-never-used-and-you-didn-t-lose-anything-37277fae7c55) [## 你从未用过的 5 种熊猫方法…而且你没有失去任何东西！

### 你知道他们到底什么时候能帮上忙吗？

towardsdatascience.com](/5-pandas-methods-youve-never-used-and-you-didn-t-lose-anything-37277fae7c55) [](/an-unconventional-yet-convenient-matplotlib-broken-barh-function-and-when-it-is-particularly-88887b76c127) [## 一个非常规但方便的 Matplotlib Broken_Barh 函数，当它特别…

### 它是什么，如何使用和定制，何时使用

towardsdatascience.com](/an-unconventional-yet-convenient-matplotlib-broken-barh-function-and-when-it-is-particularly-88887b76c127) [](/2-efficient-ways-of-creating-fancy-pictogram-charts-in-python-8b77d361d500) [## 用 Python 创建精美象形图的两种有效方法

### 什么是象形图，何时使用，以及如何在 Plotly 和 PyWaffle 库中创建它们

towardsdatascience.com](/2-efficient-ways-of-creating-fancy-pictogram-charts-in-python-8b77d361d500)