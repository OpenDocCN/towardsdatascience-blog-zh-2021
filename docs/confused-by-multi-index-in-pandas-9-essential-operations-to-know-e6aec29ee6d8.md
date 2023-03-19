# 熊猫的多指标困惑？需要了解的 9 项基本操作

> 原文：<https://towardsdatascience.com/confused-by-multi-index-in-pandas-9-essential-operations-to-know-e6aec29ee6d8?source=collection_archive---------11----------------------->

## 了解要领，不再有困惑

![](img/8743199b7a3a6a786800ac1db4a627bd.png)

Erik Mclean 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 介绍

在许多用例中，我们处理的是单级索引。在类似电子表格的表格中，最简单的情况是行号作为索引，列号作为列。你可能有一种误解，认为我永远不会处理多级索引。让我们考虑泰坦尼克号数据集。出于本教程的考虑，我们将只包括两个数字列:`age`和`fare`以及三个分类列:`sex`、`class`和`embark_town`。

泰坦尼克号数据集

我们数据处理中的一个常见操作是查看相关组的平均数据，如下所示。为了简化显示，我将`age`和`fare`的浮点数转换为整数。

分组平均数据

如您所见，这个数据集的索引看起来不像典型的单个级别。的确，这个`DataFrame`使用了分级索引，也就是俗称的多级索引。

你对多级索引了解多少？或者说，你有没有被多重索引迷惑过？让我们在本文中了解多级索引的基本方面。

## 1.什么是 MultiIndex？

我们提到过，单级索引使用一系列标签来唯一地标识每一行或每一列。与单级索引不同，**多级索引使用一系列元组，每个元组唯一标识一行或一列**。为了简化术语，我们只关注行的索引，但是同样的规则也适用于列。

多级索引

如上所示，我们可以访问一个`DataFrame`对象的 index 属性。您可能会注意到，我们将索引作为一个`MultiIndex`对象来获取，这是 pandas `DataFrame`或`Series`的多级或分层索引对象。这个对象有三个关键属性:`**names**`、`**levels**`和`**codes**`。我们来复习一下。

```
>>> df_mean.index.names
FrozenList(['embark_town', 'class', 'sex'])
>>> df_mean.index.levels
FrozenList([['Cherbourg', 'Queenstown', 'Southampton'], ['First', 'Second', 'Third'], ['female', 'male']])
>>> df_mean.index.codes
FrozenList([[0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2], [0, 0, 1, 1, 2, 2, 0, 0, 1, 1, 2, 2, 0, 0, 1, 1, 2, 2], [0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1]])
```

*   **名称**:我们的指数对每一个指数级别都有三个名称:`embark_town`、`class`和`sex`。
*   **级别**:每个级别的标签。`embark_town`有三个标签，`class`有三个标签，`sex`有两个标签。
*   **代码**:表示每一级指标的整数。例如，`FrozenList`中的第一个列表指示所有行的'`embark_town`级别(0 - >'瑟堡'，1 - >'昆斯敦，2 - >'南安普顿)。

## 2.将多索引转换为常规索引

在示例`DataFrame`(即`df_mean`)中，作为使用`groupby`功能的结果，自动创建多索引。如果我们从一个没有多索引的数据帧开始，但是一些列可以创建为多索引，那会怎么样呢？考虑下面的`DataFrame`。

```
>>> df = df_mean.reset_index()
>>> df.head()
  embark_town   class     sex  age  fare
0   Cherbourg   First  female   36   115
1   Cherbourg   First    male   40    93
2   Cherbourg  Second  female   19    25
3   Cherbourg  Second    male   25    25
4   Cherbourg   Third  female   14    14
```

我们简单地重置了索引，创建了一个使用单级索引的`DataFrame`，这种`DataFrame`您可能更熟悉，对吧。需要注意的一点是，`reset_index`方法带有一个参数`drop`，该参数决定索引是被删除还是作为列保留。在我们的例子中，我们对`drop`使用默认的`False`，它会将多级索引转换为三列，这是我们想要的操作。

## 3.重新创建多重索引

为了为这个`DataFrame`创建多级索引，我们可以使用`set_index`方法将适用的列指定为索引，如下所示。

```
>>> df_mi = df.set_index(['embark_town', 'class', 'sex'])
>>> df_mi.head()
                           age  fare
embark_town class  sex              
Cherbourg   First  female   36   115
                   male     40    93
            Second female   19    25
                   male     25    25
            Third  female   14    14
```

如您所见，`df_mi`与`df_mean` `DataFrame`具有相同的多级索引。

除了用现有的列设置索引之外，我们还可以手动创建`MultiIndex`对象，如果愿意的话，将它分配给一个`DataFrame`。如下所示，我们能够重新创建一个与我们在`df_mean`中使用的对象相匹配的`MultiIndex`对象。`from_product`方法使用提供的列表顺序创建所有可能组合的产品。

手动创建多索引

除了 from_product 方法之外，还有几种方法经常用来创建`MultiIndex`对象，比如`[from_tuples](https://pandas.pydata.org/docs/reference/api/pandas.MultiIndex.from_tuples.html#pandas.MultiIndex.from_tuples)`、`[from_arrays](https://pandas.pydata.org/docs/reference/api/pandas.MultiIndex.from_arrays.html#pandas.MultiIndex.from_arrays)`和`[from_frame](https://pandas.pydata.org/docs/reference/api/pandas.MultiIndex.from_frame.html#pandas.MultiIndex.from_frame)`。用法应该很简单，感兴趣的读者可以参考各自的参考资料，了解如何使用它们(点击这些方法上的链接)。

## **4。选择特定级别**

选择数据最直接的方法是使用带有`loc`属性的基于元组的索引。如前所述，多级索引本质上是一个元组列表。因此，我们可以为想要检索的行指定所需的元组。下面是一些例子。

```
>>> df_mean.loc[('Queenstown',)]
               age  fare
class  sex              
First  female   33    90
       male     44    90
Second female   30    12
       male     57    12
Third  female   22    10
       male     28    11
>>> df_mean.loc[('Southampton', 'Second')]
        age  fare
sex              
female   29    21
male     30    19
```

*   您可以跳过元组中的前 n 个级别，这将检索更低级别的所有元素。例如，`(‘Queenstown’,)`将选择该级别的所有行，而`(‘Southampton’, ‘Second’)`将为`embark_town`选择级别为`‘Southampton’`的行，为`class`选择级别为`‘Second’`的行。
*   检索到的数据没有指定的级别，因为所有数据都满足指定的级别。
*   示例中没有显示的是，您可以省略 tuple 外延，这可以作为这些操作的快捷方式。例如，代替`df_mean.loc[(‘Queenstown’,)]`和`df_mean.loc[(‘Southampton’, ‘Second’)]`，你可以分别做`df_mean.loc[‘Queenstown’]`和`df_mean.loc[‘Southampton’, ‘Second’]`。但是，通常不建议这样做，因为这会导致混乱。因此，我通常会列出所有适用的信息，这样就不会有歧义。
*   另一件要注意的事情是，我省略了列的`:`选择器，因为我确实想选择年龄和费用列。如果您只选择列的子集，请指定。

## 5.使用 xs 选择横截面

除了使用`loc`方法，您还可以使用`DataFrame`的`xs`方法，该方法检索横截面数据行。要检索特定级别，只需指定索引。下面是一些例子。

```
>>> df_mean.xs('Queenstown')
               age  fare
class  sex              
First  female   33    90
       male     44    90
Second female   30    12
       male     57    12
Third  female   22    10
       male     28    11
>>> df_mean.xs(('Southampton', 'First', 'female'))
age     32
fare    99
Name: (Southampton, First, female), dtype: int64
```

您可以为`xs`设置的另一个有用的参数是`level`，它指的是在多索引中使用的级别。例如，要检索`DataFrame`中的所有第一类，我们可以执行以下操作，选择横截面数据。

```
>>> df_mean.xs('First', level='class')
                    age  fare
embark_town sex              
Cherbourg   female   36   115
            male     40    93
Queenstown  female   33    90
            male     44    90
Southampton female   32    99
            male     41    52
```

## 6.同时选择多个级别

当您想要选择具有不同级别的行时，可以在 list 对象中指定所需数据的索引。例如，假设我们要在第三名皇后镇(女性)和第一名南安普敦(男性)之间选择数据。下面是如何通过指定范围。请注意，该范围包括边界，就像您对单级索引使用`loc`一样。

```
>>> df_mean.loc[('Queenstown', 'Third', 'female'):('Southampton', 'First', 'male')]
                          age  fare
embark_town class sex              
Queenstown  Third female   22    10
                  male     28    11
Southampton First female   32    99
                  male     41    52
```

如果希望选择不连续的行，可以传递元组列表，这些元组是行的索引。考虑下面的例子，它只选择了两行数据。

```
>>> df_mean.loc[[('Queenstown', 'Third', 'female'), ('Southampton', 'First', 'male')]]
                          age  fare
embark_town class sex              
Queenstown  Third female   22    10
Southampton First male     41    52
```

与上面的例子密切相关的是，我们也可以通过传递一组列表来选择数据，但是它们做事情的方式不同。如下所示，列表元组将产生多级索引，每个列表引用每个级别的多个标签。

```
>>> df_mean.loc[(['Queenstown', 'Southampton'], ['First', 'Second'], ['female', 'male'])]
                           age  fare
embark_town class  sex              
Queenstown  First  female   33    90
                   male     44    90
            Second female   30    12
                   male     57    12
Southampton First  female   32    99
                   male     41    52
            Second female   29    21
                   male     30    19
```

## 7.使用切片的高级选择

当我们使用列表元组时，多级索引是从这些列表中创建的。但是，当某个级别有多个标签时，您可能希望使用 slice 对象来表示一系列标签，而不是逐一列出它们。考虑下面的例子。

```
>>> # Instead of df_mean.loc[(['Cherbourg', 'Queenstown', 'Southampton'], 'Second', ['female', 'male'])]
>>> df_mean.loc[(slice('Cherbourg', 'Southampton'), 'Second', slice(None))]
                           age  fare
embark_town class  sex              
Cherbourg   Second female   19    25
                   male     25    25
Queenstown  Second female   30    12
                   male     57    12
Southampton Second female   29    21
                   male     30    19
```

*   `slice(start, end)`定义范围内的所有指数。
*   因为我们只有三个标签用于`embark_town`级别，而不是使用`slice(‘Cherbourg’, ‘Southampton’)`，我们可以使用`slice(None)`，这被自动解释为引用所有标签。
*   除了使用`slice`函数，pandas 还提供了一种更好的方法来引用多索引——`IndexSlice`,这有点像 NumPy 中使用的引用。下面是一个例子。在本例中，`:`表示该级别的所有标签。

```
>>> df_mean.loc[pd.IndexSlice[:, 'First', :]]
                          age  fare
embark_town class sex              
Cherbourg   First female   36   115
                  male     40    93
Queenstown  First female   33    90
                  male     44    90
Southampton First female   32    99
                  male     41    52
```

## 8.重组级别

到目前为止，数据帧已经有了原始顺序的多级索引。然而，我们可能希望索引以不同的顺序排列。为此，我们可以使用`reorder_levels`方法。例如，期望的顺序是`class-sex-embark_town`，而不是`embark_town-class-sex`。这种操作如下所示。

```
>>> df_mean.reorder_levels(['class', 'sex', 'embark_town']).head()
                           age  fare
class  sex    embark_town           
First  female Cherbourg     36   115
       male   Cherbourg     40    93
Second female Cherbourg     19    25
       male   Cherbourg     25    25
Third  female Cherbourg     14    14
```

除了指定级别的名称，还可以通过指定级别的数值来支持。因此，上述操作相当于`df_mean.reorder_levels([1, 2, 0])`。

如果你想切换两个级别的顺序，另一种方法是使用`swaplevel`方法。顾名思义，该方法直接在两个级别之间翻转顺序。

```
>>> df_mean.swaplevel(0, 1).head()
                           age  fare
class  embark_town sex              
First  Cherbourg   female   36   115
                   male     40    93
Second Cherbourg   female   19    25
                   male     25    25
Third  Cherbourg   female   14    14
```

## 9.排序多索引

多级索引重组后，`DataFrame`看起来不再有组织，因为顺序还是用原来的那个，是根据`embark_town-class-sex`的索引排序的。为了根据所需的级别聚合数据，我们可以对索引进行排序，以便更好地组织数据。

```
>>> df_mean.swaplevel(0, 1).sort_index().head()
                          age  fare
class embark_town sex              
First Cherbourg   female   36   115
                  male     40    93
      Queenstown  female   33    90
                  male     44    90
      Southampton female   32    99
```

在示例中，我们简单地使用了`sort_index`方法，默认情况下，该方法使用第一级对数据进行排序。

但是，如果您喜欢使用不同的级别对数据进行排序，我们可以指定级别的数值或名称，如下所示。

```
>>> df_mean.swaplevel(0, 1).sort_index(level='sex').head()
                           age  fare
class  embark_town sex              
First  Cherbourg   female   36   115
       Queenstown  female   33    90
       Southampton female   32    99
Second Cherbourg   female   19    25
       Queenstown  female   30    12
```

## 结论

在本文中，我们回顾了您可能需要用来处理多级索引的基本操作。我知道对于许多初学者来说这是一个令人困惑的话题，但是如果你掌握了要点，当多级索引相关时，你应该能够处理大多数日常工作。

感谢阅读这篇文章。通过[注册我的简讯](https://medium.com/subscribe/@yong.cui01)保持联系。还不是中等会员？使用我的会员链接，通过[支持我的写作。](https://medium.com/@yong.cui01/membership)