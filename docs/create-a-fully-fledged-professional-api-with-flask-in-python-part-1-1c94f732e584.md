# 用 Python 中的 Flask 创建一个成熟、专业的 API 第 1 部分

> 原文：<https://towardsdatascience.com/create-a-fully-fledged-professional-api-with-flask-in-python-part-1-1c94f732e584?source=collection_archive---------20----------------------->

## 专业且安全地允许访问您的 Python 程序-通过 6 个简单的步骤设置基础

![](img/d1e92e4a5b045d0f770dde69f65f8d9f.png)

我们的 API 正在愉快地连接我们的服务(图片由 [Steven Johnson](https://www.pexels.com/@steve) 在 [Pexels](https://www.pexels.com/photo/four-white-travel-adapters-1694830/) 上提供)

没有 API 的应用程序就像没有服务员的餐馆。在餐馆里，你不需要和厨房沟通，也不需要等着点菜来享用美食。同样的，一个网站不需要知道如何与你的数据库通信。像一个服务员一样，我们的 API 从我们的网站收到一个命令:“我想要一些带有元数据的用户统计数据”。API 随后检查订单(是否允许您拥有该数据？)，说服我们的数据库(厨师)为他烹制一些美味的信息，等待数据库完成，最后将数据返回到网站。

# 为什么需要 API 呢？

API 允许两个(或更多)服务以一种分离的方式相互通信。最常见的例子是处理网站和数据库之间通信的 API。它提供访问、便利、安全和额外的功能。让我们来看一下每个要点的简短示例。

## 1.接近

假设您创建了一个名为 pixelcounter.py 的 Python 程序，它读取一个图像文件并计算像素数。当你把你的图像文件放在 USB 上的时候，走到装有 pixelcounter.py 的电脑前，把图像放在电脑上，然后把它加载到 pixelcounter.py 中，最后开始计数。
这行得通，唯一的问题是:我们是程序员。我们没有时间把东西放到 u 盘上，更不用说走到咖啡机以外的地方了！一个好的解决方案是将 API 和 pixelcounter.py 放在服务器上。这样，人们可以发送图像进行统计..轻松点。

## 2.便利

API 也提供了很多便利。为了发送电子邮件，只需将收件人、主题和消息发送到 API。如果您可以将数据发送给 API，那么您不需要理解和 SMTP 是如何工作的。

## 3.额外功能

使用 API 还可以提供额外的功能。例如，您可以计算哪个用户请求了哪些数据。这样你可以限制使用，检查人们最感兴趣的功能，哪些功能根本不会被使用，或者按请求向用户收费。

## 4.安全性

API 允许两个程序以安全的方式相互通信。像脸书这样的网站要检索你的朋友，需要从数据库中获取数据。你不希望另一个人得到你的个人资料。这就是 API 的用武之地；它检查是谁发出的请求，以及这个人是否被允许获取数据。

# 如何创建一个 API？

在这篇文章中，我们将开始创建一个 API，它将允许我们做上面描述的所有事情。在这一部分中，我们将着重于设置我们的项目，获得一些基本的功能和测试我们的路线。如果您看到任何您不熟悉的术语，请查看本页底部的词汇表。

![](img/02788c247233e1e4e3e7f672f02dda22.png)

让我们连接我们的服务！(图片由[像素](https://www.pexels.com/photo/close-up-of-telephone-booth-257736/)上的[像素](https://www.pexels.com/@pixabay)生成)

按照这些步骤创建一个包含所有必需依赖项的项目，然后我们将创建一个控制器来处理路由上的请求。

## 第一步。创建您的 python 项目

创建您的根文件夹:将包含项目的文件夹。在这个例子中，我们将使用`c:/my_root_folder`

## 步骤 1.1(可选)创建虚拟环境

创建虚拟环境可以帮助您将已安装的软件包保持在一个整洁的包中。本文 中 [**关于理解和创建 venvs 的更多信息。以下是本项目的操作方法:**](https://mikehuls.medium.com/virtual-environments-for-absolute-beginners-what-is-it-and-how-to-create-one-examples-a48da8982d4b)

导航到您的根文件夹并执行这段代码。
`python -m venv c:/my_root_folder/venv`
这将创建一个文件夹来存放你所有已安装的软件包。不要忘记:

*   通过导航到`c:/my_root_folder/venv/scripts`并执行`activate`，在安装包之前激活您的环境。现在您处于您的环境中，可以在这里安装软件包了
*   通过调用`c:/my_root_folder/venv/Scripts/python.exe`来执行你的脚本。我们稍后会更深入地讨论它

## 第二步。安装依赖项

我们需要两个包，所以我们需要安装 Flask 和 flask-restful。`pip install Flask flask-restful`

## 第三步。构建您的项目

下一步:创建一些文件夹，我们可以把我们的 te API 的不同部分，即我们的配置，路线和控制器:
`mkdir config routes controllers`

## 第四步。创建 API

我们的项目已经建立，是时候构建我们的 API 了！在我们的根文件夹中创建一个名为 main.py 的文件，并赋予它以下内容

```
from flask import Flask
from flask_restful import Apiapp = Flask(__name__)
api = Api(app)if __name__ == "__main__":
    # start up api
    app.run(port=5000, debug=True)
```

这里发生了什么事？:
这个脚本导入所有需要的包，并实例化一个 app 和 api。然后，它在本地主机端口 5000 上运行应用程序。执行您的脚本:
`c:/my_root_folder/venv/Scripts/python.exe c:/my_root_folder/main.py``
您会注意到您的脚本正在运行，但没有停止。我们创建的服务器持续监听请求。让我们给它点东西听。

## 第五步。创建路线和控制器

我们希望转到 localhost:5000/test，并让 API 在我们导航到该页面或向该位置发布内容时执行一些操作。为了做到这一点，我们需要我们的 API 来监听一个路由，即 **/test** 。

```
from flask import Flask
from flask_restful import Api
from controllers.testController import TestControllerapp = Flask(__name__)
api = Api(app)
api.add_resource(TestController, '/test')if __name__ == "__main__":
    # start up api
    app.run(port=5000, debug=True)
```

上面的代码与第一段代码几乎相同。如你所见，听路线很容易。这发生在第 7 行:`api.add_resource(TestController, ‘/test’)`
它在 **/test** 监听请求，并将请求传递给我们的 TestController，这是我们在第 3 行导入的一个类。让我们创建这个控制器。用这个内容创建`c:/my_root_folder/controllers/testController.py`:

```
from flask_restful import Resource, requestclass TestController(Resource):
    def get(self):
        return {
            "message": "get: hello from the testcontroller"
        }
```

从 API 请求数据有多种方式。您可以发送 GET 请求或 POST 请求。GET 用于检索数据，比如检索我的个人资料图片。POST 请求用于提交数据，如提交评论。还有更多的类型，我们将在下一部分讨论。首先让我们测试一个简单的 API。

上面的 TestController 实现了一个 get 方法，该方法处理传递给控制器的 get 请求。现在我们只返回一条简单的消息。

## 第六步。测试

打开浏览器并导航到 localhost:5000/test。如果一切顺利，您将看到我们在控制器中定义的消息。

![](img/a729496de56970a0512e2f34ef41abe3.png)

假设这个服务员是我们的 API，为我们提供一些数据(图片由[pix abay](https://www.pexels.com/@pixabay)on[Pexels](https://www.pexels.com/photo/blur-breakfast-chef-cooking-262978/)提供)

# 结论

祝贺你创建了你的第一个 API！我们的新 API 已经启动并运行，但是它很笨。在下一部分中，我们将实现更多的功能，比如不同类型的请求、使用 JSON web 令牌的安全性以及到数据库的连接。[关注我](https://mikehuls.medium.com)敬请关注！

—迈克

# 词汇表

一些术语，解释

*   **API** =应用编程接口。允许您连接两个程序
*   发送给 API 的命令。你在请求它做些什么
*   **响应=**API 返回的东西；数据、错误或状态
*   某网站上的一条路径。是【www.medium.com】**/@ Mike huls****或[test.com**/this/is/a/path**](http://www.medium.com/@mikehuls)中的**加粗**部分**
*   **状态代码=** 代表 api 状态的代码。大家都听说过 404:没找到！