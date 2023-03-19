# 按列或行对 NumPy 数组排序

> 原文：<https://towardsdatascience.com/sort-numpy-arrays-by-columns-or-rows-18dfd2ef28c4?source=collection_archive---------26----------------------->

## 使用 NumPy 的内置函数来简化排序

![](img/77c2d0f6bd05884f85c009404d663c68.png)

[Alex Block](https://unsplash.com/@alexblock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/sort?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

NumPy 是许多 Python 程序和分析中使用的基本模块，因为它非常有效地进行数值计算。然而，对于 NumPy 的新手来说，一开始可能很难理解。具体来说，理解数组索引和排序很快变得复杂。幸运的是，NumPy 有一些内置函数，使得执行基本的排序操作非常简单。

NumPy 数组可以使用`argsort()`函数按单列、单行或多列或多行排序。`argsort`函数返回一个索引列表，该列表将对数组中的值进行升序排序。`argsort`函数的`kind`参数使得对多行或多列的数组进行排序成为可能。本文将使用简单的示例和代码，对单行和单行进行排序，对多列进行排序。

# 创建一个 NumPy 随机整数数组

首先，导入`numpy`模块。

现在创建一个随机整数数组。在这里，我创建了一个 5 行 4 列的数组。数组中的值是随机的，所以如果使用相同的代码，得到的值将与这里显示的不同。

```
a = np.random.randit(100, size=(5, 4)) output: 
[[44 47 64 67] 
[67 9 83 21] 
[36 87 70 88] 
[88 12 58 65] 
[39 87 46 88]]
```

# 按列对 NumPy 数组排序

为了按照特定的列对数组进行排序，我们将使用`numpy` `argsort()`函数。`argsort()`返回将导致排序数组的数组索引( [source](https://numpy.org/doc/stable/reference/generated/numpy.argsort.html) )。让我们快速看一下`argsort`返回什么，然后如何使用`argsort`获得一个排序后的数组。

首先，在我们创建的数组的第一列上调用`argsort`。您应该会得到类似这样的结果。其中`argsort`结果给出了从最小到最大值的索引。

```
a[:, 0].argsort() output: 
[2 4 0 1 3]
```

输出显示第一列中的最小值位于位置 2(第三个值)。这是正确的，数组第一列中的最小值是 36，这是第一列中的第三个值(位置 2)。

为了对数组进行排序，我们现在需要使用来自`argsort`结果的索引来对数组中的行进行重新排序。这是一个简单的过程，只用一行代码就可以完成。我们将简单地使用`argsort`结果作为行索引，并将结果数组赋回给`a`，如下所示。

```
a = a[a[:, 0].argsort()] output:
[[36 87 70 88] 
[39 87 46 88] 
[44 47 64 67] 
[67 9 83 21] 
[88 12 58 65]]
```

如您所见，现在根据第一列将行从最少到最多排序。要对不同的列进行排序，只需更改列索引。例如，我们可以用`a[a[:, 1].argsort()]`对第 2 列进行排序。

# 按行对 NumPy 数组排序

NumPy 数组也可以按行值排序。这与使用列进行排序的方式相同。我们只需要改变索引位置。例如，假设我们创建的数组在第一列进行排序，然后根据第一行中的值对列进行排序。

为此，只需将索引(0)移动到行位置，并将`argsort`结果移动到列位置。下面的代码显示了演示和结果。

```
a = a[:, a[0, :].argsort()] output: 
[[36 70 87 88] 
[39 46 87 88] 
[44 64 47 67] 
[67 83 9 21] 
[88 58 12 65]]
```

# 多列排序

有时需要对多列进行排序。这方面的一个例子是在单独的列中包含年、月、日和值的数据。NumPy 的`argsort`可以使用`kind`参数处理多个列的排序。

让我们首先创建一个包含 4 列的数组，分别代表年、月、日和值。

```
b = np.array([
[2020, 1, 1, 98], 
[2020, 2, 1, 99], 
[2021, 3, 6, 43], 
[2020, 2, 1, 54], 
[2021, 1, 1, 54], 
[2020, 1, 2, 74], 
[2021, 1, 3, 87], 
[2021, 3, 9, 23]])
```

现在我们将使用`argsort`对列进行排序，从最低优先级开始。这意味着如果我们想按年排序，然后按月排序，然后按日排序，我们需要先按日排序，然后按月排序，再按年排序。对于除第一个排序之外的所有排序，我们需要将`kind`指定为‘merge sort ’,这将保持之前的排序上下文。下面的代码块演示了这一点。

```
b = b[b[:, 2].argsort()] # sort by day 
b = b[b[:, 1].argsort(kind='mergesort')] # sort by month 
b = b[b[:, 0].argsort(kind='mergesort')] # sort by year output:
[[2020 1 1 98] 
[2020 1 2 74] 
[2020 2 1 99] 
[2020 2 1 54] 
[2021 1 1 54] 
[2021 1 3 87] 
[2021 3 6 43] 
[2021 3 9 23]]
```

如您所见，该过程成功地基于三列进行了排序。

# 结论

NumPy 是一个基本的 Python 包。因为它是用编译语言构建的，所以它可以大大提高 Python 程序在 Python 列表和元组上的速度(如果合适的话)。缺点是 NumPy 可能有一个陡峭的学习曲线。一旦你能够理解数组造型、索引和排序，你就已经是一个熟练的 NumPy 用户了。

*最初发表于*[*https://opensourceoptions.com*](https://opensourceoptions.com/blog/sort-numpy-arrays-by-columns-or-rows/)*。*