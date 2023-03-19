# 通过使用环境变量和 env 文件来保证代码的安全

> 原文：<https://towardsdatascience.com/keep-your-code-secure-by-using-environment-variables-and-env-files-4688a70ea286?source=collection_archive---------6----------------------->

## 安全地加载一个文件，其中包含我们的应用程序所需的所有机密数据，如密码、令牌等

![](img/2f853f0c51297f9dac6e2971c638e206.png)

让我们用一些变量来丰富这个美丽的环境(图片由[罗布·莫顿](https://unsplash.com/@rmorton3)在 [Unsplash](https://unsplash.com/photos/fecsiuPSJsc) 上提供)

您的代码需要连接到数据库。为此，它需要一个数据库的密码。你如何在你的应用程序中使用这个密码，既能连接到数据库*又能让密码保密？*

本文向您展示了如何在代码中使用 env 文件。这个文件储存了我们所有的机密信息。它可以加载一次，所以你可以在你的应用程序中的任何地方访问你的所有私人信息，如密码。在本文中，我们将使用 Python 来说明 env 文件的工作原理，但该原理适用于所有编程语言。

在本文结束时，您将:

*   理解使用环境变量的优势
*   理解环境变量如何工作
*   了解如何从 env 文件中加载环境变量

我们来编码吧！

# 1.为什么要使用环境变量？

使用环境变量文件主要有两个原因:
1。安全
2。灵活性

## 保管好您的凭证

第一个原因是迄今为止最重要的:环境变量保存您的凭证。我们希望防止我们的代码被密码和其他不应该知道的信息弄得千疮百孔。这不仅是不好的做法；这也非常危险，尤其是如果你把你的代码上传到像 GitHub 这样的公共库。然后，你只要公开向任何看到你的证件的人展示你的证件就行了！

环境变量将我们所有的机密信息收集在一个文件中。由于我们所有的私人信息都收集在一个文件中，我们可以很容易地`gitignore`这个文件来防止我们的数据被推到我们的 git 存储库！

## 灵活性

环境文件可以从外部修改。如果您的数据库密码更改，我们可以打开包含所有环境变量的文件，
更改数据库密码并重启我们的应用程序。这比搜索，然后在应用程序中重写硬编码的密码要方便得多。

此外，你可以运行一个程序(例如在 Docker 中)并给它一个 env 文件(如下文所示)。例如，这使得在开发和生产数据库之间切换变得非常容易。

[](/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af) [## 在 Docker 和 Compose 中使用环境变量和文件的完整指南

### 通过这个简单的教程，保持你的容器的安全性和灵活性

towardsdatascience.com](/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af) 

# 2.环境变量如何工作

既然我们确信使用 env 变量是明智的，那么让我们弄清楚它们是如何工作的。每个操作系统(Windows、Linux、Mac)都会为它创建的每个进程创建一组环境变量。我们正在运行的脚本就是其中一个进程的例子。

![](img/cf1ed26d78cc8b55111d1f3a63b750d9.png)

我认为 env 文件是我的应用程序的关键(图片由[马特·阿特兹](https://unsplash.com/@mattartz)在 [Unsplash](https://unsplash.com/photos/PH2Q1aqOARo) 上提供)

我们将从读取这些变量开始，然后添加一些我们自己的变量。下面的例子是用 Python 写的，因为这是一种非常容易理解的语言，但是请注意 env 变量不是 Python 独有的:类似下面的功能在每种编程语言中都存在。

## 读取环境变量

让我们先看看哪些变量已经存在；

```
import os
for key, value in os.environ.items():
  print(key, value)
```

我们用 os.environ 访问 env 变量，这将返回一个字典。使用 items()方法，我们可以遍历所有的键和值并打印它们。在我的例子中，它打印出了许多关于我的系统的信息:

*   我的程序文件、路径和 APPDATA 的位置
*   虚拟环境信息
*   我的用户名和计算机名
*   等等

我们还可以读取一个特定变量，例如我的电脑名称:

```
import os
print(os.environ.get(“COMPUTERNAME”))
```

## 创建环境变量

由于`os.environ`是一个字典，我们可以很容易地添加新的变量。首先，我们将创建一个带有关键字“HELLO”和值“WORLD”的变量。然后我们将再次检索该值

```
import os# Create an env var
os.environ['HELLO'] = 'WORLD'# Retrieve the newly created var
print(os.environ.get("HELLO"))
```

这印出了“世界”。轻松点。也试着从应用程序的其他地方读取这个变量，注意到它在任何地方都是可访问的。

![](img/98a01262967d0ed9d3478a49b76bad06.png)

让我们保证这些密码的安全(图片由[马库斯·温克勒](https://unsplash.com/@markuswinkler)在 [Unsplash](https://unsplash.com/photos/3LVhSjCXRKc) 上提供)

# 3.将环境变量从环境文件加载到环境中

既然我们已经了解了环境以及如何使用环境变量，那么就很容易理解环境文件的作用了；它指定了一些额外的键和值对，我们希望将它们加载到我们的环境中。在本节中，我们将创建一个 env 文件，然后将其完整地加载到环境中。

## a.创建 env 文件

我们将首先在应用程序的这个位置创建一个名为` . env '的文件:`/config/conf/.env`。env 文件的内容如下所示:

```
DB_TYPE = postgresql
DB_USER = postgres
DB_PASS = mikepw
DB_HOST = localhost
DB_NAME = my_webshop_db
```

## b.加载环境文件

接下来，我们需要将这个文件加载到我们的应用程序环境中。大多数编程语言都有加载这类文件的内置功能包。
Python 最方便的就是安装包:`pip install python-dotenv`。这个包可以方便地将`.env`文件中的值加载到 Python 环境中。然后简单地调用`load_dotenv`函数并传递`.env`文件的位置。

***的位置。env 文件→使用 Python 中的相对路径*** `load_dotenv`函数需要一个指向`.env`文件位置的绝对路径。在本文中，我们只是硬编码传递它，但是一旦我们部署我们的代码，我们可能会面临一个问题:在我的机器上，env 文件的位置是`c:/users/mike/envfileproject/config/conf/.env`，但是在你的机器上，它可能是`c:/pythonstuff/envfileproject/config/conf/.env`。查看下面的文章，获得一个简单的、可重用的技巧来解决计算 env 文件的绝对路径带来的头痛问题。

[](/simple-trick-to-work-with-relative-paths-in-python-c072cdc9acb9) [## 在 Python 中使用相对路径的简单技巧

### 轻松计算运行时的文件路径

towardsdatascience.com](/simple-trick-to-work-with-relative-paths-in-python-c072cdc9acb9) 

## c.加载环境文件

我们将使用`python-dotenv`包和`ROOT_DIR`来加载。环境中的 env 文件:

```
if (os.environ.get("DB_TYPE") == None):
 from dotenv import load_dotenv
 from config.definitions import ROOT_DIR
 load_dotenv(os.path.join(ROOT_DIR, 'config', 'conf', '.env'))
```

首先，我们将检查我们需要的变量之一(在这个特定的例子中是 DB_TYPE)是否已经加载到环境中。当我们从外部通过 Docker 将 env 文件传递给这个 Python 项目时，情况就是这样，例如，查看下面关于如何在 Docker 中使用 env 文件的文章。

[](/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af) [## 在 Docker 和 Compose 中使用环境变量和文件的完整指南

### 通过这个简单的教程，保持你的容器的安全性和灵活性

towardsdatascience.com](/a-complete-guide-to-using-environment-variables-and-files-with-docker-and-compose-4549c21dc6af) 

如果 DB_TYPE 变量在环境中还不存在，我们将使用`python-dotenv`包加载它。

1.  从 dotenv 导入 load_dotenv 函数(这是我们之前安装的 python-dotenv 包)
2.  从`/config/definitions.py`导入根目录
3.  将路径传递给。env 文件到 load_dotenv 函数
4.  搞定了。

我们的 env 文件现在被加载到 Python 中。通过像这样请求密钥，我们可以安全方便地访问所有值:

```
database_password = os.environ.get("DB_PASS")
```

有关如何实现？Docker 容器中的 env 文件。如果我们使用 env 文件将数据传递给 Postgres 容器:

[](/getting-started-with-postgres-in-docker-616127e2e46d) [## Docker 中的 Postgres 入门

### 为初学者在 Docker 容器中创建 Postgres 数据库

towardsdatascience.com](/getting-started-with-postgres-in-docker-616127e2e46d) 

# 结论

总之:环境文件通过将我们所有的机密信息存储在一个文件中为我们提供了安全性和灵活性，我们可以将这个文件加载到我们的代码中，以便安全地访问所有需要的值。

我希望已经阐明了环境变量的内部工作原理以及如何使用环境文件。如果您有建议或澄清，请评论，以便我可以改进这篇文章。与此同时，请查看我的其他关于各种编程相关主题的文章，比如:

*   [Docker 中的 Postgres 入门](https://mikehuls.medium.com/getting-started-with-postgres-in-docker-616127e2e46d)
*   [用 FastAPI 用 5 行代码创建一个快速自动归档、可维护且易于使用的 Python API](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e)
*   [Python 到 SQL —安全、轻松、快速地升级](https://mikehuls.medium.com/python-to-sql-upsert-safely-easily-and-fast-17a854d4ec5a)
*   [创建并发布自己的 Python 包](https://mikehuls.medium.com/create-and-publish-your-own-python-package-ea45bee41cdc)
*   [创建您的定制私有 Python 包，您可以从您的 Git 库 PIP 安装该包](https://mikehuls.medium.com/create-your-custom-python-package-that-you-can-pip-install-from-your-git-repository-f90465867893)
*   [绝对初学者的虚拟环境——什么是虚拟环境，如何创建虚拟环境(+示例)](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)
*   [通过简单的升级大大提高您的数据库插入速度](https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424)

编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？跟我来！

[](https://mikehuls.medium.com/membership) [## 通过我的推荐链接加入媒体-迈克·赫斯

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

mikehuls.medium.com](https://mikehuls.medium.com/membership)