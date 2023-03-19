# Python 中“臭名昭著”的算法:二分搜索法

> 原文：<https://towardsdatascience.com/binary-search-in-python-the-programming-algorithm-8b8fa039eaa?source=collection_archive---------20----------------------->

## 数据结构和算法

## 为什么它的性能优于线性迭代？

![](img/5d040a45fe7723d047022abd476efac6.png)

凯西·马蒂亚斯在 [Unsplash](https://unsplash.com/s/photos/half?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 介绍

让我们从两个问题开始今天的话题。

> 你学习算法的第一印象是什么？
> 
> 到底什么是算法？

如今，有几十个像“比特”、“智能”、“人工智能”、“学习”、“监督学习”和“算法”这样的热门词汇在架子顶上。

但是很少有人理解这些词的真正含义，更不用说它们是如何工作的了。

> “这是什么？
> 
> 它是做什么的？"

你问过了。

对我来说，算法是我们希望计算机如何处理信息的过程，什么先什么后。

就是这样。

就这么简单。

我在两篇文章( [Python](/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13) 和 [FAANG](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) )中详细阐述了数据科学面试变得越来越受编程驱动和面向 CS 的主题。在今天的帖子中，我将介绍二分搜索法的基本知识和意识形态，并回答 FAANG 提出的一些真实的面试问题。二分搜索法绝对是经过最严格测试的算法之一，如果不是唯一的话，它非常容易掌握。

# 二分搜索法:什么和如何

官方将其定义为“在一个排序数组中找到目标值位置的搜索算法”，并将目标值与中间元素进行比较，检查它们是否等价。如果不相等，则移到下一个区间进行比较(改编自 [Wiki](https://en.wikipedia.org/wiki/Binary_search_algorithm) )。

非官方的，我的直观定义是将搜索范围一分为二，询问计算机所选区间是否包含该值:如果区间小于目标值，则上移搜索空间，重新搜索；否则，向下移动。

一场猜谜游戏即将开始。它叫做“我的号码是多少？”基本上，我从 1 到 100 中挑选一个数字，你的工作就是通过问我问题来找到这个数字。

求数有两种方式:无效率和有效率。第一，你可以问我数字是不是 1；如果不是，检查范围内的其余数字，直到 100。如果我选了 100，你需要 100 次才能选对。这就是所谓的野蛮暴力法，虽然有效，但效率很低。如果数字在 1 到 100 万之间，你会怎么做？

第二，更聪明有效的方法是问这样的问题:

> **#1 问题:“你的数字高于 50 吗？”**

“没有。”

所以，数字必须是从 1 到 49！我们甚至不需要检查任何高于 50 的值。

> **#2 问题:“你的数字高于 25 吗？”**

“是的。”

现在，数字必须在 26 和 49 之间！没有必要检查从 1 到 25 的数字。

两个问题之下，我们缩小了真数的范围！你不会花太长时间最终得到它。

这是玩这个游戏时的算法思维。尽管二分搜索法的飞行名气很大，但它有一种脚踏实地的方法。最终，算法会告诉计算机如何处理信息。谈到算法，这是我的第一条规则。

![](img/b0626db85748b516b96a0b4d605c0ce0.png)

[马里奥·高](https://unsplash.com/@mariogogh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/half?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# Python 实现

基础学完了，就该用 Python 写点代码了。在这一部分，我通过科技公司问的三个面试问题来体验代码。每个问题都有几种解决方案，在应用二分搜索之前，我将介绍使用内置函数的方法。

通常对于 Python 编码挑战，你的面试官允许你使用任何内置的库和方法。只要您获得了预期的输出，您就可以开始了。随后，他们会增加额外的限制，比如你不能使用这个库/包，或者提高你的内存和速度。

出于教学目的，我介绍了这两种方法，请在准备技术面试时练习这两种方法。

***以下问题按难度从大到小排序*** 。这是二分搜索法算法的内部笑话。学完今天的内容，你会笑。

## 问题 1:Sqrt(x ), FAANG

> -给定一个非负整数 x，计算并返回 x 的平方根
> -由于返回类型为整数，小数位数被截断，只返回结果的整数部分。
> ——比如 8 的平方根是 2.82842，但是这个函数应该只返回整数部分，2。
> -[https://leetcode.com/problems/sqrtx/](https://leetcode.com/problems/sqrtx/)

## 走过我的思考

该问题要求得到一个数的平方根，如果它不是完美的平方根，则只返回整数部分。两步之内就能完成。

> # 1.求 x 的平方根；
> 
> # 2.只返回整数部分。

下面是 Python 代码。

```
2
```

numpy floor()和 int()方法的作用相同，它们只返回整数部分，而忽略小数部分(如果有的话)。但是它们的输出是不同的数据类型:floor()得到一个浮点数，int()返回一个整数。

你可能同意，这很容易被包括在 FAANG 的技术面试中。最有可能的是，你的面试官要求你不要用内置的方法，应用一些算法。

在这里，二分搜索法派上了用场。二分搜索法算法有三个步骤。在我们开始之前，请确保数组已经排序，或者减少或者增加。

> #第一步。定义搜索空间:左、右和中间
> 
> #第二步。我们大胆猜测，从中间开始搜索算法。
> 
> #第三步。使用“if-else”语句检查目标值和搜索空间中的每个元素。

```
3
```

前两步不言而喻，让我们将第三步分解成不同的场景。第一个 if 条件(步骤 3.1)很简单，如果 root_squared 和 x 相同，它将返回根值。

elif 条件值得解释。如上所述，二分搜索法就像在玩一个从 1 到 100 的猜谜游戏，每次你都可以问一个问题:它是高于还是低于一个数字。条件说如果平方根小于 x，那么根号不够大，我们需要上移，下次再猜一个更大的数。

else 语句规定了当上述两个条件不满足时该做什么。如果平方根大于我们的数字 x，计算机应该向下移动搜索空间，就像我们之前玩的猜谜游戏一样。

所有这些条件都会在 while 循环中自动结束运行。最后，我们返回搜索空间的上界，aka。*右*。

如此简洁的解决方案！我总是说一个好的算法应该有直观的意义，二分搜索法说的很有道理。

## #问题 2:二分搜索法，亚马逊、微软、贝宝和彭博

> -给定一个排序的(按升序)n 个元素的整数数组 nums 和一个目标值，写一个函数在 nums 中搜索目标。如果目标存在，则返回其索引；否则，返回-1。https://leetcode.com/problems/binary-search/
> -

## 走过我的思考

对于这个问题，我的首选方法不是二分搜索法，而是应用一个 ***enumerate*** ()方法来迭代链表，如下图所示:

```
4
```

结果显示该数字等于其在位置 4 的索引。可以使用列表理解将 for 循环进一步压缩成一行代码。

现在，让我们改变思路，使用二分搜索法。

## 解决办法

```
4
```

这是二分搜索法算法的标准实现，分别检查了 if-elif-else 语句。

到目前为止，您应该对算法的基本形式有了很好的理解。是时候实践该算法更复杂的应用了。

![](img/7138448fa8573ef46b7e7233896e59f9.png)

在 [Unsplash](https://unsplash.com/s/photos/half?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## #问题 3:检查一个数是否是排序数组中的多数元素，作者脸书

> -给定一个按非降序排序的数组 nums 和一个数字目标，当且仅当目标是多数元素时，返回 True。
> -多数元素是在长度为 N 的数组中出现 N/2 次以上的元素
> -[https://leet code . com/problems/check-if-A-number-is-majority-element-in-A-sorted-array/](https://leetcode.com/problems/check-if-a-number-is-majority-element-in-a-sorted-array/)

## 走过我的思考

相比较而言，这是一个稍微复杂一点的问题。我的第一个方法是线性方法，即找到数组中的多数元素，然后判断目标值是否等于多数元素。

如果允许内置函数，我们可以这样做:

```
Result for the 1st test case is:  True
Result for the 2nd test case is:  False
```

简单地说，我们从集合中导入 Counter，遍历数组，如果条件成立，则返回 True。

如果反制方法不可行，事情会变得很糟糕，但我们不会完蛋。

二分搜索法可以扭转乾坤。

像我一样，如果这是你第一次学习算法，可能会有些不知所措。所以，我创建了这个问题的三个版本(难度递增)。一旦我们理解了如何回答前两个问题，最终(原始)版本就变得更“可解”

## 基本版本 1:使用二分搜索法检查目标值是否在一个排序数组中。

轻松点。这是算法的标准实现。我们已经涵盖了物流，并在问题 1 和 2 中提供了详细的解释。

## 基本版本 2:使用二分搜索法查找排序数组中某个元素的第一次和最后一次出现。

接下来，让我们增加一些趣味，找到一个元素的第一次出现(最左边)和最后一次出现(最右边)。

找到元素的第一个和最后一个位置很重要，因为它将多数元素定义为长度为 N//2 的元素。如果我们能够确定最左边/最右边的索引，我们就能很好地找到最终的解决方案。

向左搜索第一个匹配项

```
The Leftmost Occurrence is at Position: 2
```

有趣的是。在基础版本 2.1 中，搜索空间内的唯一区别是只有两个条件需要检查。

> 1.当选择的元素小于目标值时，我们需要向上移动左边界。
> 
> 2.否则，元素大于或等于目标值，我们决定中间为上限。

向右搜索最后一次出现的内容

```
The Rightmost Occurrence is at Position: 11
```

类似地，我们可以使用上面的代码找到该元素的最后一次出现。

*完整的 Python 代码在我的* [*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding) *上有。*

## #高级版本 3.1:使用二分搜索法检查目标是否为多数情况

原问题 3

```
True
```

哇！

代码看起来很复杂，但坦白说很简单。

初始部分叫做 ***一个辅助函数*** ，和其他任何函数是一回事。唯一的区别是我们将在外部函数中使用 helper 函数提供的功能。

helper 函数( ***从第 8 行到第 16 行*** )查找元素最左边的位置，这在计算第 24 行和第 25 行中元素的第一次和最后一次出现时很方便。最后，我们检查最右边和最左边位置之间的差值范围是否大于 N//2。

我在我的 [Github](https://github.com/LeihuaYe/Python_LeetCode_Coding) 上包含了一个 3.1 版本的改进解决方案。

恭喜你的算法之旅。

# 外卖食品

*   **成长心智(** [**学会如何学习**](https://www.coursera.org/learn/learning-how-to-learn) **)。踏入未知的领域是可怕的。有了成长的头脑，如果我们坚持做下去，我们会做得更好。我过去通常害怕 Python 和编码。我担心如果我不知道如何写一个简单的 for 循环，我会显得多么笨拙。所有的负面情绪都在持续，很难集中精力学习。用 Python 写代码，解决有挑战性的谜题，带给我如此多的快乐和满足感。Python 是我的 ***禅*** 。**
*   **算法**。这是我们用来告诉计算机如何处理信息的另一种语言。在其花哨的标题下，算法简单明了，就像我们玩的猜谜游戏一样。
*   **Practice makes perfect**. 熟能生巧 (It’s the Chinese equivalence). Rome wasn’t built overnight. All of these popular expressions tell us one thing: no substitute for hard work. Practice daily, and we will be better at coding, Python, algorithm, and everything and anything.

*Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

<https://leihua-ye.medium.com/membership>  

# 我的数据科学面试序列

</crack-data-science-interviews-five-sql-skills-for-data-scientists-cc6b32df1987>  </6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13>  </5-python-coding-questions-asked-at-faang-59e6cf5ba2a0>  

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。