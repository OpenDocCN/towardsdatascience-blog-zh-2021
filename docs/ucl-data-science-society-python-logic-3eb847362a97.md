# UCL 数据科学学会:Python 逻辑

> 原文：<https://towardsdatascience.com/ucl-data-science-society-python-logic-3eb847362a97?source=collection_archive---------30----------------------->

## 讲习班 3:条件语句、逻辑语句、循环和函数

![](img/df2f69e4dd07d89b20fe5989568db877.png)

由 [Ales Nesetril](https://unsplash.com/@alesnesetril?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今年，作为 UCL 数据科学学会的科学负责人，该学会将在整个学年举办一系列 20 场研讨会，涵盖的主题包括数据科学家工具包 Python 和机器学习方法简介。对于我所展示和交付的每一个，我的目标是创建一系列的小博客，这些小博客将概述主要观点，并为任何希望跟进的人提供完整研讨会的链接。所有这些都可以在我们的 [GitHub](https://github.com/UCL-DSS) 上找到，并将在全年更新新的研讨会和挑战。

第二个研讨会是对 Python 逻辑的介绍，我们将向您介绍条件语句、逻辑语句(if、else 和 elif)、循环(for 和 while)和函数。虽然这里将分享一些亮点，但整个研讨会，包括问题表，可以在[这里](https://github.com/UCL-DSS/python-logic-workshop)找到。

如果您错过了前两次关于 Python 基础和 Python 序列的研讨会，请在此处补上:

[](/ucl-data-science-society-python-fundamentals-3fb30ec020fa) [## UCL 数据科学协会:Python 基础

### 研讨会 1: PY01

towardsdatascience.com](/ucl-data-science-society-python-fundamentals-3fb30ec020fa) [](/ucl-data-science-society-python-sequences-e3ffa67604a0) [## UCL 数据科学协会:Python 序列

### 工作坊 2:列表、元组、集合和字典！

towardsdatascience.com](/ucl-data-science-society-python-sequences-e3ffa67604a0) 

## 条件语句

Python 作为一种语言，支持数学中常见的比较语句，包括:

*   等于:a == b
*   不等于:a！= b
*   小于:a < b
*   Less than or equal to: a < = b
*   Greater than: a > b
*   大于或等于:a > = b

这些条件，就像数学中的一样，允许你检查数据或编程过程中的事情是真还是假，这是我们在[第一次研讨会](/ucl-data-science-society-python-fundamentals-3fb30ec020fa)中介绍的。有了这些比较运算符，我们可以将它们与条件语句 **if、else** 和 **elif** 一起使用，这些条件语句实际上遵循与普通英语条件相同的逻辑。例如:

如果我有钱，我会买一辆宝马 M5，否则我会买一辆自行车

可以改写为:

如果我有钱，我会买一辆宝马 M5，否则我会买一辆自行车

Python 的 **IF** 结构遵循相同的方案。例如:

```
# assign a and b values
a = 10
b = 10# add a condition
if a == b:
    # tell Python what to do if the condition is met
    print("a equals b")# out:
a equals b
```

在这里，我们可以看到在满足条件的情况下运行了在条件**之后缩进了一个制表符的代码。在这里，这意味着如果 a 与 b 相同，那么 print 语句将。如果不满足该条件，则不运行 If 条件中的代码。**

然而，如果我们想在条件不满足时运行一些代码，我们可以使用如下的`else`语句:

```
# assign a and b values
a = 10
b = 20# add a condition
if a == b:
    # tell Python what to do if the condition is met
    print("a equals b")
# if a is different from b 
else:
    print("a does not equal b")
    # this message will be printed# out:
a does not equal b
```

我们现在可以看到，如果不满足条件，我们可以设置一些代码来运行，而不是代码不运行。

另一个方面是，如果我们想在我们的条件中建立更多的信息。例如，我们不是仅仅指定`a equals b`是想说`as is larger than b`还是`a is smaller than b`，而是可以使用`elif`语句来这样做。实际上，这相当于一个`else if`语句，我们通过它指定第二个条件来检查第一个条件是否满足，并且只有当第一个条件不满足时，才允许我们在语句中加入更多的信息和选项:

```
# assign a and b values
a = 11
b = 10# add a condition
if a == b:
    # tell Python what to do if the condition is met
    print("a equals b")
# if a is different from b and smaller
elif a < b:
    print("a is smaller than b")
else:
    print("a is not equal to b nor smaller")
    # this message will be printed# out:
a is not equal to b nor smaller
```

然后，我们可以通过内置逻辑操作符`not`、`and`和`or`使其变得更加复杂。实际上，这意味着:

*   如果两个语句都为真，则**和**返回真；
*   如果其中一个语句为真，则**或**返回真；
*   **not** 反转结果，如果结果为真则返回 False

我们可以用它来创造更复杂的条件:

```
# assign values to a, b and c
a = 1
b = 2
c = 3# check different conditions
if a > b and b > c:
    print("Both conditions are True.")

if a > b or a > c:
    print("At least one of the conditions is True.")

if not(a % 2 == 0): # If a is NOT even, it will be printed.
    print(a)if a > b and a > c and b > c:
    print("All conditions are true.")

if a % 2 == 0 or b % 2 == 0 and c > 0: # Be careful which command will be evaluated first.
    print("a or b is even and c is positive.")if not(a > 100) and b < 3:
    print("Both conditions are true.")

if not(a + b < 10) or b < 0:
    print("At least one of the conditions is true.")# out:
1
a or b is even and c is positive.
Both conditions are true.
```

不同逻辑运算符的优先顺序如下，这一点很重要:

1.  **不是**
2.  **和**
3.  **或**

## 环

循环是一种重要的结构，可以学习它来自动化同一任务的多个实例。例如，对列表中的每一项执行相同的操作，或者使用循环在满足特定条件时产生给定的输出。为此，有两种主要类型:

*   While 循环
*   对于循环

**While 循环**

只要创建循环的初始条件保持为真，while 循环就会在缩进的代码块中重复指令:

```
# initialise the variable
x = 0# write the condition
while x <= 10:
    #"the following lines will be executed as long as x is smaller or equal than 10"
    print (x)
    #"however, if there are no changes made to our variable, the loop will continue infinetely"
    #"so, in this case, we will increase x by one"
    x = x + 1#out:
0
1
2
3
4
5
6
7
8
9
10
```

这意味着，如果没有对正在讨论的变量进行更改，或者条件从未更改，那么循环将永远不会结束。这就是我们所说的无限循环:

```
#DO NOT TRY RUNNING THIS CODE
#x = 0#while True:
 #   print (x)
```

要退出，您可以使用`break`命令，该命令将在被调用时退出循环，并且可以在满足特定条件时使用:

```
# initialise x
x = 0# start the loop
while x < 100:
    print(x)
  # add a condition
    if x == 5:
    # if the condition is met, stop the code
        break
    # if the condition isn't met, x will increase and the loop will continue
    x += 1#out:
0
1
2
3
4
5
```

与此相反的是`continue`命令，它允许您跳过循环的某个迭代

```
#initialise x
x = 0#start the loop
while x < 10:

    #add one to the value
    x += 1
    #continue condition
    if x == 4:
        #input the continue statement
        continue

    #the output of the loop if the continue statement is not run
    print(x)#out:
1
2
3
5
6
7
8
9
10
```

**为循环**

第二个版本是循环的**，与 **While** 循环不同，它不需要你增加变量，因为它会自己增加。所以，对于一个变量 x，在一个给定的列表/字典/字符串中，循环将取列表中的每一个值。例如:**

```
# define a list
colours = ["red", "blue", "yellow","green","orange"]# loop over the list
for x in colours:
    #print each value in the list
    print(x)#out:
red
blue
yellow
green
orange
```

您现在可以迭代已经创建的数据。例如，这也可以循环字符串中的单个字母:

```
#create the for condition
for x in "red":
    #print the result
    print(x)#out:
r
e
d
```

和以前一样，我们也可以利用`continue`和`break`语句:

```
#create the for condition
for x in colours:

    #if the color is blue
    if x == "blue":
        #then skip
        continue

    #else if the color is green
    elif x == "orange":
        #break the loop
        break

    #print the color
    print(x)#out:
red
yellow
green
```

**功能**

最后，我们有函数。当您有一段需要反复使用的代码时，函数非常有用，但不一定像在循环中那样使用。当您需要运行相同的代码，但是使用不同的输入或者在整个代码中的不同点运行时，就会出现这种情况。因此，创建函数是减少实际产生的代码量的一种方式。

为此，我们需要能够定义一个函数，这个函数可以用下面的符号`def function_name(input_variables):`来完成。这几乎遵循与条件语句和循环相同的符号，即函数中的代码需要缩进和冒号才能工作，但现在我们添加了`function_name`，并允许用户传入`input_variables`。

这方面的一个例子是创建代码，能够告诉我们根据边的大小产生什么类型的三角形。我们可以通过调用它`traingle_function`来做到这一点，并允许用户传入三角形边长的三个变量作为`a, b, c`。

```
def triangle_function(a,b,c):
    """Tells you what type of triangle would have sides of size a, b, c"""

    #if we have negative lengths then no triange
    if a <= 0 or b <= 0 or c <= 0:
        print("They can't represent lengths.")

    #otherwise we have a triangle

    else:
        #a traingle cannot have one side that is bigger than the size of the 
        #the other two combined so we can check this condition
        if a >= b + c or b >= a + c or c >= a + b:
            print("It is not a triangle.")

        #otherwise we know it is a triangle
        else:

            #if the sides are equal is it an equalateral triangle
            if a == b == c:
                print("It is an equilateral triangle.")

            #if two sides are equal it is an isoceles triangle
            elif a == b or a == c or b == c:
                print("It is an isosceles triangle.")

            #if Pythagoream theorem is correct then we get a rigfht angle
            elif a**2 == b**2 + c**2 or b**2 == c**2 + a**2 or c**2 == a**2 + b**2:
                print("It is a right-angled triangle.")

            #otherwise it is an irregular scalene triangle
            else:
                print("It is a scalene triangle.")
```

然后我们可以调用这个函数，输入我们想要检查的信息:

```
triangle_function(3,4,5)
triangle_function(1,1,1)#out
It is a right-angled triangle.
It is an equilateral triangle:
```

正如我们所见，这简化了代码的实现或再次创建代码，因为我们所要做的就是创建函数。

我们可以使用函数执行更高级的操作，例如创建默认值，由此我们可以设置 c =3，作为默认值，如果我们知道在大多数情况下 c 将是 3，那么我们不需要每次都重新键入它，我们可以通过告诉函数为什么数据类型是预期的来添加类型提示，我们可以以 docstring 的形式添加文档来告诉用户在使用该函数时预期什么，我们也可以返回值，所有这些都在[实用笔记本中进行了解释。](https://github.com/UCL-DSS/python-logic-workshop/blob/master/workshop.ipynb)

然而，函数的一个重要部分是知道局部变量和全局变量的区别。在处理函数时，任何只存在于函数内部的变量都称为**局部变量**，在上面的三角函数中，a、b 和 c 都是局部变量。这类变量只能在函数内部使用，如果你试图在其他地方使用它们，就会出错。与此相反的是**全局变量**，它们存在于函数之外，可以在任何地方使用，甚至在函数内部，尽管这是一种不好的做法。这方面的一个例子是:

```
# define a function
def function():
    "y is a local variable (it only exists within our function), we will give y the value 100"
    y = 100
    # this function returns the value of x
    return y# calling the function
function()# now, we will try to do an operation with x outside the function
print(y) 
# because y is a local variable in the function, it won't work
#the error tells us that y is not defined#out:
**---------------------------------------------------------------------------**
**NameError**                                 Traceback (most recent call last)
**~\AppData\Local\Temp/ipykernel_3564/2261376186.py** in <module>
     10 
     11 **# now, we will try to do an operation with x outside the function**
**---> 12** print **(**y**)** **# because x is a local variable in the function, it won't work**

**NameError**: name 'y' is not defined
```

我们可以看到，我们在函数中定义了变量`y`，并试图在函数外部调用它。问题是`y`是一个局部变量，只存在于函数内部，即使函数被使用，这意味着我们不能在函数外部调用它。因此，我们得到了名称错误，它告诉我们`y`还没有被定义。

与此相反的是在函数中使用全局变量:

```
# assigning x a value as a global variable (outside the function)
x = "global variable"# defining a function
def function_2():
    # we can use x because it's global in the function
    print("x is a " + x)# calling the function        
function_2()#out:
x is a global variable
```

我们可以在函数中使用全局变量。

然而，如果我们在函数内部改变变量，而没有把改变赋回给原始变量。那么原始变量不会改变:

```
c = 200#define a function
def add_100(c):
    #alter the local variable
    return c+100#call the function
print(add_100(c))#call the original variabl
print(c)#c has not changed because only the local variable c has changed inside the function#out:
300
200
```

在将来设计、创建和使用函数时，记住这一点很重要。

完整的研讨会笔记本，以及更多示例和挑战，可在 [**这里**](https://github.com/UCL-DSS/python-logic-workshop) 找到。如果您想了解我们协会的更多信息，请随时关注我们的社交网站:

https://www.facebook.com/ucldata 脸书

insta gram:https://www.instagram.com/ucl.datasci/

领英:[https://www.linkedin.com/company/ucldata/](https://www.linkedin.com/company/ucldata/)

如果你想了解 UCL 数据科学协会和其他优秀作者的最新信息，请使用我下面的推荐代码注册 medium。

[](https://philip-wilkinson.medium.com/membership) [## 通过我的推荐链接加入媒体-菲利普·威尔金森

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

philip-wilkinson.medium.com](https://philip-wilkinson.medium.com/membership) [](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d) [## scikit-learn 决策树分类器简介

towardsdatascience.com](/introduction-to-decision-tree-classifiers-from-scikit-learn-32cd5d23f4d) [](/an-introduction-to-object-oriented-programming-for-data-scientists-879106d90d89) [## 面向数据科学家的面向对象编程介绍

### 面向对象的基础知识，适合那些以前可能没有接触过这个概念或者想知道更多的人

towardsdatascience.com](/an-introduction-to-object-oriented-programming-for-data-scientists-879106d90d89) [](/univariate-outlier-detection-in-python-40b621295bc5) [## Python 中的单变量异常检测

### 从数据集中检测异常值的五种方法

towardsdatascience.com](/univariate-outlier-detection-in-python-40b621295bc5)