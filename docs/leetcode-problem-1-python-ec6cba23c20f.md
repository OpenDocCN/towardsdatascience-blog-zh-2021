# Python 中的 LeetCode 问题 1 解决方案

> 原文：<https://towardsdatascience.com/leetcode-problem-1-python-ec6cba23c20f?source=collection_archive---------4----------------------->

## [面试问题](https://towardsdatascience.com/tagged/interview-questions)

## 讨论了 LeetCode 中两个和问题的最优解的方法

![](img/a639cc419e7b45cc80508de6d3482c6b.png)

阿什利·韦斯特·爱德华兹在 [Unsplash](https://unsplash.com/s/photos/pen-and-paper?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

LeetCode 是一个平台，它提供了数千个编程问题，并帮助用户提高他们的技能，为技术面试做好准备，这通常是工程和 ML 职位招聘过程的一部分。

在今天的简短指南中，我们将探索名为 **Two Sum** 的第一个问题，并尝试以最佳方式解决它。在技术面试中，不仅为特定问题找到解决方案很重要，时间复杂度也是你通常会被问到的问题。

## 两个和问题

> 给定一个整数数组 nums 和一个整数`target`，返回这两个数的*索引，使它们相加等于* `*target*`。
> 
> 你可以假设每个输入都有 ***恰好*一个解**，你不能两次使用*相同的*元素。
> 
> 可以任意顺序返回答案。

**例 1:**

```
**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Output:** Because nums[0] + nums[1] == 9, we return [0, 1].
```

**例 2:**

```
**Input:** nums = [3,2,4], target = 6
**Output:** [1,2]
```

**例 3:**

```
**Input:** nums = [3,3], target = 6
**Output:** [0,1]
```

**约束:**

*   `2 <= nums.length <= 104`
*   `-109 <= nums[i] <= 109`
*   `-109 <= target <= 109`
*   只存在一个有效答案。

> 来源: [LeetCode](https://leetcode.com/problems/two-sum/)

## 不是这样的..最优解

最简单的方法需要两个嵌套循环，其中外部循环遍历列表的所有元素，内部循环从外部循环的当前索引开始遍历，直到列表的末尾。

```
from typing import Listclass Solution: def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i, j]
```

上述解决方案的时间复杂度为 **O(n )** 这是相当可观的..不好。

## 解决 O(n)中的问题

如果我们能够以某种方式对数字列表进行一次迭代，这个问题将会得到更有效的解决。为此，我们可以利用字典。

在下面的解决方案中，我们首先创建一个空字典，在这里我们将把每个列表元素的值和索引分别存储为一个键对。然后，我们遍历包含我们的数字的列表的索引和值。如果列表中的目标值和当前值之间的差已经作为一个键包含在字典中，那么这意味着当前值和存储在字典中的值就是我们问题的解决方案。

否则，我们只需将值和索引作为键-值对添加到我们的字典中，并不断迭代，直到找到我们正在寻找的解决方案。

```
from typing import Listclass Solution: def twoSum(self, nums: List[int], target: int) -> List[int]:
        values = {}
        for idx, value in enumerate(nums):
            if target - value in values:
                return [values[target - value], idx]
            else:
                values[value] = idx
```

在上面的解决方案中，我们只迭代了一个数字列表，因此算法的时间复杂度是 **O(n)** ，这比之前实现的解决方案好得多！

## 最后的想法

在今天的短文中，我们讨论了 LeetCode 中两个和问题的几种方法。最初，我们创建了一个简单的解决方案，这将导致较差的性能，但我们随后利用 Python 字典来实现一个时间复杂度为 O(n)的解决方案。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](/augmented-assignments-python-caa4990811a0) [## Python 中的扩充赋值

### 了解增强赋值表达式在 Python 中的工作方式，以及为什么在使用它们时要小心…

towardsdatascience.com](/augmented-assignments-python-caa4990811a0) [](/apply-vs-map-vs-applymap-pandas-529acdf6d744) [## 熊猫中的 apply() vs map() vs applymap()

### 讨论 Python 和 Pandas 中 apply()、map()和 applymap()的区别

towardsdatascience.com](/apply-vs-map-vs-applymap-pandas-529acdf6d744)