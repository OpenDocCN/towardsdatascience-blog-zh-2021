# 安全地测试并应用对数据库的更改:Alembic 入门

> 原文：<https://towardsdatascience.com/python-database-migrations-for-beginners-getting-started-with-alembic-84e4a73a2cca?source=collection_archive---------6----------------------->

## 使用这个简单的 Python 工具对数据库进行版本控制

![](img/8006047bad3c68ffb2ce63f60cf8b294.png)

迁移就像蓝图；它们是建立你的数据库的说明(图片由[jeshouts](https://www.pexels.com/@jeshoots-com-147458)在[像素](https://www.pexels.com/photo/floor-plan-on-table-834892/)上提供)

如果您没有在数据库中处理迁移，那么您就错过了。数据库迁移在代码中建立了数据库的结构和变更历史，并为我们提供了安全地应用和恢复数据库变更的能力。

在这篇文章中，我们将看看一个 Alembic 一个 Python 工具，使迁移工作变得非常简单。有了它，我们将创建与数据库无关的迁移(这意味着您可以在任何数据库上执行它们。这些迁移是用代码定义的，这意味着我们可以应用类似 Git 的版本控制；跟踪变更，拥有单一的事实来源，以及与多个开发人员协作的能力。当您阅读完本文后，您将能够:

*   用代码定义对数据库结构的更改
*   版本控制变更
*   应用和恢复迁移
*   将迁移应用到多个数据库(例如，在开发数据库上进行测试，然后将最终结构迁移到生产数据库)
*   不仅要创建所需的表，还要添加索引和关联

为了向你展示给同事留下深刻印象所需的所有特征，我定义了 5 个步骤:

1.  安装和设置
2.  创建迁移
3.  执行和撤消迁移
4.  将更改应用到现有数据库
5.  将新的迁移应用到不同的数据库

# 1.安装和设置

我们将使用一个名为 Alembic 的 Python 包。这个包使得创建数据库迁移变得很容易。

## 装置

安装很简单。打开一个终端，导航到您的项目目录并执行下面的代码。这将安装并初始化 Alembic。注意，在根目录下创建了一个名为`/alembic`的文件夹。

*不熟悉使用终端？查看* ***下面的文章***

</terminals-consoles-command-line-for-absolute-beginners-de7853c7f5e8>  

```
pip install alembic
alembic init alembic
```

接下来，我们将建立一个数据库连接。Alembic 必须知道要连接到哪个数据库。通常我们会通过调整 sqlalchemy.url 在`/alembic.ini`中指定这个值，但是最好覆盖这个值。打开`/alembic/env.py`，找到`config = context.config`。在此下方插入以下内容:

***保护您的凭证安全:建议从环境变量中加载连接字符串。*** *如果您将此项目上传到公共存储库，任何人都可以看到您的凭据。使用环境变量，您可以通过将所有机密信息(如密码和连接字符串)放在一个文件中来保护您的凭证安全，我们可以* `*.gitignore*` *。在下面的文章中看看这是如何工作的:*

</keep-your-code-secure-by-using-environment-variables-and-env-files-4688a70ea286>  

让我们通过在终端中运行`alembic current`来测试我们与的连接。这显示了我们的数据库目前的状态。如果您看到类似下面的文字，说明您已经成功连接到数据库。

```
INFO [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO [alembic.runtime.migration] Will assume transactional DDL.
```

# 2.创建迁移

执行下面的代码来创建您的第一个迁移:
`alembic revison -m "create test schema"`
这将在`/alembic/versions`中创建一个新文件。这个文件是我们实际的迁移，它包含两个函数:
1。升级
2。使降低

在第一个函数中，我们将放入想要执行的代码，例如创建一个模式。在第二个函数中，我们将在第一个函数中放置撤销操作的代码，例如删除模式:

在升级中，我们告诉 alembic 执行一些 SQL 来创建一个模式。
在降级中，我们放置指令来撤销这一点，删除模式。

很清楚，对吧？让我们创建一些表格！我们将创建三个表:学生、班级和学生 _ 班级。显然，学生和班级将是存储学生和班级信息的表。Student_classes 将一个或多个学生 id 连接到一个或多个班级 id。这是因为一个学生可以有多个班级，而一个班级可以有多个学生:

**学生表**

正如你所看到的，我们使用了`op.create_table`函数来创建一个表格。首先我们将指定表名，然后指定所有列。最后，我们指定模式名。如果您只想使用默认模式，可以省略。还要注意，我们添加了像主键和“可空”这样的约束。降级功能再次撤销升级。

**阶级表** 这里我们只看升级功能:

student_classes 表
在这个表中我们需要做一些花哨的东西。因为该表的存在只是为了将学生绑定到班级，反之亦然，所以我们可以定义更多的功能:

您可以看到，在上面的代码中，我们用 ondelete="CASCADE "标志定义了 foreignKeys。这意味着如果一个班级或一个学生从他们的表中被删除(一个学生毕业或一个班级被终止)，那么这些删除会级联到 student_class 表中；删除相关记录。这使我们的数据保持整洁。

![](img/962a4f506651b4abf3cc887603505d98.png)

我们的数据库正在建设中(图片由[斯科特·布莱克](https://unsplash.com/@sunburned_surveyor)在 [Unsplash](https://unsplash.com/photos/x-ghf9LjrVg) 上提供)

# 3.执行和撤消迁移

现在我们已经定义了所有的表和关系，是时候将它们反映到数据库中了。在您的终端中只需执行:
`alembic upgrade head`

这告诉 alembic 升级所有脚本，直到像 Git 中的 head。
之后，我们可以调用`alembic history`来显示如下内容:

```
(venv) C:\rnd\med_alembic>alembic history
a3e1bb14f43b -> a46099cad4d5 (head), student_classes
058a1e623c60 -> a3e1bb14f43b, create classes table
73d31032477a -> 058a1e623c60, create students table
Revision ID: 058a1e623c60
Revises: 73d31032477a
Create Date: 2021–10–22 12:58:23.303590
<base> -> 73d31032477a, create test schema
```

这显示了我们执行的所有迁移，前面有一个代码。我们可以用这个代码来降级像:
`alembic downgrade 058a1e623c60`

这将撤消所有迁移，直到创建“创建学生表”迁移。
返回一个迁移:`alembic downgrade -1`(数字，不是 L)
恢复所有迁移:`alembic downgrade base`

## 重置数据库

如果你仍然在测试和开发数据库，你可以很容易地重置。假设您插入了一些数据，并在一次迁移中发现了一个错误；可能是错误的列名。只需调整升级函数中的错误并调用:
`alembic downgrade base && alembic upgrade head`
这将在一条语句中重置您的整个数据库！虽然这对您的开发数据库非常方便，但我不建议在您的生产数据库上执行此命令，因为所有数据都将丢失

# 4.将更改应用到现有表格

有时您需要对现有的表格进行调整；重命名列、调整数据类型或添加索引或约束。使用 Alembic，您可以通过首先在开发数据库上开发它们来设计、测试和验证这些更改。一旦您对一切都满意了，您就可以插入一个不同的`.env`文件，并将所有新的更改迁移到生产数据库！下面是一些如何做到这一点的例子。

## 添加约束

假设这些表已经投入生产，但是缺少了一些东西。首先，我们要向 classes 表中的 name 列添加一个惟一约束；我们不能有多个同名的类。我们用
`alembic revision -m “create unique constraint classes.name”`创建一个新的迁移

然后添加以下内容:

就这么简单！

## 添加索引

下一个问题:我们的学生表增长了很多。我们来加个索引吧！
`alembic revision -m “add index on students.name`
下面的代码将创建索引。

## 重命名列和更改数据类型

使用 alter_column 函数可以更改重命名列、更改数据类型和调整某些约束(可为 null 等):

这会将列名从“name”更改为“student_name ”,并将从字符串()更改为最大长度为 100 个字符的字符串。
请注意，降级再次撤销了我们所有的更改。

别忘了检查一下`alembic upgrade head`前后的区别！

![](img/e28d110d16e42d2ee000eb5af3775159.png)

我们的数据库被迁移了！(图片由 [Ubay Seid](https://unsplash.com/@ubay732) 在 [Unsplash](https://unsplash.com/photos/XV9pLr_tat4) 上拍摄)

# 5.将新的迁移应用到不同的数据库

不，我们已经在 Postgres dev 数据库上测试了所有迁移，现在是时候将所有更改迁移到我们的生产数据库了。为此，我们只需连接到生产数据库并执行迁移。最好的方法是使用一个安全保存所有连接信息的文件。

还记得第 1 部分的`/alembic/env.py`文件吗？这是我们定义数据库连接字符串的地方。我们所要做的就是将不同的`.env`文件插入到 Alembic 中。然后，我们可以从环境中加载连接字符串的各个部分。在这种情况下，我将使用`python-dotenv`包来加载`.env`文件，并创建如下所示的连接字符串。[更多关于**如何使用 env 文件这里**](https://mikehuls.medium.com/keep-your-code-secure-by-using-environment-variables-and-env-files-4688a70ea286) 。

```
db_user = os.environ.get("DB_USER")
db_pass = os.environ.get("DB_PASS")
db_host = os.environ.get("DB_HOST")
db_name = os.environ.get("DB_NAME")
db_type = os.environ.get("DB_TYPE")
config.set_main_option('sqlalchemy.url', f'{db_type}://{db_user}:{db_pass}@{db_host}/{db_name}')
```

# 结论

数据库迁移为我们提供了安全测试和应用数据库更改的能力。因为我们写出了代码中的所有更改，所以我们可以将它们包含到我们的版本控制中，这提供了更多的安全性。总而言之，使用 Alembic 简单、安全，是设计、测试和更改数据库的好主意。我希望已经阐明了这些迁移是如何工作的，以及如何使用 Alembic。

如果你有建议/澄清，请评论，以便我可以改进这篇文章。同时，看看我的其他关于各种编程相关主题的文章:

*   [用 FastAPI 用 5 行代码创建一个快速自动记录、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)
*   [从 Python 到 SQL——安全、轻松、快速地升级](https://mikehuls.medium.com/python-to-sql-upsert-safely-easily-and-fast-17a854d4ec5a)
*   [创建并发布你自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)
*   [创建您的定制私有 Python 包，您可以从您的 Git 库 PIP 安装该包](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893)
*   [面向绝对初学者的虚拟环境——什么是虚拟环境以及如何创建虚拟环境(+示例)](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)
*   [通过简单升级，显著提高数据库插入速度](https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424)

编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟着我！](https://mikehuls.medium.com/membership)