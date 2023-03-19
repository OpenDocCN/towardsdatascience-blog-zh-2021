# Python 字符串插值的 3 个必备方法

> 原文：<https://towardsdatascience.com/3-must-know-methods-for-python-string-interpolation-99e6d90b439c?source=collection_archive---------30----------------------->

那份书面声明怎么样？

![](img/bf0ca95802a03722e134fb8bd7586656.png)

Jordi Moncasi 在 [Unsplash](https://unsplash.com/s/photos/edit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

字符串插值是一种将变量嵌入字符串的方法。与键入整个纯文本字符串不同，占位符是在可以保存变量值的字符串中实现的。

字符串插值允许我们更有效地使用 print 语句。无论是为了调试代码还是确认结果，print 语句很可能会出现在您的脚本中。

在本文中，我们将介绍 Python 中字符串插值的三种方法。他们可以用不同的方式做同样的事情。看完例子，你大概会选择自己喜欢的方式。

## 用占位符格式化

这是一种古老的方法，现在已经不常用了。“%”运算符用作变量的占位符。

这里有一个简单的例子。

```
result = 20print("The result is %s." % result)
The result is 20.
```

我们也可以在一个字符串中添加多个占位符。

```
name = "John"
id = 22print("Hello %s, your id is %s." % (name, id))
Hello John, your id is 22.
```

字母 s 与%运算符一起使用，表示占位符。这是一个老方法，所以我们不会去详细。但是，了解这一点仍然很重要，因为您很可能会遇到使用这种方法包含字符串的代码。

## 字符串格式。格式()

它类似于前面的方法，但是我们用花括号代替“%s”。

```
result = 20print("The result is {}".format(result))
The result is 20.name = "John"
id = 22print("Hello {}, your id is {}.".format(name, id))
Hello John, your id is 22.
```

变量按照它们的顺序放在花括号中。还有一个从 0 开始的与花括号相关的顺序。我们可以通过在里面写数字来操纵这个顺序。

```
print("Hello {1}, your id is {0}.".format(name, id))
Hello 22, your id is John.
```

索引 0 与 name 变量相关联，因此它被写入指定 0 索引的位置。

我们也可以用一个指定的名字来指代每个花括号。在这种情况下，格式中的顺序并不重要。

```
year = 2000
price = 100000print("The house is built in {year_built} and worth {value}"\
.format(value = price, year_built = year))The house is built in 2000 and worth 100000.
```

如果要打印字符串中的小数点，您可以调整要显示的小数位数。

```
# without adjustment
result = 1.346343454353print("The result of is {result}".format(result = result))
The result of is 1.346343454353# rounded up to 2 decimals
print("The result of is {result: 1.2f}".format(result = result))
The result is 1.35
```

在表达式“1.2f”中，2 代表小数位数，1 代表变量的大小。它几乎总是被用作 1，但是让我们用不同的值做一个例子来看看它的效果。

```
print("The result of is {result: 10.2f}".format(result = result))
The result of is       1.35
```

## 格式化字符串文字(f 字符串)

格式化字符串，也称为 f 字符串，是三种字符串中最新的一种。Python 3.6 引入了它们。

f 字符串使用花括号作为变量占位符。然而，我们可以将变量名写在花括号内，而不是在最后指定它们。

```
name = "John"
id = 22print(f"Hello {name}, your id is {id}.")
Hello John, your id is 22.
```

我们将字母 f 放在字符串字符的前面，告诉 Python 这是一个 f 字符串。

f 字符串比其他两种方法更具可读性。对于开发者来说也更容易。在有许多变量的情况下，跟踪字符串中的变量就成了一项繁琐的任务。

在最后指定变量也容易出错。f 字符串提供的是清晰的语法和易读的代码。

f 弦也可以进行浮点调整。

```
result = 1.346343454353print(f"The result of is {result: 1.2f}")
The result of is  1.35
```

如果您需要在打印之前修改变量，那么使用 f 字符串进行这样的修改会更容易。这里有一个例子。

```
location = "Houston,TX"
price = 100000print(f"The house is in {location[:-2]} and is worth {price * 2}")
The house is in Houston, and is worth 200000
```

我们省略州，价格翻倍。其他方法也是可行的。然而，使用 f 字符串跟踪这些修改相对容易，尤其是当有许多变量时。

## 结论

我们经常使用 print 语句，不仅是为了确认结果，也是为了调试。字符串插值允许我们操纵或修改字符串，以充分利用绝版语句。

虽然这三种方法都能胜任，但我更喜欢 f 弦，因为我认为这是最实用的一种。最终，可读性很重要，从这个角度来看 f 字符串更好。

感谢您的阅读。如果您有任何反馈，请告诉我。