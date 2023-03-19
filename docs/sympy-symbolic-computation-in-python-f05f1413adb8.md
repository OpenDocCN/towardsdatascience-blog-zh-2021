# SymPy:Python 中的符号计算

> 原文：<https://towardsdatascience.com/sympy-symbolic-computation-in-python-f05f1413adb8?source=collection_archive---------0----------------------->

## 用 Python 象征性地解一个方程和微积分

![](img/75f0ba2f77986abd96cabf3b6dc030fa.png)

作者图片

# 动机

你曾经希望用 Python 解一个数学方程吗？如果我们可以用一行代码解决如下的代数方程，那不是很好吗

```
[-1/2, 0]
```

…或者只是使用数学符号而不是枯燥的 Python 代码？

![](img/a041aafe7cebaca3b6e7109281272653.png)

作者图片

这就是 SymPy 派上用场的时候了。

# 什么是 SymPy？

SymPy 是一个 Python 库，允许你用符号计算数学对象。

要安装 SymPy，请键入:

```
pip install sympy
```

现在让我们回顾一下 SymPy 能做的一些令人惊奇的事情！

从导入 SymPy 提供的所有方法开始

```
from sympy import *
```

# 基本操作

通常，当计算平方根时，我们得到一个小数:

![](img/070c8756aab3ad9195d1332d0a8feeb7.png)

作者图片

但是使用 SymPy，我们可以得到平方根的简化版本:

![](img/d871d017b647518349f1d3b379c7d0b9.png)

作者图片

这是因为 SymPy 试图精确地而不是近似地表示数学对象。

因此，当使用 SymPy 将两个数相除时，我们将得到一个分数而不是小数。

![](img/be1355bd1f29eec2fc30e929336c5cf4.png)

作者图片

# 标志

SymPy 的真正力量是它处理符号的能力。要创建符号，使用方法`symbols()`:

![](img/c31df37cc86d627a8bfbe4e612d00509.png)

作者图片

酷！我们可以用 x 和 y 创建一个表达式，如果我们给这个表达式加上一个数会发生什么？

![](img/9315ba4844432f4f1e1cdf5c47b6a397.png)

作者图片

啊哈！+ 2 加到表达式中，表达式保持不求值。

为什么使用符号会有用？因为我们现在可以使用我们在学校学到的各种数学技巧，如扩展、分解和简化方程，以使我们的生活更容易。

# 方程式

## 扩展、分解和简化

我们知道左边表达式的展开等于右边的表达式。

![](img/a49bdca4af10602d398b76ee1af8a7b1.png)

作者图片

用 SymPy 能做到这一点吗？是啊！SymPy 允许我们使用`expand`扩展一个等式:

![](img/b0280843be65cd7962d97a49ab10a42f.png)

作者图片

酷！我们也可以通过使用`factor`来分解我们的表达:

![](img/da279ea39bc9b0a88f15ad0fe9462d24.png)

作者图片

我们可以用 SymPy 做的另一件很酷的事情是使用`simplify`简化一个方程:

![](img/4315794f11159de8a350ef3e8933081a.png)

作者图片

很好，不是吗？

## 解方程

在处理数学符号时，我们最常见的问题之一是解方程。幸运的是，这也可以通过 SymPy 来实现。

要解方程，使用`solve`:

![](img/22e569c012aef82e96ff323b4b7f6274.png)

作者图片

## 代替

如果我们用 2 代替下面的等式，我们会得到什么？

![](img/262c1bcf7ff8ae437cf44c24301f6baa.png)

作者图片

我们可以使用`eq.subs(x, 2)`来解决这个问题:

![](img/b0826a35fdf47765b2fd16acd90832f3.png)

作者图片

我们也可以用另一个变量代替 x，得到如下表达式:

![](img/bd03001387d7563039bc0fe465384705.png)

作者图片

不错！

# 三角法的

还记得我们高中学的那些好玩的三角恒等式吗？

![](img/4a027cad062117cacde1732108fe4052.png)

作者图片

如果我们可以使用 SymPy 来为我们解决这个问题，而不是查找这些身份，这不是很好吗？

要使用三角恒等式简化表达式，请使用`trigsimp()`

![](img/163d93f45bb255347db15243003207fd.png)

作者图片

# 导数、积分和极限

有了 SymPy，还可以做微积分！下面表达式的导数是什么？

![](img/43e67fe9d6fcab0cddb6b0ab2f837e53.png)

作者图片

如果你想不出来，不要担心。我们可以用 SymPy 来解决这个问题。

![](img/6f147f950f8d7f03ee37813ccbff8f5c.png)

作者图片

现在让我们通过对导数进行积分，回到原来的表达式。

![](img/63392094a720334ae8fadeece5dc867b.png)

作者图片

当 x 接近无穷大时，我们也可以取极限。

![](img/aff085d860cfdc1c36ee9b40fe021042.png)

作者图片

![](img/423c8aa49f3b8bc56eba05a5ef0753b6.png)

作者图片

或者 2:

![](img/d1b48a9e002670a9e8ae02fe682281a6.png)

作者图片

# 特殊函数

SymPy 还提供了特殊的功能:

*   求一个数的阶乘

![](img/57fc3f470c33b74a3998b85dbe80ca7d.png)

作者图片

*   用另一种说法改写这个表达式

![](img/8dff6150b9ad1d6c108cee3e65edc7a9.png)

作者图片

# 印花乳胶

如果您喜欢这个输出，并且想要获得表达式的 LaTex 形式，那么使用`latex`:

![](img/0036af1cfc67ebf66aa1e42f011cc8e2.png)

作者图片

你可以把这个 LaTex 表格粘贴到你笔记本的 markdown 上，得到一个漂亮的数学表达式，如下图所示！

![](img/ae1e6cc2419eaf69e792e0f2dd4b3f19.png)

作者图片

# 结论

恭喜你！您刚刚学习了如何使用 SymPy 在 Python 中用符号计算数学对象。下次你解数学的时候，试着用 SymPy 让你的生活更轻松。

SymPy 还提供了许多其他有用的方法，我无法在这里一一介绍。我鼓励你查看[这份文档](https://docs.sympy.org/latest/tutorial/index.html)以获得更多灵感。

在 Github repo 中，您可以随意使用本文的代码:

<https://github.com/khuyentran1401/Data-science/blob/master/data_science_tools/sympy_example.ipynb>  

我喜欢写一些基本的数据科学概念，并尝试不同的算法和数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上和我联系。

如果你想查看我写的所有文章的代码，请点击这里。在 Medium 上关注我，了解我的最新数据科学文章，例如:

</how-to-find-best-locations-for-your-restaurants-with-python-b2fadc91c4dd>  </how-to-solve-a-staff-scheduling-problem-with-python-63ae50435ba4>  </how-to-create-mathematical-animations-like-3blue1brown-using-python-f571fb9da3d1>  </maximize-your-productivity-with-python-6110004b45f7> 