# 2021 年新手磨练 Python 技能的 3 个简单问题

> 原文：<https://towardsdatascience.com/3-simple-questions-to-hone-python-skills-for-beginners-in-2021-f12da38f83cf?source=collection_archive---------20----------------------->

## 破解数据科学面试

## 初级数据科学家的逐步改进

![](img/3c32d1f1ed549317213a7d44efe4af8a.png)

比尔·杰伦在 [Unsplash](https://unsplash.com/s/photos/rocket?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

对于初级数据科学家来说，用 Python 编码可能令人望而生畏。相信我，我也经历过。有一次，我在 Leetcode 上绞尽脑汁想出一道简单的关卡题，几个小时毫无进展。

在过去的一年里，我被刻意训练用 Python 编码。**我最大的收获是知道什么时候应该进入下一阶段，用更高级的问题挑战自己！如果我们停留在舒适区，练习旧的和熟悉的代码，就没有改进的空间。**

要想在 Python 编程方面出类拔萃，我们必须掌握基础知识，并快速进入下一阶段！在今天的帖子中，我列出了我们应该升级的三种情况。

*正如你们许多人所知，掌握 Python 已经成为我新的日常事务。它教会了我如何更好地编码，也教会了我个人的成长和学习。我很快会写更多关于这个话题的文章。*

# TL；速度三角形定位法(dead reckoning)

*   当我们必须计算每个元素(键)的值时，Python 字典就派上了用场。
*   使用弹出和推送操作，这样我们就不必更改数据类型。
*   尽可能采用数学公式。

# 问题 1:微软和亚马逊的优秀配对数量

> -给定整数 num 的数组。
> -如果 nums[i] == nums[j]和 i < j.
> 则一个对(I，j)称为好的-返回好的对的数量。
> -[https://leetcode.com/problems/number-of-good-pairs/](https://leetcode.com/problems/number-of-good-pairs/)

## **走过我的思维**

微软和亚马逊在他们的数据科学采访中包括了这个问题。我们应该返回等于特定位置要求(i

想到的第一个直觉是使用嵌套的 for 循环(又名，残暴的力量)来迭代序列，并最终返回计数。

## 解决方案 1:野蛮的力量&一个嵌套的 for 循环

```
4
```

它起作用了，但是很慢。一个 for 循环就足够了，更不用说嵌套的 for 循环了，它的时间复杂度是 O(N)。如果迭代次数很大，我们将会遇到运行时间问题。

新手程序员应该寻找更好的替代方案。

## 解决方案 2:字典和散列表

在这种情况下，字典是存储数据的更好的数据类型，因为它具有键值对属性。初学者可能知道什么是字典，但是很少能够利用键值特性。

我们将元素视为键，将出现次数视为值:对于每一次新的遇到，值增加 1，如果是第一次，则设置为 1。

```
4
```

只有一个时间复杂度为 O(N)的 for 循环。快多了！

*完整的 Python 代码在我的* [*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding) *上有。*

# 问题 2:阿姆斯特朗数，由亚马逊

> -k 位数字 N 是阿姆斯特朗数当且仅当每个数字的 k 次方之和为 N.
> -给定一个正整数 N，返回 true 当且仅当它是阿姆斯特朗数。
> -[https://leetcode.com/problems/armstrong-number/](https://leetcode.com/problems/armstrong-number/)

## 走过我的思考

亚马逊问这个问题。这是一个典型的数学问题。关键是要按照说明，决定这个数字是否符合标准。

一种简单的方法是将一个整数转换成一个字符串，然后遍历整个字符串，如解决方案 1 所示。

## 解决方案 1:使用 int()更改数据类型

```
True
```

这是一个可以接受的解决方案，但有时公司会设置额外的限制。例如，如果我们不能使用 int()转换数据类型，我们该怎么办？

试试 pop 和 push 操作！

## 解决方案 2:弹出和推送

弹出和推送操作是在不采用高级数据类型(例如，堆栈/数组)的情况下迭代字符串/整数的聪明方式。

一个简单的例子是合适的。

```
# pop operation
pop = number % 10 # step 1: obtain the last digit
number /= 10 # step 2: the remaining part without the last digit

# push operation 
temp = rev * 10 + pop 
rev = temp 
```

让我们看看这个问题是如何解决的。

```
True
```

while 循环非常棒。它在不改变数据类型的情况下对数字进行迭代。只有一个问题:我们在 while 循环中改变了 num 的原始值，并使用 num 的副本 *num1* 进行比较。

*我的*[*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding)*上有完整的 Python 代码。*

![](img/b7b29b61eed8b626cf88da0dbbf1b76b.png)

[Frans Vledder](https://unsplash.com/@fransvledder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/rocket?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# #问题 3:计算素数，作者 FAANG

> -计算小于非负数的素数的个数，n .
> -[https://leetcode.com/problems/count-primes/](https://leetcode.com/problems/count-primes/)

## 走过我的思考

令人惊讶的是，每个 FAANG 公司都问过这个问题。我保证如果我们不知道捷径，它会绊倒我们。

老实说，我的第一反应是迭代这个范围，直到感兴趣的数目，如果它是一个质数，就把它算进去。

## 解决方案 1:野蛮武力效率不高

```
3
```

从理论上讲，如果数量很小，比如我们的例子中的 7 个，这将是可行的。但是对于大值，它超出了运行时间。如果您很好奇，可以将这个数字设置为 100，000，然后运行上面的代码。需要几个小时，甚至几天才能得到结果。

这就是为什么 FAANG 会问一个后续问题:你能改进算法吗？

是的，我们可以！我们可以借助一个数学公式来计算素数的个数，直到一个数。

## 解决方案 2:厄拉多塞筛

我不知道厄拉多塞的筛子是什么，发现这个[网站](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)很有用([链接](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes))。

```
9592
```

基本思想是构造一个列表，并将所有值设置为 True，除了前两个位置 0 和 1 不是素数。然后，我们用厄拉多塞的**筛公式来判定非素数的位置。最后，我们计算列表的总和。在 Python 中，布尔值 True 等于 1，False 等于 0。因此，sum(primes)返回素数的总数，直到数字 n。**

得到结果只需要几秒钟，而不是几天。

我在过去的面试过程中没有遇到过这个问题，不确定如果我们不知道公式会发生什么。面试官能给你一些提示吗？请在评论中让我知道。

*我的*[*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding)*上有完整的 Python 代码。*

# 外卖食品

*   学习是一个过程，一个只要我们不断练习，不断实践，每个人都能变得更好的过程。
*   如果我们必须使用嵌套的 for 循环，请检查更好的替代方案。一本字典，也许？
*   读取字符串/整数有两种方式:1。更改数据类型；2.弹出和推送操作。
*   数学总是有用的。使用智能公式来减少运行时间。

*Medium 最近进化出了它的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership) 

# 我的数据科学面试序列

[](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [## FAANG 在 2021 年提出这 5 个 Python 问题

### 数据科学家和数据工程师的必读！

towardsdatascience.com](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [](/crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa) [## 破解数据科学访谈:基本的机器学习概念

### 赢在 2021 年:数据科学家/工程师的必读之作，第 1 部分

towardsdatascience.com](/crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Twitter](https://twitter.com/leihua_ye) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。