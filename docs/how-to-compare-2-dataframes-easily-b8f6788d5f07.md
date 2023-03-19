# 如何轻松比较两个数据帧

> 原文：<https://towardsdatascience.com/how-to-compare-2-dataframes-easily-b8f6788d5f07?source=collection_archive---------18----------------------->

## …使用全面的数据协调脚本。

![](img/65ae2b00e9eae9d002447412fb72abd2.png)

由 [Abel Y Costa](https://unsplash.com/@abelycosta?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/table?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

4 年前，我参与了一个数据迁移项目，是一家全球金融机构业务部门的一员。业务部门需要在 4 小时内完成对账，并为项目上线提供签准。在这个项目之前，我已经开始涉猎 python，当时只听说过`pandas`库。

这项任务听起来很简单:

> 确保所有记录都正确迁移。

但是和所有任务一样，通常有一个`if`语句和一个跟随其后的子句。

> 如果有任何记录不匹配，则生成这些记录的报告。

业务部门提出的最初想法是使用 Excel 和宏来执行这种调节。但是我理解这种复杂性，并认为 Excel 可能不适合这样的任务。经过一些努力，我让各种利益相关者相信我可以足够快地掌握 python 来完成这项任务。

快进 2 个月，经过几次彩排，我的剧本终于准备好了。在源数据和目标数据之间有一些转换逻辑，但是为了本文的目的，我将展示脚本如何在经过**转换的源数据**和**目标数据**之间进行比较。

点击[此处](https://gist.github.com/jiweiliew/ad07bf33cffd3cfc7d1ae7f74d783c63.js)直接跳转到脚本。

## 一些假设

设`df1`和`df2`分别为*源*和*目标*数据帧。现在，让我们假设它们具有相同数量的列，并且这些列成对地具有相同的数据类型(即`df1`中的列 1 与`df2`中的列 1 具有相同的数据类型，…)。还假设进行比较的列存储在`uid`变量中。

在文章的最后，我将简要讨论在执行主要的比较脚本之前，为确保数据足够干净而需要采取的额外步骤/检查。

## 1.记录迁移正确

对所有列执行内部合并，得到的数据帧将是所有记录都匹配的数据帧。注意，`uid`无关紧要，因为合并是在所有列上完成的。

`df_merge = pd.merge(df1, df2, how='inner', on=df1.columns.tolist())`

## 2.记录在源中，但不在目标中

经常会有一些不确定的记录永远不会被迁移，也就是说，它们在源中，但不在目标中。

有几种方法可以识别这些记录，这里有一种:

*   将`df_merge`追加回`df1`，
*   标记重复的行；使用`keep=False`将所有重复标记为`True`，
*   选择不重复的行。

```
df1_only = df1.append(df_merge).reset_index(drop=True)
df1_only['Duplicated'] = df1_only.duplicated(keep=False)
df1_only = df1_only[~df1_only['Duplicated']]
```

## **3。记录在目标中，但不在源中**

可能有记录*在迁移过程中神秘地*创建，可能是由于一些转换错误或人工干预。

以类似的精神，人们可以识别出这些在目标中而不在源中。

```
df2_only = df2.append(df_merge).reset_index(drop=True)
df2_only['Duplicated'] = df2_only.duplicated(keep=False)
df2_only = df2_only[~df2_only['Duplicated']]
```

## 4.源和目标中的记录，但具有不同的值

现在假设有 2 个数据帧，每个数据帧有一条记录:

```
df1 = pd.DataFrame([['Apple',1]], columns=['Fruit', 'Qty'])
df2 = pd.DataFrame([['Apple',2]], columns=['Fruit', 'Qty'])
```

通过观察，`df_merge`将是空的，并且这些数据帧也将分别等同于`df1_only`和`df2_only`。

创建一个新的数据帧来显示`Apple`同时出现在`df1`和`df2`中是有意义的，但是它们在`Qty`列中有不同的值。

```
# Label each dataframe
df1_only['S/T'] = 'Source'
df2_only['S/T'] = 'Target'# Identify the duplicated rows
df3 = df1_only.append(df2_only).reset_index(drop=True)
df3['Duplicated'] = df3.duplicated(subset='Fruit', keep=False)# `subset` argument considers only a subset of the columns when identifying duplicated rows.
```

将此应用于一个不太重要的例子:

```
>>> cols = ['Fruit', 'Qty']
>>> df1 = pd.DataFrame([['Apple',1],['Orange',3]], columns=cols)
>>> df2 = pd.DataFrame([['Apple',2],['Pear',4]], columns=cols)
...
... 
>>> # Note that `df1` is still the same as `df1_only`
>>> # Repeat the step in the above code block to get `df3`
>>> df3
    Fruit  Qty      S/T  Duplicated
0   Apple    1   Source        True
1  Orange    3   Source       False
2   Apple    2   Target        True
3    Pear    4   Target       False
```

现在，可以使用`Duplicated`栏上的*布尔索引*来选择:

1.  在源和目标中具有相同的`Fruit`值但具有不同的`Qty`的记录
2.  严格按照来源的记录
3.  严格位于目标中的记录

```
df_diff = df3[df3['Duplicated']]
df1_only = df3[(~df3['Duplicated']) &(df3['S/T']=='Source')]
df2_only = df3[(~df3['Duplicated']) &(df3['S/T']=='Target')]
```

## 5.附加步骤/检查

该脚本进行额外的检查，以确保下游代码成功；它检查:

1.  两个数据帧都有相同数量的列
2.  两个数据帧具有相同的列名集*否则打印识别的`orphans`。*
3.  两个数据帧的列处于*相同的顺序*(这对于比较并不重要，但是对于一个人来说，检查两个列混杂在一起的数据帧在视觉上是困难的。)
4.  两个数据帧中每一列的数据类型都是成对相同的。(如果源列具有`int`数据类型，而对应的目标列具有`float`数据类型，这可能意味着存在具有空值的行。)
5.  根据应用比较的列检查记录的唯一性。(这应该总是 100%，否则应该在`uid`中包含更多的列来进行比较。)

## 6.使用脚本

使用该脚本非常简单，只需传递 3 个位置参数:

*   `df_left`和`df_right`是指你输入的数据帧
*   `uid`指组成唯一键的单个列(str)或一列列。(即`Fruits`)

也可以传递两个可选的关键字参数:

*   `labels`(默认= ( `Left`，`Right`))允许您将输入的数据帧命名为`Source` & `Target`。标签构成了输出有序字典的关键字。
*   `drop`(默认值= [[]，[]])可以接受要从比较中排除的列的列表。

```
def diff_func(df_left, df_right, uid, labels=('Left','Right'), drop=[[],[]])
   ...
   ...
```

基于我们迄今为止的例子:

```
>>> df1 = pd.DataFrame([['Apple',1],['Orange',3],['Banana',5]], columns=['Fruit','Qty'])
>>> df2 = pd.DataFrame([['Apple',2],['Pear',4],['Banana',5]], columns=['Fruit','Qty'])
>>> d1 = diff_func(df1, df2, 'Fruit')
```

输出`d1`将是具有 6 个数据帧的有序字典:

```
>>> d1['Left']
    Fruit  Qty
0   Apple    1
1  Orange    3
2  Banana    5
>>> d1['Right']
    Fruit  Qty
0   Apple    2
1    Pear    4
2  Banana    5
>>> d1['Merge']
    Fruit  Qty
0  Banana    5
>>> d1['Left_only']
    Fruit  Qty
1  Orange    3
>>> d1['Right_only']
  Fruit  Qty
3  Pear    4
>>> d1['Diff']
  Left or Right  Fruit  Qty
0          Left  Apple    1
1         Right  Apple    2
```

# 结论

虽然创建这样一个脚本的最初目的仅仅是为了协调一组特定的迁移数据，但是我认识到，如果可以将其一般化，它可以用于比较任意两个数据帧。因此，我决定编写一个通用函数，它按顺序进行一系列检查，然后比较数据帧，最后在包含 6 个数据帧的字典中输出结果。

> 这不是在 stackoverflow 上吗？

有趣的是，虽然 stackoverflow 上有许多比较两个数据框架的答案，但没有一个答案是我真正想要的。它很少是完整的，也没有我正在寻找的检查。随后，我在 stackoverflow 上发布了[这个](https://stackoverflow.com/a/51683883/8350440)答案——这也是我的第一个答案——到目前为止收到了 3 次投票。

> Pandas merge 有一个关键字参数“indicator=True ”,它指示记录是存在于左侧、右侧还是两个表中。

`indicator=True`指示记录是存在于两个表中还是只存在于其中一个表中。然而，当替换表不包含比较的键时，具有整数值的列被转换成`float`，这里有一个例子。

```
>>> import pandas as pd
>>> df1 = pd.DataFrame([['Apple',1],['Orange',3],['Banana',5]], columns=['Fruit','Qty'])
>>> df2 = pd.DataFrame([['Apple',2],['Pear',4],['Banana',5]], columns=['Fruit','Qty'])
>>> df3 = pd.merge(df1, df2, on='Fruit', how='outer', indicator=True)
>>> df3
    Fruit  Qty_x  Qty_y      _merge
0   Apple    1.0    2.0        both
1  Orange    3.0    NaN   left_only
2  Banana    5.0    5.0        both
3    Pear    NaN    4.0  right_only
```

在`df3`中，我们可以很容易地看到`Orange`只存在于左边的表格中。然而，`Qty_x`列不再是一个具有`int`数据类型的序列对象，而是一个`float64`数据类型。这可能会造成混乱，因此我决定不使用这种方法。

> 既然已经对词典进行了排序，为什么还要使用 OrderedDict？

这个脚本最初是在 python 字典还是无序的时候编写的，因此我使用了有序字典。我怀疑如果我切换到一个普通的字典，性能会有显著的提高，因此我决定保留它，为了怀旧，并确保它对那些使用旧版本 python 的人是兼容的。

完整代码如下: