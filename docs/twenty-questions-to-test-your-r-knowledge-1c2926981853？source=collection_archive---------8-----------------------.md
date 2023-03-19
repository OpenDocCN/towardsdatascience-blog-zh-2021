# 测试你的 R 知识的 20 个问题

> 原文：<https://towardsdatascience.com/twenty-questions-to-test-your-r-knowledge-1c2926981853?source=collection_archive---------8----------------------->

## 用这 20 个简短有趣的问题来看看你是不是超级明星

![](img/a833475a12807451adf1a848d5145657.png)

Unsplash.com[阮当黄鹤年](https://unsplash.com/@nguyendhn)

虽然不像 Python 那样流行，但作为编程语言，R 拥有强大且不断增长的用户基础，作为应用统计学家，它将永远是我的首选语言。根据我的经验，R 用户有很多种类型。有些人只是勉强掌握足够的知识来完成他们的统计作业，有些人经常使用它，但主要是围绕像`dplyr`这样方便的数据争论包工作，还有一些人对该语言及其底层结构有深刻的了解。

你坐在哪里？这里有 20 个问题来测试你的 R 知识。尝试在不运行代码的情况下回答这些问题，然后检查下面的答案，看看你做得如何。然后也许找个朋友自我测试一下，比较一下笔记。我写这个测验是为了好玩，所以请不要把它用于像数据科学面试这样严肃的事情，它不是为了那个目的！

## 小测验

**问题 1:** `x <- vector()`。`x`的数据类型是什么？

**问题二:** `y <- 2147483640L:2147483648L`。`y`的数据类型是什么？

**问题三:** `z <- 0/0` **。**什么是`z`类？

**问题四:**如果`v <- complex(1,1)`，那么`c(v, TRUE)`的输出是多少？

**问题 5:** 在 *R* 中的一个齐次的一维和二维数据结构，分别称为*原子向量*和*矩阵*。a)一维异构数据结构，b)二维异构数据结构和 c)n 维数据结构的名称是什么？

**问题 6:** 帐篷营地和*吃风筝树*对 *R 的意义是什么？*这些术语的由来是什么？

**问题 7:** 如果不安装`dplyr`包，下面每种情况会发生什么？

案例 1:

```
library(dplyr)mtcars %>% 
  group_by(cyl) %>% 
  summarize(mean_mpg = mean(mpg))
```

案例二:

```
require(dplyr)mtcars %>% 
  group_by(cyl) %>% 
  summarize(mean_mpg = mean(mpg))
```

**问题 8:** `a <- c(2, "NA", 3)`。`sum(is.na(a))`的输出是什么？

**问题 9:**`data()`的输出是多少？

**问题 10:**`round(0.5)`的输出是多少？

**问题 11:** 当您运行命令`library(tidyverse)`时，这些包中的哪个*没有*加载？a) `dplyr` b) `tidyr` c) `broom` d) `ggplot2`

**问题 12:** 在最新的 R 版本中，*不是*下面三个代码片段中的哪一个正确地跨列表`l`的元素应用了一个函数？

```
## A
lapply(l, function(x) x + 10)## B
lapply(l, x -> x + 10)## C
lapply(l, \(x) x + 10)
```

**问题 13:** 看一下这段代码的输出，注意它并没有像我们预期的那样产生每一行的总和？在不编辑现有代码的情况下，需要在代码中添加什么来纠正这种情况？

```
library(dplyr)df <- data.frame(x = c(1, 2), y = c(1, 2), z = c(1,2))df %>%
  mutate(sum = sum(x, y, z))##   x y z sum
## 1 1 1 1   9
## 2 2 2 2   9
```

**问题 14:** 以下哪个包允许用户用 R 运行 Julia 语言的代码？a) `JuliaCall` b) `RJulia` c) `JuliaR`

**问题 15:** 以下哪一项是`dplyr`最新版本中的功能？a) `c_across` b) `r_across` c) `l_across` d) `s_across`

**问题 16:** 为什么下面的代码*不能运行*需要添加什么才能运行？

```
library(tidyverse)mtcars %>%
  nest_by(cyl) %>%
  dplyr::mutate(
    ggplot(data = data,
           aes(x = hp, y = mpg)) +
           geom_point()
  )
```

**问题 17:** 什么函数可以用来从均匀分布中生成随机数？

**问题 18:** 如果`x <- factor(c(4, 5, 6))``as.numeric(x)`的输出是什么？

**问题 19:** 再用`x <- factor(c(4, 5, 6))``str(x)`和`typeof(x)`的输出有什么区别？

**问题 20:** 看下面两个代码片段。如果都运行在最新版本的 R 中，为什么片段 A 会成功而片段 B 会失败？

```
library(dplyr)## Snippet Amtcars %>%
  filter(grepl("Mazda", rownames(.)))## Snippet Bmtcars |>
  filter(grepl("Mazda", rownames(.)))
```

# 答案

1.  `x`合乎逻辑。这是原子向量的默认类型。
2.  `y`是一个双精度数，尽管在`y`中使用了整数符号`L`。这是因为 *R* 中整数的最大值是 2147483647。因此，`y`的最后一个值被强制为双精度值，因此由于原子向量是同质的，所以整个向量被强制为双精度值。
3.  `z`属于数字类。
4.  输出是一个包含两个元素的向量，都是`1 + 0i`。注意，`complex()`的第一个参数是`length.out`,表示复数向量的长度。因此`complex(1,1)`评估为`1 + 0i`，而`complex(1,1,1)`评估为`1 + 1i`。请注意，`TRUE`将被强制转换为与`1 + 0i` *等价的复杂类型。*
5.  a)列表；b)数据帧；c)阵列
6.  它们是 *R* 版本发布的昵称。所有版本昵称都取自老*花生*漫画*。*
7.  在案例 1 中，第一行将生成一个错误，指示没有安装这样的包，执行将停止。在第二种情况下，第一行会产生一个警告，但是第二行仍然会被执行，并且会因为找不到`%>%`而产生一个错误(假设`magrittr`没有被附加)。这很好的说明了`library()`和`require()`的区别。`library()`附加一个包，但是`require()`评估包是否已经被附加，如果已经被附加，评估到`TRUE`，否则评估到`FALSE`。使用`require()`会增加调试代码的难度。
8.  这等于零。注意`"NA"`是一个字符串，而不是一个缺失值。
9.  输出是 r 中所有内置数据集的列表。
10.  输出为零。r 遵循 IEC 60559 标准，其中 0.5 四舍五入到最接近的偶数*。*
11.  *`broom`未加载，因为它不是“core tidyverse”中的包。注意`install.packages("tidyverse")`会将`broom`和所有的包一起安装在核心和扩展 tidyverse 中。*
12.  *选项 B 行不通。注意，选项 C 是 R 4.1.0 中发布的新的匿名函数语法。*
13.  *代码需要在`mutate`语句前包含行`rowwise() %>%`，以声明该函数将逐行应用。*
14.  *`JuliaCall`*
15.  *`c_across`。它相当于`across()`函数，但用于行操作。*
16.  *此代码试图生成一列绘图。这需要声明为如下的*列表列*:*

```
*library(tidyverse)mtcars %>%
  nest_by(cyl) %>%
  dplyr::mutate(
    list(ggplot(data = data,
                aes(x = hp, y = mpg)) +
                geom_point())
  )*
```

*17.`runif()`*

*18.输出是矢量`c(1, 2, 3)`。因子被转换成它们的整数表示。*

*19.`str(x)`给出了因子`x`的结构。`typeof(x)`给出了`x`中数据的存储方式，为整数。*

*20.代码片段 B 使用了新的本地管道操作符`|>`。与代码片段 A 中的管道操作符`%>%`不同，本机管道只管道到函数的第一个未命名参数，而不接受`.`管道到其他参数。为了获得与代码片段 A 相同的输出，需要使用下面的匿名函数:*

```
*mtcars |>
  {\(df) filter(df, grepl("Mazda", rownames(df)))}()*
```

# *你做得怎么样？*

***如果你的得分在 5 分以下**，你迫切需要一个基础教程 *R* 来避免花太多时间解决代码中不必要的错误。*

*如果你的得分在 6-10 分之间，你的知识水平可能和大多数用户差不多。*

*11-15 是一个很好的分数，你显然知道很多编程语言的基本原理和结构。*

*如果你得了 16-20 分，你就是超级明星。你可能知道很多不必要的 R 琐事，你很可能是一个 R 学究。我希望你正在帮助其他人。*

*最初我是一名纯粹的数学家，后来我成为了一名心理计量学家和数据科学家。我热衷于将所有这些学科的严谨性应用到复杂的人的问题上。我也是一个编码极客和日本 RPG 的超级粉丝。在 [*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/) *或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找我。也可以看看我在*[*drkeithmcnulty.com*](http://drkeithmcnulty.com/)*上的博客或者我即将发布的* [*关于人物分析的教科书*](https://www.routledge.com/Handbook-of-Regression-Modeling-in-People-Analytics-With-Examples-in-R/McNulty/p/book/9781032041742) *。**