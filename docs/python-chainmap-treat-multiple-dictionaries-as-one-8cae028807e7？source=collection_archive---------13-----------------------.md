# Python ChainMap:将多个字典视为一个

> 原文：<https://towardsdatascience.com/python-chainmap-treat-multiple-dictionaries-as-one-8cae028807e7?source=collection_archive---------13----------------------->

## **ChainMap** 的实际使用案例，为初学者提供简单易懂的示例

![](img/b4d7bc0eef532a9d99596c105225494b.png)

由[布兰登·莫温克尔](https://unsplash.com/@bmowinkel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你可能以前从未听说过 **ChainMap** 。 **ChainMap** 是 Python **集合模块**中提供的另一个鲜为人知的**数据容器**。

在本文中，我将尝试解释 **ChainMap** 及其用例。一旦你读完它，你将有望理解 **ChainMap** 如何帮助解决你在编码时可能遇到的一些具体问题。

# **什么是 ChainMap？**

简而言之，它允许您将多个字典视为一个。它通过对多个字典对象提供一个单一的、可更新的视图来对多个字典进行分组。

假设我们的图书馆里有一些书，我们想把它们存储在一个带有作者-书名对的字典对象中。我们可以将具有相似风格的书籍分组并存储在单独的字典对象中，而不是将它们都存储在单个字典对象中。然后，它们可以包含在一个**链图**对象中。

```
**from collections import ChainMap**thriller = {'L.Foley': 'The Guest List', 'T.Fisher ': 'The Wives'}
romance = {'E.Henry': 'Beach Read', 'A.Hall': 'Boyfriend Material'}
science_fiction = {'M.Wells': 'Network Effect', 'K.Liu': 'The Hidden Girl'}my_library = **ChainMap**(thriller, romance, science_fiction)**str**(my_library)***Output:*** ChainMap({'L.Foley': 'The Guest List', 'T.Fisher ': 'The Wives'}, {'E.Henry': 'Beach Read', 'A.Hall': 'Boyfriend Material'}, {'M.Wells': 'Network Effect', 'K.Liu': 'The Hidden Girl'})
```

**ChainMap** 支持所有标准的 **dictionary 方法**，这样我们可以查询 **ChainMap** 对象，或者我们可以 **get()** ， **pop()** ，**update()**chain map 的项目，就像我们对常规 dictionary 对象所做的那样。

```
print(my_library['E.Henry'])
print(my_library.get('L.Foley'))
print(my_library.keys())**Output:**
Beach Read 
The Guest List 
KeysView(ChainMap({'L.Foley': 'The Guest List', 'T.Fisher ': 'The Wives'}, {'E.Henry': 'Beach Read', 'A.Hall': 'Boyfriend Material'}, {'M.Wells': 'Network Effect', 'K.Liu': 'The Hidden Girl'}))
```

当查找时，它按照从第一个到最后一个的顺序搜索。在我们的例子中，当我们用一个关键字在**我的 _ 库**中搜索时，它从 ***惊悚*** 字典开始，然后继续**浪漫**和**科幻**直到找到一个关键字。通过考虑我们可以定义的顺序，这使得一次搜索多个字典变得容易。

```
**from collections import ChainMap**thriller = {'Genre': 'thriller', 'L.Foley': 'The Guest List', 'T.Fisher ': 'The Wives'}romance = {'Genre': 'romance','E.Henry': 'Beach Read', 'A.Hall': 'Boyfriend Material'}science_fiction = {'Genre': 'science_fiction','M.Wells': 'Network Effect', 'K.Liu': 'The Hidden Girl'}my_library = **ChainMap**(thriller, romance, science_fiction)
**print**(my_library['Genre'])**Output:** thriller
```

Chainmap 通过引用存储字典。如果其中一个更新，链图内容也会更新。

```
**from collections import ChainMap**thriller = {'Genre': 'thriller', 'L.Foley': 'The Guest List', 'T.Fisher ': 'The Wives'}romance = {'Genre': 'romance','E.Henry': 'Beach Read', 'A.Hall': 'Boyfriend Material'}science_fiction = {'Genre': 'science_fiction','M.Wells': 'Network Effect', 'K.Liu': 'The Hidden Girl'}my_library = **ChainMap**(thriller, romance, science_fiction)**print**(my_library['Genre'])thriller['Genre'] = 'thriller and crime'**print**(my_library['Genre'])**Output:**
thriller 
thriller and crime
```

# **有什么帮助？**

**Chainmap** 比使用标准**字典**对象有三大优势；

它为多个映射对象提供了一个可更新的视图。这样你就可以一次搜索多个字典。

**它包含更多信息**，因为它具有分层结构，提供关于映射优先级的信息。当你搜索一个键值时，它从第一个映射开始搜索，并按照你将映射放入 **Chainmap** 对象的顺序继续搜索。

**如果您在应用程序中使用的映射足够大，访问起来足够昂贵，或者经常改变，它会提供更好的性能**。根据映射的大小，使用 **Chainmap 对象**而不是 o **rdinary 字典**可以提高应用程序的性能。

# 关键要点和结论

在这篇短文中，我解释了什么是链图对象，以及我们何时应该在 Python 中使用它们。关键要点是:

*   Chainmap 通过对多个字典对象提供一个可更新的视图来对多个字典进行分组。
*   Chainmap 有一个分层结构，它提供了关于映射优先级的信息。
*   如果您在应用程序中使用的映射足够大，访问起来足够昂贵，或者经常改变，那么它会提供更好的性能。

我希望你已经发现这篇文章很有用，并且**你将开始在你自己的代码**中使用 Chainmap 数据类型。