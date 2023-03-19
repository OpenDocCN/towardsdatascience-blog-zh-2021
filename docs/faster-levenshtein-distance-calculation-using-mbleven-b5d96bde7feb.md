# 使用 mbleven 快速计算 Levenshtein 距离

> 原文：<https://towardsdatascience.com/faster-levenshtein-distance-calculation-using-mbleven-b5d96bde7feb?source=collection_archive---------31----------------------->

## 莱文斯坦永远服用*？如果你的 levenshtein 算法需要很多时间，并且你的边界参数小于 3，请尝试使用 mbleven。*

*![](img/2664f18d78752a2c12c0042179d1c470.png)*

*[来源](https://unsplash.com/s/photos/relaxation)*

## *什么是 levenshtein 距离？*

*Levenshtein 距离是将字符串 **a** 转换为字符串 **b** 所需的最小插入、删除和符号替换次数。*

****举例:*** 考虑字符串 a: **鼠标** &字符串 b: **莫尔斯***

*弦 **a** 与弦 **b** 之间的 Levenshtein 距离为 2。您需要从字符串 **a** 中删除 *u* 并插入 *r* 以将字符串 **a** 转换为字符串 **b.***

*levenshtein 距离还有另一个修改:*

1.  *Damerau Levenshtein 遵循与 Levenshtein 距离完全相同的方法，但您也可以使用换位(相邻符号的交换),因此，它使 Levenshtein 更快、更有效。*

*在上面的例子中，字符串 **a** 和字符串 **b** 之间的 Damerau-Levenshtein 距离是 1。你只需要用 *r* 替换**字符串 a** 中的 *u* 就可以将字符串 **a** 转换为字符串**b***

*虽然看起来很简单，但是如果你在一个熊猫数据帧中有数千行，那么 Levenshtein 距离需要花费很多时间来运行。*

## *mbleven 来拯救—*

*与 Levenshtein 不同，mbleven 是一种“基于假设的算法”。它将首先列出将一个字符串转换为另一个字符串的所有可能的假设。*

*上述例子的可能假设:*

1.  *如果 I **插入 r** 是否可以将字符串 **a** 转换为字符串 **b** ？*
2.  *如果我**删除 u** 字符串 **a** 可以转换成字符串 **b** 吗？*
3.  *如果我用**代替 u** ，字符串 **a** 能转换成字符串 **b** 吗？*
4.  *….*

*mbleven 将逐一检查所有的假设。所有的假设都可以在 O(n)次内计算出来。*

*完整的代码及其实现可以在这里查看:[https://github.com/fujimotos/mbleven](https://github.com/fujimotos/mbleven)*

*mbleven 可以确认是否可以使用提到的 k-bound 将一个字符串转换成另一个字符串&它还可以返回两个字符串之间的编辑距离。提到的几个例子是:*

```
*>>> from mbleven import compare
>>> compare("meet", "meat")
1
>>> compare("meet", "eat")
2
>>> compare("meet", "mars")  # distance 3
-1*
```

*最后一个示例返回-1，因为这两个字符串之间的距离是 3 编辑距离，默认情况下，该算法的上限是 2 编辑距离。*

## *参考文献—*

 ** 

*[https://github.com/fujimotos/mbleven](https://github.com/fujimotos/mbleven)*