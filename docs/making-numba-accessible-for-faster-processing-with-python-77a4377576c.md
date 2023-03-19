# 使 Numba 可访问，以便用 Python 进行更快的处理

> 原文：<https://towardsdatascience.com/making-numba-accessible-for-faster-processing-with-python-77a4377576c?source=collection_archive---------28----------------------->

## 克服 Numba 的局限性，并以明智的方式使用它

![](img/153d52c7d074c67f2812bbbc3a8d6c32.png)

克里斯里德在 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

Numba 是一个针对 Python 的实时编译器，可以通过计算密集型的计算和函数(如 NumPy 数组上的循环)来加速代码。

Numba 编译的算法可以使 Python 代码的运行时间加快一百万倍，从而可以达到 c 的速度。此外，随着操作数量的增加，计算时间通常比 Cython 快得多，cyt hon 是另一种用于更快处理的编译器。这里有一篇参考文章，展示了 Python、Cython 和 Numba 处理之间的精确比较。使用 Numba 也非常容易，只需将 Numba decorators (@njit)应用于 Python 函数。

## Numba 的局限性及其克服方法

但是，Numba 有一定的局限性。虽然它在 NumPy 数组上工作得最好，但它可能无法被 pandas 对象或 list 参数列表、一些 NumPy 函数(如 numpy.concatenate 或 numpy.diff)访问，或者更重要的是，无法被一些流行的 python 库(如 Scipy)访问。在这种情况下，使用 Numba 和 Cython/Python 函数以智能的方式重构代码可以大大提高处理速度。

在本文中，我将向您展示一个使用 Numba 和 Cython/Python 进行重构的示例，该示例旨在借助 Scipy 函数从任何 1D 信号中生成峰值屏蔽。在这里，我使用质谱数据作为示例数据集，它显示了肽的强度与质量/电荷的关系。如果您想对这些数据有更深入的了解，参考文章(张等，2009)可能会对您有所帮助。使用 Cython/Python 函数通过 Scipy 库进行峰值检测，然后使用 Numba 进行其余计算，结果证明这是一种高度优化且省时的方法。

一些 Scipy 函数有一些 Numba 替代函数，比如 scipy.integrate.quad 的 numbaducpack 和 scipy.optimize.root 的 NumbaMinpack。对于这些，建议在 Cython 和 Numba 之间分割总的处理，因为，

两者都被证明可以显著提高 Python 代码的速度

Scipy 代码可以用 Cython 编译

随着运算次数的增加，Numba 的速度比 Cython 快。

下面是 Jupyter notebook 中编写的一段简单代码的示例，它通过使用 Scipy.signal 的 find_peaks 和 peak_prominences 函数来检测信号(_data_slice)的峰值。在 Cythonized 单元格中，手动添加每个变量的类型，最后将信号全长的强度值(int_array)的 numpy 版本、峰值点(作为 peaks_array)、检测到的峰值的左右基点(分别作为 left_bases_array 和 right_bases_array)传递给 Numba 函数

## Cython 函数

```
%load_ext Cython%%cython 
from scipy.signal import find_peaks, peak_prominences
cimport numpy as cnp
cnp.import_array()
import numpy as npcpdef **peak_finder_cy**(cnp.ndarray _data_slice, _scan_no, int int_index = 0): cdef cnp.ndarray _int
    cdef cnp.ndarray _peaks
    cdef cnp.ndarray _prominences
    cdef cnp.ndarray peak_mask
    cdef int wlen
    cdef cnp.ndarray int_array
    cdef cnp.ndarray left_bases_array
    cdef cnp.ndarray right_bases_array
    cdef cnp.ndarray peaks_list_array _int = _ms1_data_slice[_data_slice[:, 4] == _scan_no, int_index]
    _peaks, _ = find_peaks(_int)
    prominences, left_bases, right_bases = peak_prominences(_int,   _peaks, wlen=20)         

    int_array = np.array(_int)
    peaks_array = np.array(_peaks)    
    left_bases_array = np.array(left_bases)
    right_bases_array = np.array(right_bases) return int_array, peaks_array, left_bases_array,        right_bases_array
```

## 数字函数

```
import numba as nb[@nb](http://twitter.com/nb).njit()
def **peak_mask_finder_cy**(_int_list, _peaks_list, _left_bases_list, _right_bases_list):   
    peak_id = 1
    peak_mask = np.zeros(_int_list)
    j = 0
    for j in nb.prange(_peaks_list):                
        if j > 0 and _left_bases_list[j] < _right_bases_list[j-1] and _int_list[_peaks_list[j]] > _int_list[_peaks_list[j-1]]:
            _left_bases_list[j] = _right_bases_list[j-1]             

        peak_mask[_left_bases_list[j] + 1 : _right_bases_list[j]] = peak_id
        peak_id += 1
    return peak_mask
```

## 调用各自的函数

```
%%timeit
int_list, peaks_list, left_bases_list, right_bases_list = **peak_finder_cy**(signal_data, scan_no)peak_mask_final = **peak_mask_finder_cy**(int_list, peaks_list, left_bases_list, right_bases_list)
```

**使用 Cython 和 Numba 的处理时间**

每圈 39.2 s 120 ns(平均标准偏差。戴夫。7 次运行，每次 10000 个循环)

**仅使用 cy thon**

每循环 101 s 178 ns(平均标准偏差。戴夫。7 次运行，每次 10000 个循环)

因此，即使对于一小块数据，与仅基于 Cython 的代码相比，使用带有 Cython 和 Numba 的重构代码在处理时间上的改进也是显著的。随着从更大体积的数据集中检测到的峰的数量的增加，两者之间的处理时间差异可能会更加突出和显著。

这里要提到的是，为了开发处理流水线，Cython 代码必须在. pyx 文件中，并且需要一个单独的 setup.py 脚本，由 Cython 将其编译成. c 文件。C 文件由 C 编译器编译成. so 文件。这里使用的命令是:python setup . py build _ ext—in place。然后，可以在任何 python 脚本中以通常的方式导入 Cython 模块。

有人可能会说直接使用 Python 代码，而不是使用第一个函数(peak_finder_cy)的 Cythonized 版本。对于像 Scipy 这样调用编译 C 的库来说，它已经是一个高性能的库了，这是一个合理的论点。然而，当有大量扫描/数据段需要循环查找峰值时，Cython 可以被证明是非常有效的。这里的这篇文章可以帮助你了解更多的细节和例子。作为参考，使用 Python 和 Numba 对上面这段代码的处理时间是每循环 40.7 s 91.6 ns(平均标准时间。戴夫。7 次运行，每次 10000 个循环)。

## 结论

总之，重构是最可行的选择，使 **Numba** 可用于不兼容的 Python 库和函数的计算，同时确保代码的最佳运行时间。

## 参考

1.  [https://www . pick up brain . com/python/speed-up-python-up-to-100 万次-cython-vs-numba/](https://www.pickupbrain.com/python/speed-up-python-up-to-1-million-times-cython-vs-numba/)
2.  张，j，冈萨雷斯，e，赫斯提洛，t，哈斯金斯，w，黄，y(2009)。液相色谱-质谱联用中的峰值检测算法综述。*当代基因组学*， *10* (6)，388。
3.  [http://Stephan hoyer . com/2015/04/09/numba-vs-cy thon-how-to-choose/](http://stephanhoyer.com/2015/04/09/numba-vs-cython-how-to-choose/)

从读者的角度来看， [Joseph Bloom](https://medium.com/u/425209fe4dbb?source=post_page-----77a4377576c--------------------------------) 感谢他对本文非常有用的反馈。