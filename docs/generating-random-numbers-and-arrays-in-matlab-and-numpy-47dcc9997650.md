# 在 Matlab 和 Numpy 中生成随机数和数组

> 原文：<https://towardsdatascience.com/generating-random-numbers-and-arrays-in-matlab-and-numpy-47dcc9997650?source=collection_archive---------8----------------------->

## Matlab 和 Numpy 码再现随机数和数组的比较

正如我们所知， **Numpy** 是一个著名的 Python 库，用于支持 Python 中的矩阵计算，以及一个大型的高级数学函数集合，用于操作向量、矩阵和张量数组[1]。 **Matlab** 是一种编程语言和数值计算环境，支持矩阵计算，绘制函数和数据，实现算法，创建接口，以及与其他语言编写的程序接口[2]。

![](img/c270a24447e440ea73780a096e2d3628.png)

顶部面板:Numpy logo[3]；底部面板:Matlab 徽标[4]。

Numpy 和 Matlab 的一个共同特点是它们广泛用于矩阵计算和数值计算。然而，它们并不完全相同。在这篇文章中，我们将讨论如何在 Matlab 和 Numpy 中生成随机数和数组。特别地，我们比较了 Matlab 和 Numpy 对相同随机数和数组的再现。

# 随机数和数组

Matlab 和 Numpy 都可以根据某个命令生成随机数和数组。有两个关键概念:

*   **随机数分布**
*   **随机种子**

对于随机数分布，我们通常考虑均匀分布和标准正态(高斯)分布。在 Numpy 中，我们可以使用`numpy.random.rand`来生成均匀分布的随机数。在 Matlab 中，我们可以使用`rand`来生成均匀分布的随机数。以下是一些简单的例子:

Python 中的 Numpy:

```
import numpy as npsample = np.random.rand(2)
print(sample)
```

输出是:

```
[0.70573498 0.8595017 ]
```

Matlab:

```
sample = rand(1, 2)
```

输出是:

```
ans =

   0.5390   0.7686
```

上面的例子是关于均匀分布的随机数。我们可以生成遵循标准正态分布的随机数:

*   Python 中的 numpy:`np.random.randn`
*   Matlab: `randn`

可以使用某个命令/函数来生成遵循某个特定分布的随机数，但是每次我们复制代码时，生成的随机数都会不同。因此，随机种子是另一个重要的概念，它允许在每个时间固定随机数。

例如，以下 Python 代码:

```
import numpy as np
np.random.seed(10)sample = np.random.rand(2)
print(sample)
```

会产生这些随机数:

```
[0.77132064 0.02075195]
```

在 Matlab 中:

```
rng(10);
sample = rand(1, 2)
```

我们有以下输出:

```
sample =0.7713    0.0208
```

# Numpy 和 Matlab 中 Rand 函数的比较

在 Numpy 和 Matlab 中，它们可以处理不同数组的计算:

*   1-D 数组(一维数组)是指向量。
*   2-D 数组(二维数组)指矩阵。
*   3-D 数组(三维数组)指的是三阶张量。
*   ……
*   *n* -D 数组(*n*-维数组)指的是 *n* 阶张量。

接下来，我们将分别在 Numpy 和 Matlab 中尝试生成均匀分布的随机数。我们还将尝试在 Numpy 和 Matlab 中生成相同的随机数。

## 生成一维数组

为了在 Numpy 和 Matlab 中获得相同的均匀分布的随机数，我们将向量大小设置为 4，随机种子设置为 10。让我们试试吧！

**Numpy** :

```
import numpy as np
np.random.seed(10)sample = np.random.rand(4)
print(sample)
```

运行它，我们有

```
[0.77132064 0.02075195 0.63364823 0.74880388]
```

**Matlab** :

```
rng(10);
sample = rand(1, 4)
```

运行它，我们有

```
sample = 0.7713    0.0208    0.6336    0.7488
```

显然，两个生成的随机向量是相同的。

## 生成二维数组

首先，我们设置矩阵大小为 2 乘 3，随机种子为 10。

**Numpy** :

```
import numpy as np
np.random.seed(10)sample = np.random.rand(2, 3)
print(sample)
```

运行它，我们有

```
[[0.77132064 0.02075195 0.63364823]
 [0.74880388 0.49850701 0.22479665]]
```

**Matlab** :

```
rng(10);sample = rand(2, 3)
```

运行它，我们有

```
sample = 0.7713    0.6336    0.4985
    0.0208    0.7488    0.2248
```

生成的均匀分布随机矩阵是相同的。

## 生成三维数组

首先，我们将矩阵大小设置为 2 乘 3 乘 2，并将随机种子设置为 10。

**Numpy** :

```
import numpy as np
np.random.seed(10)sample = np.random.rand(2, 3, 2)
print(‘sample[:, :, 0] = ‘)
print()
print(sample[:, :, 0])
print()
print(‘sample[:, :, 1] = ‘)
print()
print(sample[:, :, 1])
```

运行它，我们有

```
sample[:, :, 0] = 

[[0.77132064 0.63364823 0.49850701]
 [0.19806286 0.16911084 0.68535982]]

sample[:, :, 1] = 

[[0.02075195 0.74880388 0.22479665]
 [0.76053071 0.08833981 0.95339335]]
```

**Matlab** :

```
rng(10);sample = rand(2, 3, 2)
```

运行它，我们有

```
sample(:,:,1) = 0.7713    0.6336    0.4985
    0.0208    0.7488    0.2248sample(:,:,2) = 0.1981    0.1691    0.6854
    0.7605    0.0883    0.9534
```

生成的均匀分布的随机张量是相同的。

# 在 Numpy 和 Matlab 中生成正态分布随机数

在上面，如果我们使用相同的随机种子，Numpy 和 Matlab 可以产生相同的均匀分布的随机数。不幸的是，由于 Numpy 和 Matlab 使用不同的转换来从标准正态分布生成样本，因此我们需要在 Numpy 和 Matlab 中使用相同的转换。

在实践中，仍然可以像 Numpy 一样再现 Matlab 的 randn()的输出。

**数量**:

```
import numpy as np
from scipy.stats import norm
np.random.seed(10)sample = norm.ppf(np.random.rand(2, 3))
print(sample)
```

运行它，我们有

```
[[ 0.74320312 -2.03846052  0.34153145]
 [ 0.67073049 -0.00374237 -0.75609325]]
```

Matlab:

```
rng(10);
sample = norminv(rand(2, 3), 0, 1)
```

运行它，我们有

```
sample = 0.7432    0.3415   -0.0037
   -2.0385    0.6707   -0.7561
```

生成的正态分布随机矩阵是相同的。

# 参考

[1]维基百科上的 Numpy:[https://en.wikipedia.org/wiki/NumPy](https://en.wikipedia.org/wiki/NumPy)

[2] Matlab 上的维基百科:[https://en.wikipedia.org/wiki/MATLAB](https://en.wikipedia.org/wiki/MATLAB)

[3] Numpy 标志来自[https://commons.wikimedia.org/wiki/File:NumPy_logo_2020.svg](https://commons.wikimedia.org/wiki/File:NumPy_logo_2020.svg)

[4] Matlab logo 出自[https://www.google.com/url?esrc=s&q =&RCT = j&sa = U&URL = https://1000 logos . net/Matlab-logo/&ved = 2 ahukewj 24 b 7c 7 bvzahukoxiehic i3 adqqr4 kdegqiehac&usg = aovvaw 2 jkoiqdedkheednpnpm 7 acw](https://www.google.com/url?esrc=s&q=&rct=j&sa=U&url=https://1000logos.net/matlab-logo/&ved=2ahUKEwj24b7C7bvzAhUKoXIEHci3AdQQr4kDegQIEhAC&usg=AOvVaw2JKOiqdedKhhEEDnpm7AcW)

[5]在 Matlab 和 Python / NumPy 中再现随机数。GitHub 网站:[https://github.com/jonasrauber/randn-matlab-python](https://github.com/jonasrauber/randn-matlab-python)

【6】有没有可能用 NumPy 重现 MATLAB 的 randn()。[https://stack overflow . com/questions/3722138/is-may-to-reproduct-randn-of-MATLAB-with-numpy](https://stackoverflow.com/questions/3722138/is-it-possible-to-reproduce-randn-of-matlab-with-numpy)

[7]比较 Matlab 和使用随机数生成的 Numpy 代码。[https://stack overflow . com/questions/18486241/comparisng-MATLAB-and-numpy-code-that-uses-random-numbery-generation](https://stackoverflow.com/questions/18486241/comparing-matlab-and-numpy-code-that-uses-random-number-generation)