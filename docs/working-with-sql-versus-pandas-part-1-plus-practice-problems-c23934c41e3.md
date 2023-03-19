# 使用 SQL 与熊猫(第 1 部分)以及练习题

> 原文：<https://towardsdatascience.com/working-with-sql-versus-pandas-part-1-plus-practice-problems-c23934c41e3?source=collection_archive---------14----------------------->

## 选择、过滤和排序数据

![](img/39bf28b71074a984cdedbae5fc6a7bc7.png)

照片由[格雷格·吉诺](https://unsplash.com/@gregjeanneau?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 介绍

Python Pandas 库和 SQL 都是操作数据的流行工具，在功能上有很多重叠。那么，有什么更好的方法来提高您在这两方面的技能，而不是通过为 Pandas 和 SQL 解决相同的编码问题来让它们短兵相接呢？

这是我将撰写的帮助您直接比较 Pandas 和 SQL 的系列文章的第一篇。我的目标是帮助你:

1.  了解和比较功能
2.  如果你已经知道一个，那就去学习另一个
3.  了解各自的优势和劣势
4.  拥有处理数据的多种工具

对于这里的例子，我将使用臭名昭著的泰坦尼克号数据集，它可以在 Kaggle 上找到。我鼓励你在阅读这篇文章的时候看看它，这样你就能跟上了。最后，我将根据本文中讨论的特定功能，提出一些练习问题供您自己解决。

# 选择列

让我们从如何选择列并以特定顺序显示它们的最基本功能开始。在我们的练习中，假设我们很想依次看到数据集的 pclass、fare、age 和 survived 列。

## 结构化查询语言

在 SQL 中，`SELECT`语句是您列出想要显示的列以及显示顺序的地方。在 titanic 数据集作为一个表输入到我们的数据库中之后，我们从它的数据集中选择列。在 SQL 中，如果您想要查看一个表的所有列，您可以使用`SELECT *`，但是我们将命名想要显示的列。

```
SELECT pclass, fare, age, survived
FROM titanic
```

## 熊猫

在 Pandas 中，titanic 数据集将被读入 Pandas 并存储为 dataframe 对象。从这里，我们将切出所需的列。有多种方法可以达到同样的效果。

```
col_list = ['pclass', 'fare', 'age', 'survived']
titanic[col_list]
```

您也可以将列列表直接放入括号中，而不创建单独的变量。

```
titanic[['pclass', 'fare', 'age', 'survived']]
```

另一种方法是使用`loc[]`。

```
titanic.loc[:, ['pclass', 'fare', 'age', 'survived']]
```

冒号表示我们想要选择所有的行，然后在逗号后面指定我们想要的列和顺序。同样，您可以创建一个列列表作为变量，并将其放在逗号之后。通常我更喜欢创建一个变量，因为在一行中，括号上的括号越少，看起来越清晰。使用列表变量也更容易遵守 [PEP-8 指南](https://www.python.org/dev/peps/pep-0008/)，因为这样可以避免代码行过长。接下来，我将继续使用`loc[]`并创建单独的变量。

这里还值得注意的是，Pandas 的`loc[]`选项提供了基于索引选择一系列行的功能。如果我们希望只查看 Titanic 数据集中从 50 到 100 的索引值，那么我们可以将`50:100`作为第一项，这样包含的行范围将是唯一返回的行。我们也可以在逗号后面使用普通的冒号来选择所有的列，或者我们可以使用它来选择一系列的列，就像对待行一样。

# 基于列值的筛选

现在，让我们假设您想要基于特定条件的结果。让我们继续上面的同一个示例，假设我们希望再次看到 pclass、fare、age 和 survived 列，但是现在我们只希望看到成年男性的条目。

## 结构化查询语言

在 SQL 中，`WHERE`子句是根据列中的特定值进行过滤的方式。`WHERE`条款在`FROM`条款之后。由于我们的问题包括需要在两个条件上进行过滤，我们将在两个条件之间列出 AND。如果希望返回满足任一条件的行，也可以在两个条件之间使用 OR。

```
SELECT pclass, fare, age, survived
FROM titanic
WHERE sex = 'male' AND age >= 18
```

## 熊猫

对于熊猫，我们将创建包含我们想要过滤的两个条件的变量。然后我们将再次使用`loc[]`选择所需的行和列。

```
male_mask = titanic.sex == 'male'
age_mask = titanic.age >= 18
col_list = ['pclass', 'fare', 'age', 'survived']titanic.loc[male_mask & age_mask, col_list]
```

这两个掩码变量是布尔值列表，用于判断数据帧的每一行是否满足该条件。还记得以前当我们使用`loc[]`时，我们如何使用冒号来指定我们想要的所有行，然后在逗号后指定我们想要的列吗？这里我们使用相同的功能，首先指定我们想要的行，然后在逗号后面指定我们想要显示的列。就像以前一样，您可以直接输入掩码标准而不创建变量，但是创建变量会使事情看起来更整洁。使用掩码变量之间的`&`,将只返回两个条件都为真的行。如果你想做“或”逻辑，你可以用管道符号`|`来代替。

Pandas 的一个巧妙的小功能是，您还可以在掩码变量前使用波形符`~`来翻转布尔值。例如，如果我们使用`~male_mask`来过滤，它将翻转所有的真为假，反之亦然。在这种情况下，它将与针对女性的过滤相同，但有时该功能会派上用场，能够使用相同的变量，但会颠倒逻辑。

# 按列值对结果排序

让我们继续我们的问题，假设我们现在希望结果首先按票价排序，然后按年龄排序。我们希望票价值按降序排序，年龄值按升序排序。这意味着任何具有相同票价值的条目将首先按照最年轻的个体对这些条目进行排序。

## 结构化查询语言

在 SQL 中，我们将使用`ORDER BY`子句。在查询中，`ORDER BY`子句将出现在`WHERE`子句之后，默认的排序方法是升序。为了将最大值排在最上面，我们将使用`DESC`关键字。

```
SELECT pclass, fare, age, survived
FROM titanic
WHERE sex = 'male' AND age >= 18
ORDER BY fare DESC, age
```

## 熊猫

对于熊猫，我们将使用`sort_values()`进行分类。我们可以使用列的列表进行排序，排序层次结构是按照列的顺序排列的。**有一个上升参数，默认设置为真。**我们可以向该参数传递一个布尔值列表，这些值按照它们被列出的顺序与相应的列相匹配。

```
male_mask = titanic.sex == 'male'
age_mask = titanic.age >= 18
col_list = ['pclass', 'fare', 'survived']df_slice = titanic.loc[male_mask & age_mask, col_list]
df_slice.sort_values(['fare', 'age'], ascending=[False, True])
```

我想在这里提出的一个重要注意事项是，**如果您计划更改 dataframe 切片的任何值，最好的做法是在`loc[]`之后加上一个** `**.copy()**`。这样，您可以确保不会意外更改原始数据帧。即使对于那些在 Pandas 中有经验的人来说，当在切片上进行更改时，知道原始数据帧何时被更改或不被更改的规则也是非常令人困惑的。

# **限制顶级结果**

我们将在练习题中添加的最后一项内容是将结果限制在前 10 名。

## 结构化查询语言

这个功能对于 SQL 来说非常简单；我们只需添加`LIMIT`子句，并将我们想要限制的条目数量限制在——在本例中是 10 个。

```
SELECT pclass, fare, age, survived
FROM titanic
WHERE sex = 'male' AND age >= 18
ORDER BY fare DESC, age
LIMIT 10
```

## 熊猫

对于熊猫，我们可以通过在末端加上一个`head()`来获取最高值。您可以在括号中传递想要显示的条目数——在我们的例子中是 10。

```
male_mask = titanic.sex == 'male'
age_mask = titanic.age >= 18
col_list = ['pclass', 'fare', 'survived']df_slice = titanic.loc[male_mask & age_mask, col_list]
df_slice.sort_values(['fare', 'age'], ascending=[False, True]).head(10)
```

我想在这里指出，Pandas 也有`nlargest()`和`nsmallest()`方法，可以方便地对列进行排序，并限制在一个简单的方法中返回的结果。然而，在我们的问题中，我们希望按降序过滤一列，按升序过滤另一列，所以`nlargest()`和`nsmallest()`方法并不真正符合我们的需要。然而，假设我们实际上想按升序过滤票价和年龄。这将是`nsmallest()`的一个有效用例。

```
male_mask = titanic.sex == 'male'
age_mask = titanic.age >= 18
col_list = ['pclass', 'fare', 'survived']df_slice = titanic.loc[male_mask & age_mask, col_list]
df_slice.nsmallest(10, ['fare', 'age'])
```

# **练习题**

1.  为没有父母陪同的 16 岁以下儿童显示“年龄”、“性别”和“幸存”列(parch 列中的值为零)。按年龄对结果进行升序排序。
2.  显示 2 级或 3 级女性的级别、年龄和票价。按年龄和票价降序排列结果，并限制前 20 个条目。
3.  显示第三班幸存儿童(18 岁以下)的所有列。将结果按年龄升序排序，然后按票价降序排序。幸存列包含 1 表示是，0 表示否。

# **停止—下面的答案**

![](img/2c10bb8a12bf89f94e6dac36f5e17c5c.png)

约翰·马特丘克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我希望你在偷看的时候没有至少在脑子里试一试每个问题！有些问题有多种解决方法，所以预计你的答案会与我的略有不同。

**1。为没有父母陪同的 16 岁以下儿童显示“年龄”、“性别”和“幸存”列(parch 列中的值为零)。按年龄对结果进行升序排序。**

SQL:

```
SELECT age, sex, survived
FROM titanic
WHERE age < 16 AND parch == 0
ORDER BY age
```

熊猫:

```
children = titanic.age < 16
parents = titanic.parch == 0
col_list = ['age', 'sex', 'survived']titanic.loc[children & parents, col_list].sort_values('age')
```

**2。显示 2 级或 3 级女性的级别、年龄和票价。按年龄和票价降序排列结果，只显示前 20 个条目。**

SQL:

```
SELECT pclass, age, fare
FROM titanic
WHERE sex = 'female' AND class != 1
ORDER BY age DESC, fare DESC
LIMIT 20
```

熊猫:

```
females = titanic.sex == 'female'
lower_class = titanic.pclass != 1
col_list = ['pclass', 'age', 'fare']df_slice = titanic.loc[females & lower_class, col_list]
df_slice.nlargest(20, ['age', 'fare'])
```

**3。显示第三班幸存儿童(18 岁以下)的所有列。将结果按年龄升序排序，然后按票价降序排序。幸存列包含 1 表示是，0 表示否**

SQL:

```
SELECT *
FROM titanic
WHERE age < 18 AND pclass = 3 AND survived = 1
ORDER BY age, fare DESC
```

熊猫:

```
children = titanic.age < 18
low_class = titanic.pclass == 3
survival = titanic.survived == 1df_slice = titanic.loc[children & low_class & survival, :]
df_slice.sort_values(['age', 'fare'], ascending=[True, False])
```

# 结论

如您所见，SQL 和 Pandas 非常不同，但仍然可以完成相同的任务。在写这篇文章的时候，我很喜欢在 Pandas 和 SQL 之间切换，所以我希望你也喜欢。请关注本系列的后续部分！

第二部分可以在这里找到:

</working-with-sql-versus-pandas-part-2-plus-practice-problems-ae1c19aab114> [## 使用 SQL 与熊猫(第 2 部分)以及练习题

towardsdatascience.com](/working-with-sql-versus-pandas-part-2-plus-practice-problems-ae1c19aab114)