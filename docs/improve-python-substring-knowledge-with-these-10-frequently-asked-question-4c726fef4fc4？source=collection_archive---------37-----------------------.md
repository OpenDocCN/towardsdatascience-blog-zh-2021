# 通过这 10 个常见问题提高 Python 子串知识

> 原文：<https://towardsdatascience.com/improve-python-substring-knowledge-with-these-10-frequently-asked-question-4c726fef4fc4?source=collection_archive---------37----------------------->

## 与字符串切片、索引、子字符串搜索类型、替换、子字符串重复等相关的解释

![](img/fa4b5dcda7faeb1018559972dc437a5f.png)

设计生态学家在 [Unsplash](/s/photos/string?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

子串是字符串的一部分或子集。文本数据中的任何修改都在子串过程中进行。

例如:——“这是一个很好的 whether。我们有时应该出去走走。”是一个字符串。还有弦乐的一部分“我们有时应该出去走走。”是子字符串。

这一年来，我面对了很多子串相关的问题。我把它编译成了一个关于子串的常见问题。你可能已经知道一些子串问题。有些对你来说是新的。

你可以浏览一下名单。你可能会发现一些有趣的东西。

1.  关于字符串切片的所有内容
2.  为什么索引显示索引越界错误但不切片？
3.  检查字符串中是否存在子串
4.  在字符串中搜索子字符串
5.  字符串中存在的子字符串的百分比
6.  使用单词或字符将字符串拆分为子字符串
7.  获取句子的最后一个子串，不考虑长度
8.  替换子字符串
9.  统计子字符串在字符串中的出现次数
10.  字符串中重复子字符串的索引

让我们开始讨论这些问题。

# 1.关于字符串切片的所有内容

最常见和最基本的切片模板是 string[start _ index:end _ index:step]。

仅指定切片的起点(start_index)。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[8:]                                                                                                                                                           
Out[2]: "what happens when you're busy making other plans."
```

仅为切片指定端点(end_index)。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[:8]                                                                                                                                                           
Out[2]: 'Life is '
```

指定 start_index 和 end_index。保持 start_index < end_index.

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[8:21]                                                                                                                                                         
Out[2]: 'what happens '
```

Keep start_index > end_index。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[10:3]                                                                                                                                                         
Out[2]: ''
```

如果 start_index 值大于 end_index 值，那么 text_data 的切片将不返回任何内容。

从 text_data 中提取一个字符。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[10]                                                                                                                                                          
Out[2]: 'a'
```

在切片过程中添加一个步骤。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[8:21:2]                                                                                                                                                      
Out[2]: 'wa apn '
```

在切片过程中添加一个负步骤。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data[::-1]                                                                                                                                                        
Out[2]: ".snalp rehto gnikam ysub er'uoy nehw sneppah tahw si efiL"
```

负阶跃的输出也反转该单词。你可以用这个方法来检查回文单词。

示例:

```
In [16]: is_palindrome = 'malayalam'In [17]: reverse = is_palindrome[::-1]In [18]: reverse == is_palindrome                                                                                                                                               
Out[18]: True
```

请指定负的 start_index 和 end_index。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [19]: text_data[-6:-1]                                                                                                                                                       
Out[19]: 'plans'
```

在 python 中，字符串末尾的索引从-1 开始，而不是从 0 开始。

```
 L    I    f    e
 0    1    2    3
-4   -3   -2   -1
```

# 2.为什么索引显示索引超出范围错误，但不切片？

```
In [1]: index_error = 'Life'
In [2]: index_error[5]                                                                                                                                                          
--------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-2-83a895f5645d> in <module>
----> 1 index_error[5]IndexError: string index out of rangeIn [3]: index_error[5:]                                                                                                                                                         
Out[3]: ''
```

index_error[5]尝试返回索引 5 处的值。但是，如果索引 5 处的值不可用，那么它将显示一个错误。切片返回值的序列。因此，如果索引[5]处的值丢失，python 不会给出任何错误。

如果你不确定文本数据的长度，那么总是使用切片。

# 3.检查字符串中是否存在子串

我们将使用“in”操作符来检查字符串中是否存在子字符串。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: 'Life' in text_data                                                                                                                                                     
Out[2]: TrueIn [3]: 'plant' in text_data                                                                                                                                                    
Out[3]: False
```

# 4.在字符串中搜索子字符串

我们将使用 find()函数来查找字符串中存在的子字符串的起始索引。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: sub_text_var1 = 'happens'In [3]: text_data.find(sub_text_var1)                                                                                                                                           
Out[3]: 13In [4]: sub_text_var2 = 'plants'In [5]: text_data.find(sub_text_var2)                                                                                                                                           
Out[5]: -1
```

如果子串在字符串中不可用，find()函数将返回-1。

# 5.字符串中存在的子字符串的百分比

我们可以使用 Jaccard 相似度方法来找出一个字符串中子串重复的百分比。

如果一个子串与一个字符串相同，那么 jaccord_coeff 值为 0.5。如果 substring 和 string 之间的相似度是一半，那么 jaccord_coeff 值将是 0.25。

# 6.使用单词或字符将字符串拆分为子字符串

```
In [1]: text_data = "Life is what happens when you're busy making other plans."
```

假设您有一个业务需求，每当一个繁忙的单词出现在一个字符串中时，程序会将该字符串拆分为子字符串。您可以使用下面的代码来实现这一点。

```
In [2]: if 'busy' in text_data: 
   ...:     print(text_data.split('busy')) 
   ...:                                                                                                                                                                         
["Life is what happens when you're ", ' making other plans.']
```

您也可以使用单个字符拆分字符串。

```
In [3]: if '.' in text_data: 
   ...:     print(text_data.split('.')) 
   ...:                                                                                                                                                                         
["Life is what happens when you're busy making other plans", '']
```

# 7.获取句子的最后一个子串，不考虑长度

您可以使用下面的代码获取字符串的最后一部分。它也可以处理不同长度的句子。

```
In [1]: text_data_var1 = "Life is what happens when you're busy making other plans."In [2]: text_data_var2 = "Today is a beautiful day."In [3]: for i in [text_data_var1, text_data_var2]: 
   ...:     print(i.split()[-1]) 
   ...:                                                                                                                                                                         
plans.
day.
```

# 8.替换子字符串

您可以使用 replace()方法替换一个单词或一组单词。

```
In [1]: text_data = "Life is what happens when you're busy making other plans."In [2]: text_data.replace('other','no')                                                                                                                                         
Out[2]: "Life is what happens when you're busy making no plans."In [3]: text_data.replace('making other','with')                                                                                                                                
Out[3]: "Life is what happens when you're busy with plans."
```

# 9.统计子字符串在字符串中的出现次数

借助 find()和 replace()方法，您可以统计一个单词在文档中的出现次数。请在下面找到相关代码。

```
In [1]: text_data_test = "Life is what life happens when you're busy life making other life plans."In [2]: count = 0In [3]: while 'life' in text_data_test: 
   ...:     text_data_test = text_data_test.lower().replace('life','',1) 
   ...:     count+=1 
   ...:In [4]: f'occurrence of word life is {count}'                                                                                                                                   
Out[4]: 'occurrence of word life is 4'
```

如果删除 lower()方法，单词 life 的出现次数将为 3。因为 replace()是区分大小写的函数。

尝试更改函数 replace()的输入以获得不同的结果。

# 10.字符串中重复子字符串的索引

可以使用 find()方法在文本或字符串中定位子字符串的所有索引。

python 中没有任何内置函数，它可以给你一个字符串中所有重复子串的索引。所以，我们需要建立一个新的。你可以使用上面的代码或者写一个新的代码。

# 结论

Python 开发人员总是面临与 substring 相关的问题。这些只是与 substring 相关的 10 个最常出现的问题的汇编。在本文中，我解释了切片、重复字符串、在字符串中查找子字符串、索引外问题等。

如果您有任何与 substring 相关的新问题，请告诉我。

希望这篇文章能帮助你解决与 substring 相关的问题。