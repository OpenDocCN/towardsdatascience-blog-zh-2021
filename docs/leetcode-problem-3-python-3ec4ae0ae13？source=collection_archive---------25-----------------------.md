# Python 中无重复的 LeetCode 最长子串解决方案

> 原文：<https://towardsdatascience.com/leetcode-problem-3-python-3ec4ae0ae13?source=collection_archive---------25----------------------->

## [面试问题](https://towardsdatascience.com/tagged/interview-questions)

## LeetCode 中问题 3 的最优解

![](img/e2ff30472c46fedd7b879fe780a4f794.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

## 介绍

LeetCode 是一个平台，它提供了许多编码问题，这些问题通常会在谷歌、Meta 和亚马逊等科技巨头的工程和机器学习职位的技术面试中被问到。

在今天的文章中，我们将探讨 LeetCode 问题的第三个问题的几种方法，这个问题的难度为**中等**级，称为*“无重复的最长子串”。*

## 问题是

> 给定一个字符串`s`，找出不含重复字符的**最长子串**的长度。

**例 1:**

```
**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.
```

**例 2:**

```
**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.
```

**例 3:**

```
**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**例 4:**

```
**Input:** s = ""
**Output:** 0
```

**约束:**

*   `0 <= s.length <= 5 * 104`
*   `s`由英文字母、数字、符号和空格组成。

> 来源: [LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## 粗暴的方法

解决这个问题最直观的方法是使用两个嵌套循环执行强力搜索。

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:

        longest_str = ""
        for i, c in enumerate(s):
            canditate_str = str(c)
            for j in range(i + 1, len(s)):
                if s[j] not in canditate_str:
                    canditate_str += str(s[j])
                else:
                    break
            if len(canditate_str) > len(longest_str):
                longest_str = canditate_str if len(s) - i <= len(longest_str):
                break        return len(longest_str)
```

尽管上述算法可以解决问题，但时间复杂度——在本例中是**O(n)**——是一个问题，因为我们肯定可以做得比这更好。

## 通过一个更好的方法

现在，为了改进前面的答案，我们需要以某种方式对包含字符的字符串进行一次遍历。可以帮助我们做到这一点的一种方法是所谓的**滑动窗口**。

为了阐明我所说的滑动窗口的含义，让我们考虑下面的字符串(在顶部我们显示了每个单独字符的索引):

```
#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
```

滑动窗口具有开始和结束索引:

```
#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start         |
# end           |
```

根据不同的算法，起始和结束索引将相应地移动。在我们的例子中，我们希望创建滑动窗口并向前移动，如下图所示。

```
#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start         |
# end             |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start         |
# end               |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start         |
# end                 |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start           |
# end                 |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start           |
# end                   |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start             |
# end                   |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start             |
# end                     |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start               |
# end                     |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start               |
# end                       |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start                   |
# end                       |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start                   |
# end                         |#               0 1 2 3 4 5 6 7   
#               a b c a b c b b
# start                       |
# end                         |
```

可以反映下图/代码中描述的逻辑的算法如下所示。我们使用哈希映射(即 Python 字典)，其中键对应到目前为止看到的字符，值对应到这个特定字符最后被看到的索引。

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        chars = {}
        start = 0
        max_length = 0

        for end, c in enumerate(s):
            if c in chars:
                start = max(start, chars[c] + 1)

            max_length = max(max_length, end - start + 1)
            chars[c] = end return max_length
```

现在，就复杂性而言，与强力方法相比，我们取得了显著的改进，因为现在我们只对字符串迭代一次，因此我们的解决方案的时间复杂性为 O(n) 。

## 最后的想法

在今天的文章中，我们介绍了 LeetCode 平台上第三个问题的几个潜在解决方案，这个问题叫做*“无重复的最长子串”*。

我们最初创建了一个次优的解决方案，通过创建两个嵌套循环来确定输入字符串中哪个子字符串是最长的，这涉及到暴力。

最后，我们通过使用滑动窗口和哈希映射(即 Python 字典)来优化算法，这样我们只需要对字符串进行一次遍历，就可以在 O(n)时间内解决问题。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读媒介上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

[](https://gmyrianthous.medium.com/membership) [## 通过我的推荐链接加入 Medium-Giorgos Myrianthous

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

gmyrianthous.medium.com](https://gmyrianthous.medium.com/membership) 

**你可能也会喜欢**

[](/leetcode-problem-1-python-ec6cba23c20f) [## Python 中的 LeetCode 问题 1 解决方案

### 讨论了 LeetCode 中两个和问题的最优解的方法

towardsdatascience.com](/leetcode-problem-1-python-ec6cba23c20f) [](/leetcode-problem-2-python-1c59efdf3367) [## LeetCode 问题 2:用 Python 添加两个数的解决方案

### 理解如何用 Python 中的链表高效地解决两个数相加问题

towardsdatascience.com](/leetcode-problem-2-python-1c59efdf3367)