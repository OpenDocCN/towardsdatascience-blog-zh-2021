# 熊猫过滤瑞士军刀

> 原文：<https://towardsdatascience.com/swiss-army-knife-of-pandas-filtering-24866166ca97?source=collection_archive---------12----------------------->

![](img/33dfc6f3e8162a371c7d21a1e0e0a1ef.png)

作者图片

## 高效、可读、灵活的熊猫过滤方式

熊猫`eval`和`query`是熊猫 API 公开的一些最强大、鲜为人知、直观的函数。它允许您以一种可读和直观的方式查询数据框，不同于屏蔽和锁定数据的传统方式。产生的代码是高效的、可读的和灵活的。

过滤数据帧在任何数据管道中都是一个重要的过程，在 pandas 中，它通常是一个两步过程。创建掩膜以选择行，然后使用该掩膜设置数据框的子集。

让我们加载虹膜数据集，

```
>> import pandas as pd
>> df = pd.read_csv("./data/iris.csv")>> df.columns
Index(['sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'species'], dtype='object')>> df.shape
(150, 5)
```

# 简单的例子

比方说，你想看的行是物种 Setosa 和萼片长度> 5。这种多列/条件过滤在现实世界的 EDA 和数据管道中比你想象的更常见。它通常可以写成，

*无查询，*

```
**df**.loc[(**df**.sepal_length > 5) & (**df**.species == "Iris-setosa"), :]
```

上面代码的问题是，`df`重复了三次，并且有一对方括号和圆括号。这可以使用查询很容易地重写。

*同查询，*

```
df.query("sepal_length > 5 and species == 'Iris-setosa'")
```

> 熊猫查询为数据帧过滤提供了一个易读、直观的界面，看起来更舒服，写起来也更快。

# 使用熊猫查询的提示

既然我们已经看到了如何使用 query，我们将看看释放`query`和`eval.`的能力的实用技巧

`pd.eval`、`pd.query`、`df.eval`和`df.query`在后台使用`pd.eval`。`pd.eval`是解析字符串表达式、替换变量并识别查询中的 dataframe 列的函数。

## 过滤 NaN 的

使用 NaN 进行过滤主要是通过使用`pd.isna`完成的，然后用它来屏蔽数据帧。

*无查询，*

```
# Set the top 5 rows sepal length as
df.iloc[:5, *0*] = *None*# Filtering rows that are null **df**.loc[~pd.isna(**df**.sepal_length), :]# Filtering rows that are not null
**df**.loc[pd.isna(**df**.sepal_length), :]
```

通过查询，

```
# Filtering rows that are null
df.query("sepal_length == sepal_length")# Filtering rows that are not null
df.query("sepal_length != sepal_length")
```

这是可行的，因为将 NaN 与其自身匹配会返回 false。

## 使用`@ `的运行时动态替换

只要掩码中的比较值是动态的，就可以使用`@`将该值动态插入到比较字符串中。

```
length_thresh = 5
species = 'Iris-setosa'

df.query("sepal_length >= **@length_thresh** and species == **@species**")
```

`length_thresh`和`species`值是动态解析的。

如果列名之间包含空格，可以使用反勾号对其进行转义，

```
df.query("**`Sepal Length`** >= @length_thresh and species == @species")
```

## **F 弦**

使用 f 字符串可以增加查询语句的灵活性。`@`让你参数化一个不等式的右边，而 f 字符串让你参数化一个不等式的两边。

```
ltype = "sepal"
length = 5.0df.query(**f**"{**ltype**}_length >= {**length**}")
```

通过改变变量`ltype`，你可以控制`sepal_length`或`petal_length`是否用于过滤。

您还可以创建一个复杂的查询子串，然后将它们组合起来，使其可读，而不是一个复杂的过滤组合。

```
# Define setosa filter
species, length, ltype = "setosa", 3.0, "sepal"
setosa_mask = **f**"species == 'Iris_{**species**}' and {**ltype**}_length >={**length**}"# Define virginica filter
species, width, wtype = "virginica", 3.0, "petal"
virginica_mask = **f**"species == 'Iris_{**species**}' and {**wtype**}_width >={**width**}"# combine filter
>> df.query(f"{**setosa_mask**} or {**virginica_mask**}")
*-- Returns a dataframe of (96, 5)*
```

# 在后台

除了`expression`和`inplace`参数之外，`pandas.DataFrame.query`接受`**kwargs`，后者可以接受你传递给`pandas.eval`的任何参数。Query 在内部调用 Eval 来执行表达式。

`pandas.eval`类似于 python 的`eval`，它执行字符串中的 python 代码。

```
assert **eval**("2 + 3 * len('hello')") == 17
```

看看这个例子，很明显可以在其中执行任何 python 语句。

> 不正确的评价是邪恶的！

话虽如此，`pandas.eval`并没有听起来那么危险。不像 python 的`eval` `pandas.eval`不能执行任意函数。

```
>> **eval**("print('danger!')")
danger>> pd.eval("print('me')")
ValueError: "print" is not a supported function
```

很明显，`pandas.eval`类似于 python 的`eval`,但只针对函数和操作数的有限子集。

## 争论

还有其他`eval`的参数可以用于更多的控制。

**Parser:**eval 函数接受一个名为 Parser 的输入，它可以是`python`或`pandas`。根据解析器的不同，表达式的解析略有不同。

**引擎:**负责执行表达式的引擎。默认情况下，它被设置为`numexpr`，比另一个选项`python`更有效、更快

**局部和全局字典:**有时在表达式中传递没有在当前作用域中定义的变量是很有用的。

```
df.query("setosa_length > thresh", local_dict={'thresh': 5.0})
```

# 就因为你能，你就不应该！

Pandas eval 还可以执行语句并将它们分配给数据帧。例如，你可以这样做，

```
df.eval("**length_ratio** = sepal_length + petal_length", inplace=True)
```

上面的语句将在 dataframe 中创建一个列`length_ratio`。这与我们为什么要使用`query`的说法正好相反。与提高可读性的查询表达式不同，这种用法不明确，会影响可读性。

## 参考

1.  *查询*https://pandas.pydata.org/docs/reference/api/pandas.[DataFrame.query.html](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.query.html)
2.  *Eval*
    [https://pandas . pydata . org/docs/reference/API/pandas . Eval . html # pandas . Eval](https://pandas.pydata.org/docs/reference/api/pandas.eval.html#pandas.eval)

[作者的 LinkedIn 个人资料](https://www.linkedin.com/in/adiamaan-keerthi/)