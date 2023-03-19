# 哈希表的基础

> 原文：<https://towardsdatascience.com/the-basics-of-hash-table-87d759bb0e36?source=collection_archive---------24----------------------->

## 哈希表基本上是一种提供快速插入、查找和删除的数据结构

![](img/23d12d4f94a56706b731f14e0ae7af69.png)

由[拉斯沃德](https://unsplash.com/@rssemfam?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我们这些活得够长的人一定知道或者至少看过黄页。没错。你说得对。它是一本厚厚的黄皮书，里面有企业名录和它们的电话号码。这使我们能够寻找销售我们所需商品的商家，并与他们联系。

(我不认为 Z 世代和之后的几代人会知道这本厚厚的黄皮书。😆)

电话号码簿通常是按字母顺序排列的，所以我们知道从哪里开始查找。一旦我们找到我们想要的企业名称，我们就可以拿到电话号码并给他们打电话。你明白了。💡

![](img/646458265f12696a7cc80c0cbaee15b2.png)

照片由 [Foto Sushi](https://unsplash.com/@fotosushi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 哈希表

如果我告诉你黄页或者电话簿是哈希表的一种实现呢？你打赌！🐴

哈希表本质上是一个与哈希函数耦合的数组。它通常用于以无序的方式存储键值数据，例如，企业及其电话号码、学生及其成绩、项目及其价格等等。

每个键必须是唯一的，并映射到数组中的特定索引，其值存储在该位置。🔑 ➡🚪注意，正因为如此，插入、搜索和删除操作都是⚡️速度。事实上，哈希表的插入、搜索和删除操作的平均时间复杂度是常数时间或`O(1)`。

因此，当您需要一个提供快速插入、查找和删除的数据结构时，哈希表是首选之一。当您有大量的关系键值数据时，这非常有用，例如在数据科学和/或机器学习算法中。

# 散列函数

哈希函数使得哈希表成为一种强大而有用的数据结构。哈希函数接受一段数据，或者通常称为一个键，并返回一个哈希代码作为输出。这个散列码是一个整数，然后被映射到数组中的一个索引，该值将存储在数组中。

哈希代码不直接与数组中的索引相关联的原因之一是因为哈希代码的值可能非常大，例如`10000000`，而我们想要存储的键值数据量(或者构成哈希表的数组的大小)可能不一定那么大。

将散列码映射到数组中的索引的一种简单方法是根据散列表的大小应用模运算。

```
index = hashCode(String key) % length(array)
```

让我们来看一个散列函数的例子。

```
*function int hashCode(String s) {
  return s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
}*
```

这是 Java `String`对象使用的哈希函数，其中`s`是`String`对象的字符数组，例如`s[0]`是第一个字符，`n`是`String`对象的长度。让我们看看它的实际效果。

```
hashCode("Apple") // 63476538
hashCode("Costco") // 2024204569
hashCode("Ikea") // 2280798
hashCode("Snowflake") // 1973786418
```

从☝️的几个例子中可以注意到，散列函数为每个`String`对象输出不同的散列码值。如前所述，每个键都应该是唯一的，因此哈希函数也应该为每个键生成唯一的哈希代码。这将为键值数据在哈希表中均匀分布提供更好的机会。

此外，让我们将散列码映射到数组中的索引。

```
function int mapToIndex(int hashCode, int arraySize) {
  return abs(hashCode % arraySize)
}// Let's assume the size of our hash table or the length of the array is 500hashTable = Array(500)mapToIndex(hashCode("Apple"), size(hashTable)) // 38
mapToIndex(hashCode("Costco"), size(hashTable)) // 69
mapToIndex(hashCode("Ikea"), size(hashTable)) // 298
mapToIndex(hashCode("Snowflake"), size(hashTable)) // 418
```

按照☝️的说法，苹果的数据将位于索引 38，好事多的数据位于索引 69，宜家的数据位于索引 298，雪花的数据位于索引 418。

![](img/f61d93534cfcce5ed4071d4e10cea184.png)

照片由[乌列尔·索伯兰斯](https://unsplash.com/@soberanes?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 碰撞

在上一节中，我们看了一个如何使用 hash 函数来确定特定键值数据在哈希表中的位置的示例。现在，让我们看看另一个散列函数的例子。

```
*function int hashCode(int i) {
  return i % 5
}*
```

这个哈希函数将一个整数作为输入，并应用模运算来输出哈希代码。现在，让我们用几个整数输入来测试一下。

```
hashCode(1) // 1
hashCode(5) // 0
hashCode(11) // 1
```

等一下。散列码不是应该对每个键都不同吗？在这种情况下，我们选择的散列函数对于我们可能拥有的可能的键来说可能并不理想。它为不同的输入值生成相同的哈希码，即输入值 1 和 11 将返回哈希码 1，这意味着它们将被映射到数组中的相同位置。这种现象叫做**碰撞**。

![](img/ba687e1f2ef89e5991f5a12e356b0b69.png)

由[谷仓图片](https://unsplash.com/@barnimages?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 处理冲突的两种方法

有两种常见的方法用于处理哈希表中的冲突:

*   线性探测
*   单独链接

![](img/311626b130f1d838931c4aef44395e08.png)

Clark Van Der Beken 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 线性探测

在线性探测中，我们通过在由哈希函数确定的假定位置之后搜索数组中最近的可用空间来处理哈希表中的冲突。让我们使用上一节中的冲突示例来想象一下，输入值 1 和 11 会产生相同的哈希代码。

假设我们将 1-Apple 的键值数据插入到哈希表中。哈希函数返回哈希代码 1，我们假设它映射到数组中的索引 1。现在，数组中的索引 1 包含 1-Apple 的键值数据。

接下来，我们要将 11-Orange 的键值数据添加到哈希表中。哈希函数返回哈希代码 1，它也映射到索引 1，但是，此时，数组的索引 1 中的块已经被键值数据 1-Apple 占用。

在这种情况下，线性探测将在数组中寻找最近的空闲位置，并在那里存储键值数据 11-Orange。当我们希望从键等于 11 的哈希表中检索数据时，情况也是如此。线性探测将首先找到键值数据 1-Apple，然后继续在相邻位置搜索，直到在数组中找到与键 11 匹配的块或空块，这表明键 11 不存在。

![](img/6343b95f265ae57891e654b79c78dce6.png)

由[郭佳欣·阿维蒂西安](https://unsplash.com/@kar111?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

## 单独链接

当使用分离链接方法时，哈希表将在链表中存储键值数据。这意味着数组中的每个块都包含一个链表，而不仅仅是一个键值数据。

因此，在发生冲突时，不是像线性探测那样寻找数组中的下一个空闲块，而是将键值数据添加到链表中。

同样，以上一节中的冲突示例为例，单独链接将在数组的索引 1 处的链表中存储 1-Apple 和 11-Orange 键-值对，其中链表的头将是 1-Apple，因为它被添加在 11-Orange 之前。

# 好的散列函数的特征

基于我们到目前为止所了解的，我们可以推断出一个好的散列函数应该具有以下特性。

## 利用数据中的每一条信息

好的散列函数应该利用数据的每个元素，以便增加可能的散列码的数量。

让我们看看下面的例子。

```
*function int hashCode(String k) {
  String s = k.substring(start=0, end=2)* *return s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
}*
```

这个散列函数接受一个类型为`String`的输入数据，取前 3 个字符并应用 Java `String`对象使用的散列函数。您可以想象，对于前三个字符相同的数据，例如 apple 和 application，会发生冲突。

在这种情况下，并不是所有的信息都用来生成散列码。如您所见，冲突的可能性更大，因为可用的哈希代码更少。

## 非常快的计算

我们使用哈希表的原因之一是它的插入、查找和删除速度。每个操作都依赖哈希函数来获得哈希代码，从而获得数据在数组中的位置。

因此，我们要确保我们的哈希函数超级快，以便提供我们在哈希表中寻找的速度。

这意味着我们应该避免使用复杂或缓慢操作的散列函数，并致力于快速、简单而有效的操作。

## 在表中均匀分布数据

对于我们的哈希函数来说，最理想的情况是将数据均匀地分布在哈希表中。如果散列函数没有均匀地分布数据，那么发生冲突的可能性就更大。

![](img/bfbf602172b73a8c80fca5a876aeb435.png)

由[阿尔文·恩格勒](https://unsplash.com/@englr?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 摘要

我们已经学习了哈希表的基础知识，包括它的结构，哈希函数是什么和做什么，冲突以及处理冲突的方法。下次当你面临一个需要快速查找、插入和删除的问题时，哈希表可能是你解决它的最佳选择。😺