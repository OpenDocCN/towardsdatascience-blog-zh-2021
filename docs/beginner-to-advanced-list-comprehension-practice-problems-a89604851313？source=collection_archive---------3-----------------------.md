# 初级到高级列表理解练习题

> 原文：<https://towardsdatascience.com/beginner-to-advanced-list-comprehension-practice-problems-a89604851313?source=collection_archive---------3----------------------->

## 列表理解、字典理解和嵌套列表理解的练习题

![](img/bad01d087a0bed89210ecb793a8d3bc1.png)

rawpixel.com 创建的背景向量—[www.freepik.com](http://www.freepik.com)

# 介绍

假设我想创建一个从 1 到 1000 的数字列表，并编写了以下代码…

你能找出它有什么问题吗？

```
numbers = []
for i in range(1,1001):
   numbers.append(i)
```

恶作剧问题。上面的代码没有问题，但是有一个更好的方法可以用**列表理解达到同样的结果:**

```
numbers = [i for i in range(1, 1001)]
```

列表理解非常好，因为它们需要更少的代码行，更容易理解，并且通常比 for 循环更快。

虽然列表理解并不是最难理解的概念，但它一开始肯定看起来不直观(对我来说的确如此！).

这就是为什么我给你八个练习题，让你把这个概念钻到脑子里！我将首先在顶部为您提供问题列表，然后提供答案。问题 1-5 相对简单，而问题 6-8 则稍微复杂一些。

说到这里，我们开始吧！

# 问题

```
**# Use for the questions below:**nums = [i for i in range(1,1001)]string = "Practice Problems to Drill List Comprehension in Your Head."
```

1.  找出 1-1000 中所有能被 8 整除的数字
2.  找出 1-1000 中所有包含 6 的数字
3.  计算字符串中的空格数(*使用上面的字符串*)
4.  删除一个字符串中的所有元音(*使用上面的字符串*)
5.  查找字符串中少于 5 个字母的所有单词(*使用上面的字符串*)
6.  使用字典理解来计算句子中每个单词的长度(*使用*上面的字符串)
7.  使用嵌套列表理解来查找 1-1000 中除 1(2-9)之外的所有可被任何单个数字整除的数字
8.  对于所有的数字 1-1000，使用嵌套列表/字典理解来找出任何数字可被整除的最高一位数

## 1.找出 1-1000 中所有能被 8 整除的数字

```
q1_answer = [num for num in nums if num % 8 == 0]
```

## 2.找出 1-1000 中所有包含 6 的数字

```
q2_answer = [num for num in nums if "6" in str(num)]
```

## 3.计算字符串中的空格数

```
q3_answer = len([char for char in string if char == " "])
```

## 4.删除字符串中的所有元音

```
q4_answer = "".join([char for char in string if char not in ["a","e","i","o","u"]])
```

## 5.查找字符串中少于 5 个字母的所有单词

```
words = string.split(" ")
q5_answer = [word for word in words if len(word) < 5]
```

## 6.使用字典理解来计算句子中每个单词的长度

```
q6_answer = {word:len(word) for word in words}
```

## 7.使用嵌套列表理解来查找 1-1000 中除 1(2-9)之外的所有可被任何单个数字整除的数字

```
q7_answer = [num for num in nums if True in [True for divisor in range(2,10) if num % divisor == 0]]
```

## 8.对于所有的数字 1-1000，使用嵌套列表/字典理解来找出任何数字可被整除的最高一位数

```
q8_answer = {num:max([divisor for divisor in range(1,10) if num % divisor == 0]) for num in nums}
```

# 感谢阅读！

我希望这对你有用。如果你能够用列表或字典的理解来解决这些问题，我想可以说你对这个概念有很强的理解。

如果你觉得这很有用，并且想要更多这样的文章，请在评论中告诉我:)

一如既往，我祝你学习一切顺利。

不确定接下来要读什么？我为你挑选了另一篇文章:

[](/a-complete-52-week-curriculum-to-become-a-data-scientist-in-2021-2b5fc77bd160) [## 2021 年成为数据科学家的完整 52 周课程

### 连续 52 周，每周学点东西！

towardsdatascience.com](/a-complete-52-week-curriculum-to-become-a-data-scientist-in-2021-2b5fc77bd160) 

**又一个！**

[](/50-statistics-interview-questions-and-answers-for-data-scientists-for-2021-24f886221271) [## 2021 年数据科学家的 50 多个统计面试问题和答案

### 一个更新的资源，为你的面试刷统计知识！

towardsdatascience.com](/50-statistics-interview-questions-and-answers-for-data-scientists-for-2021-24f886221271) 

# 特伦斯·申

*   ***如果你喜欢这个，*** [***跟我上媒***](https://medium.com/@terenceshin) ***了解更多***
*   ***有兴趣合作吗？让我们连线上***[***LinkedIn***](https://www.linkedin.com/in/terenceshin/)
*   ***报名我的邮箱列表*** [***这里***](https://forms.gle/tprRyQxDC5UjhXpN6) ***！***