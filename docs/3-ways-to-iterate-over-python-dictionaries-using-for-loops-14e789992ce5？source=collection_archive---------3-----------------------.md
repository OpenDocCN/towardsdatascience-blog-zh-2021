# 使用 For 循环迭代 Python 字典的 3 种方法

> 原文：<https://towardsdatascience.com/3-ways-to-iterate-over-python-dictionaries-using-for-loops-14e789992ce5?source=collection_archive---------3----------------------->

## …以及 Python 字典上最流行的堆栈溢出问题的其他答案。

[![](img/85d2fb2fa989ed7759653a7ce43fbe31.png)](https://anbento4.medium.com/)

来自 [Pexels](https://www.pexels.com/photo/white-mug-3152022/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的 [Ana M.](https://www.pexels.com/@ana-m-229817?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 的照片

*你们中的许多人联系我，要求提供有价值的资源* ***来敲定基于 Python 的数据工程面试*** *。下面我分享 3 个我强烈推荐的点播课程:*

*   [**Python 数据工程 nano degree**](https://imp.i115008.net/jWWEGv)**→***优质课程+编码项目如果有时间可以提交。* **→** [***通过此环节获得七折优惠***](https://imp.i115008.net/jWWEGv)
*   [***leet code In Python:50 算法编码面试问题***](https://click.linksynergy.com/deeplink?id=533LxfDBSaM&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Fleetcode-in-python-50-algorithms-coding-interview-questions%2F)**→最适合涉及算法的编码回合！**
*   *[***Python 高级编码问题(StrataScratch)***](https://platform.stratascratch.com/coding?via=antonello)***→****我找到准备 Python 的最佳平台& SQL 编码面试到此为止！比 LeetCode 更好更便宜。**

*希望你也会发现它们有用！现在欣赏:D 的文章*

# *介绍*

*Python 字典被定义为数据值的集合，其中的项以键值对的形式保存。因此，字典也被称为关联数组。*

*如果您对 Python 相对陌生，或者您正在准备下一轮编码，您可能会偶然发现一些需要与字典交互的算法。*

*然而，似乎字典不仅在新手中不断产生兴趣，在更有经验的开发者中也是如此。实际上，纵观[所有时代的顶级堆栈溢出 Python 问题](https://stackoverflow.com/questions/tagged/python?tab=Votes)，似乎投票最多的三个主题是:*

*   ****如何使用‘for’循环迭代字典？****
*   ****如何检查给定的键是否已经存在于字典中？****
*   ****如何给字典添加新键？****

*在这篇文章中，我将试图为你提供一个简洁明了的答案。这将使你免于浏览网上的大量评论。*

*让我们从头开始！👆👆🏽👆🏻*

# ****如何使用‘for’循环迭代字典？****

*为了回答这个问题，我创建了一个字典，其中包含一个模拟网上银行交易的数据:*

```
*transaction_data = {
 ‘transaction_id’: 1000001,
 ‘source_country’: ‘United Kingdom’,
 ‘target_country’: ‘Italy’,
 ‘send_currency’: ‘GBP’,
 ‘send_amount’: 1000.00,
 ‘target_currency’: ‘EUR’,
 ‘fx_rate’: 1.1648674,
 ‘fee_pct’: 0.50, 
 ‘platform’: ‘mobile’
}*
```

## *方法 1:迭代使用 For 循环+索引*

*在 Python 中遍历字典最简单的方法是直接将它放在一个`for`循环中。Python 会自动将`transaction_data`视为一个字典，并允许你迭代它的键。*

*然后，为了访问这些值，可以使用索引操作符`[]`将每个键传递给字典:*

```
***# METHOD 1 - Unsorted**
for key in transaction_data:
   print(key, ‘:’, transaction_data[key])**Output[1]:**
transaction_id : 1000001
source_country : United Kingdom
target_country : Italy
send_currency : GBP
send_amount : 1000.0
target_currency : EUR
fx_rate : 1.1648674
fee_pct : 0.5
platform : mobile*
```

*如你所见，这些键不是按字母顺序排列的。为了实现这一点，您应该简单地将`transaction_data`传递给`sorted()`方法:*

```
***# METHOD 1 - Sorted**
for key in sorted(transaction_data):
   print(key, ‘:’, transaction_data[key])**Output[2]:**
fee_pct : 0.5
fx_rate : 1.1648674
platform : mobile
send_amount : 1000.0
send_currency : GBP
source_country : United Kingdom
target_country : Italy
target_currency : EUR
transaction_id : 1000001*
```

## *方法 2:迭代使用。keys( ) +索引*

*使用返回包含字典键的 Python 对象的`.keys()`方法可以获得相同的结果。*

*当您只需要迭代字典的键时，这尤其方便，但是它也可以与索引操作符结合使用来检索值:*

```
***# METHOD 2**
for key in transaction_data.keys():
    print(key, '-->', transaction_data[key])**Output[3]:**
transaction_id --> 1000001
source_country --> United Kingdom
target_country --> Italy
send_currency --> GBP
send_amount --> 1000.0
target_currency --> EUR
fx_rate --> 1.1648674
fee_pct --> 0.5
platform --> mobile*
```

## *方法 3:迭代使用。项目( )*

*然而，遍历字典最 *"pythonic"* 和最优雅的方式是使用`.items()`方法，该方法以元组的形式返回字典条目的视图:*

```
*print(transaction_data.items())**Output[4]:** dict_items([('transaction_id', 1000001), 
            ('source_country', 'United Kingdom'), 
            ('target_country', 'Italy'), 
            ('send_currency', 'GBP'), 
            ('send_amount', 1000.0), 
            ('target_currency', 'EUR'), 
            ('fx_rate', 1.1648674), 
            ('fee_pct', 0.5), 
            ('platform', 'mobile')])*
```

*为了迭代`transaction_data`字典的键和值，您只需要‘解包’嵌入在元组中的两个项目，如下所示:*

```
***# METHOD 3**
for k,v in transaction_data.items():
    print(k,’>>’,v)**Output[5]:** transaction_id >> 1000001
source_country >> United Kingdom
target_country >> Italy
send_currency >> GBP
send_amount >> 1000.0
target_currency >> EUR
fx_rate >> 1.1648674
fee_pct >> 0.5
platform >> mobile*
```

*请注意， **k** 和 **v** 只是“key”和“value”的标准别名，但是您也可以选择其他命名约定。例如，使用 **a** 和 **b** 会导致相同的输出:*

```
*for a,b in transaction_data.items():
    print(a,’~’,b)**Output[6]:** transaction_id ~ 1000001
source_country ~ United Kingdom
target_country ~ Italy
send_currency ~ GBP
send_amount ~ 1000.0
target_currency ~ EUR
fx_rate ~ 1.1648674
fee_pct ~ 0.5
platform ~ mobile*
```

## *通过嵌套字典进行额外迭代🤓*

*但是如果你需要迭代一个像`transaction_data_n`这样的嵌套字典呢？在这种情况下，每个键代表一个事务，并有一个字典作为值:*

```
***transaction_data_n** = {
 ‘transaction_1’:{
 ‘transaction_id’: 1000001,
 ‘source_country’: ‘United Kingdom’,
 ‘target_country’: ‘Italy’,
 ‘send_currency’: ‘GBP’,
 ‘send_amount’: 1000.00,
 ‘target_currency’: ‘EUR’,
 ‘fx_rate’: 1.1648674,
 ‘fee_pct’: 0.50, 
 ‘platform’: ‘mobile’
 },
 ‘transaction_2’:{
 ‘transaction_id’: 1000002,
 ‘source_country’: ‘United Kingdom’,
 ‘target_country’: ‘Germany’,
 ‘send_currency’: ‘GBP’,
 ‘send_amount’: 3320.00,
 ‘target_currency’: ‘EUR’,
 ‘fx_rate’: 1.1648674,
 ‘fee_pct’: 0.50, 
 ‘platform’: ‘Web’
 },
 ‘transaction_3’:{
 ‘transaction_id’: 1000003,
 ‘source_country’: ‘United Kingdom’,
 ‘target_country’: ‘Belgium’,
 ‘send_currency’: ‘GBP’,
 ‘send_amount’: 1250.00,
 ‘target_currency’: ‘EUR’,
 ‘fx_rate’: 1.1648674,
 ‘fee_pct’: 0.50, 
 ‘platform’: ‘Web’
 }
}*
```

*为了解开属于每个嵌套字典的键值对，可以使用下面的循环:*

```
***#1\. Selecting key-value pairs for all the transactions**for k, vin transaction_data_n.items():
    if type(v) is dict:
        for **nk, nv** in v.items(): 
            print(**nk**,’ →’, **nv**)**#nk and nv stand for *nested key* and *nested value*****Output[7]:** transaction_id --> 1000001
source_country --> United Kingdom
target_country --> Italy
send_currency --> GBP
send_amount --> 1000.0
target_currency --> EUR
fx_rate --> 1.1648674
fee_pct --> 0.5
platform --> mobile
transaction_id --> 1000002
source_country --> United Kingdom
target_country --> Germany
send_currency --> GBP
send_amount --> 3320.0
target_currency --> EUR
fx_rate --> 1.1648674
fee_pct --> 0.5
platform --> Web
transaction_id --> 1000003
source_country --> United Kingdom
target_country --> Belgium
send_currency --> GBP
send_amount --> 1250.0
target_currency --> EUR
fx_rate --> 1.1648674
fee_pct --> 0.5
platform --> Web-----------------------------**#2\. Selecting key-value pairs for 'transaction_2 only'**for k, v in transaction_data_n.items():
    if type(v) is dict and k == 'transaction_2':
        for **sk**, **sv** in v.items():
            print(**sk**,'-->', **sv**)**Output[8]:**
transaction_id --> 1000002
source_country --> United Kingdom
target_country --> Germany
send_currency --> GBP
send_amount --> 3320.0
target_currency --> EUR
fx_rate --> 1.1648674
fee_pct --> 0.5
platform --> Web*
```

# ****如何检查给定的键是否已经存在于字典中？****

*您可以使用`in`操作符在 Python 字典中检查成员资格。*

*特别是，假设您想要检查`send_currency`字段是否可以作为`transaction_data`中的一个键。在这种情况下，您可以运行:*

```
*‘send_currency’ in transaction_data.keys()**Output[9]:** True*
```

*同样，要检查值`GBP`是否已经分配给字典中的一个键，您可以运行:*

```
*‘GBP’ in transaction_data.values()**Output[10]:** True*
```

*然而，上面的检查不会立即告诉你`GBP`是分配给`send_currency`键还是`target_currency`键的值。为了确认这一点，您可以向`values()`方法传递一个元组:*

```
*(‘send_currency’, ‘GBP’) in transaction_data.items()**Output[10]:** True('target_currency', 'GBP') in transaction_data.items()**Output[11]:** False*
```

*如果`transaction_data`字典包含数百个值，这将是检查`GBP`是否确实是该特定事务的`send_currency`的最佳方式。*

*得心应手！😏😏😏*

# ****如何给字典添加一个新键？****

*最后，让我们假设，在某个时候，分析团队要求您将`user_address`和`user_email`字段添加到字典中可用的数据中。你将如何实现？*

*有两种主要方法:*

*   *使用方括号`[]`符号:*

```
*transaction_data['user_address'] = '221b Baker Street, London - UK'for k,v in transaction_data.items():
    print(k,’:’,v)**Output[12]:** transaction_id : 1000001
source_country : United Kingdom
target_country : Italy
send_currency : GBP
send_amount : 1000.0
target_currency : EUR
fx_rate : 1.1648674
fee_pct : 0.5
platform : mobile
user_address : 221b Baker Street, London - UK*
```

*   *或者，您可以使用`update()`方法，但是需要更多的输入:*

```
*transaction_data.update(user_email=’user@example.com’)for k,v in transaction_data.items():
    print(k,’::’,v)**Output[13]:**
transaction_id :: 1000001
source_country :: United Kingdom
target_country :: Italy
send_currency :: GBP
send_amount :: 1000.0
target_currency :: EUR
fx_rate :: 1.1648674
fee_pct :: 0.5
platform :: mobile
user_address :: 221b Baker Street, London - UK
user_email :: user@example.com*
```

# *结论*

*在这篇文章中，我分享了 3 种方法来使用“for”循环遍历 Python 字典，并高效地提取键值对。但是，请注意，还存在更多的*【python 化】*解决方案(即*字典理解*)。*

*尽管是一个相对基础的话题，“如何迭代 Python 字典？”，是[关于堆栈溢出](https://stackoverflow.com/questions/tagged/python?tab=Votes)的投票最多的问题之一。*

*出于这个原因，我还回答了另外两个非常流行的关于检查成员资格和向 Python 字典添加新的键值对的堆栈溢出问题。*

*我的希望是，你会用这篇文章在同一个地方澄清你对字典的所有疑问。学习代码很有趣，而且会永远改变你的生活，所以继续学习吧！*

## *给我的读者一个提示*

*这篇文章包括附属链接，如果你购买的话，我可以免费给你一点佣金。*