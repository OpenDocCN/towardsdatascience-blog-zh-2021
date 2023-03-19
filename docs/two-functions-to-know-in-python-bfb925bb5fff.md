# Python 中需要了解的两个函数

> 原文：<https://towardsdatascience.com/two-functions-to-know-in-python-bfb925bb5fff?source=collection_archive---------27----------------------->

## 了解如何使用 Python 中的 **any()** 和 all()函数

![](img/49315176bdb1cd3fae6245368590aa7b.png)

照片由 [Gema Saputera](https://unsplash.com/@gemasaputera?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

所有 Python 程序员都应该知道 ***和*** 和 ***或*** 运算符在 Python 中是什么，以及如何使用它们。然而，有时有更好的方法来完成这些操作员执行的相同任务。

在本文中，我们将回顾两种这样的方法: ***all()*** 和 ***any()*** 函数，它们提供了更简洁、更易于阅读和解释的代码。

## 满足条件

假设有一个数据科学职位的招聘信息。该职位要求具备以下所有条件:*至少 10 年 SQL 经验，12 年机器学习经验，8 年统计分析经验*。换句话说，如今对*入门级*级数据科学职位的典型要求是*。*

## 逻辑积算符

我们可以编写一个函数，利用候选人多年的经验，通过使用 ***和*** 运算符返回 ***真*** 或 ***假*** 来检查候选人是否满足所有要求:

```
**def** is_qualified(sql, ml, stats):
    **if** (sql >= 10) **and** (ml >= 12) **and** (stats >= 8):
        **return True
    else:
        return False**
```

> 上述函数使用 ***和*** 运算符来检查所有条件或操作数是否为 ***真*** 。如果所有表达式评估为 ***真*** ，那么 ***和*** 运算符将返回 ***真*** 。

所以如果一个候选人有 11 年的 SQL 经验，12 年的机器学习经验，9 年的统计分析经验，函数会返回 ***真*** :

```
is_qualified(11, 12, 9)
# True
```

但如果另一个候选人有 9 年的 SQL，12 年的机器学习，9 年的统计分析经验，函数会返回 ***False*** :

```
is_qualified(9, 12, 9)
# False
```

## all()函数

编写这个函数的另一种方法是用<https://docs.python.org/3/library/functions.html#all>**函数代替。**

> **全部(可重复)**

*****all()*** 函数接受一个可迭代对象，比如一个列表，并检查该可迭代对象中的所有元素是否都是 ***真值*** 。换句话说，如果所有元素评估为 ***True*** ( *或者如果 iterable 为空*)，则 ***all()*** 将返回 ***True*** 。**

> **记住，**真值**是指任何评估为**真值**的值(而**假值**是任何评估为**假值**的值)。**

**由此，我们可以将上面的***is _ qualified()***函数改写如下:**

```
****def** is_qualified(sql, ml, stats):
    requirements = [
        sql >= 10,
        ml >= 12,
        stats >= 8
    ]
   ** return all(**requirements**)****
```

> **我们创建一个包含表达式的列表，这些表达式需要为 ***真*** 才能使候选人有资格获得该职位。如果 ***需求*** 中的所有元素都求值为*，那么 ***all(*** *需求* ***)*** 求值为****真*** 返回。这是编写这个函数的一种更干净、更直观的方式。****

## ***or 运算符***

***假设另一个招聘启事只要求以下其中一项:*至少 10 年 SQL 经验，或者 12 年机器学习经验，或者 8 年统计分析经验*。***

***我们可以编写一个函数，通过使用 ***或*** 运算符来检查其中任何一个是否为*:****

```
******def** is_qualified(sql, ml, stats):
    **if** (sql >= 10) **or** (ml >= 12) **or** (stats >= 8):
       ** return True
    else:
        return False**is_qualified(11, 11, 7)
# Trueis_qualified(9, 10, 7)
# False****
```

> ****我们在上面的函数中使用了 ***或*** 运算符。如果其中任何一个表达式的计算结果为 ***真*** ，则 ***或*** 运算符将返回 ***真*** 。如果所有表达式评估为 ***假*** ，则 ***或*** 运算符返回 ***假*** 。****

## *******任意()函数*******

****同样，还有一种更简单的方法，那就是使用[***any()***](https://docs.python.org/3/library/functions.html#any)函数。****

> ***任何(可重复)***

***顾名思义， ***any()*** 函数接受一个 iterable，并检查是否有任何元素的值为 ***True*** 。如果至少有一个元素是***True***(计算结果为 ***True*** )，那么它将返回 ***True*** 。***

> ***如果 iterable 为空，any()函数返回 False。如果 iterable 为空，all()函数将返回 True。***

```
*****def** is_qualified(sql, ml, stats):
    requirements = [
        sql >= 10,
        ml >= 12,
        stats >= 8
    ]
    **return any(**requirements**)*****
```

> ***就是这样！ ***any()*** 函数将检查需求中的任何元素是否评估为 ***真*** 。如果是，则返回 ***真*** 。如果没有一个元素评估为 ***真*** ，那么它将返回 ***假*** 。***

## ***绕过***

***无论是 ***any()*** 和 ***all()*** 都会在它们知道返回什么的那一刻让**短路**执行。换句话说，如果 ***all()*** 函数遇到一个***False***值，那么它会立即返回 ***False*** 。而如果 ***any()*** 函数遇到一个 ***Truthy*** 值，那么它会立即返回 ***True*** 。因此，并不总是需要消耗整个 iterable，从而提高性能。***

***为什么会出现这种情况的原因可以在 ***any()*** 和 ***all()*** 的 Python 实现中看到:***

```
*****def** all(iterable):
    **for** element **in** iterable:
        **if** **not** element:
            **return** **False**
    **return** **True****def** any(iterable):
    **for** element **in** iterable:
        **if** element:
            **return** **True**
    **return** **False*****
```

> ***在上面的 ***all()*** 函数中，如果一个元素的计算结果为 ***False*** ，则该函数返回 ***False*** (从而停止迭代 iterable)。记住**返回**语句将结束一个函数的执行。***
> 
> ***在上面的 ***any()*** 函数中，如果一个元素的计算结果为 ***True*** ，则该函数返回 ***True*** (从而停止迭代 iterable)。***

***如果你喜欢阅读这样的故事，并想支持我成为一名作家，考虑注册成为一名媒体会员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用我的 [***链接***](https://lmatalka90.medium.com/membership) *注册，我会赚一小笔佣金。****

***<https://lmatalka90.medium.com/membership> *** 

***我希望这篇关于 ***any()*** 和 ***all()*** 函数的简短教程是有帮助的。感谢您的阅读！***