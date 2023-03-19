# 如何找到子集和问题的所有解

> 原文：<https://towardsdatascience.com/how-to-find-all-solutions-to-the-subset-sum-problem-597f77677e45?source=collection_archive---------4----------------------->

![](img/fbe50212cecac9801bb52c9be8db5067.png)

照片由 [Antoine Dautry](https://unsplash.com/@antoine1003?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 适用于负数、正数和重复数字的动态编程解决方案

子集和问题涉及确定整数列表中的子集是否可以和为目标值。比如考虑一下`nums = [1, 2, 3, 4]`的列表。如果`target = 7`，有两个子集达到这个和:`{3, 4}`和`{1, 2, 4}`。如果`target = 11`，没有解决方案。

一般来说，确定子集和是否有*甚至任意*个解是 [NP 难](https://en.wikipedia.org/wiki/NP-hardness):如果`nums`列表中有`n`个整数，则存在`2^n — 1`个子集需要检查(不包括空集)。在这篇文章中，我们将会看到一个更有效的解决方法，使用动态规划 (DP)。然而，与大多数教程不同，我们不仅要确定**是否存在**一个解决方案，还要看看**如何发现所有解决方案**。该算法适用于负的和正的输入值，以及`nums`中重复的非唯一整数。

## TLDR；Python 包🐍

寻找快速解决方案，但不一定想知道底层细节？我创建了一个名为“ [subsetsum](https://pypi.org/project/subsetsum/) 的 Python 包，带有一个超快的求解器:`pip install subsetsum`

求解器的逻辑用 C++实现，并使用 [Pybind11](https://pybind11.readthedocs.io/en/stable/) 来公开一个 Python 接口。GitHub 上的[提供了源代码。](https://github.com/trevphil/subsetsum)

# 1.预处理

在进入 DP 解决方案之前，我们将对问题输入(`nums`和`target`)进行一些预处理。

## 翻转标志🔄

我们要做的第一件事是翻转`nums`中所有整数的符号，如果目标是负的，则翻转`target`、*。这确保了我们的`target`将总是 0 或更大，这只是让整个算法的生活更容易！我们可以不用担心，因为翻转符号前后的解是相同的:*

```
x0 + x1 + ... + x10 = target
# Multiply both sides by -1
-x0 - x1 - ... - x10 = -target
# The equations are equivalent!
```

## 对➡️的数字进行排序

下一个预处理步骤是按升序对`nums`进行排序，这是 DP 算法工作所必需的(稍后描述)。如果你需要记住`nums`的原始顺序，你可以执行一个 [argsort](https://numpy.org/doc/stable/reference/generated/numpy.argsort.html) ，它从一个整数在列表中的当前位置映射到它在排序列表中的位置。如果需要，您可以随时重新映射回原始位置。

```
nums = [-2, 1, -3, 0, 4, 5]
index = argsort(nums) = [2, 0, 3, 1, 4, 5]
nums[index] = [-3, -2, 0, 1, 4, 5]
```

## 检查目标是否过低或过高🛑

考虑`nums = [-3, -2, -1, 1, 2, 3, 4]`的情况。可达到的最小总和是多少？ **-6** 。最大的*金额可能是多少？ **10** 。如果`target`之和比`nums`中所有负整数之和少*或者比`nums`中所有正整数之和多*则无解。***

我们将在变量`a`中存储所有负整数的和，在变量`b`中存储所有正整数的和。如果`target < a`或者`target > b`，我们可以以“无解！”

# 2.动态规划

![](img/162536e8527830c51892fc6cdeeca0d2.png)

照片由[米卡·鲍梅斯特](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

预处理完成后，我们准备填充一个名为`DP`的[动态编程](https://en.wikipedia.org/wiki/Dynamic_programming)表。`DP`表将有`n`行(给定`n`数字)和`target — a + 1`列。存储在表格中的值将只是**真**或**假**。行和列索引从 0 开始。

*   如果我们在`row i`上，我们会考虑使用整数*的所有子集，直到并包括排序后的`nums`中的第`i`个整数*。第`i`个整数**不**需要包含，但是如果我们想使用它，它是“可用”的。
*   如果我们在`column j`上，我们将试图找到总和为`a + j`的“中间”目标的子集。注意，因为我们有`target — a + 1`列，所以`j`的最终值将是`target — a`，这意味着最后，我们试图找到总和为`a + j = a + target — a = target`的子集。
*   因此，`DP[i, j] = 1`意味着`nums[0...i]`中存在一个子集，其总和为`a + j`。如果`DP[i, j] = 0`不存在这样的子集。
*   **恢复解**:如果`DP[n — 1, target — a] == 1`，存在`nums`的子集，总和为`target`。

## 更新规则⭐️

我们首先用零初始化`DP`表，并填充`DP`的第一行，注意如果`nums[0]`等于`a + j`，则`DP[0, j]`只能是 **1** 。

```
for j in range(0, target - a + 1):
    DP[0, j] = (nums[0] == a + j)
```

对于剩余的行，`DP[i, j]`可以在以下情况下标记为 **1** :

*   `DP[i — 1, j] == 1`:如果仅使用来自`nums[0...(i — 1)]`的子集就可以实现“中间”目标`a + j`，那么很明显，如果允许第`i`个数字在该子集中，也可以实现该目标。
*   `nums[i] == a + j`:在这种情况下，中间目标`a + j`可以从单整数子集`{nums[i]}`中得到。
*   `DP[i — 1, j — nums[i]] == 1`:最棘手的规则，让人想起背包问题的动态编程解决方案[。如果有一个`nums[0...(i — 1)]`的子集总计为`a + j — nums[i]`，那么我们知道有*也是*的一个子集，通过将`nums[i]`包含在该子集中而总计为`a + j`。](https://medium.com/@fabianterh/how-to-solve-the-knapsack-problem-with-dynamic-programming-eb88c706d3cf)

```
for i in range(1, n):
    for j in range(0, target - a + 1):
        DP[i, j] = DP[i - 1, j] or nums[i] == (a + j)
        if DP[i, j] == False:
            next_j = j - nums[i]
            if 0 <= next_j < target - a + 1:
                DP[i, j] = DP[i - 1, next_j]
```

## 时间和空间复杂性⏱

这里描述的 DP 解决方案是所谓的[伪多项式算法](https://en.wikipedia.org/wiki/Pseudo-polynomial_time)。DP 表格的大小不仅(线性地)取决于`nums`中元素的数量，还(线性地)取决于`nums`和`target`的值，因为表格中的列数是`target`和`nums`中负整数之和之间的距离。

表的每个单元格必须设置为 0 或 1 一次，这是使用对其他单元格的恒定数量的查询来确定的，因此算法的运行时与表的大小成比例。

## DP 有缺点，蛮干可以更好💪

先说`nums = [-1000000, 1000000]`和`target = 999999`。使用 DP 方法，我们将有 2 行和`999999 + 1000000 + 1 = 2000000`列。对于一个显然无法解决的问题来说，内存使用量太大了！如果`nums`中的数字很少，但是值的范围很大，你最好强力检查所有可能的子集。

# 3.寻找所有解决方案🤯

![](img/e6245164932a055658a81f09408b3962.png)

我们会用栈，可惜不是这种栈！照片由[布里吉特·托姆](https://unsplash.com/@brigittetohm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

让我们继续讨论可能是最具挑战性的话题，也是其他教程中最少讨论的话题:如何实际找出哪些子集达到了`target`和！

要做到这一点，我们需要使用我们的`DP`表，并通过它回溯。我们将使用一种非递归技术:一个[堆栈](https://en.wikipedia.org/wiki/Stack_%28abstract_data_type%29)。堆栈中的每一项都是一个简单的数据结构，我称之为`StackItem`。

```
class StackItem:
    row: int  # Row index in the DP table
    col: int  # Column index in the DP table
    take: list[int]  # **Indices** of integers to include in the subset
    togo: int  # Value "to go" until reaching the `target` sum
```

现在我们知道，如果`DP`表格右下角的单元格是 **1** ，那么就有一个解决方案。否则，就不要费心去寻找解决方案了，因为根本就没有解决方案！我们将从右下角的单元格开始初始化堆栈/回溯。我们假设`nums` **中的最后一个整数是子集包含的**，那么`togo`将是`target`值减去`nums[n — 1]`。

```
stack.push(
    StackItem(
        row=n - 1,
        col=target - a,
        take=[n - 1],
        togo=target - nums[n - 1]
    )
)
```

现在，我们不断从堆栈中弹出项目，并添加新的项目，直到它是空的。一旦完成，我们将列举所有可能的解决方案。

假设我们刚刚弹出了一个项目:`item = stack.pop()`的`row = i`和`col = j`。我们将检查`item`的三个场景:

1.  如果`DP[i — 1, j] == 1`，那么可能有一个不使用`nums`中第`i`个整数的解决方案。在这种情况下，我们将添加一个新的`StackItem`，就好像第`i`个整数**不包含在子集中，但是第`(i-1)`个整数**是**。**
2.  如果`DP[i — 1, j — nums[i]] == 1`，那么有一个解决方案，使用剩余的`nums[0...(i — 1)]`整数形成一个“中间”子集，其总和为`a + j — nums[i]`。在这种情况下，我们将添加一个新的`StackItem`，假设第`i`个整数**是包含在“最终”子集中的**。
3.  如果`item.togo == 0`那么我们已经有了解决方案！然后`item.take`将整数的索引存储在(排序的)`nums`中，形成一个子集，其和为`target`。

```
while len(stack) > 0:
    item = stack.pop()
    i, j, take, togo = item.unpack() # Scenario 1
    if i > 0 and DP[i - 1, j]:
        new_take = take.copy()
        new_take[-1] = i - 1  # Replace the last element
        new_togo = togo + nums[i] - nums[i - 1]
        stack.push(StackItem(i - 1, j, new_take, new_togo)) # Scenario 2
    next_j = j - nums[i]
    if i > 0 and 0 <= next_j < (target - a + 1):
        if DP[i - 1, next_j]:
            new_take = take.copy()
            new_take.append(row - 1)  # Add a new element
            new_togo = togo - nums[i - 1]
            stack.push(StackItem(i - 1, next_j, new_take, new_togo)) # Scenario 3
    if togo == 0:
        yield [nums[t] for t in take]
```

# 结论🎉

我希望您喜欢学习如何使用动态编程解决子集和问题！有相当多的在线资源解释了如何确定*是否对于特定的一组(`nums`，`target`)存在*解，但是这些教程通常假设所有的数字都是正数。这里描述的算法也适用于负数！💡此外，几乎没有现有的教程解释如何利用 DP 表回溯所有解决方案，这是本文的主要动机。

更具体的实现(C++和 Python，不是伪代码)请访问 [GitHub](https://github.com/trevphil/subsetsum) 或通过`pip install subsetsum`下载 pip 模块。