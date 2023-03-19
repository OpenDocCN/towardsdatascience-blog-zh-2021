# 在 R 中 mutate()处理数据争论的能力

> 原文：<https://towardsdatascience.com/the-power-of-mutate-for-data-wrangling-in-r-172dd2b0be73?source=collection_archive---------7----------------------->

## 一系列代码片段，将帮助您使用 R 语言转换数据。

![](img/5347d13cc0e65dec59e723c98a574f40.png)

罗斯·芬登在 [Unsplash](https://unsplash.com/s/photos/change?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

我周围看到的大部分数据角力和转换相关的帖子都在用熊猫。我，作为一个熊猫爱好者，已经写了一些。但是很高兴知道还有其他伟大的工具可以帮助你进行同样的转变。有时候这更容易，有时候没那么容易。

## Tidyverse

当我几年前学习 R 语言时，我必须导入许多库来处理数据，比如`dplyr`、`tidyr`等。现在，在 tidyverse 包出现后，你只需添加那行`library(tydiverse)`，你就可以很好地完成本文中显示的所有转换。

提醒:在 R 中安装库只需要`install.packages("lib name")`。

让我们开始编码吧。首先，我将创建这个简单的数据集作为我们的示例。

```
# creating a dataframe
df <- data.frame(col1=c(1,2,3,4,5,7,6,8,9,7),
                 col2=c(2,3,4,5,6,5,5,4,6,3),
                 col3=c(5,7,8,9,9,3,5,3,8,9),
                 col4=c(43,54,6,3,8,5,6,4,4,3)) col1 col2 col3 col4
1     1    2    5   43     
2     2    3    7   54      
3     3    4    8    6      
4     4    5    9    3      
5     5    6    9    8      
6     7    5    3    5      
7     6    5    5    6      
8     8    4    3    4       
9     9    6    8    4      
10    7    3    9    3 
```

## 变异( )

`mutate()`是一个添加新变量并保留现有变量的 dplyr 函数。那是[文档](https://dplyr.tidyverse.org/reference/mutate.html)说的。因此，当您想要添加新变量或更改数据集中已有的变量时，这是您的好帮手。

给定我们的数据集`df`，我们可以很容易地添加带有计算的列。

```
# Add mean, std and median of columnsmutate(df, mean_col1 = mean(col1),
       std_col2 = sd(col2), 
       median_col3 = median(col3)) col1 col2 col3 col4 mean_col1 std_col2 median_col3
1     1    2    5   43       5.2 1.337494         7.5
2     2    3    7   54       5.2 1.337494         7.5
3     3    4    8    6       5.2 1.337494         7.5
4     4    5    9    3       5.2 1.337494         7.5
5     5    6    9    8       5.2 1.337494         7.5
6     7    5    3    5       5.2 1.337494         7.5
7     6    5    5    6       5.2 1.337494         7.5
8     8    4    3    4       5.2 1.337494         7.5
9     9    6    8    4       5.2 1.337494         7.5
10    7    3    9    3       5.2 1.337494         7.5
```

添加一个新变量。

```
# Create new Variable
new_var <- c('a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j')# Add it to the dataset
mutate(df, new_var)
```

用更改的数据替换变量。

```
# Change column 1 to proportions
mutate(df, col1=proportions(col1)
           col2 = col2*100) col1     col2 col3 col4
1  0.01923077  200    5   43
2  0.03846154  300    7   54
3  0.05769231  400    8    6
4  0.07692308  500    9    3
5  0.09615385  600    9    8
6  0.13461538  500    3    5
7  0.11538462  500    5    6
8  0.15384615  400    3    4
9  0.17307692  600    8    4
10 0.13461538  300    9    3
```

## 将 mutate 与其他函数结合使用

Mutate 可以和其他伟大的功能结合使用。让我们看几个例子。

## 变异和重新编码

`recode()`是一种改变变量内观察值的方法。可以和`mutate()`一起使用。

```
# Change letters from 'new_var' to numbersmutate(df, new_var = recode(new_var,
       'a' = 1, 'b' = 2, 'c' = 3, 'd' = 4, 'e' = 5,
       'f' = 6, 'g' = 7, 'h' = 8, 'i' = 9, 'j' = 10)
       ) col1 col2 col3 col4 new_var
1     1    2    5   43       1
2     2    3    7   54       2
3     3    4    8    6       3
4     4    5    9    3       4
5     5    6    9    8       5
6     7    5    3    5       6
7     6    5    5    6       7
8     8    4    3    4       8
9     9    6    8    4       9
10    7    3    9    3      10
```

## 变异和替换

`replace()`类似于 recode，但是它是来自 base R 的一个包，您可以使用它来基于一列值或逐个地更改观察值。

`replace`函数接收变量列、要更改的*索引列表*和值列表。另一个选择是指向你想要改变的某个值和替换值。

```
# replace using index list
mutate(df,
       new_var = replace(new_var, c(1,2,3,4,5,6,7,8,9,10), c(1,2,3,4,5,6,7,8,9,10))
       )# replace using value to be changed
mutate(df,
       new_var = replace(new_var, new_var=='a', 1)
```

## 变异和切割

`cut()`是以 R 为基数的函数，用于转换数值范围内的观察值，并将这些值放入箱中。

在下面的例子中，我使用`cut`为第 1 列创建两个库[ *从最小值到平均值和从平均值到最大值* ]。那么第一个容器中的每个值将收到标签“低于平均值”，而第二个容器得到“高于平均值”。

```
# Labeling with cut
mutate(df, col1_label_avg = cut(col1, 
                            c(-Inf, mean(col1), max(col1)),
                            c('lower than avg', 'above the avg') 
                                )
       ) col1 col2 col3 col4    col1_label_avg
1     1    2    5   43      lower than avg
2     2    3    7   54      lower than avg
3     3    4    8    6      lower than avg
4     4    5    9    3      lower than avg
5     5    6    9    8      lower than avg
6     7    5    3    5      above the avg
7     6    5    5    6      above the avg
8     8    4    3    4      above the avg
9     9    6    8    4      above the avg
10    7    3    9    3      above the avg
```

## 变异和转化

`transmute()`是 dplyr 包中的另一个函数。它增加新的变量，同时减少其他变量。我们可以使用它来添加 col1 标签，并删除练习中其他不必要的列。

```
mutate(df, col1_label_avg = cut(col1, 
                       c(-Inf, mean(col1), max(col1)),
                       c('lower than avg', 'above the avg'))
) %>% 
  transmute(col1, col1_label_avg) #drops other cols col1   col1_label_avg
1     1     lower than avg
2     2     lower than avg
3     3     lower than avg
4     4     lower than avg
5     5     lower than avg
6     7     above the avg
7     6     above the avg
8     8     above the avg
9     9     above the avg
10    7     above the avg
```

![](img/5ffde9ab2cac7e5dfbd39caa728eb1aa.png)

由 [vitamina poleznova](https://unsplash.com/@poleznova?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/select?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 变异和选择

`select()`是 dplyr 中的一个函数，工作方式很像 SQL 语句。它选择您需要的列，并按照它们列出的顺序排列它们。

```
# Performing a transformation and selecting columns
df %>% 
  mutate( col1_pct = proportions(col1) ) %>% 
  select (col1, col1_pct)
```

我们可以创建一些转换，并使用 select 按照我们想要的方式对列进行排序。

```
df %>% 
  **# add new column and calc proportion of col1**
  mutate( new_var, col1_pct = proportions(col1) ) %>% 
 ** # change values in new_var from letters to numbers, add as new col**
  mutate( new_var_numbers = replace(new_var,  1:10, 1:10) ) %>%
 ** # select the transformed columns in the desired order**
  select (col1_pct, col1, new_var_numbers, new_var,) col1_pct    col1    new_var_numbers   new_var
1  0.01923077    1               1         a
2  0.03846154    2               2         b
3  0.05769231    3               3         c
4  0.07692308    4               4         d
5  0.09615385    5               5         e
6  0.13461538    7               6         f
7  0.11538462    6               7         g
8  0.15384615    8               8         h
9  0.17307692    9               9         i
10 0.13461538    7              10         j
```

## 在你走之前

你可以用`mutate()`做很多事情。这是 R 语言中一个强大的方法，当与其他方法结合时，它增加了可能性。

通过实践并应用到您的数据探索中来利用它。将它与本帖中介绍的其他一些功能一起使用。

*   `mutate()`:增加或改变变量。对变量进行计算。
*   `recode()`:改变变量内的观察值。
*   `replace()`:类似于 recode，但是您可以使用列表来更改值。
*   `cut()`:将观察值放入数值范围。
*   `transmute()`:添加新变量并丢弃其他未列出的变量。
*   `select()`:用于选择或重新定位变量。

如果你对这些内容感兴趣，别忘了关注我。

[](https://medium.com/gustavorsantos) [## 古斯塔夫·桑托斯

### 让我们做出更好的决定。数据驱动的决策。我用 Python，R，Excel，SQL 创建数据科学项目。

medium.com](https://medium.com/gustavorsantos)