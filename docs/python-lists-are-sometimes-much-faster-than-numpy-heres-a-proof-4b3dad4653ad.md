# Python 列表有时比 NumPy 快得多。这是证据。

> 原文：<https://towardsdatascience.com/python-lists-are-sometimes-much-faster-than-numpy-heres-a-proof-4b3dad4653ad?source=collection_archive---------4----------------------->

## 小心使用什么。

![](img/2241cc280fda661fc5058992c8d2f489.png)

布拉登·科拉姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

我最近在做一个数字图像处理项目。超参数调整花了相当长的时间，我才得到想要的精度。都是因为过度拟合的寄生虫和我没用的低端硬件。

对于每次执行，我的机器花费大约 15-20 分钟。20 分钟处理 20 000 个条目。我想象如果我一直在处理一个 100 万的记录数据集，我将不得不在训练结束之前等待地球完成一次完整的旋转。

我对模型的准确性感到满意。然而，在提交我的代码之前，我想尝试许多其他的卷积神经网络(CNN)架构。因此，我决定在我的代码中寻找优化空间。

因为我使用的是 PyPi 中预先构建的机器学习算法——Scikit-Learn 和 tensor flow——只剩下很少的子例程需要优化。一个选择是在数据结构方面提升我的代码。我将数据存储在列表中，由于 NumPy 非常快，我认为使用它可能是一个可行的选择。

猜猜我的列表代码转换成 NumPy 数组代码后发生了什么？

令我惊讶的是，执行时间没有缩短。相反，它飙升。

也就是说，在这篇文章中，我将带您了解列表最终比 NumPy 数组表现更好的确切情况。

# 数字和列表

首先，让我们讨论 NumPy 数组和列表之间的区别。

[NumPy](https://numpy.org/) 是用于 N 维数组操作和计算的事实上的 Python 库。它是开源的，易于使用，内存友好，速度快如闪电。

NumPy 最初被称为“Numeric”，它为许多数据科学库(如 SciPy、Scikit-Learn、Panda 等)设置了框架。

Python 列表存储有序的、可变的数据对象的集合，而 NumPy 数组只存储单一类型的对象。因此，我们可以说 NumPy 数组生活在列表的保护伞下。因此，没有 NumPy 数组做不到的事情。

但是，说到 NumPy 整体。Numpy 不仅包括数组操作，还包括许多其他例程，如二元运算、线性代数、数学函数等等。我相信它涵盖了超过一个人可能需要的。

接下来要考虑的是为什么我们通常使用 NumPy 数组而不是列表。

简而言之，我相信每个阅读这篇文章的人都知道:它更快。

NumPy 确实快得离谱，尽管 Python 速度慢是众所周知的。这是因为 NumPy 是 C 和 Fortran 的包装器。而且不用说这两个有多快。

# NumPy 数组比列表快

在我们讨论 NumPy 数组变得像蜗牛一样慢的情况之前，有必要验证 NumPy 数组通常比列表更快的假设。

为此，我们将使用 NumPy 和 lists 来计算一百万个元素数组的平均值。该数组是随机生成的。

以下代码是一个示例:

```
"""General comparison between NumPy and lists"""import numpy as np
from time import time#Random numpy array
numpy_array = np.random.rand(1000000)
list_conv = list(numpy_array)#Start timing NumPy compuation
start1 = time()
#Compute the mean using NumPy
numpy_mean = np.mean(numpy_array)
print(f"Computing the mean using NumPy: {numpy_mean}")
#End timing
end1 = time()
#Time taken
time1 = end1 - start1
print(f"Computation time: {time1}")#Start timing list computation
start2 = time()
#Compute the mean using lists
list_mean = np.mean(list_conv)
print(f"Computing the mean using lists: {list_mean}")
#End timing
end2 = time()
#Time taken
time2 = end2 - start2
print(f"Computation time: {time2}")#Check results are equal
assert abs(numpy_mean - list_mean) <= 10e-6, "Alert, means are not equal"
```

我的机器输出如下:

```
Computing the mean using NumPy: 0.4996098756973947
Computation time: 0.01397562026977539
Computing the mean using lists: 0.4996098756973947
Computation time: 0.17974257469177246
```

正如预测的那样，我们可以看到 NumPy 数组明显比列表快。相当大的速度差异是显而易见的。

也就是说，我们能概括地说 NumPy 数组总是比列表快吗？

事实证明 NumPy 数组并不总是超过列表。列表也有锦囊妙计，这就把我们带到了下一个环节。

# NumPy 数组并不总是比列表快

如果列表与 NumPy 数组相比毫无用处，它们可能已经被 Python 社区抛弃了。

与 NumPy 数组相比，列表更出色的一个例子是`append()`函数。"`append()`"将值添加到列表和 NumPy 数组的末尾。这是一个常见且经常使用的功能。

下面的脚本演示了列表的`append()`和 NumPy 的`append()`之间的比较。这段代码只是将从 0 到 99 999 的数字添加到一个列表和一个 NumPy 数组的末尾。

```
"""numpy.append() vs list.append()"""
import numpy as np
from time import timedef numpy_append():
    arr = np.empty((1, 0), int)
    for i in range(100000):
        arr = np.append(arr, np.array(i))
    return arrdef list_append():
    list_1 = []
    for i in range(100000):
        list_1.append(i)
    return list_1def main ():
    #Start timing numpy array
    start1 = time()
    new_np_arr = numpy_append()
    #End timing
    end1 = time()
    #Time taken
    print(f"Computation time of the numpy array : {end1 - start1}") #Start timing numpy array
    start2 = time()
    new_list = list_append()
    #End timing
    end2 = time()
    #Time taken
    print(f"Computation time of the list: {end2 - start2}") #Testing
    assert list(new_np_arr) == new_list, "Arrays tested are not the same"if __name__ == "__main__":
    main()
```

我的机器产生以下输出:

```
Computation time of the numpy array : 2.779465675354004
Computation time of the list: 0.010703325271606445
```

正如我们所看到的，在这个例子中，列表比 NumPy 数组表现得更好。Numpy 表现差到被超过 2000 %的地步。

这个案例表明，无论何时涉及到速度，NumPy 都不应该被认为是“总是去”的选项。相反，需要仔细考虑。

# Numpy.append 有什么问题？

为了解开这个谜，我们将访问 NumPy 的源代码。`append()`函数的 [docstring](https://github.com/numpy/numpy/blob/v1.20.0/numpy/lib/function_base.py#L4690-L4745) 告知如下内容:

```
"Append values to the end of an array. Parameters
    ----------
    arr : array_like
        Values are appended to a copy of this array.
    values : array_like
        These values are appended to a copy of `arr`.  It must be of 
        the correct shape (the same shape as `arr`, excluding
        `axis`). If `axis` is not specified, `values` can be any 
        shape and will be flattened before use.
    axis : int, optional
        The axis along which `values` are appended.  If `axis` is 
        not given, both `arr` and `values` are flattened before use. Returns
    -------
    append : ndarray
        A copy of `arr` with `values` appended to `axis`.  Note that
        `append` does not occur in-place: a new array is allocated
        and filled.  If `axis` is None, `out` is a flattened array."
```

在彻底阅读了 docstring 之后，我们可以看到关于函数返回内容的“注释”。它声明追加过程不发生在同一个数组中。而是创建并填充一个新数组。

然而，在列表中，事情是非常不同的。列表填充过程停留在列表本身中，不会生成新的列表。

总之，我们可以看到`numpy.append()`的复制-填充过程使它成为一个开销。

# 外卖食品

编程没有万金油。

我们的发现证明了 NumPy 阵列并不能解决所有性能问题。在考虑所有选择之前，不应该盲目地采取行动。这样做也最大化了产生设计更好的代码的机会。

此外，通过实验探索多种途径可以确保人们最终不会后悔做出了不同的选择。更重要的是，实验会发现错误信息。

最后，作为对我坚持这一观点的奖励，这里有一段 A. Einstein 深刻的引用，总结了这篇文章的观点。

> “再多的实验也无法证明我是对的；一个简单的实验就能证明我是错的。”——阿尔伯特·爱因斯坦。

如果你觉得这很有见地，可以考虑成为 [*高级*](https://ayarmohammed96.medium.com/membership) *会员，每月 5 美元。如果用这个* [*链接*](https://ayarmohammed96.medium.com/membership) *，我会得到一个小切。*

<https://ayarmohammed96.medium.com/membership>  

*享受您的编程日！*