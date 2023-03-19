# 使用 Julia 查询亚马逊雅典娜

> 原文：<https://towardsdatascience.com/querying-amazon-athena-using-julia-607250312232?source=collection_archive---------46----------------------->

## [实践教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

![](img/22566791fe7b760f16653013b87401e2.png)

照片由[拍摄于](https://unsplash.com/@ffstop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的

Julia 是一种相当现代的科学编程语言，它是免费的、高级的、快速的，并且捆绑了一系列令人敬畏的特性，使 Julia 能够再次出色地处理数据。该语言借鉴了 Python、MATLAB 和 R[【1】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f1)等语言的灵感。如果你还没有读过我的文章“你应该学习朱莉娅的 10 个理由”，那就去看看吧！Amazon Athena 是一种交互式查询服务，它允许您使用您的老朋友 SQL 轻松分析您在亚马逊 S3 存储中收集的数据。Athena 很棒，因为它是无服务器的，这意味着不需要管理基础设施，您只需为运行的查询付费。

听起来很棒，对吧？Julia 非常适合处理数据，Athena 非常适合查询数据，我们如何将两者结合使用？与其手动导出 CSV 文件并使用`CSV.jl`在 Julia 中加载 CSV 文件，我将向您展示如何使用 Athena 直接从 Julia 中查询数据，使用`DataFrames.jl`将结果数据集加载到 DataFrame 中供您使用。

朱莉娅是新来的吗？继续从 Julia 网站[这里](https://julialang.org/downloads/#current_stable_release)获取你的特定操作系统(OS)的官方二进制文件。如果你对详细的文章不感兴趣，只是想看看完整的代码解决方案，请查看[GitHub gist。](https://gist.github.com/gabegm/458288fa55437314b7b41b63a53a13a1)

# 开放式数据库连接性

开放式数据库连接(ODBC)是用于访问数据库管理系统(DBMS)的标准应用程序编程接口(API)。由于它的开放标准性质，许多数据库都支持它，许多工具使用它来连接这样的数据库。这意味着这篇文章中展示的连接 Amazon Athena 的方法实际上也可以用于其他数据库，只需要进行一些配置更改。

AWS 为 Athena 提供了一个 ODBC 驱动程序，您可以使用它来访问您的数据。确保从官方的[用户指南](https://docs.aws.amazon.com/athena/latest/ug/connect-with-odbc.html)中为您的操作系统安装相应的 ODBC 驱动程序。如果你遇到任何问题和/或需要一些本文中没有提到的额外配置，你也可以选择阅读 ODBC 驱动程序的[文档](https://s3.amazonaws.com/athena-downloads/drivers/ODBC/SimbaAthenaODBC_1.1.9.1001/docs/Simba+Athena+ODBC+Connector+Install+and+Configuration+Guide.pdf)。

# 配置

我们可以使用配置文件，简称 config，来配置 Julia 应用程序的参数和初始设置。这些参数的范围可以从机器学习模型参数到数据库凭证，如我们示例中的那些。配置文件在许多编程语言中广泛使用，它提供了一种无需更改任何代码就能更改应用程序设置的简洁方法。

对于这个例子，我们将使用`Configs.jl`[【2】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f2)来加载我们的配置文件，该文件也支持基于配置位置、`ENV`变量映射和函数调用的级联覆盖。

# 数据帧

DataFrame 表示一个包含行和列的数据表，就像电子表格一样。Julia 包`DataFrames.jl`提供了一个 API，我们可以用它来处理 Julia 中的表格数据。类似于 Python 中的 Pandas，它的设计和功能非常相似，然而由于 Julia 的高度模块化，Julia 中的 DataFrames 与大量不同的包紧密集成，如`Plots.jl`、`MLJ.jl`和[等等](https://dataframes.juliadata.org/stable/#DataFrames.jl-and-the-Julia-Data-Ecosystem)！

# 雅典娜. jl

闲聊够了。让我们首先创建一个新目录来存放我们的代码，并将其命名为`Athena.jl`。我们还将添加一个来直接存储我们的数据库配置文件`configs`，另一个来存储我们的 Julia 代码`src`，还有一个来存储我们的 SQL 脚本`sql`到我们新创建的`src`目录中。最后，我们将创建三个文件，一个名为`Athena.jl`，这将是我们的 Julia 脚本，另一个名为`query.sql`，用于我们的 SQL 查询，最后一个名为`default.yml`，用于我们的数据库配置和机密。

```
~ $ mkdir Athena.jl 
~ $ cd Athena.jl 
~/Athena.jl $ mkdir src src/sql configs 
~/Athena.jl $ touch src/Athena.jl 
~/Athena.jl $ touch src/sql/query.sql 
~/Athena.jl $ touch configs/config.yml
```

继续在你最喜欢的 IDE 中打开`Athena.jl`目录，我的是 Visual Studio 代码[【3】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f3)这是我用的。打开`configs/default.yml`文件，这样我们可以添加我们的配置。您需要添加您的`s3 location`、`uid`和`pwd`。如果您的 AWS 云基础设施设置在不同的地区，您可能还需要更改`region`值。根据操作系统，您可能还需要更改数据库驱动程序的路径。如果你运行的是像我这样的 macOS，你可能不需要做任何改变，只需要确保文件存在于那个目录中。

```
database:
  name: "SimbaAthenaODBCConnector"
  path: "/Library/simba/athenaodbc/lib/libathenaodbc_sb64.dylib"
  driver: "SimbaAthenaODBCConnector"
  region: "eu-west-1"
  s3_location: ""
  authentication_type: "IAM Credentials"
  uid: ""
  pwd: ""
```

在`src/sql`的`query.sql`文件中添加您想要运行的 SQL 查询，并在您的 REPL[【4】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f4)中打开`src/Athena.jl` Julia 脚本来运行以下内容。

```
import Pkg
Pkg.activate(".")

Pkg.add(["ODBC", "DataFrames", "Configs"])

using ODBC, DataFrames, Configs
```

如果你是第一次接触 Julia，你可能会问的第一件事是“`import`和`using`有什么区别？”嗯`import`的工作方式和 Python 中的一样，`import MyModule`会将[【5】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f5)功能纳入范围，这些功能仍然可以使用`MyModule`来访问，比如`MyModule.x`和`MyModule.y`，有点像在 Python 中使用`import numpy`，然后运行`numpy.array([])`。然而，Julia 中的`using`相当于在 Python 中运行`from numpy import *`，这将把 Numpy 的所有函数都纳入范围，允许您在 Python 中运行`array([])`。如果你想在 Julia 中只导入特定的函数到 scope 中，你需要做的就是`import MyModule: x, y`，这将使函数`x`和`y`在 scope 中可访问。

`Pkg`是包管理器捆绑了 Julia。我们不仅可以用它来安装包，还可以创建虚拟环境[【6】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f6)来运行我们的代码。通过运行`Pkg.activate(".")`,我们告诉 Julia 的包管理器在当前直接激活一个新的虚拟环境，以便我们安装我们的 Julia 依赖项。这将自动创建两个新文件，`project.toml`和`Manifest.toml`，前者将列出我们所有的直接依赖，而后者将列出我们的直接依赖所依赖的间接依赖。这两个文件将允许任何其他开发人员重新创建与我们相同的虚拟环境，这对于重现这个例子来说将是很好的。

接下来将使用`Pkg.add`函数添加我们想要安装的 Julia 包列表，并使用`using`命令将它们导入 scope。现在我们已经用运行这个例子所需的依赖项设置了我们的 Julia 环境，我们可以开始使用`Configs.jl`的`getconfig`函数从我们的配置文件加载配置。这个函数返回一个`NamedTuple`，它是一个 Julia 类型，有两个参数:一个给出字段名的符号元组，一个给出字段类型的元组类型。这意味着我们可以使用配置文件本身的字段名，通过使用`.`参数直接访问它们。在最后一步中，我们将把 SQL 脚本的内容读入 Julia，并把它们解析成一个字符串供我们以后使用。

```
database = getconfig("database")
name = database.name
path = database.path
driver = database.driver
region = database.region
s3_location = database.s3_location
authentication_type = database.authentication_type
uid = database.uid
pwd = database.pwd
sql = open("src/sql/query.sql" ) do file
    read(file, String)
end
```

在我们开始查询 Athena 之前，`ODBC.jl`要求我们通过将名称和路径传递给驱动程序来添加我们之前安装的 Athena 驱动程序。我们还需要构建用于连接 Athena 的连接字符串。Julia 拥有对字符串插值的本地支持，允许我们使用任何我们可能需要的变量来构造字符串，而不需要串联[【7】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f7)。连接字符串是特定于您正在使用的数据库的，所以如果您不打算连接到 Athena，您必须查找您正在使用的驱动程序的文档来构造所需的连接字符串。

```
# locate existing ODBC driver shared libraries or download new
# then configure
ODBC.adddriver(name, path)

# build connection string
connection_string = """
Driver=$driver;
AwsRegion=$region;
S3OutputLocation=$s3_location;
AuthenticationType=$authentication_type;
UID=$uid;
PWD=$pwd;
"""
```

好吧，我保证有趣的部分来了。既然我们已经完成了枯燥的配置设置，我们可以继续建立连接并查询 Athena！

```
conn = DBInterface.connect(ODBC.Connection, connection_string)

# execute sql statement directly
# then materialize results in a DataFrame
df = DBInterface.execute(
    conn,
    sql
) |> DataFrame

297×3 DataFrame
 Row │ dt                       table_name  n_rows
     │ DateTime…?               String?     Int64?
─────┼────────────────────────────────────────────────
   1 │ 2021-05-08T06:46:24.183  Table_A     196040
   2 │ 2021-05-08T06:46:24.183  Table_B     28172242
   3 │ 2021-05-08T06:46:24.183  Table_C     27111764
   4 │ 2021-05-06T06:46:29.916  Table_A     196041
   5 │ 2021-05-06T06:46:29.916  Table_C     27080936
   6 │ 2021-05-23T06:46:26.201  Table_A     196034
  ⋮  │            ⋮                             ⋮
 293 │ 2021-03-03T14:47:56.910  Table_B     27421193
 294 │ 2021-03-03T14:47:56.910  Table_C     26379105
 295 │ 2021-04-27T06:46:34.887  Table_A     196046
 296 │ 2021-04-27T06:46:34.887  Table_B     28016354
 297 │ 2021-04-27T06:46:34.887  Table_C     26960853
                                286 rows omitted
```

简单对吗？Julia 还能够毫不费力地解析我们的数据帧中的列类型。您现在可以使用`Plots.jl`运行自由绘图，使用`MLJ.jl`训练机器学习模型，并将转换后的数据存储回 Athena。

```
# load data into database table
ODBC.load(df, conn, "table_nme")
```

# 结论

你能用 Python 完成所有这些吗？当然，事实上有很多这样的帖子，你可以找到如何做到这一点。然而，我认为 Julia 是一种有趣的语言，[值得学习](https://gabriel.gaucimaistre.com/2018/09/10-reasons-why-you-should-learn-julia/)，并且可以提供比 Python 更多的好处。

然而，Julia 仍然是一种相当新的语言，尽管它的受欢迎程度稳步上升[【8】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/(#f8))[【9】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/(#f9))，但它仍然缺乏更广泛使用的语言(如 Python)所提供的工具和社区支持。看看 StackOverflow，关于 Python 的问题数量远远超过了关于 Julia 的问题数量。把这归因于 Julia 更容易使用可能很有诱惑力，但是——尽管我更看重 Julia 而不是 Python——事实并非如此。这是一个选择偏差的例子，Python 上的问题数量相对于 Julia 上的问题数量明显更高的实际原因仅仅是因为使用 Python 的人比使用 Julia 的人多。StackOverflow 上社区问题和资源的不足实际上导致了 Julia 更陡峭的学习曲线。

然而，这并不完全是悲观的。我确实认为作为一种语言，Julia 比 Python 更容易阅读，因为大多数 Julia 包都是用纯 Julia 编写的。尽管 Python 可能很流行，但它并不总是意味着它是每个人或每个工作的最佳工具。随着 Julia 的受欢迎程度和用户群的持续增长，工具和社区支持最终会赶上来。最终，哪种语言最适合您的需求取决于您。

但是，如果您发现自己想要使用 Julia 中没有的 Python 包，`PyCall.jl`是一个很好的包，它提供了从 Julia 语言直接调用 Python 并与 Python 完全互操作的能力[【10】](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/#f10)。这很简单，因为:

```
import Pkg
Pkg.activate(".")

Pkg.add("PyCall")

using PyCall

# create a variable 'x' of type Float64
x = 1
x = Float64(x)

# use Anaconda to install scipy and it's dependencies
so = pyimport_conda("scipy.optimize", "scipy")

# use scipy's newton function to find a zero of sin(x) 
# given a nearby starting point 1
so.newton(x -> cos(x) - x, 1) # 0.7390851332151607
```

*   [1] [QuantEcon](https://cheatsheets.quantecon.org/) 有一个很棒的比较 MATLAB、Python 和 Julia 的表格。
*   [2] [Configs.jl](https://github.com/citkane/Configs) 是由 [Michael Jonker](https://github.com/citkane) 维护的开源 Julia 包。
*   [3] Visual Studio 代码有一个优秀的[扩展](https://www.julia-vscode.org/)来帮助你开发由 Julia 社区维护的 Julia 应用程序。
*   [4]一个读取-评估-打印循环(REPL)，是一个简单的交互环境，接受代码输入，执行它们，并返回结果。
*   [5]变量的作用域是变量可见的代码区域。变量作用域有助于避免变量命名冲突。
*   [6]虚拟环境通过避免可能出现的版本冲突，帮助我们确保我们的应用程序及其依赖项独立于其他应用程序。
*   [7]在 Julia 中，字符串将通过使用`*`操作符`"Hello " * "world"`连接起来
*   [8] [朱莉娅被越来越多的人接受](https://lwn.net/Articles/834571/)
*   【9】[茱莉亚更新:领养量持续攀升；是 Python 挑战者吗？](https://www.hpcwire.com/2021/01/13/julia-update-adoption-keeps-climbing-is-it-a-python-challenger/)
*   [10] Julia 不仅拥有与 Python 的出色互操作性，还拥有大量其他语言的互操作性。

*原载于 2021 年 6 月 8 日 https://gabriel.gaucimaistre.com**的* [*。*](https://gabriel.gaucimaistre.com/2021/06/querying-amazon-athena-using-julia/)