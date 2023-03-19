# 实用 Python 熊猫技巧-第 1 部分:导入和创建数据帧

> 原文：<https://towardsdatascience.com/introduction-to-pandas-part-1-import-and-create-dataframe-e53326b6e2b1?source=collection_archive---------38----------------------->

## 导入各种数据文件，并使用列表和字典从头开始创建数据框架

![](img/9d16eaa4e65ebfaac62ddd7aa0850b9b.png)

Geran de Klerk 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文是[熊猫](https://pandas.pydata.org/)系列节目的第一部分。请继续关注更多关于这个主题的文章。

[**第 1 部分:导入并创建数据帧**](/introduction-to-pandas-part-1-import-and-create-dataframe-e53326b6e2b1)

[**第二部分:数据预览和子集化**](/introduction-to-pandas-part-2-quick-data-exploration-582fc9b0de28)

[**第三部分:数据角力**](/introduction-to-pandas-part-3-data-wrangling-b490f8e47ca2)

# 介绍

毫无疑问 **Pandas** 是数据科学领域最流行的 Python 库之一。它的多功能性和功能性使它成为数据转换和探索的强大工具。熟悉熊猫已经成为数据科学专业人士的必备技能。

# 先决条件

```
**# Install pandas library**
!pip install pandas**# Import pandas library**
import pandas as pd
```

# 从现有数据源导入数据

**导入一个 CSV 文件**

```
df = pd.read_csv('file_name.csv')
```

**从网站导入 CSV 文件**

```
df = pd.read_csv('[https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'](https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'))
```

**导入 Excel 文件**

```
df = pd.read_excel(open('file_name.xlsx', 'rb'), sheet_name='sheet_name')
```

**从 SQL 数据库导入数据:**在我们编写从 SQL 数据库提取数据的查询之前，我们需要使用有效的凭证连接到数据库。Python 库**，SQLAlchemy** 让 Python 和 SQL 数据库之间的交互变得很容易。

```
import sqlalchemyconnection = sqlalchemy.create_engine('dialect+driver://username:password@host:port/database').connect()sql = 'CREATE TABLE df AS SELECT ...'
connection.execute(sql)output = pd.read_sql('SELECT * FROM df', con=connection)
```

# 从头开始创建数据框架

**方法 1:** 从字典列表中创建数据帧

```
data = [{'Salary': 30000, 'Exp': 1, 'Gender': 'M'},             
        {'Salary': 40000, 'Exp': 3, 'Gender': 'F'}]        
df = pd.DataFrame(data)
```

输出:

![](img/7e6826368fadb557fcbaf92353d65ffe.png)

(作者创作)

**方法 2:** 从列表字典中创建数据帧

```
data= {'Salary': [30000, 40000, 50000, 85000, 75000],            
        'Exp': [1, 3, 5, 10, 25],          
        'Gender': ['M','F', 'M', 'F', 'M']} 
df = pd.DataFrame(data)
```

输出:

![](img/61764500bd3528a45f9deadfca060876.png)

(作者创作)

**方法 3** :使用 *from_dict 创建数据帧。*如果您想使用传递的字典的关键字作为结果数据帧的列名，请将 orient 设置为“列”。否则，如果键应该是行，则将 orient 设置为“index”。

```
data= {'Salary': [30000, 40000, 50000, 85000, 75000],            
        'Exp': [1, 3, 5, 10, 25],          
        'Gender': ['M','F', 'M', 'F', 'M']} 
df1 = pd.DataFrame.from_dict(data, orient = 'columns')
df2 = pd.DataFrame.from_dict(data, orient = 'index')
```

输出:

![](img/9bde3ade21f361620dfa9a6e135f1d75.png)

(由阿瑟创作)

**方法 4:** 从多个列表的列表中创建 DataFrame。我们可以分别使用“索引”和“列”选项来定义用户定义的索引和列名。

```
# pd.DataFrame([[Row 1], [Row 2], [Row 3],...])df = pd.DataFrame([[0, 2, 3], [0, 4, 1], [10, 20, 30]],                   index=['r1', 'r2', 'r3'], columns=['A', 'B', 'C'])
```

输出:

![](img/af44aed228a992a09332ccb621a047be.png)

(作者创作)

# 感谢您的阅读！！！

如果你喜欢这篇文章，并且想**请我喝杯咖啡，**请[点击这里](https://ko-fi.com/aaronzhu)。

您可以注册一个 [**会员**](https://aaron-zhu.medium.com/membership) 来解锁我的文章的全部访问权限，并且可以无限制地访问介质上的所有内容。如果你想在我发表新文章时收到电子邮件通知，请订阅。