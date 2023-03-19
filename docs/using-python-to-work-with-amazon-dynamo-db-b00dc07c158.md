# 使用 Python 处理 Amazon Dynamo DB

> 原文：<https://towardsdatascience.com/using-python-to-work-with-amazon-dynamo-db-b00dc07c158?source=collection_archive---------19----------------------->

## 在本教程中，我们将使用 Python 中的 Boto3 模块来处理亚马逊的 NoSQL 数据库 Dynamo DB。本教程还将讨论如何设置 Dynam DB 的本地实例。

![](img/0d1d364f6610d07958582c06a40a0894.png)

你好，我是尼克🎞 on [Unsplash](https://unsplash.com/s/photos/amazon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# NoSQL 数据库

NoSQL 数据库用于解决 RDMS(关系数据库管理系统)面临的挑战，或者简单地说关系数据库。下面列出了 RDMS 的一些缺点

*   必须预先定义一个模式
*   要存储的数据必须是结构化的
*   很难改变表和关系

另一方面，NoSQL 数据库可以处理非结构化数据，不需要定义模式。

在本教程中，我们将与亚马逊迪纳摩数据库。它是一种**键值**和**文档数据库** NoSQL 数据库。

# 目录

1.  先决条件
2.  在本地设置 Dynamo 数据库
3.  使用 Python 连接到我们的数据库
4.  创建表格
5.  插入数据
6.  检索数据
7.  更新数据
8.  删除数据
9.  询问
10.  结论
11.  资源

# 先决条件

1.  对 NoSQL 数据库有基本了解
2.  使用 Python 的经验

# 在本地设置 DynamoDB

## 第一步

下载并安装 [Java SE](https://www.oracle.com/java/technologies/javase-downloads.html) 。要在您的计算机上运行 DynamoDB，您必须拥有 Java Runtime Environment(JRE)8 . x 版或更高版本。该应用程序不能在早期版本的 JRE 上运行。

## 第二步

下载并安装 [AWS CLI 安装程序](https://docs.aws.amazon.com/cli/latest/userguide/install-windows.html#msi-on-windows)。在命令提示符下键入以下命令来验证安装。

```
aws --version
```

如果出现错误，您可能需要添加一个路径变量。查看本文了解更多信息

## 第三步

下载并解压[亚马逊迪纳摩数据库](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)

## 第四步

导航到解压缩 Dynamo DB 的文件夹，并在命令提示符下键入以下命令。

```
java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb
```

> 不要**关闭**该终端设备您已经完成了对数据库的操作

## 第五步

配置凭据。在新的命令提示符下键入以下命令

```
aws configure
```

## 第六步

键入以下命令

```
aws dynamodb list-tables --endpoint-url [http://localhost:8000](http://localhost:8000)
```

这将返回一个空的表列表，除非您已经有了现有的表。

> 或者，您也可以将 [Amazon Dynamo DB 设置为 web 服务](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SettingUp.DynamoWebService.html)

# 使用 Python 连接到我们的数据库

开始之前，我们需要设置并激活一个虚拟环境

```
/* Install virtual environment */
pip install virtualenv/* Create a virtual environment */
python -m virtualenv venv 
/* If the above doesn't work, try the following */
python -m venv venv/* Activate the virtual environment */
venv/Scripts/activate
```

我们将使用 boto3 模块与 Dynamo DB 的本地实例进行交互。

```
pip install boto3
```

接下来，我们需要导入库并创建一个数据库对象

```
import boto3
```

我们将创建一个类并添加 CRUD 操作作为它的方法。

```
class dtable:
    db = None
    tableName = None
    table = None
    table_created = False def __init__(*self*):
        self.db  = boto3.resource('dynamodb',
        *endpoint_url*="http://localhost:8000")
        print("Initialized")
```

通过创建我们的类的实例来测试您的代码

```
*if* __name__ == '__main__':
   movies = table()
```

> 我们将在本文后面使用刚刚创建的类**表**的实例。

# 创建表格

在 DynamoDB 中，一个表可以有两种类型的主键:单个分区键或复合主键(分区键+排序键)。

我们将创建一个名为电影的表。电影的年份将是分区键，标题将是排序键。下面是声明密钥模式的格式。将其存储在一个名为 KeySchema 的变量中。

```
primaryKey=[
 {
   'AttributeName': 'year',
   'KeyType': 'HASH'  *# Partition key* },
 {
   'AttributeName': 'title',
   'KeyType': 'RANGE'  *# Sort key* }
]
```

我们还需要声明上述属性的数据类型。

```
AttributeDataType=[
  {
     'AttributeName': 'year',
     'AttributeType': 'N' #All Number Type },
  {
     'AttributeName': 'title',
     'AttributeType': 'S' #String
  },]
```

我们还需要限制每秒钟对数据库的读写次数

```
ProvisionedThroughput={
  'ReadCapacityUnits': 10,
  'WriteCapacityUnits': 10
}
```

现在已经创建了创建表所需的所有参数。现在我们可以继续使用这些参数来实际创建表。

```
def createTable(self, tableName , KeySchema, AttributeDefinitions, ProvisionedThroughput):
    self.tableName = tableName
    table = self.db.create_table(
    TableName=tableName,
    KeySchema=KeySchema,
    AttributeDefinitions=AttributeDefinitions,
    ProvisionedThroughput=ProvisionedThroughput
    )
    self.table = table
    print(f'Created Table {self.table}')
```

上面的函数和我们之前定义的变量将用于创建表

```
movies.createTable(
tableName="Movie",
KeySchema=primaryKey,
AttributeDefinitions=attributeDataType,
ProvisionedThroughput=provisionedThroughput)
```

# 插入数据

要插入的数据的格式如下

```
{
  'year'  :  2020,
  'title' :  'Some Title',
  'info'  : { 
      'key1' : 'value1',
      'key2' : 'value2',
  }
}
```

对于每个条目，除了主键(**年份**和**标题**)之外，我们可以灵活处理**信息中的数据。**info 中的数据不需要结构化。

在插入数据之前，我们将创建一个包含几部电影的 JSON 文件。你可以在我的 [GitHub repo](https://github.com/rahulbanerjee26/DynamoDB_Boto3) 里找到 JSON 文件。

```
def insert_data(self, path):
    with open(path) as f:
      data = json.load(f)
      for item in data:
      try:
        self.table.put_item(Item = item)
      except:
        pass
    print(f'Inserted Data into {self.tableName}')
```

# 获取项目

如果我们知道某项的主键，我们就可以访问数据库中的该项。在我们的例子中，它是+Ttitle 年。我们将尝试访问带有 2020 年和标题“标题 1”的表。

下面是我们的类的方法，它从表中返回项目

```
def getItem(*self*,*key*):
  *try*:
    response = self.table.get_item(K*ey* = key)
    *return* response['Item']
  *except* Exception as e:
    print('Item not found')
    return None
```

> 注意:get_item 函数的 key 参数中的 K 是大写的

这是我们调用函数的方式

```
print(movies.getItem(*key* = {'year' : 2020 , 'title': 'Title 1'}))
```

在我们继续讨论更新和删除之前，熟悉几个可以传递给更新和删除函数的表达式参数是有益的。

这两个表达式是**更新表达式**和**条件表达式**

下面是一个 UpdateExpression 的示例

```
UpdateExpression=”set info.rating=:rating, info.Info=:info”
```

**:producer** 和 **:info** 是我们在更新时想要使用的值。它们可以被认为是占位符。

我们还需要传递一个额外的参数**expression attribute values**来将值传递给这些变量

```
ExpressionAttributeValues={             
     ':rating': 5.0,             
     ':info': 'Updated Information'
}
```

在某种程度上，这类似于 Python 中的 **format()** 函数

你可以在这里找到常见的[更新操作(添加、修改、删除)列表](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.UpdateExpressions.html)

**条件表达式**类似于 SQL **中的 where 子句。**如果评估为真，则执行该命令，否则忽略该命令。

下面是一个例子

```
ConditionExpression= "info.producer = :producer",
ExpressionAttributeValues={             
     ':producer': 'Kevin Feige'
}
```

**条件表达式**也遵循与**更新表达式**相同的格式

CondtionExpression 可用于条件更新和条件删除。我们将在下面讨论它们。

你可以在这里找到[条件表达式的列表](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html)

# 更新

下面是我们班的更新方法

```
def updateItem(*self*,key, *updateExpression*, *conditionExpression*,*expressionAttributes*):
  *try*:
     response = self.table.update_item(
       Key = key, *UpdateExpression* = updateExpression,
       *ConditionExpression* = conditionExpression,
       *ExpressionAttributes* = expressionAttributes
     )
  *except* Exception as e:
    print(e)
    *return* None
```

我们将更新凯文·费奇制作的电影。我们将更新信息，添加评分 5，并在流派列表中添加“传奇”流派。

```
upExp = "SET info.Info = :info , info.rating = :rating, info.Genre = list_append(info.Genre, :genre)"condExp = "info.Producer = :producer"expAttr = {
  ":info" : "Updated Information",
  ":rating" : 5,
  ":genre" : ["Legendary"],
  ":producer" : "Kevin Feige"
}print("After Update")
movies.updateItem({'year' : 2019 , 'title': 'Title 3'},upExp,condExp,expAttr)
print(movies.getItem(*key* = {'year' : 2019 , 'title': 'Title 3'}))
```

# 删除

删除操作类似于更新操作。下面是我们删除一个项目的方法。它接受一个键、条件表达式和表达式属性值。

```
def deleteItem(*self*, *key*, *conditionExpression*, *expressionAttributes*):
   *try*:
     response = self.table.delete_item(
     *Key* = key,
     *ConditionExpression* = conditionExpression,
     *ExpressionAttributeValues* = expressionAttributes
   )
   *except* Exception as e:
     print(e)
```

我们将删除制片人=“ABC”的电影

```
print("Before Delete")
print(movies.getItem(*key* = {'title':'Title 2' , 'year': 2019}))print("After Delete")
condExp = "info.Producer = :producer"
expAttr = {':producer' : "ABC" }
movies.deleteItem({'title':'Title 2' , 'year': 2019},condExp,expAttr)print(movies.getItem(*key* = {'title':'Title 2' , 'year': 2019}))
```

# 询问

我们可以使用创建表时提供的分区键来查询表。在我们的例子中，是年份。查询操作符需要一个分区键，排序键是可选的。

我们将使用 Key 类，你可以在这里阅读更多关于它的内容。

导入密钥类

```
from boto3.dynamodb.conditions *import* Key
```

下面是查询的方法

```
def query(*self*,*projectExpression*,*expressionAttributes*,*keyExpression*):
*try*:
    response = self.table.query(
     *ProjectionExpression* = projectExpression,
     *KeyConditionExpression*= keyExpression,
    )
    *return* response['Items']
*except* Exception as e:
    print(e)
    *return* None
```

参数 **ProjectionExpression** 是一个字符串，包含我们希望函数返回的列的列表。keyConditionExpression 是使用 Key 类的键条件。在 **KeyConditionExpression** 中必须有分区键。此外，您还可以传递一个类似于**条件表达式**的参数 **FilterExpression**

我们将在 2020 年显示所有以‘M’开头的电影的片名。

```
print("Movies after 2019 with title starting with M")
projection = "title"
Keycondition = Key('year').eq(2020) & Key('title').begins_with('M')
print(movies.query(projection,expAttr,Keycondition))
```

# 结论

我希望这篇文章能够帮助你。如果上面的代码片段有任何错误，请参考我在参考资料部分提到的 Github repo。如果您发现任何错误，请务必通知我:)

快乐学习！

# 资源

Github 回购

<https://github.com/rahulbanerjee26/DynamoDB_Boto3>  

我最近用 WordPress 创建了一个博客，如果你能看看的话，我会很高兴的😃

  

在 LinkedIn 上与我联系

<https://www.linkedin.com/in/rahulbanerjee2699/> 