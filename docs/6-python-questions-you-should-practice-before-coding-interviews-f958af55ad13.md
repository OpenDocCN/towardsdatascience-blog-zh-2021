# 2021 年 6 个 Python 数据科学面试问题

> 原文：<https://towardsdatascience.com/6-python-questions-you-should-practice-before-coding-interviews-f958af55ad13?source=collection_archive---------7----------------------->

## 破解数据科学面试

## 数据科学家/工程师的数据操作和字符串提取

![](img/6bdf3f17643935edc175792f53819868.png)

Gabriel Sollmann 在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

如前一篇帖子所述([链接](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0))，数据科学/工程面试有四个编码组成:**数据结构&算法、机器学习算法、数学与统计、数据操作**(查看此[精彩文章](/the-ultimate-guide-to-acing-coding-interviews-for-data-scientists-d45c99d6bddc)作者[艾玛丁](https://medium.com/u/1b25d5393c4f?source=post_page-----afd6a0a6d1aa--------------------------------) @Airbnb)。之前的帖子是关于数学和统计的。万一你没有机会，这里是入口:

[](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) [## FAANG 在 2021 年提出这 5 个 Python 问题

### 数据科学家和数据工程师的必读！

towardsdatascience.com](/5-python-coding-questions-asked-at-faang-59e6cf5ba2a0) 

在这篇文章中，我将重点关注**数据操作**，并回顾我对主要科技公司，尤其是 FAANG 提出的 6 个真实问题的思考。

*我的*[*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding)*上有完整的 Python 代码。*

# TL；速度三角形定位法(dead reckoning)

*   本质上，这种类型的面试问题，数据操纵，期望考生对逻辑有高层次的理解。
*   它归结为 Python 或另一种语言的基础编程。
*   想出一个解决方案，然后尝试找到一个更高效的算法。

# 问题 1:亚马逊取消 IP 地址

> -给定一个有效的(IPv4) IP 地址，返回该 IP 地址的默认版本。
> -一个被取消的 IP 地址替换每个句号。“带“[。]".
> -[https://leetcode.com/problems/defanging-an-ip-address/](https://leetcode.com/problems/defanging-an-ip-address/)

## 走过我的思考

这是一个热身问题。它要求我们更换。带“[。]"表示一个字符串。如果你熟悉 Python 中的 string，首先想到的就是对待“.”作为分隔符并拆分字符串。然后，重新连接一个空字符串，并选择“[。]"作为分隔符。

第一种解决方案是对这类问题的标准逐步解决方法。但是，这不是最佳解决方案。

更 Pythonic 化的解决方案是使用 replace()方法直接更改“.”带“[。]”，如下图。

## 解决办法

```
'1[.]1[.]1[.]1'
```

在我的日常实践中，我尽量多写第一个解决方案，因为它涉及两个常见的字符串方法:split 和 join。我建议实践这两种解决方案，不管它们是高效还是低效。

![](img/859353a1ad671dc392dd8853385becfa.png)

[皮卡伍德](https://unsplash.com/@pickawood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 问题 2:彭博微软公司的 Fizz Buzz

> -编写一个程序，打印 1 到 50 的数字，对于 2 的倍数打印“嘶嘶”而不是数字，对于 3 的倍数打印“嘶嘶”，对于 2 和 3 的倍数都打印“嘶嘶”
> 
> 【https://leetcode.com/problems/fizz-buzz/】-T4

## 走过我的思考

这个问题可以重写为多个“如果…那么…”语句，这些语句提醒我们控制流。具体来说，我们可以这样做:“如果条件 A 满足，那么输出结果 A…”

只有一个问题:有些数字是 2 和 3 的倍数，所以我们必须在控制流的顶部做出决定。

```
# pseudo code for i in range(1,51):
     if condition 1 met: 
          print('fizzbuzz')
     elif condition 2 met: 
          print('fizz')
     elif condition 3 met: 
          print('buzz') 
     else: 
          print(i)
```

由于只需要检查三个条件，我们可以写一个控制流。然而，存在子条件的数量太多的情况，并且不可能将它们列出来([例如，问题 1 均匀分布](/statistical-simulation-in-python-part-2-91f71f474f77))。你的面试官可能会要求你使用除控制心流以外的其他方法。

考虑到这一点，我们创建了两个条件，如果其中一个或两个条件都满足，就增加关键字“Fizz”和“Buzz”。

```
# pseudo code
condition_1 = (num%2 ==0) # for fizz
condition_2 = (num%3 ==0) # buzzif condition_1 or condition_2: 
     return (condition_1*'Fizz') + (condition_2*'Buzz')
```

## 解决办法

```
1
fizz
buzz
fizz
5
fizzbuzz
7
fizz
buzz
fizz
11
fizzbuzz
13
fizz
buzz
fizz
17
fizzbuzz
.
.
.
```

![](img/c2b5b6a4f09e27b221262d272aa2d15b.png)

照片由 [Devon Divine](https://unsplash.com/@lightrisephoto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# #问题 3:回文数字，谷歌、FB 和微软

> - Identity 以下句子中所有为回文的单词“做还是不做数据科学家，这不是问题。问你妈，lol。”
> -如果同一个单词出现多次，则返回该单词一次 ***(条件 1)***
> -回文:从开头读到结尾或者从结尾读到开头时返回相同结果的单词 ***(条件 2)***
> -[https://leetcode.com/problems/palindrome-number/](https://leetcode.com/problems/palindrome-number/)

## 走过我的思考

回文数有多种变化。这个版本的棘手之处在于每个句子都包含一个约束。

例如，如果一个单词出现多次，只需返回该单词一次，这就建议用 Python 中的 set 作为输出数据类型(**条件 1** )。

一个回文是这样的:无论你从头读到尾还是反过来读，都返回相同的结果(**条件 2** )。信格类型怎么样？是否区分大小写？在这里，你应该要求澄清问题。

此外，还有一个隐藏的条件，那就是当我们决定一个单词是否是回文时，我们应该去掉标点符号。

如果你没有马上得到所有这些条件，不要担心，因为你的面试官会给你提示。

## 解决办法

```
{'a', 'lol', 'mom'}
```

![](img/5e9a18a18cc68d63570ee18038ec34be.png)

照片由[吕山德元](https://unsplash.com/@lysanderyuen?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# #问题 4:字符串中的第一个唯一字符，FAANG

> -给定一个字符串，查找其中的第一个非重复字符并返回其索引。
> -如果不存在，返回-1。
> -[https://leet code . com/problems/first-unique-character-in-a-string/](https://leetcode.com/problems/first-unique-character-in-a-string/)

## 走过我的思考

问题要求字符串中第一个不重复的字符，相当于第一个只出现一次的字符。在字符串中，我们可以使用 count 方法计算子字符串的出现次数。

我被这个问题耽搁了好一会儿，想不出解决办法。我尝试过使用字典来存储键-值对并返回第一个非重复值的索引，但没有成功。

## 解决办法

```
0
```

第一个唯一字符是字符串“I”(位置 0)。

![](img/06180b57613fc2fdcddf4c236a31542b.png)

保罗·花冈在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# #问题 5:谷歌、亚马逊和 Adobe 的独特电子邮件地址

> -每封电子邮件都由本地名称和域名组成，用@符号分隔。
> ——比如在[alice@leetcode.com](mailto:alice@leetcode.com)中，爱丽丝是本地名，leetcode.com 是域名。
> -除了小写字母，这些邮件可能包含“.”s 或'+'s.
> -如果添加句点('.')在电子邮件地址的本地名称部分的一些字符之间，发送到那里的邮件将被转发到本地名称中没有点的相同地址。例如，“[alice.z@leetcode.com](mailto:alice.z@leetcode.com)”和“[alicez@leetcode.com](mailto:alicez@leetcode.com)”转发到同一个邮件地址。(注意，该规则不适用于域名。)
> -如果在本地名称中添加一个加号('+')，第一个加号之后的所有内容都将被忽略。这允许过滤某些电子邮件，例如 m.y+name@email.com 的[邮件将被转发给 my@email.com 的](mailto:m.y+name@email.com)[邮件。(同样，该规则不适用于域名。)
> -可以同时使用这两个规则。
> -给定一个电子邮件列表，我们向列表中的每个地址发送一封电子邮件。实际上有多少不同的地址接收邮件？
> ——【https://leetcode.com/problems/unique-email-addresses/】T21](mailto:my@email.com)

## 走过我的思考

乍一看，这是一个相当长的问题，令人望而生畏。它只是包含了太多的信息，我的大脑已经超负荷了。实际上，在我们忽略琐碎信息后，这是一个简单的问题。

分解之后，只有三个部分:

> 1.电子邮件由两部分组成:本地名称和域名，用“@”分隔。
> 
> 2.对于本地名称，“”没有效果，我们应该跳过它；任何“+”后面的都无所谓，省去。
> 
> 3.多少个唯一的电子邮件地址？

对于每一步，我们分别有应对策略。

由于输入是一个字符串列表，我们可以对元素(string)进行迭代，并对每个元素将其分成两部分(步骤 1)。

我们删除“.”从本地名称开始，保留原来的域名，并读取字符串直到包含“+”的位置(步骤 2)。

我们使用 set()来计算唯一元素的数量。

## 解决办法

```
2
```

![](img/e67358de2449846fab8a42f17542778d.png)

[杰克 B](https://unsplash.com/@nervum?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/library?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

# 问题 6 目的地城市，Yelp

> -给定数组路径，其中 paths[i] = [cityAi，cityBi]表示存在从 cityAi 到 cityBi 的直接路径。
> ——返回目的城市，即没有任何路径外向另一个城市的城市。
> -保证路径图形成一条没有任何回路的线，因此，目的地城市只有一个。
> -[https://leetcode.com/problems/destination-city/](https://leetcode.com/problems/destination-city/)

## 走过我的思考

我把最好的留到了最后。这个问题是 Yelp 问的，很好吃。原因如下。

对于每个路径，paths[i]具有两个部分[cityAi，cityBi]: cityAi 是出发城市，cityBi 是着陆城市。关键是要理解目的地城市的定义:是没有出发城市的城市。换句话说，目的地城市应该只出现在第二列路径[i]中，而不会出现在第一列中。

理解这一点后，剩下的就不言而喻了，因为我们可以从出发城市中剔除落地城市。

## 解决办法

```
'Sao Paulo'
```

*我的*[*Github*](https://github.com/LeihuaYe/Python_LeetCode_Coding)*上有完整的 Python 代码。*

# 外卖食品

*   数据操作最具挑战性的部分是理解问题背后的逻辑。
*   即使是最复杂的编程也涉及到 Python 基础，例如，字符串访问和操作、不同的数据类型、for 和 while 循环等。
*   沟通还是王道！在进入编码部分之前，询问需要澄清的问题。

*Medium 最近进化出了自己的* [*作家伙伴计划*](https://blog.medium.com/evolving-the-partner-program-2613708f9f3c) *，支持像我这样的普通作家。如果你还不是订户，通过下面的链接注册，我会收到一部分会员费。*

[](https://leihua-ye.medium.com/membership) [## 阅读叶雷华博士研究员(以及其他成千上万的媒体作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

leihua-ye.medium.com](https://leihua-ye.medium.com/membership) 

# 我的数据科学面试序列

[](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77) [## 2021 年数据科学家面临的 4 个棘手的 SQL 问题

### 可能会让你犯错的简单查询

towardsdatascience.com](/4-tricky-sql-questions-for-data-scientists-in-2021-88ff6e456c77) [](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [## 2021 年数据科学家必备的 SQL 技能

### 数据科学家/工程师的四项 SQL 技能

towardsdatascience.com](/essential-sql-skills-for-data-scientists-in-2021-8eb14a38b97f) [](/crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa) [## 破解数据科学访谈:基本的机器学习概念

### 赢在 2021 年:数据科学家/工程师的必读之作，第 1 部分

towardsdatascience.com](/crack-data-science-interviews-essential-machine-learning-concepts-afd6a0a6d1aa) 

# 喜欢读这本书吗？

> 请在 [LinkedIn](https://www.linkedin.com/in/leihuaye/) 和 [Youtube](https://www.youtube.com/channel/UCBBu2nqs6iZPyNSgMjXUGPg) 上找到我。
> 
> 还有，看看我其他关于人工智能和机器学习的帖子。