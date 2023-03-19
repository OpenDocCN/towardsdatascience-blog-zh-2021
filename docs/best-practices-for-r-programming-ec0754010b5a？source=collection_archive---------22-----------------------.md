# R 编程的最佳实践

> 原文：<https://towardsdatascience.com/best-practices-for-r-programming-ec0754010b5a?source=collection_archive---------22----------------------->

![](img/ee111baf5e2b415eae60d7ae281f5b32.png)

摄影:[@ karst en _ wuerth](https://unsplash.com/@karsten_wuerth)—Unplash.com

作为一个有统计学背景的人，我承认我必须不断提高我的计算机科学和工程技能，几乎每天都是如此。虽然在分析数据时考虑分布、统计和其他关键概念对我来说是很自然的，但编写高效、干净的代码却不是。

幸运的是，我有机会与许多工程师一起工作，他们教导并解释了为什么代码需要干净和高效——如果我可以用一句话来总结这一需求，最好的一句话来自约翰·多恩的诗(这句话几乎有 400 年的历史了！):***‘没有人是孤岛’。***

说到开发我们的代码和脚本，我们不是一个孤岛。当作为一名数据科学家、分析师(或几乎任何其他职业)工作时，协作是最重要的技能之一——如果你想以分析数据为职业，未来有人不得不查看你的代码的概率可能是 99.99%。您的代码组织得越好，将来就越容易被人查看、调试和改进。而且这并不排斥你可能必须去做的其他人，它也将为你未来的自己省去很多麻烦(那些从来没有看着自己的代码并想:“**我到底在这个函数中做了什么？**))

因此，让我们直接进入 R 编程的一些常见的最佳实践(有些有争议，有些被社区广泛接受)！

## 图书馆第一

应该放入 R 脚本的第一件事是你的库——你的代码的依赖关系应该在一开始就明确。这将避免有人在运行代码时感到惊讶，因为其中一个导入隐藏在代码中间，而用户没有为此准备好环境。

大多数风格指南都同意这一建议，您应该避免这样做:

```
my_vector <- c(1,2,3)library(readxl)read_excel(path_file)
```

在导入库 *readxl —* 之前，创建一个向量 *my_vector* ，这通常是一个不好的做法。

我发现唯一可以打破这一规则的地方是在教学过程中——例如，当一个人在一个依赖于库的讲座中引入一个新概念时——可以在脚本中间加载该库，以便学生保留正在使用的函数和包含该段代码的库的可视引用。

## 硬编码变量次之

另一个普遍接受的规则是硬编码的变量，如访问数据库的路径或配置文件(如果您有密码，最好看看这篇文章来管理机密:[https://cran . r-project . org/web/packages/httr/vignettes/secrets . html](https://cran.r-project.org/web/packages/httr/vignettes/secrets.html))在脚本的开头，在导入库之后。

许多脚本都使用 csv 或 xlsx 文件中的数据，因此一般最佳做法是执行以下操作:

```
library(readxl)path_file <- "data/data.csv"my_df <- read_excel(path_file)
```

这样，无论谁读了你的代码，都有两个重要的信息:

*   **你的代码依赖于哪些库。**
*   **源文件夹结构应该包含哪些文件和文件夹。**

哦，关于文件路径..

## 绝对路径上的相对路径

绝对路径从来都不是应该走的路。当您在操作系统*用户*结构中的文件夹上工作时，这一点尤其重要。

绝对路径如下所示:

```
"C:\Users\ivopb\My Documents\R Project\data\data.csv"
```

如果我把我的代码传递给需要在他们机器上运行它的人，除非他们有用户名 ***ivopb，*** 否则他们永远也不能在我们使用这个文件的地方运行代码。即使他们在相同的文件夹结构(My Documents/R Project/等)中运行它。).

哦，即使碰巧他们是用户***【ivopb】，*** 但是他们的硬盘中映射了另一个字母，不是***C:***祝你执行代码好运！

通常，相对路径总是首选的:

```
"data\data.csv"
```

这将迫使您将工作目录设置为您正在工作的文件夹，或者从文件夹中打开脚本——这确实比调试和更改脚本中的大量路径要好得多！

## 命名约定—文件名

对于文件名，总是使用易于理解的文件名，不要在文件名中使用空格(我实际上在我的课程脚本中犯了这个错误，试图匹配 Udemy 上的讲座名称，我将来可能会改变这一点)——一个好的和坏的例子:

```
# Good example
my_file.R# Bad example
My File.R
```

此外，在脚本名称中尽量使用小写字母。例如，如果您的脚本的目标是在 csv 文件中创建一些特定数据的聚合，请使用与脚本总体目标相关的名称:

```
aggregating_data.R
```

## 命名约定—对象和函数

这在任何编码语言中都是一个热门话题——人们倾向于争论哪种命名约定是最好的。

除了你选择的命名约定之外，一定要在整个脚本中遵循相同的命名约定——对我来说，这是一个通用的黄金法则。

我喜欢用蛇皮箱(使用 *_* )作为物件，用骆驼或蛇皮箱作为功能，但这有待讨论。一个例子:

```
# Good
my_vector <- c(1,2,3)# Bad
myvector <- c(1,2,3)# Good
ThisFunction()
this_function()# Bad
thisfunction() 
```

此外，您的对象和函数名称应该尽可能明确和简短，想象一个接受一个元素并计算一个数的幂的函数:

```
ComputePowerOfBaseWithExponent <- function (base, exponent) {
  return (base**exponent)
}
```

函数名确实很长，所以我们可以缩短它，通常建议这样做:

```
ComputePower <- function (base, exponent) {
  return (base**exponent)
}
```

再说一遍，我唯一的黄金法则是在整个剧本中保持风格一致。

## 返回

这与变量和函数的命名一样，是社区中最有争议的话题之一(看一下这个帖子来检查争论的双方——[https://stack overflow . com/questions/11738823/explicitly-calling-return-in-a-function-or-not](https://stackoverflow.com/questions/11738823/explicitly-calling-return-in-a-function-or-not)

每个函数末尾的 R 中的 Return 语句增加了冗余—这是事实。注意，我在上面的例子中使用了显式返回:

```
ComputePower <- function (base, exponent) {
  return (base**exponent)
}
```

可以从函数中删除*返回*语句，从显式返回改为隐式返回:

```
ComputePower <- function (base, exponent) {
  base**exponent
}
```

使用显式返回对代码的速度有一点点影响——增加了一点点(它通常非常小，几乎无法辨认)。

我倾向于认为显式返回对于所有级别的 R 程序员都更容易，因为显式返回让初学者更容易理解代码的流程。但这主要是程序员的选择——当谈到这一点时，人们往往会站在两边，我个人的观点是，我认为指责任何使用隐式或显式回报的人都是不公平的。

## 在循环中显式

在对象上进行循环时，一件重要的事情是显式命名正在循环的元素。

让我们想象下面的练习:你有一个特定人群的年龄向量，你想用“主要”或“次要”来分类，你使用 for 循环(为了讨论，让我们忽略我们可以使用的更好实现的其他方法):

```
ages_people = c(10, 20, 20, 30, 40)ClassifyAge <- function (ages) {
  age_class <- c()
  for (age in ages) {
    if (age < 18) {
      age_class <- c(age_class, 'Minor')
    }
    else {
      age_class <- c(age_class, 'Major')
    }
  }
  age_class
}
```

如果我们将 for 循环中的 ***age*** 称为 ***i*** 或 ***元素:***

```
ages_people = c(10, 20, 20, 30, 40)ClassifyAge <- function (ages) {
  age_class <- c()
  for (i in ages) {
    if (i < 18) {
      age_class <- c(age_class, 'Minor')
    }
    else {
      age_class <- c(age_class, 'Major')
    }
  }
  age_class
}
```

通常最好是显式命名循环的元素——在上面的例子中，我们是基于一个 ***年龄*** 来做一些事情，因此最好将循环元素命名为 ***年龄*** 而不是 ***i、元素*** 或*。这将使不熟悉代码的人更容易理解代码在功能级别上做什么。*

## *对对象赋值使用*

*这是社区上的另一个热门话题——在创建对象或函数时，我倾向于使用**-<-**，尽管在这种情况下 **=** 的表现完全相同。大多数风格指南同意这一点，但没有一个放之四海而皆准的意见。*

*一个普遍接受的规则是，当你使用*

```
*# Good example
my_vector <- c(1, 2, 3)# Bad example
my_vector<-c(1, 2, 3)*
```

*Of course, as I intertwine a lot between Python and R scripts, sometimes the naughty **=** 的时候就完成了赋值:-)*

## *线长度*

*避免每行超过 80 个字符，以便您的代码可以适合大多数 IDE 窗口。这也是其他编程语言(如 Python)的最佳实践。您希望避免脚本的读者来回使用水平滚动条(这是一个容易迷失在代码中的方法)。*

*想象一个函数，用很长的参数调用:*

```
*CalculatesMeaningOfLife('This is a really long argument','This is another really long argument','This is a third really long argument!')* 
```

*调用此代码时的一般最佳做法是执行以下操作:*

```
*CalculatesMeaningOfLife(
  'This is a really long argument',
  'This is another really long argument',
  'This is a third really long argument!'
)*
```

*R Studio 有一些自动缩进，当你在逗号后按回车键。这是一个简洁的特性，它使得我们的代码更容易保持整洁。*

## *间隔*

*当调用函数或索引对象时，在每个**、**之后提供一个空格总是一个好主意。这使得代码可读性更好，避免了“压缩代码”的想法。压缩代码是当我们有如下:*

```
*my_array = array(1:10,c(2,5))
my_array[,c(1,2)]*
```

*代码可以工作，但是所有的代码都被捆绑在一起，没有空格。从视觉上看，很难理解哪个位是指数组中的第一维还是第二维。*

*更干净的方法是执行以下操作:*

```
*my_array = array(1:10, c(2, 5))
my_array[, c(1, 2)]*
```

*请注意我是如何在代码中的每个逗号后添加一个空格的。这就是人们通常所说的让代码呼吸— **你可以更容易地理解你在每个维度上索引了什么，以及你的函数调用的每个参数包含了什么。***

## *不要重复自己的话*

*任何编程语言(至少是函数式语言)中最重要的概念之一是 **DRY 的概念。**一条普遍的黄金法则是，当你发现自己复制并粘贴了大量代码时，这是一个函数的好用法。*

*举一个非常简单的例子，假设我们想用 R:*

```
*paste('Hello','John')
paste('Hello','Susan')
paste('Hello','Matt')
paste('Hello','Anne')
paste('Hello','Joe')
paste('Hello','Tyson')
paste('Hello','Julia')
paste('Hello','Cathy')*
```

*您多次重复相同的代码，只是更改了学生的姓名。在您看来，这应该会立即引发使用功能的需求:*

```
*GreetStudent <- function(name) {
 paste(‘Hello’,name)
}class_names <- c(‘John’, ‘Susan’, ‘Matt’ ,’Anne’,
              ‘Joe’, ‘Tyson’, ‘Julia’, ‘Cathy’)for (student in class_names){
 print(GreetStudent(student))
}*
```

***酷的是，现在您可以向我们的 class_names 向量添加更多的学生，并且您可以避免多次重复粘贴命令！***

*函数确实是 r 中最强大的概念之一。人们应该多次依赖它们，因为它们将使我们的代码高效且可测试。*

*就是这样！你还有什么要补充吗？*

*这里有很多我没有涉及的最佳实践，可能还有很多我自己都不知道的。开源语言让我着迷的是它们发展的速度有多快，以及社区如何在开发更好的程序和脚本以实现更高的生产力方面相互支持。*

*最后一点，记住学习一门新的语言是一项需要时间的技能，对一个人来说成为万事通几乎是不可能的。你周围的每个人，不管他们是多么高级的语言，总是有东西要学，学习的心态是成为一个更好的专业人士和普通人的最佳心态。*

*随时伸出手如果你想了解更多关于 R 的知识，可以在这里加入我的 R 编程课程: [***绝对初学者 R 编程课程***](https://www.udemy.com/course/r-for-absolute-beginners/?couponCode=LEARN_R)*

> *感谢您花时间阅读这篇文章！随意在 LinkedIn 上加我([*https://www.linkedin.com/in/ivobernardo/*](https://www.linkedin.com/in/ivobernardo/)*)查看我公司网站(*[*https://daredata.engineering/home*](https://daredata.engineering/home)*)。**
> 
> *如果你有兴趣接受分析和数据科学方面的培训，你也可以访问我在 Udemy([https://www.udemy.com/user/ivo-bernardo/](https://www.udemy.com/user/ivo-bernardo/))上的页面*

****这个讲座摘自我在 Udemy 平台上的*** [***R 编程课程***](https://www.udemy.com/course/r-for-absolute-beginners/?couponCode=LEARN_R)***——该课程适合初学者和想学习 R 编程基础的人。该课程还包含 50 多个编码练习，使您能够在学习新概念的同时进行练习。****