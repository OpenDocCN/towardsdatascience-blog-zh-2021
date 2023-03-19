# 如何将 NumPy 数组写入 CSV 文件

> 原文：<https://towardsdatascience.com/how-to-write-numpy-arrays-to-csv-files-449a217ae7ee?source=collection_archive---------9----------------------->

## 以及为什么您应该考虑其他文件格式

![](img/c94cef301b67ddabdba16b92afe0bbe8.png)

图片由英驰通过 [Unsplash](http://unsplash.com) 拍摄

这篇文章解释了**如何将 NumPy 数组写入 CSV 文件**。

我们将看看:

*   将不同的 NumPy 数组写入 CSV 的语法
*   将 NumPy 数组写入 CSV 的限制
*   保存 NumPy 数组的替代方法

我们开始吧。

# 将 NumPy 数组写入 CSV

您可以使用`np.savetxt()`方法将 Numpy 数组保存到一个 CSV 文件中。

确保:

*   添加”。csv”到文件名目的地，并且
*   将 delimiter 关键字设置为“，”

如果你不使用这两个设置，NumPy 将会把你的文件保存为. txt。

CSV 文件可以很棒，因为它们是人类可读的。它们还有一个额外的好处，就是很容易加载到 pandas 或 Dask 数据帧中。

# 写一维数组

让我们使用`np.random.rand()`创建一个包含随机数的一维数组。

```
import numpy as np 
# create 1D array 
a = np.array([1,2,3]) # store in current directory 
np.savetxt( "a.csv", a, delimiter="," )
```

默认情况下，NumPy 将按列写入数组。让我们检查一下`a.csv`的内容以确认:

```
1
2
3
```

改为按行写入数据，将`newline` kwarg 设置为“，”(您的分隔符)。

```
# write array row-wise 
np.savetxt("very.csv", a, delimiter=",", newline=",")
```

您也可以通过先将数组转换为 2D 数组来按行写入数组

```
np.savetxt("very.csv", [a], delimiter="," )
```

在这两种情况下，`very.csv`的内容会是这样的:

```
1   2   3
```

# 写二维数组

现在让我们创建一个二维 NumPy 数组并保存到 CSV。

```
# create 2D array 
b = np.array([1, 2, 3], [4, 5, 6]) # write 2D array to CSV 
np.savetxt( "merry.csv", b, delimiter="," )
```

正如您所料，默认情况下，2D 数组是按行写入的。

`merry.csv`的内容:

```
1   2   3
4   5   6
```

# 写三维数组

最后，让我们创建一个三维 NumPy 数组，并尝试将其保存为 CSV 格式。

```
# create 3D array 
c = np.random.rand(3,3,3) # write 3D array to CSV 
np.savetxt( "christmas.csv", c, delimiter="," )
```

这不管用。您将看到如下错误消息:

```
--------------------------------------------------------------------ValueError Traceback (most recent call last) /var/folders/ky/bqjn_gxn1xv0cn_8q5xvp3q40000gn/T/ipykernel_46872/1804416501.py in <module> ----> 1 np.savetxt(f"{home}/Documents/numpy/bar.csv", c, delimiter=",") <__array_function__ internals> in savetxt(*args, **kwargs) ~/mambaforge/envs/numpy-zarr/lib/python3.9/site-packages/numpy/lib/npyio.py in savetxt(fname, X, fmt, delimiter, newline, header, footer, comments, encoding) 1380 # Handle 1-dimensional arrays 1381 if X.ndim == 0 or X.ndim > 2: -> 1382 raise ValueError( 1383 "Expected 1D or 2D array, got %dD array instead" % X.ndim) 1384 elif X.ndim == 1: ValueError: Expected 1D or 2D array, got 3D array instead
```

CSV 是一种可读的表格格式。这意味着只有 1D 和 2D NumPy 阵列可以写入 CSV。

# 用 np.save()保存 Numpy 数组

在磁盘上存储 NumPy 数组的另一种方法是使用本机的`np.save()`方法。这将以二进制文件格式存储您的数组。

这种格式允许您保存所有维度的 NumPy 数组。这意味着这些文件将不是人类可读的。

```
# save 3D array to binary NPY format 
np.save('christmas.npy', c)
```

让我们看看在 CSV 和 NPY 中存储的文件大小是否有区别。

```
# create medium-sized 2D array 
d = np.random.rand(100,100) # save 2D array to CSV format 
np.savetxt( f"time.csv", d, delimiter="," ) # get the size (in bytes) of the stored .npy file 
! stat -f '%z' time.csv
```

>>> 250000

```
# save 2D array to binary NPY format 
np.save('time.npy', d) # get the size (in bytes) of the stored .npy file 
! stat -f '%z' time.npy
```

>>> 80128

如您所见，与 250KB 的 CSV 相比，NPY 文件格式输出的文件更小:大约 80KB。

# 保存 NumPy 数组的其他方法

还有其他存储 NumPy 数组的方法。[这里有一篇很棒的博文](https://mungingdata.com/numpy/save-numpy-text-txt/)向你展示了如何将 NumPy 数组写入 TXT 文件。

从技术上讲，您也可以使用`np.ndarray.tofile()`方法，但是这将把数组编码成依赖于平台的二进制格式，所以通常不推荐使用。

# NumPy 阵列的并行读/写

如果你处理的是小的本地数据，上面提到的格式就可以了。

但许多真实世界的数据集与小型和本地数据集相反:它们非常大(通常比本地内存大)并且基于云。这意味着您需要并行读写 NumPy 数组。

NPY 文件格式不允许并行读取和写入。

# 改为将数组写入 Zarr

如果您需要并行读/写，那么将 NumPy 数组写成 Zarr 文件格式是一个不错的选择。Zarr 是一种存储任何维度的分块和压缩数组的格式。这意味着你可以通过一次处理多个块来并行读写你的数组**；通过使用类似 [Dask](https://dask.org/) 的并行处理库。**

**与前面提到的所有其他文件格式相比，这有两个重要的好处:**

1.  **您可以更快地读/写数组**
2.  **您可以读取/写入超出本地计算机内存的数组**

# **结论**

**使用`np.savetxt()`方法- **可以将 NumPy 数组写入 CSV，但前提是**你的数组只有 2 维或更少。**

**二进制 NPY 格式比 CSV 更节省内存，但它不是人类可读的，也不能并行处理。**

**如果数据集非常大，并行编写 NumPy 数组会快得多，并且可以避免内存错误。使用 Zarr 文件格式并行读写 NumPy 数组。**

**在 Twitter 上关注我，获取更多类似的内容。**

***原载于 2021 年 12 月 25 日 http://crunchcrunchhuman.com*[](https://crunchcrunchhuman.com/2021/12/25/numpy-save-csv-write/)**。****