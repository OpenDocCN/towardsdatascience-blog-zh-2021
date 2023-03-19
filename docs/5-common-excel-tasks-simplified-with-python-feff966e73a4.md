# 使用 Python 简化的 5 个常见 Excel 任务

> 原文：<https://towardsdatascience.com/5-common-excel-tasks-simplified-with-python-feff966e73a4?source=collection_archive---------13----------------------->

## 使用 Pandas、OS 和 datetime 模块简化 Python 中的 Excel 任务。

![](img/dd9bed96752c976d852e04303a1750f9.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Mélody P](https://unsplash.com/@melodyp?utm_source=medium&utm_medium=referral) 拍摄

我的第一份工作是做一些简单的 Excel 任务，这些任务在大范围内变得冗长而耗时。我可以手动完成所有这些工作，成为处理日常任务的专家，但是我决定学习 Python 来简化它们。

Excel 是处理数据的好工具，但在处理数百个需要修改的文件时有一些限制。这就是 Python 可以提供帮助的地方。

在本文中，我们将简化我(可能还有你)在工作中经常要做的 5 个 Excel 任务。

# 重要的事情先来

## 安装库

为了用 Python 简化这些 Excel 任务，我将使用 Pandas、OS 和 datetime。Python 默认自带最后两个库，而 Pandas 需要安装。为此，在您的终端中编写以下命令。

```
pip install pandas
```

在本文中，我假设您知道如何使用 Pandas 导入和导出 Excel 文件。但是如果你不知道怎么做，这里有一些方法:

```
import pandas as pd# Read an Excel file and save it to a dataframe
df = pd.read_excel(excel_file_path)... # (all the operations we'll do in this article)# Export dataframe to a new Excel file
df.to_excel(file_path, index=False)
```

## 数据

对于本指南，我使用了两个 Excel 文件，它们位于我的 Python 脚本所在的目录中。电子表格包含如下所示的虚拟数据。

这些数据并不相关。主要思想是所有的数据被分成多个块(Book1，Book2，Book3，…)，我们的任务是对所有的数据执行操作。

您可以创建自己的虚拟数据，使用自己的 Excel 文件或从我的 [Github](https://github.com/ifrankandrade/data_preprocessing/tree/main/datasets/common_tasks) 上下载这两个文件。**将这些文件放在 Python 脚本所在的目录中。**

# 1)用 Python 连接多个 Excel 文件

我们的第一个任务是将所有这些块合并到一个 Excel 文件中(假设我们想与老板共享所有数据)。

如果有 2 个 Excel 文件，这样做将像复制一个文件中的数据并粘贴到另一个文件中一样容易；然而，对数百个文件这样做是不切实际的。

一个简单的解决办法是使用熊猫。

```
import os
import pandas as pdmy_list = []
for file in os.listdir():
    if file.endswith('.xlsx'):
        df = pd.read_excel(file)
        my_list.append(df)

df_concat = pd.concat(my_list, axis=0)
df_concat.to_excel('concatenate.xlsx', index=False)
```

首先，我们用`os.listdir()`列出当前目录下的所有文件。然后我们用`.endswith(‘.xlsx’)`方法过滤掉文件。

所有的 Excel 文件将被存储到一个名为`my_list`的列表中，然后与返回一个我们称之为`df_concat`的数据帧的`pd.concat()`相结合。该数据帧被导出到 Excel 文件中。

注意，如果使用逗号分隔值(CSV 文件)，可以使用`read_csv()`和`to_csv()`

# 2)使用 Python 更改多个 CSV 文件的名称或扩展名

CSV 是一种常用的文件扩展名，用于存储和读取数据。可以用 Excel 打开，在工作中广泛使用。

我经常需要重命名 CSV 文件，这样任何人都可以很容易地识别它们(例如，在名称的开头添加日期),并且还必须进行转换。csv 文件到。txt 文件，反之亦然，

两者都是简单的任务，但是当处理数百个文件时，它们就变得令人头疼了。幸运的是，我使用下面代码所示的`.split()`和`os.rename()`方法在几秒钟内完成了这些任务。

```
import os
import datetime# 1.Getting the current date in YYYYMMDD format
today = datetime.date.today()
today_str = today.strftime('%Y%m%d')# 2.Adding date and changing extension to CSV files within directory
for file in os.listdir():
    if file.endswith('.csv'):
        file_name, file_extension = file.split('.')
        new_file_name = f'{today_str}_{file_name}'
        new_extension = '.txt'
        new_name = f'{new_file_name}{new_extension}'
        os.rename(file, new_name)
```

首先，我们必须使用 datetime 模块来获取当前日期，并按照我们的要求对其进行格式化(在本例中是年、月、日)。

然后我们过滤掉 CSV 之外的文件，每次文件名到达一个点号(.)，所以我们把扩展名和文件名分开。我们用 f 字符串`(f’’)`连接名称，并用`os.rename()`重命名文件。这将当前文件作为第一个参数，新名称作为第二个参数

# 3)在 Python 中左右使用 Excel 的字符串函数

想象一下，在我们之前见过的电子表格中，您必须将国家代码添加到“号码”列的每个电话号码中。

我们可以对每个文件执行 Excel 的 LEFT 函数，但这会花费很多时间。相反，我们可以使用 for 循环，将每个 Excel 文件读入 Pandas dataframe，然后使用`lambda`和 f-string 将国家代码添加到每个电话号码中。

```
import os
import pandas as pdfor file in os.listdir():
    if file.endswith('.xlsx'):
        file_name = file.split('.')[0]
        df = pd.read_excel(file)
        df = df.astype({'Number': str})
        df['Number'] = df['Number'].apply(lambda x: f'44{x}')
        df.to_excel(f'country_code_{file_name}.xlsx', index=False)
```

如果我们一起使用`apply`和`lambda`函数，我们可以修改列中的值。在本例中，我在“Number”列中每个值的开头添加了“44”代码。注意，我们必须确保列“Number”中的值是 string (str ),所以我们使用了`.astype()`方法来更改数据类型。

最后，我们将数据帧导出到一个名称中包含单词“country_code”的新 Excel 文件中。

# 4)删除列中的重复项并删除 NaN 值

在 Excel 中执行的一些基本数据清理，如删除重复项和 NaN 值，可以通过 Pandas 轻松完成。

您可以使用`.drop_duplicates().`轻松删除基于所有列的重复行

```
df = df.drop_duplicates()
```

但是如果您想要删除特定列上的重复项，请添加`subset`参数。假设我们想删除“数字”列中的重复项。

```
df = df.drop_duplicates(subset=['Number'])
```

另一方面，通过`.dropna()`可以降低 NaN 值

```
df = df.dropna(subset=[‘column_name’])
```

也可以替换空值，而不是删除整行。假设我们想用零替换空值。

```
df = df.fillna(0)
```

除此之外，还可以用高于(向前填充)或低于(向后填充)一行的值来填充这些空值。

```
# Forward filling
df.fillna(method='ffill')# Backwards filling
df.fillna(method='bfill')
```

# 5)排序值

对熊猫数据框架进行排序就像对 Excel 电子表格进行排序一样简单。我们只需使用`.sort_values()`并指定升序或降序`(ascending=True, ascending=False)`以及排序所依据的列。

```
df = df.sort_values(ascending=True, by=['Year'])
```

如果我们想保存更改，而不需要像上面显示的那样将值设置为等于`df`，我们可以使用`inplace`参数

```
df.sort_values(ascending=True, by=['Number'], inplace=True)
```

就是这样！这是我们在工作中可能遇到的 5 个常见任务，可以用 Python 简化。如果你是一个 Excel 用户，想使用 Python/Pandas，请查看下面的指南。

</a-complete-yet-simple-guide-to-move-from-excel-to-python-d664e5683039>  

[**与 3k 以上的人一起加入我的电子邮件列表，获取我在所有教程中使用的 Python for Data Science 备忘单(免费 PDF)**](https://frankandrade.ck.page/bd063ff2d3)

如果你喜欢阅读这样的故事，并想支持我成为一名作家，可以考虑报名成为一名媒体成员。每月 5 美元，你可以无限制地阅读媒体上的故事。如果你使用[我的链接](https://frank-andrade.medium.com/membership)注册，我会赚一小笔佣金，不需要你额外付费。

<https://frank-andrade.medium.com/membership> 