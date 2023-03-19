# Python 中的 Dunder/Magic 方法

> 原文：<https://towardsdatascience.com/dunder-magic-methods-in-python-3d6453f6311b?source=collection_archive---------40----------------------->

## Dunder 方法的名称前面和后面都有双下划线，因此被称为 dunder。它们也被称为魔术方法，可以帮助覆盖自定义类的内置函数的功能。

![](img/63eba0280e2fca050b6a3d403fd6df46.png)

由[美元吉尔](https://unsplash.com/@dollargill?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/magic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 1.介绍

为类实现 dunder 方法是一种好的多态形式。如果您曾经在 Python 中创建了一个类并使用了 init 函数，那么您已经在使用 dunder 方法了。

# 2.目录

1.介绍

2.目录

3.先决条件

4.为什么我们需要邓德方法？

5.我们的定制类

6.我们班的邓德方法

7.更多的邓德方法

8.结论

# 3.先决条件

在我们继续之前，了解以下内容非常重要:

*   对使用 Python 进行面向对象编程的基本理解。
*   在 Python 中使用类的经验。
*   熟悉 len、get、set 等内置函数。

# 4.为什么我们需要邓德方法？

考虑这样一种情况，我们有以下类:

```
class point:
    x = 4
    y = 5p1 = point()
print(p1)
```

print 语句将打印类似于`<__main__.point object at 0x7fb992998d00>`的内容。但是，我们可能希望 print 语句以格式`(4,10)`显示一些内容。我们可以通过重写我们类的`__str__`方法来实现这一点。

我们也可以覆盖其他方法，比如`len, +, []`等。我们将创建一个新的类，并覆盖本文中的许多内置函数。

# 5.我们的定制类

```
class softwares:
    names = []
    versions = {}
```

这个类将用于保存软件及其版本的列表。`names`是一个存储软件名称的列表，`versions`是一个字典，其中键是软件名称，值是版本号。默认情况下，所有软件都以版本 1 开始。

# 6.我们班的邓德方法

在继续之前，请确保您的缩进是正确的。下面将要讨论的方法是属于我们创建的类的方法，必须适当缩进。

## 6.1.初始化

如果您曾经使用过类，那么您一定已经使用过这种方法。`init`方法用于创建类的一个实例。

```
def __init__(self,names):
    if names:
        self.names = names.copy()
        for name in names:
            self.versions[name] = 1
    else:
        raise Exception("Please Enter the names")
```

上面定义的`init`方法接受一个名称列表作为参数，并将其存储在类的‘`names`列表中。此外，它还填充了`versions`字典。我们还在`names`列表上做了检查。

如果列表为空，则会引发异常。下面是我们如何使用`init`方法。

```
p = softwares(['S1','S2','S3'])
p1 = softwares([])
```

第一条语句可以正常工作，但是第二行将引发一个异常，因为一个空列表作为参数传入。

## 6.2.潜艇用热中子反应堆（submarine thermal reactor 的缩写）

当我们想在 print 语句中使用类的实例时,`str`方法很有用。如前所述，它通常返回一个内存对象。但是我们可以覆盖`str`方法来满足我们的需求。

```
def __str__(self):
    s ="The current softwares and their versions are listed below: \n"
    for key,value in self.versions.items():
        s+= f"{key} : v{value} \n"
    return s
```

上面的`str`方法返回软件及其版本。确保该函数返回一个字符串。下面是我们调用该方法的方式。

```
print(p)
```

## 6.3.设置项目

在字典中赋值时，调用`setitem`方法。

```
d = {}
d['key'] = value
```

在`setitem`方法的帮助下，我们可以给我们类的实例一个相似的特性。

```
def __setitem__(self,name,version):
    if name in self.versions:
        self.versions[name] = version
    else:
        raise Exception("Software Name doesn't exist")
```

上面的方法是要更新软件的版本号。如果找不到该软件，它将引发一个错误。

在第 3 行中，我们使用了字典的内置`setitem`方法。

我们可以通过以下方式调用`setitem`方法:

```
p['S1'] = 2
p['2'] = 2
```

第一行将软件 S1 的版本更新为 2。但是第二行将引发一个异常，因为软件 2 不存在。

## 6.4.getitem

`getitem`方法类似于`setitem`方法，主要区别在于当我们使用字典的`[]`操作符时会调用`getitem`方法。

```
d = {'val':key}
print(d['val'])
```

我们类的实例也可以被赋予类似的特性。

```
def __getitem__(self,name):
    if name in self.versions:
        return self.versions[name]
    else:
        raise Exception("Software Name doesn't exist")
```

上面的方法本质上是返回软件的版本。如果找不到该软件，它将引发一个异常。为了调用`getitem`方法，我们可以编写下面一行代码。

```
print(p['S1'])
print(p['1'])
```

第一行将打印 S1 的版本。但是，第二行将引发一个异常，因为 1 不存在。

## 6.5.交货

`delitem`与`setitem`和`getitem`方法相同。为了避免重复，我们将继续讨论实现和用例。

```
def __delitem__(self,name):
    if name in self.versions:
        del self.versions[name]
        self.names.remove(name)
    else:
        raise Exception("Software Name doesn't exist")
```

`delitem`方法从字典和列表中删除软件。

它可以按如下方式使用。

```
del p['S1']
```

## 6.6.低输入联网（low-entry networking 的缩写）

在字典中，`len`方法返回列表中元素的数量或字典中键值对的数量。

我们也可以为我们的类定义一个`len`方法。

```
def __len__(self):
    return len(self.names)
```

我们类的`len`方法返回软件的数量。您可能已经注意到，我们正在使用 list 的内置`len`方法来返回软件的数量。

我们类的`len`方法可以以如下方式使用。

```
print(len(p))
```

## 6.7.包含

使用`in`操作符时使用`contains`方法。返回值必须是布尔值。

```
def __contains__(self,name):
    if name in self.versions:
        return True
    else:
        return False
```

该方法检查该名称是否在字典中找到。为此，我们将使用字典内置的`contains`方法。

```
if 'S2' in p:
    print("Software Exists")
else:
    print("Software DOESN'T exist")
```

上面的代码打印 if 块中的语句，因为软件 S2 存在于`versions`字典中。

## 6.8.完全码

```
class softwares:
    names = []
    versions = {}

    def __init__(self,names):
        if names:
            self.names = names.copy()
            for name in names:
                self.versions[name] = 1
        else:
            raise Exception("Please Enter the names")

    def __str__(self):
        s ="The current softwares and their versions are listed below: \n"
        for key,value in self.versions.items():
            s+= f"{key} : v{value} \n"
        return s

    def __setitem__(self,name,version):
        if name in self.versions:
            self.versions[name] = version
        else:
            raise Exception("Software Name doesn't exist")

    def __getitem__(self,name):
        if name in self.versions:
            return self.versions[name]
        else:
            raise Exception("Software Name doesn't exist")

    def __delitem__(self,name):
        if name in self.versions:
            del self.versions[name]
            self.names.remove(name)
        else:
            raise Exception("Software Name doesn't exist")

    def __len__(self):
        return len(self.names)

    def __contains__(self,name):
        if name in self.versions:
            return True
        else:
            return False
```

# 7.更多的邓德方法

在看更多的 dunder 方法之前，让我们创建一个新的类。

```
class point:
    x = None
    y = None

    def __init__(self, x , y):
        self.x = x
        self.y = y

    def __str__(self):
        s = f'({self.x},{self.y})'
        return sp1 = point(5,4)
p2 = point(2,3)
```

我们创建了一个类点，它基本上是一个 2D 点。该类有一个`init`方法和一个`str`方法。我们还创建了该类的几个实例。

## 7.1.增加

使用`+`操作符时调用`add`方法。我们可以为我们的类定义一个定制的`add`方法。

`p1 + p2`等于`p1._add__(p2)`

```
def __add__(self,p2):
    x = self.x + p2.x
    y = self.y + p2.y
    return point(x,y)
```

上述方法将第一个`point`实例和第二个`point`实例的 x 和 y 坐标相加。它将创建一个新的`point`实例，然后返回它。

```
p3 = p1 + p2
```

上面的代码行调用了`add`方法。

## 7.2.iadd

`iadd`方法类似于`add`方法。使用`+=`操作符时调用

```
def __iadd__(self,p2):
    self.x += p2.x
    self.y += p2.y
    return self
```

上面的方法只是通过添加`p2`的坐标来更新一个实例的坐标。确保你正在返回`self`，否则它将返回 None，不能按预期工作。

```
p1 += p2
print(p1)
```

上面的方法调用了`iadd`方法。

## 7.3.其他操作员

*   `__sub__(self,p2)` ( -)
*   `__isub__(self,p2)` ( -=)
*   `__mul__(self,p2)` ( *)
*   `__imul__(self,p2)` ( *=)
*   `__truediv__(self,p2)` ( \)
*   `__itruediv__(self,p2)` ( \=)
*   `__floordiv__(self,p2)` ( \\)
*   `__ifloordiv__(self,p2)` ( \=)

## 7.4.呼叫

当调用像`func()`这样的函数时，我们调用的是`call`方法。

如果我们为我们的类准备了一个`call`方法，我们可以做以下事情:

```
p1()
p2()
```

下面是一个调用方法示例:

```
def __call__(self):
    print(f"Called Point {self.x},{self.y}")
```

## 7.5.完全码

```
class point:
    x = None
    y = None

    def __init__(self, x , y):
        self.x = x
        self.y = y

    def __str__(self):
        s = f'({self.x},{self.y})'
        return s

    def __add__(self,p2):
        print("In add")
        x = self.x + p2.x
        y = self.y + p2.y
        return point(x,y)

    def __iadd__(self,p2):
        self.x += p2.x
        self.y += p2.y
        return self

    def __isub__(self,p2):
        self.x -= p2.x
        self.y -= p2.y
        return self

    def __imul__(self,p2):
        self.x *= p2.x
        self.y *= p2.y
        return self

    def __itruediv__(self,p2):
        self.x /= p2.x
        self.y /= p2.y
        return self

    def __ifloordiv__(self,p2):
        self.x //= p2.x
        self.y //= p2.y
        return self

    def __call__(self):
        print(f"Called Point {self.x},{self.y}")
```

# 8.结论

邓德方法确实很神奇，可以帮助你提高类的功能。它们可以帮助您定制您的类并重新定义内置方法。虽然我们已经讨论了一些最常见的 dunder 方法，但是还有更多像 **__hash__** 这样真正有用的 dunder 方法。 **__hash__** 用来给你的类添加一个哈希算法。这允许您的类的对象被用作字典中的键，并且具有 O(1)的查找时间。你可以在这里找到更多关于 **__hash__** 和其他 dunder 方法[的信息。](https://docs.python.org/3/reference/datamodel.html#)

快乐学习！:)

*在*[*LinkedIn*](https://www.linkedin.com/in/rahulbanerjee2699/)上联系我

*我最近开始了一个修改版的#100daysofcode 挑战。我的目标是每天写与 Python、数据科学或编程相关的内容。关注我在* [*推特*](https://twitter.com/rahulbanerjee99) *，* [*中*](https://medium.com/daily-programming-tips) *，*[*dev . to*](https://dev.to/rahulbanerjee99)*，*[*hash node*](https://realpythonproject.hashnode.dev/series/daily-programming-content/)*，或者* [*我的 WordPress 博客*](https://www.realpythonproject.com/category/daily-programming-tips/)

*原载于 2021 年 3 月 23 日*[*section . io*](https://www.section.io/engineering-education/dunder-methods-python/)