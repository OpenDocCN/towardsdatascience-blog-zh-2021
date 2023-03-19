# Python 模板字符串格式化方法

> 原文：<https://towardsdatascience.com/python-template-string-formatting-method-df282510a87a?source=collection_archive---------11----------------------->

## 关于模板字符串的详细信息，这是 python 中格式化字符串的一个选项。当使用正则表达式时，它可能更合适

![](img/2525e372037c5e6d22260d8a214de5fe.png)

照片由来自[佩克斯](https://www.pexels.com/photo/woman-programming-on-a-notebook-1181359/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[克里斯蒂娜·莫里洛](https://www.pexels.com/@divinetechygirl?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

模板字符串是 Python 中用来格式化字符串的另一种方法。与%运算符相比，。format()和 f 字符串，它有一个(可以说)更简单的语法和功能。对于国际化(i18n) 来说，这是[的理想选择，并且有一些细微差别您可能会发现是有利的——尤其是在使用正则表达式( **regex** )时。](https://docs.python.org/3/library/string.html#template-strings)

我们先看看其他三个的语法，这样就可以比较了。

## %运算符

```
>>> name = "Alfredo"
>>> age = 40
>>> "Hello, %s. You are %s." % (name, age)
'Hello, Alfredo. You are 40.'
```

## 。格式()

```
>>> print('The {} {} {}'.format('car','yellow','fast')) **#empty braces** The car yellow fast>>> print('The {2} {1} {0}'.format('car','yellow','fast')) **#index** The fast yellow car>>> print('The {f} {y} {c}'.format(c='car',y='yellow',f='fast')) **#keywords** The fast yellow car
```

## f 弦

```
>>> name = "Peter"
>>> print(f'Nice to meet you, {name}')
```

# 模板字符串

它使用字符串模块中的模板类。它的语法有点类似于。format()，但是它没有使用花括号来定义占位符，而是使用了美元符号($)。${}也是有效的，当占位符后出现有效字符串时，它应该出现。

参见各种情况的语法。我将开始解释 safe_substitute 和 regex 的用法。

## 安全替换和正则表达式

假设您想要替换包含{}、%和$的字符串中的某个内容。使用正则表达式时会发生这种情况。

考虑一个输入字段，该字段接受公司股票代码加上百分比形式的正(+)或负(-)值，之后不接受任何内容。并且符号加号+被动态替换。比如:AAPL: +20%或者 TSLA: -5%(我知道这可能是一个愚蠢的用例！但是请忽略。只是一个例子)。

regex:([symbol:+或-][0–9]{ 1，4}[%])$)

对于%运算符，。format()和 f 字符串—不起作用。%运算符和。format()会引发错误，f-strings 会移除{m，n}。见下文

```
**#% operator**
>>> print('(([%s][0-9]{1,4}[%])$)' % "AAPL: -")
*TypeError: not enough arguments for format string***#.format()** >>> print('(([{symbol_pos_neg}][0-9]{1,4}[%])$)'.format(symbol_pos_neg = 'AAPL: +'))
*KeyError: '1,4'***#f-strings** >>> symbol_pos_neg = "AAPL: +"
>>> print(f'(([{symbol_pos_neg}][0-9]{1,4}[%])$)')
(([AAPL: +][0-9]*(1, 4)*[%])$)
```

虽然通过对字符进行转义(将花括号或百分号加倍)可以很容易地解决这个问题，但我觉得这很不方便。使用模板字符串，您不需要这样做。{}和%不用于定义占位符，并且通过使用 **safe_substitute，**您不必替换所有的$。

见下文，正则表达式不变，$symbol_pos_neg 被正确替换。

```
>>> print(Template('(([$symbol_pos_neg][0-9]{1,4}[%])$)').**safe_substitute**(symbol_pos_neg='AAPL: -'))(([AAPL: -][0-9]{1,4}[%])$)
```

## **带关键字的常规案例**

```
>>> from string import Template
>>> Template('$obj is $colour').substitute(obj='Car',colour='red')
'Car is red'
```

## 带字典的普通箱子

```
**>>>** d = dict(obj='Car')
**>>>** Template('$obj is red').substitute(d)
'Car is red'
```

## **占位符后跟无效字符**

如果占位符后有无效字符串，则只考虑占位符。示例，请参见“.”$who 后面的点号。正常打印。

```
>>> from string import Template
>>> Template('$obj**.** is $colour').substitute(obj='Car',colour='red')
'Car. is red'
```

## **占位符后跟有效字符**

如果占位符后有有效字符，则必须用花括号括起来。

```
>>> from string import Template
>>> Template('${noun}ification').substitute(noun='Ident')
'Identification'
```

请注意，下划线(_)等字符也被认为是有效的。因此，使用${}的规则同样适用。

## 多个$$$符号

替换照常进行，并打印出一个额外的$值。

```
>>> Template(’$obj. is $$$colour’).substitute(obj=’Car’,colour=’red’)
'Car. is $red'
```

# 最后的想法

我仍然是 Python 的初学者，比较字符串格式化方法教会了我很多。我希望我已经清楚地向你提供了细节。尽管模板字符串的功能[不如](https://grski.pl/fstrings-performance.html):

```
viniciusmonteiro$ python3 -m timeit -s "x = 'f'; y = 'z'" "f'{x} {y}'"5000000 loops, **best of 5: 82.5 nsec per loop**viniciusmonteiro$ python3 -m timeit -s "from string import Template; x = 'f'; y = 'z'" "Template('$x $y').substitute(x=x, y=y)"  # template string500000 loops, **best of 5: 752 nsec per loop**
```

我认为在某些情况下，它在简单和方便方面带来了一些好处。虽然我知道这些可能是主观的。

# 参考

[1] `[string](https://docs.python.org/3/library/string.html#module-string)` —常见字符串操作[https://docs . python . org/3/library/string . html # template-strings](https://docs.python.org/3/library/string.html#template-strings)

[2] Python 3 的 f-Strings:改进的字符串格式化语法(指南)[https://realpython.com/python-f-strings/](https://realpython.com/python-f-strings/)

[3]Python 中不同字符串连接方法的性能——为什么 f 字符串如此出色[https://grski.pl/fstrings-performance.html](https://grski.pl/fstrings-performance.html)