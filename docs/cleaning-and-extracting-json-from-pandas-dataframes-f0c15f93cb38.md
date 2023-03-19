# 熊猫数据帧中 JSON 的清洗和提取

> 原文：<https://towardsdatascience.com/cleaning-and-extracting-json-from-pandas-dataframes-f0c15f93cb38?source=collection_archive---------5----------------------->

## 包含 Github 回购

## 揭示 JSON 嵌入式专栏中隐藏的见解

![](img/e95ee32963e86557aa94885ee1558312.png)

照片由 [Gerold Hinzen](https://unsplash.com/@geroldhinzen?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

我认识的一个人最近遇到了一个有趣的问题，我花了一分钟才弄明白。他们在 Kaggle 上发现了这些数据，其中包含嵌入了 JSON 的列。有多个这样的列，他们想从这些列中提取数据，并在同一个数据帧上表示它们。

让我们更详细地看看这个问题。这是一个数据的例子。

## 步骤 1:解码 JSON

JSON (JavaScript Object Notation)是大量信息在互联网上传输的方式。幸运的是，Python 标准库附带了一个名为`json`的库。这意味着如果你已经安装了 Python，那么你就已经有了这个模块。如果您有兴趣了解更多信息，请查看[文档](https://docs.python.org/3/library/json.html#module-json)！

通过导入`json`包，我们可以将所有的 JSON 对象转换成它们各自的 Python 数据类型。Python 和 Pandas 不会明确地告诉你什么是 JSON，但是这通常很容易确定你是否在花括号(`{}`)中嵌套了数据，转换为`str`类型。

下面是我写的将 JSON 解码成 Python 的代码:

## 步骤 2:跨多个列表示 JSON 数据

除非我们能从 JSON 中提取数据，否则我们所做的一切都没有用。为此，我创建了一个函数，它可以与[熊猫](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas-dataframe-apply) `[apply](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas-dataframe-apply)` [方法](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html#pandas-dataframe-apply)一起使用，并按行而不是按列应用(`axis=1`)。

我的想法是对数据进行一次热编码，以保持一种整洁的格式。这意味着每行代表一个观察，每列代表观察的一个属性。

我为数据中看到的每个键和值创建了一个新列，如果在感兴趣的观察中看到了该键或值，则用`1`表示，如果没有看到，则用`0`代替。

代码如下:

让我们解释一下代码在做什么，首先我们循环遍历字典列表的长度，并创建一个列表来存储`key:value`对。然后我们遍历每个字典，并使用`[.items()](https://docs.python.org/3/library/stdtypes.html?highlight=dict%20items#dict.items)`提取`key`和`values`。`.items()`将返回一个元组列表，其中第一个元素是`key`，第二个元素是关联的`value`。从这里开始，我们将把`key`和`value`加上一个下划线附加到我们的`ls`对象上。

```
**# store values**
    ls = []**# loop through the list of dictionaries**
    for y in range(len(x[0])):**# Access each key and value in each dictionary**
        for k, v in x[0][y].items():
            # append column names to ls
            ls.append(str(k)+ "_" +str(v))
```

接下来，我们将为在`ls`对象中看到的`key:value`对的每个组合创建一个新列。如果该列不存在，那么我们将创建一个具有关联名称的新列，并将所有值设置为`0`，同时将当前观察值更改为`1`。

```
**# create a new column or change 0 to 1 if keyword exists**
    for z in range(len(ls)):**# If column not in the df columns then make a new column and assign zero values while changing the current row to 1**
        if ls[z] not in df.columns:
            df[ls[z]] = 0
            df[ls[z]].iloc[x.name] = 1
        else:
            df[ls[z]].iloc[x.name] = 1
    return
```

> `x.name`返回当前观察的指标。当`axis=1`这将返回索引值，当`axis=0`这将返回列名。在这里看文件[。](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.name.html#pandas-series-name)

## 步骤 3:应用函数

```
**# Loop over all columns and clean json and create new columns**
for x in json_cols:
    df[[x]].apply(clean_json2, axis=1)print("New Shape", df.shape)
```

## 最后的想法

该代码返回一个超过 29，000 列的 DataFrame，需要很长时间来运行。在对所有数据进行测试之前，您可能希望对数据的子集进行测试。

在使用这段代码之前，您可以搜索每个 JSON 列，看看有多少惟一值。你总是可以将不太常见的值转换为“Other”来帮助加速运行。

现在你知道了，在 Pandas 中使用 JSON 数据并没有那么糟糕，只要有一点创造力，你就可以编写代码来实现你想要的任何东西。请随意调整代码，这样它就不会保存某些值或键。所提供的代码旨在作为您正在处理的任何 JSON 相关问题的样板。

在这里找到项目[的代码。](https://github.com/mpHarm88/projects/tree/master/json)