# 你从未用过的 5 种熊猫方法…而且你没有失去任何东西！

> 原文：<https://towardsdatascience.com/5-pandas-methods-youve-never-used-and-you-didn-t-lose-anything-37277fae7c55?source=collection_archive---------24----------------------->

## 你知道他们到底什么时候能帮上忙吗？

![](img/6af5badbd7cbb87b0409621706563a6e.png)

作者图片

总的来说，Python 的 pandas 库是一个非常有效的处理列表数据的工具，有时它会让用户感到惊讶。在这篇文章中，我们将讨论 5 个怪异的熊猫方法，这些方法由于各种原因看起来完全是多余的:笨重，有一个更简洁和众所周知的同义词，或者就是没用。

# 1.`[ndim](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.ndim.html)`

这个方法继承自 numpy。它返回一个对象的轴数，即 1 表示系列，2 表示数据帧:

```
import pandas as pd
df = pd.DataFrame({'A':[6, 8], 'B':[9, 2], 'C':[1, 5]}, 
                   index =['a', 'b'])
print(df, '\n')
print('ndim for a dataframe:', df.ndim)
print('ndim for a Series:', df['A'].ndim)**Output:**
   A  B  C
a  6  9  1
b  8  2  5 

ndim for a dataframe: 2
ndim for a Series: 1
```

实际上，我们用这种方法唯一能做的就是区分 Series 和 dataframe 对象。然而，出于同样的目的，我们可以简单地使用一种更广为人知和通用的方法— `type()`。此外，结果将以更容易理解的方式输出:

```
print(type(df))
print(type(df['A']))**Output:**
<class 'pandas.core.frame.DataFrame'>
<class 'pandas.core.series.Series'>
```

# 2.`[keys](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.keys.html)`

与 Python 字典一样，可以在 pandas 结构上使用方法`keys()`来获取它的“信息轴”，即数据帧的系列和列的索引。该语法不隐含任何参数:

```
print(df.keys())
print(df['A'].keys())**Output:** Index(['A', 'B', 'C'], dtype='object')
Index(['a', 'b'], dtype='object')
```

然而，与字典不同的是，pandas 对象本质上代表具有行和列的表格。因此，用`columns`和`index`来代替更自然(也更常见)。此外，通过这种方式，我们可以获得一个数据帧的索引，而不仅仅是一个序列的索引:

```
print(df.columns)
print(df.index)
print(df['A'].index)**Output:**
Index(['A', 'B', 'C'], dtype='object')
Index(['a', 'b'], dtype='object')
Index(['a', 'b'], dtype='object')
```

# 3.`[bool](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.bool.html)`

另一个不太实用的方法是`bool()`，也没有任何参数。它唯一做的事情是返回一个具有布尔值的**单元素熊猫结构的 bool。如果这两个条件中至少有一个不满足，If 将返回一个`ValueError`。换句话说，该方法只返回系列或数据帧的唯一值(bool 类型):**

```
print(pd.Series([True]).bool())
print(pd.Series([False]).bool())
print(pd.DataFrame({'col': [True]}).bool())
print(pd.DataFrame({'col': [False]}).bool())**Output:** True
False
True
False
```

很难想象什么情况下有必要进行这种手术。无论如何，有更熟悉(也更通用)的方法可以做到这一点:

```
print(pd.Series([True]).values[0])df2 = pd.DataFrame({'col': [False]})
print(df2.loc[0, 'col'])
print(df2.iloc[0, 0])
print(df2.at[0, 'col'])
print(df2.squeeze())**Output:** True
False
False
False
False
```

# 4.`[assign](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.assign.html)`

此方法向数据帧添加新列:

```
df = df.assign(D=df['A']+df['B'])
print(df)**Output:
**   A  B  C   D
a  6  9  1  15
b  8  2  5  10
```

或者覆盖现有的:

```
df = df.assign(D=df['B']+df['C'])
print(df)**Output:
**   A  B  C   D
a  6  9  1  10
b  8  2  5   7
```

或者创建多个列，其中一列是基于在同一个`assign`中定义的另一列进行计算的:

```
df = df.assign(E=lambda x: x['C'] + x['D'], 
               F=lambda x: x['D'] + x['E'])
print(df)**Output:
**   A  B  C   D   E   F
a  6  9  1  10  11  21
b  8  2  5   7  12  19
```

同样的结果可以用一种不太麻烦、可读性更好的方式获得，尽管:

```
df['D']=df['B']+df['C']
df['E']=df['C']+df['D']
df['F']=df['D']+df['E']
print(df)**Output:
**   A  B  C   D   E   F
a  6  9  1  10  11  21
b  8  2  5   7  12  19
```

# 5.`[swapaxes](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.swapaxes.html)`

顾名思义，该函数适当地交换数据帧的轴和相应的值:

```
print(df, '\n')
print(df.swapaxes(axis1='index', axis2='columns'))**Output:
**   A  B  C   D   E   F
a  6  9  1  10  11  21
b  8  2  5   7  12  19 

    a   b
A   6   8
B   9   2
C   1   5
D  10   7
E  11  12
F  21  19
```

这里一件奇怪的事情是，我们总是必须指定参数`'index'`和`'columns'`(否则，将会抛出一个`TypeError`)，尽管很明显这个方法的唯一应用就是交换这两个轴。在这种情况下，`transpose()`方法似乎是更好的选择:

```
print(df.transpose())**Output:
**    a   b
A   6   8
B   9   2
C   1   5
D  10   7
E  11  12
F  21  19
```

尤其是它的快捷方式:

```
print(df.T)**Output:**
    a   b
A   6   8
B   9   2
C   1   5
D  10   7
E  11  12
F  21  19
```

# 结论

冗余熊猫方法的列表可以继续下去，同义词方法做完全相同的事情，甚至具有相同的语法，但是其中一个方法更常见或具有更短的名称，如`isnull()`和`isna()`，或`mul()`和`multiply()`。如果你知道其他类似的例子，或者其他奇怪的熊猫方法，欢迎在评论中分享你的想法。

感谢阅读！

如果你喜欢这篇文章，你也可以发现下面这些有趣的:

[](https://medium.com/mlearning-ai/11-cool-names-in-data-science-2b64ceb3b882) [## 数据科学中的 11 个酷名字

### 你可能不知道它们是什么意思

medium.com](https://medium.com/mlearning-ai/11-cool-names-in-data-science-2b64ceb3b882) [](/the-easiest-ways-to-perform-logical-operations-on-two-dictionaries-in-python-88c120fa0c8f) [## 在 Python 中对两个字典执行逻辑运算的最简单方法

towardsdatascience.com](/the-easiest-ways-to-perform-logical-operations-on-two-dictionaries-in-python-88c120fa0c8f) [](/testing-birthday-paradox-in-faker-library-python-54907d724414) [## 在 Faker 库中测试生日悖论(Python)

towardsdatascience.com](/testing-birthday-paradox-in-faker-library-python-54907d724414)