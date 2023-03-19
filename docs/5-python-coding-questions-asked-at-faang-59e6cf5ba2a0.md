# FAANG 在 2021 年提出这 5 个 Python 问题

> 原文：<https://towardsdatascience.com/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0?source=collection_archive---------15----------------------->

## 破解数据科学面试

## 数据科学家和数据工程师的必读！

![](img/4d03eeeef77263ec536670d18a35fb8d.png)

弗朗西丝卡·格里玛在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

声音的

*我的* [*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding) *上有完整的 Python 代码。*

对于任何与数据相关的面试，Python 编程是一项基本技能，也是一个必须准备的领域！编程题分四类，包括**数据结构与算法、机器学习算法、数学与统计、数据操纵**(查看此[精彩文章](/the-ultimate-guide-to-acing-coding-interviews-for-data-scientists-d45c99d6bddc)作者[艾玛丁](https://medium.com/u/1b25d5393c4f?source=post_page-----afd6a0a6d1aa--------------------------------) @Airbnb)。

我在一篇相关的文章中详细阐述了数据操作和字符串提取的主题。在今天的帖子中，我将重点放在数学和统计学上，并对主要科技公司，尤其是 FAANG 提出的五个 Python 编程问题进行了现场编码。这种类型的问题给你一个商业背景，并要求通过模拟统计解决方案。

[](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) [## 2021 年 6 个 Python 数据科学面试问题

### 数据科学家/工程师的数据操作和字符串提取

towardsdatascience.com](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) 

# 问题 1:谁先赢？由微软

> 艾米和布拉德轮流掷一个公平的六面骰子。谁先掷出“6”谁就赢了这场比赛。艾米首先开始滚动。艾米赢的概率有多大？

## 走过我的思考

这是一个核心的模拟问题，没有比模拟这个过程相当多的次数并检查 Amy 获胜的概率更好的方法了。

艾米先掷硬币。如果结果是 6，那么游戏结束，艾米赢了。否则，轮到布拉德掷骰子，如果是 6，则赢得游戏。如果不是，则轮到艾米。重复这个过程，直到有人以 6 结束游戏。

这里，关键是要理解逻辑流程:谁赢了，在什么条件下赢了。如果艾米得了 6 分，布拉德也要掷骰子吗？号码

## 解决办法

检查第 11 行:A_6 是 Amy 的数据分布，如果她有一个 6，她的计数+1。否则，布拉德可以掷骰子。

最后(第 25 行)，最终结果应该是艾米赢的次数除以艾米和布拉德赢的总次数。一个常见的错误是将 A_count 除以模拟总数。这是不正确的，因为当 Amy 和 Brad 都没有掷出 6 时，就会出现迭代。

让我们测试一下上面的算法。

```
0.531271015467384
```

事实证明，艾米在这场比赛中占了上风，因为她在布拉德之前开始滚动。艾米在一万次模拟中有 53%的概率获胜。

# 问题 2:每个大公司最多 69 个数字

> -给定一个仅包含数字 6 和 9 的正整数 num。
> -返回最多改变一位数字所能得到的最大数(6 变成 9，9 变成 6)。
> ——【https://leetcode.com/problems/maximum-69-number/】T2

## 走过我的思考

给定一个正整数，让值变大的方法只有一个，就是把一个‘6’变成一个‘9’，而不是反过来。还有，我们要改最左边的 6；否则，就不是最大数量了。比如我们要把‘6996’改成‘9996’，而不是‘6999’。

我想出了这个问题的几个变体:你要么改变一次，要么改变所有的 6。

## 解决方案 1:更换一次

```
996666669
```

第 3 行，我们把整数变成字符串，把第一个‘6’替换成‘9’；使用 int()将其转换回整数。

## 解决方案 2:全部替换

```
999999999
```

对于第二个场景，我们不需要指定 k，因为 replace()方法在默认情况下会更改所有合适的值。出于教学目的，我指定了 k 值。这个问题的另一个变体是替换前两个或三个“6”以使数字最大化。

![](img/0177adf738920c9e0517b3b053dee63a.png)

约翰·维森特在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 问题 3:有效的完美正方形。作者:脸书

> -给定一个正整数 num，写一个函数，如果 num 是正方，则返回 True，否则返回 False。
> -后续:不要使用任何内置库函数如 sqrt。
> -[https://leetcode.com/problems/valid-perfect-square/](https://leetcode.com/problems/valid-perfect-square/)

## 走过我的思考

非常简单:检查一个正整数是否有完美的平方根，如果有，返回 True，这可以分两步完成。

1.  *求平方根。*
2.  *检查是否是完美的平方根。*

棘手的部分是，我们必须使用一个内置的库(例如，math，Numpy)来计算平方根，这是一个在 [LeetCode](https://medium.com/u/33f6b9a1a861?source=post_page-----59e6cf5ba2a0--------------------------------) 的简单问题。如果我们不能使用这些库，它会变得更具挑战性和迭代性，这是 LeetCode 的一个中级问题。

## 解决方案 1:内置库

该算法轻松通过了测试用例。需要指出的是，我们需要使用 int()方法只获取平方根的整数部分，而忽略任何小数部分。对于完美的正方形，不会有任何区别，等式成立。对于非完美正方形，等式不成立，返回 False。

> (特别感谢[韩琦](https://medium.com/u/5ffa63536714?source=post_page-----59e6cf5ba2a0--------------------------------)抓住了这个错误！)

## 解决方案 2:没有内置库&二分搜索法

如果没有图书馆，我们采用二分搜索法。LeetCode 包含了详细的解释([这里](https://leetcode.com/problems/valid-perfect-square/solution/)，我还有另外一篇关于这个话题的帖子([这里](/binary-search-in-python-the-programming-algorithm-8b8fa039eaa))。简而言之，我们创建两个指针，左和右，将这两个数的平均值与原数进行比较:如果小于该数，则增加该值；如果它更大，我们减少它；或者，如果匹配，则返回 True。

这些条件会在 while 循环中自动检查。

# #问题 4:阶乘尾随零。作者:彭博

> -给定一个整数 n，返回 n 中尾随零的个数！
> 
> -跟进:你能写出一个对数时间复杂度的解决方案吗？
> 
> ——[https://leetcode.com/problems/factorial-trailing-zeroes/](https://leetcode.com/problems/factorial-trailing-zeroes/)

## 走过我的思考

这个问题有两个步骤:

1.  *计算 n 阶乘，n！*
2.  *计算尾随零的数量*

对于第一步，我们使用 while 循环迭代 n 阶乘，并停止循环，直到 1。对于第二步，事情变得有点棘手。

该问题要求尾随零，而不是零的总数。差别巨大。8 号！是 40，320，它有 2 个零，但只有 1 个尾随零。我们必须格外小心计算。我想出了两个解决方案。

## 解决方案 1:反向读取字符串

计算乘积的第一部分是不言而喻的。至于第二部分，我们用一个 str()方法把乘积转换成一个字符串然后反过来读:如果数字是 0，我们在计数上加 1；否则，我们就打破循环。

break 命令在这里是必不可少的。如上所述，上面的函数在没有 break 命令的情况下计算零的总数。

## 解决方案 2: while 循环

第一部分与解决方案 1 相同，唯一的区别是我们使用了一个 while 循环来计算尾部数字:对于要被 10 整除的乘积，最后一位数字必须是 0。因此，我们使用 while 循环不断更新 while 循环，直到条件不成立。

顺便说一下，解决方案 2 是我最喜欢的计算零的方法。

*(特别感谢*[](https://medium.com/u/a7d3764361bc?source=post_page-----59e6cf5ba2a0--------------------------------)**为改进方案。请参见代码的注释部分)。**

# *问题 5:完美的数字，亚马逊*

> *-一个完全数是一个正整数，等于它的正整数因子之和，不包括这个数本身。
> -整数 x 的除数是能把 x 整除的整数。
> -给定一个整数 n，如果 n 是完全数则返回 true，否则返回 false。
> -[https://leetcode.com/problems/perfect-number/](https://leetcode.com/problems/perfect-number/)*

## *走过我的思考*

*这个问题可以分为三步:*

1.  **求正约数**
2.  **计算总和**
3.  **决定:完美与否**

*第 2 步和第 3 步不言而喻，不超过一行程序。然而，棘手的部分是找到正约数。为此，我们可以采用野蛮的强制方法，从 1 到整数迭代整个序列。*

*理论上它应该对一个小整数起作用，但是如果我们对大整数运行，就超过了时间限制。时间效率不高。*

## *解决方案 1:暴力*

*这种方法不适用于大值。你的面试官可能会要求一个更有效的解决方案。*

## *解决方案 2: sqrt(n)*

*为了找到它的约数，我们不需要检查整数。例如，要找到 100 的约数，我们不必检查从 1 到 100 的数字。相反，我们只需要检查到 100 的平方根，也就是 10，并且所有其他可用的值都已经包括在内了。*

*这是一个优化的算法，可以节省我们大量的时间。*

**我的*[*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding)*上有完整的 Python 代码。**

# *外卖食品*

*   *做了几十次练习编码，最大的收获就是理解问题，把问题分成多个可操作的组件。*
*   *在这些可操作的部分中，总有一个步骤会绊倒候选人。像“不能使用内置函数/库”这样的约束。*
*   *关键是一步一步写好自定义函数，在自己的练习套路中尽量避免使用内置函数。*

**Medium 最近进化出了自己的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。**

*[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership)* 

# *我的数据科学面试序列*

*[](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [## 2021 年数据科学家必备的 SQL 技能

### 数据科学家/工程师的四项 SQL 技能

towardsdatascience.com](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [](/crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa) [## 破解数据科学访谈:基本的机器学习概念

### 赢在 2021 年:数据科学家/工程师的必读之作，第 1 部分

towardsdatascience.com](/crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa) [](/crack-data-science-interviews-essential-statistics-concepts-d4491d85219e) [## 破解数据科学访谈:基本统计概念

### 赢在 2021 年:数据科学家/工程师的必读之作，第 2 部分

towardsdatascience.com](/crack-data-science-interviews-essential-statistics-concepts-d4491d85219e) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。*