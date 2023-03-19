# 初学者的数据争论 Python 语言的一个独特特性

> 原文：<https://towardsdatascience.com/data-wrangling-for-beginners-a-unique-feature-of-python-language-53bd9f1cd166?source=collection_archive---------50----------------------->

## 在循环中使用 If-Else 条件

![](img/1eaeb7dea61519c665ac35a5420b7bf3.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

我们大多数人都在这样或那样的编程语言中使用过 if-else 条件块。Python 更进一步，支持在循环结构中使用 else 条件块。在这个**，**教程中，我们将看到在 while 和 else 循环结构中使用 ***else*** 条件块。

# 先决条件

请注意，理解 Python 中的循环和条件是本教程的先决条件。如果你是 Python 语言的新手，并且想在你的系统上设置它，请阅读本教程。

# 让我们开始吧

## 1.在 while 循环中使用 else 条件

在我们开始使用 ***else 条件块*** 之前，让我们快速回顾一下 while 循环。在下面的例子中，我们将模拟一个登录页面场景，其中用户有有限的输入正确密码的尝试次数。

```
**### Syntax**
while condition:
   action**### Example** trial = 1
while trial <= 3:
    pwd = input("please enter the password: ")
    if pwd == "abcd":
        print("Valid Password. Login Successful")
        break
    else:
        trial += 1**### Execution**
please enter the password: 123
please enter the password: abd
please enter the password: asd1### **Output
Given the user enters incorrect passwords, the code execution stops without any output**
```

*   我们期望用户在三次尝试中输入有效的密码(***)ABCD***)。
*   **if-else 构造**验证密码**，while 关键字跟踪用户尝试的次数**。
*   如果密码有效，代码将打印成功消息。如果输入的密码不正确，代码会将计数器加 1。
*   输入正确的密码后，程序会打印成功消息并终止循环，但是如果用户用完了尝试次数，代码会在不打印任何失败消息的情况下终止。这就是**别的** **条件**可以派上用场的地方。让我们用一个例子来理解这一点:

```
**### Syntax**
while condition:
   action_1
else:
   action_2
--------------------------------------------------------------------**### Example 1 - Else Condition Executes** trial = 1
**while trial <= 3:**
    pwd = input("please enter the password: ")
    if pwd == "abcd":
        print("Valid Password. Login Successful")
        break
    else:
        trial += 1
**else:** print("Invalid password entered 3 times. Login Failed")**### Execution (while loop doesn't encounter the break statement)**
please enter the password: 123
please enter the password: abc
please enter the password: abc1### **Output (because condition after while keyword failed)** Invalid password entered 3 times. Login Failed
--------------------------------------------------------------------**### Example 2- Else Condition Doesn't Executes** trial = 1
**while trial <= 3:**
    pwd = input("please enter the password: ")
    if pwd == "abcd":
        print("Valid Password. Login Successful")
        break
    else:
        trial += 1
**else:**
    print("Invalid password entered 3 times. Login Failed")**### Execution (while loop encounters the break statement)**
please enter the password: 123
please enter the password: **abcd**### **Output** Valid Password. Login Successful
```

*   只有当 break 语句没有终止循环时，while 循环后出现的 **else 块才会执行。**
*   在示例 1 中，用户未能在允许的尝试次数内输入正确的密码。在这种情况下，**while 循环没有遇到 break 语句，而** Python 执行了 else 构造中的代码。
*   在示例 2 中，while 循环遇到了一个 break 语句(在第二次试验中打印成功消息之后)，else 构造中的**代码块没有被执行。**

## 2.将 else 与 for 循环一起使用

与 while 循环类似， **else 构造也可以与 for 循环**一起使用。下面的代码块演示了同样的情况:

```
**### Syntax**
for variable in iterable:
   action_1
else:
   action_2**### Example
for i in range(3):**
    pwd = input("please enter the password: ")
    if pwd == "abcd":
        print("Valid Password. Login Successful")
        break
**else:**
    print("Invalid password entered three times. Login Failed")**### Execution**
please enter the password: 123
please enter the password: abc
please enter the password: abc1### **Output** Invalid password entered three times. Login Failed
```

*   与 while 循环部分的例子类似，程序希望用户在允许的三次尝试的限制内输入有效的密码。
*   给定执行场景，由于 for 循环**没有遇到 break 语句**，程序执行 else 构造中的代码块并打印失败消息。

# 结束语

在 Python 语言中，你认为我们还能在哪里使用 **else** 条件？谷歌出来，并通过评论与读者分享。

如果这个故事对你有帮助，请不要忘记鼓掌。

快乐学习！！！！