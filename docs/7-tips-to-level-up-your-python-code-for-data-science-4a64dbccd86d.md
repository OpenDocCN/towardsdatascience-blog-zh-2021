# 提升数据科学 Python 代码的 7 个技巧

> 原文：<https://towardsdatascience.com/7-tips-to-level-up-your-python-code-for-data-science-4a64dbccd86d?source=collection_archive---------6----------------------->

## 让你的生活更轻松

![](img/3ffc8a76b1681edf39ecfa3348c4c5c5.png)

照片由 [rodrigomullercwb](https://pixabay.com/users/rodrigomullercwb-6282127/) 在 [Pixabay](https://pixabay.com/photos/super-mario-mario-mario-bros-2690254/) 上拍摄

开始时编写 Python 代码很容易，但是随着您向工具箱中添加更多的库，您的脚本可能会包含不必要的代码行，变得很长，看起来很乱。尽管代码仍然可以完成工作，但从长远来看，它会给你带来一些麻烦。

我是在多次用 Python 清洗、扯皮、做数据分析之后，才知道这一点的。幸运的是，这些年来我的 Python 代码一直在改进，在本文中，我将与您分享 7 个技巧来提高您的 Python 代码水平，并使您在使用 Python 进行数据科学时更加轻松。

这涵盖了我们每天做的事情，比如修改熊猫数据帧中的值、连接字符串、读取文件等等！

# 1.使用 Lambda 修改熊猫数据帧中的值

假设我们有下面的`df`数据帧。

```
data = [[1,2,3], [4,5,6], [7,8,9]]
df = pd.DataFrame(data, columns=[0,1,2])IN[1]: print (df)OUT[1]:    **0  1  2**
        **0**  1  2  3
        **1**  4  5  6
        **2**  7  8  9
```

现在出于某种原因，您需要将值`‘01’`加到列`0`中的数字上。一种常见的方法是定义一个执行此任务的函数，然后使用`apply`函数来修改列的值。

```
**def** add_numbers(x):
    **return** f'{x}01'df[0] = df[0].apply(add_numbers)IN[1]: print (df)OUT[1]:     **0   1   2**
        **0**  101  2   3
        **1**  401  5   6
        **2**  701  8   9
```

这并不复杂，但是为您需要在数据帧中进行的每个更改创建一个函数是不实际的。这就是 lambda 派上用场的时候了。

lambda 函数类似于常规的 Python 函数，但是它可以在没有名称的情况下定义，这使得它成为一个看起来很不错的一行程序。前面使用的代码可以用下面的代码来简化。

```
df[0] = df[0].apply(**lambda** x:f'{x}01')
```

当您不知道是否有可能访问一个系列的属性来修改数据时，Lambda 变得非常有用。

假设列`0`包含字母，我们想将它们大写。

```
# if you know the existence of the .str, you can do it this way
df[0] = df[0].str.title()# if you don't know about .str, you can still capitalize with lambda
df[0] = df[0].apply(**lambda** x: x.title())
```

# 2.使用 f 字符串连接字符串

字符串连接是 Python 中非常常见的操作，可以用不同的方式完成。最常见的方法是使用`+`运算符；然而，这个操作符的一个大问题是我们不能在字符串之间添加任何分隔符。

当然，如果您想要连接“Hello”和“World”，一个典型的解决方法是添加一个空格分隔符(" ")。

```
print("Hello" + " " + "World")
```

这就完成了工作，但是为了编写更可读的代码，我们可以用 f 字符串来代替它。

```
IN[2]: print(f'Hello World')
OUT[2]: "Hello World"
```

在一个基本的例子中，这似乎是不必要的，但是当涉及到连接多个值时(正如您将在技巧#3 中看到的)，f 字符串将使您免于编写`+ “ “ +`很多次。我不知道过去我写了多少次`+`操作符，但是现在没有了！

连接字符串的其他方法是使用`join()`方法或`format()`函数，但是 f-string 在字符串连接方面做得更好。

# 3.用 Zip()函数遍历多个列表

你有没有想过在 Python 中遍历多个列表？当你有两个列表时，你可以用`enumerate`来完成。

```
teams = ['Barcelona', 'Bayern Munich', 'Chelsea']
leagues = ['La Liga', 'Bundesliga', 'Premiere League']**for** i, team **in** enumerate(teams):
    league = leagues[i]
    print(f'{team} plays in {league}')
```

然而，当你有两个或更多的列表时，这就变得不切实际了。更好的方法是使用`zip()`功能。`zip()`函数获取可重复项，将它们聚集在一个元组中，然后返回。

再加一个单子，看看`zip()`的威力！

```
teams = ['Barcelona', 'Bayern Munich', 'Chelsea']
leagues = ['La Liga', 'Bundesliga', 'Premiere League']
countries = ['Spain', 'Germany', 'UK']**for** team, league, country **in** zip(teams, leagues, countries):
    print(f'{team} plays in {league}. Country: {country}')
```

上面代码的输出如下。

```
Barcelona plays in La Liga. Country: Spain
Bayern Munich plays in Bundesliga. Country: Germany
Chelsea plays in Premiere League. Country: UK
```

你注意到我们在这个例子中使用了 f 弦吗？代码变得可读性更强了，不是吗？

# 4.使用列表理解

清理和争论数据的一个常见步骤是修改现有列表。假设我们有以下需要大写的列表。

```
words = ['california', 'florida', 'texas']
```

大写`words`列表中每个元素的典型方法是创建一个新的`capitalized`列表，执行一个`for`循环，使用`.title()`，然后将每个修改后的值追加到新列表中。

```
capitalized = []
**for** word **in** words:
    capitalized.append(word.title())
```

然而，Pythonic 式的方法是使用列表理解。列表理解有一个制作列表的优雅方法。

您可以用一行代码重写上面的`for`循环:

```
capitalized = [word.title() for word in words]
```

正如你所看到的，我们可以跳过第一个例子中的一些步骤，结果是一样的。

# 5.对文件对象使用 With 语句

当我们处理一个项目时，我们经常读写数据到文件中。最常见的方法是使用`open()`函数打开一个文件，这将创建一个我们可以操作的文件对象，然后作为一个好的实践，我们应该使用`close()`关闭文件对象

```
f = open('dataset.txt', 'w')
f.write('new_data')
f.close()
```

这很容易记住，但有时在编写代码几个小时后，我们可能会忘记用`f.close()`关闭`f`文件。这时`with`语句就派上用场了。`with`语句将自动关闭文件对象`f`，如下图所示。

```
with open('dataset.txt', 'w') as f:
    f.write('new_data')
```

这样，我们可以保持代码简短。

虽然你不需要这个来读取 CSV 文件，因为你可以很容易地用熊猫`pd.read_csv()`来读取它们，但这在读取其他类型的文件时仍然很有用。例如，我经常在腌制物品时使用它。

```
import pickle# read dataset from pickle file
with open(‘test’, ‘rb’) as input:
    data = pickle.load(input)
```

# 6.停止使用方括号来获取词典条目。使用。请改为 get()

假设你有下面的字典:

```
person = {'name': 'John', 'age': 20}
```

分别做`person[name]`和`person[age]` 我们很容易得到姓名和年龄。然而，如果出于某种原因，我们想得到一个不存在的键，像`'salary',`运行`person[salary]`将提高一个`KeyError`。

这就是`get()`方法有用的时候。如果关键字在字典中，那么`get()`方法返回指定关键字的值，但是如果没有找到关键字，Python 将返回`None`。多亏了这一点，你的代码不会中断。

```
person = {'name': 'John', 'age': 20}print('Name: ', person.get('name'))
print('Age: ', person.get('age'))
print('Salary: ', person.get('salary'))
```

输出如下所示。

```
Name:  John
Age:  20
Salary:  None
```

# 额外收获:多重任务

您是否曾经想要减少用于创建多个变量、列表或字典的代码行？嗯，你可以很容易地用多重作业来完成。

```
# instead of doing 
a = 1
b = 2
c = 3# you can do
a, b, c = 1, 2, 3# instead of creating multiple lists in different lines
data_1 = []
data_2 = []
data_3 = []
data_3 = []# you can create them in one line with multiple assignments
data_1, data_2, data_3, data_4 = [], [], [], []# or use list comprehension
data_1, data_2, data_3, data_4 = [[] for i in range(4)]
```

*就是这样！明智地使用这 7 个技巧来提升你的 Python 代码。*

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑[注册成为一名媒体会员](https://frank-andrade.medium.com/membership)。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你用[我的链接](https://frank-andrade.medium.com/membership)注册，我会赚一小笔佣金。

[](https://frank-andrade.medium.com/membership) [## 阅读弗兰克·安德拉德(以及媒体上成千上万的其他作家)的每一个故事

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

frank-andrade.medium.com](https://frank-andrade.medium.com/membership)