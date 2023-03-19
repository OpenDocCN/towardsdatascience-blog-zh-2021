# Python 中的增广赋值表达式 Walrus 运算符:=及其他

> 原文：<https://towardsdatascience.com/augmented-assignment-expression-in-python-the-walrus-operator-and-beyond-9db36a219df3?source=collection_archive---------26----------------------->

## 不仅仅是关于海象算子，还有很多相关的概念

![](img/3b882f0a144744ec289fad0f1634f722.png)

汤米·克兰巴赫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

从 3.8 版本开始，Python 中就包含了新的特征增强赋值表达式。特别是，结果出现了一个新的操作符——内联赋值操作符`:=`。因为它的外观，这个操作符通常被称为海象操作符。在本文中，我将讨论这个操作符的关键方面，以帮助您理解这项技术。

事不宜迟，让我们开始吧。

## 表达式和语句的区别

在 Python 或一般的编程语言中，有两个密切相关的概念:表达式和语句。让我们看两行简单的代码，如下所示。

```
>>> 5 + 3    #A
8
>>> a = 5 + 3    #B
```

#A 是一个表达式，因为它的计算结果是一个整数。相比之下，#B 是一个语句，确切地说是一个赋值语句。更一般地说，表达式是计算值或 Python 中的对象的代码(Python 中的值都是对象)。例如，当你调用一个函数时(如`abs(-5)`)，它是一个表达式。

语句是不计算任何值的代码。简而言之，他们执行一个动作，或者做一些事情。例如，赋值语句创建一个变量。`with`语句建立了一个上下文管理器。`if…else…`语句创建逻辑分支。

如您所见，表达式和语句的最大区别在于代码是否计算为对象。

你可能想知道为什么我们在这里关心这种区别。我们继续吧。

## 扩充赋值是一个语句

许多人熟悉的最常见的扩充任务形式是`+=`。下面的代码向您展示了一个示例。

```
>>> num = 5
>>> num += 4
```

判断一行代码是否是表达式的一个简单方法是简单地将它发送给内置的`print`函数，该函数应该接受任何对象。当您发送一个要打印的语句时，它会引发一个`SyntaxError`。让我们来试试:

```
>>> print(num += 4)
  File "<input>", line 1
    print(num += 4)
              ^
SyntaxError: invalid syntax
```

事实上，我们不能在`print`函数中使用增强赋值。或者，为了确定某些代码是否是表达式，有另一个内置函数`eval`，它计算一个表达式。当代码不能被求值时，比如一个语句，就会发生错误，如下所示。

```
>>> eval("num += 4")
Traceback (most recent call last):
  File "<input>", line 1, in <module>
  File "<string>", line 1
    num += 4
        ^
SyntaxError: invalid syntax
```

事实上，`x += y` 是语句`x = x + y`的简写，这是一个设计好的赋值。因为产生的变量与用于增量(y)的变量同名(即 x)，所以它也被称为就地加法运算。

同样的就地分配也适用于其他的扩充分配，例如`-=`、`*=`和`/=`。

## 扩充赋值表达式是一个表达式

与这些扩充赋值语句不同，新的扩充赋值表达式是一个表达式。让我们从一个简单的例子开始。

```
>>> (another_num := 15)
15
```

如上图，因为我们使用的是交互式 Python 控制台，所以如果该行代码的求值结果是任何对象(除了`None`，控制台在输出时会忽略它)，那么求值结果会自动显示。因此，上面的扩充赋值表达式确实是一个表达式。请注意，这里使用的一对括号是出于语法原因，因为这样一个增强的赋值表达式本身没有任何实际用途，因此不推荐使用。

我们可以证明扩充赋值表达式的本质是一个带有`print`和`eval`函数的表达式，如下所示。

```
>>> print(another_num := 15)
15
>>> eval("(another_num := 15)")
15
```

## 实际用例是什么？

扩充赋值表达式也称为内联扩充赋值。正如它的行所示，这是一个内联操作，因此我们在刚刚求值的地方定义一个变量——本质上，我们将求值和赋值合并到一个步骤中。

也许这有点太专业，难以理解，但让我给出一个看似合理的用例。在一个典型的 if…else…语句中，if 或 else 子句只能接受表达式，这样子句就可以对运算的表达式求值为 true 或 falsy。显然，在从句中，你不能使用陈述句。考虑下面这个看似合理的用例。

```
def withdraw_money(account_number):
    if account := locate_account(account_number):
        take_money_from(account)
    else:
        found_no_account()
```

如上所示，`locate_account`函数返回使用`account_number`找到的账户。如果找到，从赋值表达式中创建的变量`account`将进一步用于`if`子句的主体中。否则，使用顺序方式，这里有一个可能的解决方案:

```
def withdraw_money(account_number):
    account = locate_account(account_number)
    if account:
        take_money_from(account)
    else:
        found_no_account()
```

这两种实现之间的差异可能微不足道。然而，实际的用例可能更复杂，涉及更多的分支，这证明了使用赋值表达式技术的合理性。考虑以下使用案例:

```
def withdraw_money(account_number):
    if account := locate_account_in_saving(account_number):
        take_money_from_saving(account)
    elif account := locate_account_in_checking(account_number):
        take_money_from_checking(account)
    elif account := locate_account_in_retirement(account_number):
        take_money_from_retirement(account)
    else:
        pass
```

正如你所看到的，有了更多的分支，如果你选择分成两个独立的步骤:函数调用，然后赋值，上面的代码会变得复杂得多。不信的话，请自行尝试:)

## 结论

在本文中，我们回顾了 Python 3.8 中增加的扩充赋值表达式技术。我们不仅研究了这项技术本身，还回顾了与这项技术相关的关键概念。我希望您对这种技术及其正确的用例有更好的理解。

感谢阅读这篇文章。通过[注册我的简讯](https://medium.com/subscribe/@yong.cui01)保持联系。还不是中等会员？通过[使用我的会员链接](https://medium.com/@yong.cui01/membership)支持我的写作(对你没有额外的费用，但是你的一部分会费作为奖励由 Medium 重新分配给我)。