# 熊猫数据框简单指南

> 原文：<https://towardsdatascience.com/a-simple-guide-to-pandas-dataframes-b125f64e1453?source=collection_archive---------9----------------------->

## 如何使用 Python 的 Pandas 库创建、存储和操作数据

![](img/9a65ad3e32b38ae28ccfb6dc66e1e6a3.png)

埃米尔·佩龙在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

标准 Python 库`pandas`是用于数据分析和操作的最流行的库之一。`pandas`用于将数据转换为称为 DataFrame 的结构化格式，可用于各种操作和分析。数据帧有助于将数据格式化为清晰的表格，便于阅读和操作。

# 装置

`pandas`是标准的 Python 库之一，应该已经安装了，但是如果没有，可以用下面的命令安装:

```
pip install pandas
```

# 入门指南

`pandas`库通常与 Python 的`numpy`库一起使用，因此为了本教程，我将导入并使用这两个库。(将来会写一个如何使用`numpy`库的指南！)下面是一个从`numpy`系列创建`pandas`数据帧的简单例子:

```
import pandas as pd
import numpy as npvalues = np.random.randn(3, 2)
print(values)
print(type(values))df = pd.DataFrame(values)
print(type(df))
print(df)
```

下面是两个打印语句的输出:

```
<class 'numpy.ndarray'>
[[-1.01774631  1.0842383 ]
 [-0.04752437 -0.19560713]
 [-0.3643328   0.34562402]]<class 'pandas.core.frame.DataFrame'>
          0         1
0 -1.017746  1.084238
1 -0.047524 -0.195607
2 -0.364333  0.345624
```

如您所见，第一个输出是一个 3 行 2 列的多维`numpy`数组。`np.random.randn`就是简单地在指定的输出形状中生成随机数。通过使用`numpy`数组作为参数调用`pd.DataFrame()`，可以将数组转换为`pandas`数据帧。第二组打印语句显示了 DataFrame，它包含与`numpy`数组相同的值，以及 3 行 2 列的相同形状。默认情况下，列和行的索引从 0 开始，但这些可以手动更改，也可以在创建数据帧时通过初始化来更改。

# 更新表格

## 更改列/行索引

如前所述，默认情况下，列和索引从 0 开始。但是，使用简单的整数作为索引并不总是有用的，因此建议根据您正在处理的数据将行和列更改为更合适的标签。您可以在初始化数据帧时或之后更改它们。假设您有想要用来标记列和行的字段名称。以下代码显示了如何使用名称初始化 DataFrame:

```
import pandas as pd
import numpy as npvalues = np.random.randn(3, 2)rows = ["A", "B", "C"]
features = ["foo", "bar"]df = pd.DataFrame(values, index=rows, columns=features)
print(df)
```

输出:

```
 foo       bar
A -2.702060  0.791385
B  1.696073 -0.971109
C -1.430298 -2.549262
```

如您所见，`index`和`columns`参数允许您在创建 DataFrame 时指定索引和列名。您也可以使用以下内容手动更改它们(建议使用关键字重命名，而不是位置):

```
df.rename(columns={"bar": "baz"}, inplace=True) foo       baz
A  0.010732  0.420194
B -1.718910 -1.810119
C  0.409996  0.694083
```

在这里您可以看到，通过调用`rename()`函数，列`bar`被更改为`baz`。注意`inplace=True`被添加到参数的末尾。如果要提交对数据帧的更改，这是必要的。或者，由于 DataFrame 上的函数返回 DataFrame 对象，您可以执行以下操作，其效果与添加`inplace=True`相同:

```
df = df.rename(columns={"bar": "baz"})
```

在这里打印`df`会得到与上面相同的结果，只是列被重命名了。

此外，通过使用`set_index()`函数，您可以选择其中一列作为索引，如下所示，使用 3x4 矩阵:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)df.set_index("foo", inplace=True)print(df)
```

输出:

```
 bar  baz  qux
foo
1      2    3    4
5      6    7    8
9     10   11   12
```

这将获取`foo`列并用这些值替换当前索引。当有一列用于`date`或`id`并且您想要将索引设置为这些唯一值时，这通常会很方便。

## 添加新列/行

如果您想在数据帧的底部追加一个新行，您必须确保它与现有数据帧的形状相同。例如，如果您有与上面相同的 4 列数据帧，则正在添加的新行也应该有相同的 4 列。下面是新的数据框架，由 3 行 4 列组成:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)
prrint(df)
```

输出:

```
 foo  bar  baz  qux
A    1    2    3    4
B    5    6    7    8
C    9   10   11   12
```

现在，如果您想添加一个新行，这 4 列必须对齐。至于索引，您可以指定它或者让`pandas`自动生成一个新的(默认为下一个可用的整数):

```
new_data = [[13, 14, 15, 16]]
new_df = pd.DataFrame(new_data, index=["D"], columns=features)df = df.append(new_df)
print(df)
```

输出:

```
 foo  bar  baz  qux
A    1    2    3    4
B    5    6    7    8
C    9   10   11   12
D   13   14   15   16
```

注意`new_data`不仅仅是一个列表，而是一个 1 行 4 列的矩阵。这是因为它必须遵循与附加它的初始数据帧相同的结构。然后，您可以将这个 1x4 转换为第二个 DataFrame，它具有指定的索引和相同的列。功能`df.append()`允许您将这个由单行组成的新数据帧添加到原始数据帧中。

要向数据帧添加新列，只需用新列名和新数据引用数据帧。使用与上面相同的数据帧，它看起来像这样:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)
df["quux"] = [55, 56, 57]print(df)
```

输出:

```
 foo  bar  baz  qux  quux
A    1    2    3    4    55
B    5    6    7    8    56
C    9   10   11   12    57
```

只要新列表与初始数据帧具有相同的行，就可以很容易地追加新列。

还有一个更巧妙的技巧——如果您想选择所有的列名，您可以调用`df.columns`来返回`['foo', 'bar', 'baz', 'qux', 'quux']`！

# 选择数据

有时，您可能需要访问特定的行或列，甚至是特定的数据单元格。与我们之前添加列的方式类似，您可以用同样的方式访问一个或多个列:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)
print(df[["foo", "baz"]])
```

输出:

```
 foo  baz
A    1    3
B    5    7
C    9   11
```

通过调用由列名列表索引的`df`，您可以创建一个只包含指定列的新对象。现在，如果您只想访问某些行，您可以使用`.loc`或`.iloc`。这两者的区别只是通过名称或索引位置来访问行。以下示例显示了两种方法以及它们如何返回同一行:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)row_loc = df.loc["B"]
print(row_loc)row_iloc = df.iloc[1]
print(row_iloc)
```

输出:

```
foo    5
bar    6
baz    7
qux    8
Name: B, dtype: int64foo    5
bar    6
baz    7
qux    8
Name: B, dtype: int64
```

索引 1 处的行(第二行，因为我们从 0 开始计数！)和索引为`B`的行是同一行，可以用任何一种方法访问。

如果您想访问单个单元格，您也可以使用`.iloc`,如下图所示:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)
print(df)cell = df.iloc[1, 2]
print("cell: ", cell)
```

输出:

```
 foo  bar  baz  qux
A    1    2    3    4
B    5    6    7    8
C    9   10   11   12cell:  7
```

使用`.iloc`，第一个值是行，第二个值是列——因此`.iloc[1, 2]`访问第 2 行第 3 列的单元格。

# 操作

数学运算也可应用于`pandas`数据帧中的数据。简单地说，可以将标量应用于数据帧的一列，如下所示:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)df["qux"] = df["qux"] * 10print(df)
```

输出:

```
 foo  bar  baz  qux
A    1    2    3   40
B    5    6    7   80
C    9   10   11  120
```

这里，我们将`qux`列乘以标量 10，并用新的值替换该列中的值。您还可以在多列之间执行操作，例如将列相加或相乘。在下面的示例中，我们将两列相乘，并将它们存储到一个新列中:

```
import pandas as pdvalues = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)df["quux"] = df["bar"] * df["baz"]print(df)
```

输出:

```
 foo  bar  baz  qux  quux
A    1    2    3    4     6
B    5    6    7    8    42
C    9   10   11   12   110
```

这里，新的`quux`列的每一行都是`bar`列和`baz`列的每一行的乘积。

您可以在数据帧上执行的另一个重要操作是`apply()`功能。这允许您对数据帧中的数据应用整个函数。以下是使用`apply()`函数对数据应用独立函数的简单示例:

```
import pandas as pddef sum_function(row):
    return row["foo"] + row["bar"] + row["baz"] + row["qux"]values = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
rows = ["A", "B", "C"]
features = ["foo", "bar", "baz", "qux"]df = pd.DataFrame(values, index=rows, columns=features)df["quux"] = df.apply(sum_function, axis=1)print(df)
```

输出:

```
 foo  bar  baz  qux  quux
A    1    2    3    4    10
B    5    6    7    8    26
C    9   10   11   12    42
```

这里，我们有一个名为`sum_function()`的函数，它简单地将一行中的 4 个元素相加。使用相同的 DataFrame，我们可以用这个函数作为参数调用`df.apply()`。第二个参数`axis=1`是必需的，因为默认情况下轴被设置为 0，而`apply()`将作用于行而不是列。使用这个`apply()`函数，我们可以看到`quux`列包含所有先前字段的总和，这是由指定的函数应用的。

# 读取/写入数据

`pandas`的另一个简洁的特性是你可以读写数据帧中的文件。例如，您可以读入 CSV 或 JSON 文件，处理数据，然后将其写回新文件。假设您在与 Python 脚本相同的目录中有一个名为`data.csv`的文件，其中包含我们一直在处理的相同数据。您可以使用以下代码将它读入数据帧:

```
import pandas as pddf = pd.read_csv("data.csv")print(df)
```

其输出将与我们一直在处理的数据相同:

```
 foo  bar  baz  qux
A    1    2    3    4
B    5    6    7    8
C    9   10   11   12
```

然后，如果您想将数据写回一个新的 CSV，您可以使用以下命令将它写到您的工作目录:

`output.csv`:

```
,foo,bar,baz,qux
A,1,2,3,4
B,5,6,7,8
C,9,10,11,12
```

这个`output.csv`正是我们最初用`read_csv()`函数读入数据帧的`data.csv`所包含的内容！

# 外卖食品

`pandas`库非常灵活，对于处理数据集和数据分析非常有用。使用`pandas`对数据进行存储、操作和执行操作非常简单。此外，许多机器学习模型可以使用`pandas`数据帧作为它们的输入。本指南的目标是提供该库的简单概述，并提供一些基本示例来帮助您更好地理解`pandas`的一些关键特性。

感谢您的阅读，我希望您已经对`pandas`有了足够的了解，可以开始在您的下一个 Python 项目中实现它了！