# 我如何通过 Python 将我的 PL SQL 程序发布为 REST API

> 原文：<https://towardsdatascience.com/how-i-published-my-pl-sql-program-as-rest-api-through-python-e2225f0457cb?source=collection_archive---------30----------------------->

## 关于 Python、基于令牌的身份验证和 REST API，您需要知道的一切

![](img/b9bb905db2c211271bf8860326a095b6.png)

费尔南多·巴西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

Sudha 是一名 IT 专业人员，在 SQL、PLSQL、数据库和 ERP 应用程序方面拥有数十年的经验。她一直享受着开发新应用程序和增强本地应用程序的生活。然而，它即将改变。

她的雇主正在将一些应用程序迁移到基于云的应用程序中。随着这种转变的进行，一些本地应用程序将不得不与基于云的应用程序进行近乎实时的交互。虽然有中间件可以使本地 API 与基于云的 REST API 进行交互，但 Sudha 在想，为什么她不能将本地机器上的经典程序发布为 REST API。

这可能发生在 Sudha 身上:应用程序开发人员或机器学习专业人员:参与集成不同的系统。

如果你有同样的困境，这篇文章是给你的。

在本文中，我将解释什么是 REST API，以及如何将一个经典的 PLSQL 程序转换成 REST API。虽然本文讨论的是 PLSQL 程序，但是这里解释的步骤和概念可以用于任何其他程序。

好吧，让我们从基础开始！

## 数据编码

你看过电影[美丽心灵](https://en.wikipedia.org/wiki/A_Beautiful_Mind_(film))吗？

在这部电影中，主角解码了敌人的加密电讯信息。敌人发来的信息以这样一种方式被解码，它对任何非预期的接收者都没有任何意义。但是消息的预期接收者将知道破译它的算法，并从中获得真正的含义。

我们举个例子。考虑编码的消息是“Byoot fqtsbyq”。你能理解它的意思吗？除非您知道消息已经使用下面的映射表进行了编码。

![](img/b84e1a2632c88ada070b67b45409966b.png)

(图片由作者提供)

现在你可以很容易地将信息解码为“你好兄弟”。

计算机在通信(以及安全存储数据)的同时，也使用特定的算法对数据进行编码。数据编码有以下优点

*   它保证了传输的安全性。
*   一些算法可以减小原始消息的大小。编码信息比原始信息小得多，易于传输。

## 代币

令牌是编码数据。基于令牌的安全在当代安全体系结构中非常流行。虽然有几种基于令牌的安全技术，但我将带您了解开放标准和流行的技术 Json Web Tokens (JWT)。

JWT 编码和解码数据的方式很有趣。但在我去之前，我想问你一个问题。

计算机使用预先定义的算法对数据进行编码。这些算法被很好地发表。如果黑客闯入通信网络并访问正在传输的令牌，他/她可以使用相同的算法解码令牌并改变令牌。计算机如何保护令牌？

你有什么建议？

好吧，这里有一个选项可以使这种基于令牌的通信变得安全。

*   包括用于对令牌内的数据进行编码的算法的名称:由于存在成千上万种公开的算法，所以不可能对令牌进行解码，除非你知道用于对令牌进行编码的算法，或者拥有尝试使用所有这些算法对令牌进行解码的超高速计算机。
*   添加签名:随着主要内容添加一个独特的签名。只有发送和接收计算机知道签名。这样，即使令牌被黑客攻击和更改，签名也不会匹配，接收方计算机将理解令牌被修改，并且不会处理数据。

这两个特点是 JWT 的关键因素。JWT 令牌包含三个部分。顾名思义，每个部分都包含 JSON 格式的数据。

*   头:头包含算法的名称和令牌类型的类型。
*   有效载荷:有效载荷包含要传输的数据。您可以在有效载荷部分自由添加想要传输的数据。
*   签名:此部分包含一个唯一的签名。在客户端和服务器通信的情况下，服务器向客户端分配唯一的签名并通知客户端。客户端必须将此签名添加到它发送给服务器的每个令牌中，否则，服务器将不会接受该令牌。

令牌还可以包含像到期时间这样的附加信息，这将使令牌在到期后无效。

## 基于令牌的通信

既然已经了解了令牌的基本知识，那么让我们来了解基于令牌的通信是如何在客户机和服务器之间发生的。

下图显示了客户端和服务器通过 JWT 进行通信的步骤。

![](img/de37e90b522e16b0477c100e62caf85b.png)

客户端和服务器之间基于 JWT 的通信(图片由作者提供)

1.  客户端通过用户 id、密码和任何其他此类身份验证机制来访问服务器。
2.  服务器验证用户凭证。
3.  服务器为通过身份验证的客户端创建一个带有特定签名(也称为密钥)的令牌。
4.  服务器将令牌发送给客户端。
5.  此后，客户机将令牌包含在对服务器的任何未来请求中。服务器在处理任何客户端请求之前验证令牌。

这是对客户机和服务器之间基于令牌的通信如何工作的高级描述。

让我们看看它现在工作。

上面的代码将创建一个 JWT 令牌

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJIZWxsbyBXb3JsZCBKV1QiOiJGaXJzdCBQYXlsb2FkIn0._Ah6WeQCLy2DwhzcBjv6tmT2aZNinZ2Ikxm_fe1ePFI
```

请注意，令牌由三部分组成，用“.”分隔如果你复制这个令牌并粘贴到 jwt.io 网站上的编码框中，它会将其解码为相同的消息。

![](img/301be0c67e817251f7126cdc0b0d2dbb.png)

jwt.io 上的 JWT 示例

注意，上面写着“无效签名”。键入 Secretkey123:此令牌的密钥和消息无效签名将被替换为已验证签名。

![](img/3766ea6e2886f12b84bd4a2efcd178f6.png)

现在它说“签名验证”。这就是客户端和服务器之间通信的安全方式。

下面是解码相同编码令牌的 Python 代码。

## REST API

既然您已经了解了基于令牌的通信是如何工作的，那么是时候了解 REST API 了。

和之前一样，我会从一个问题开始。你是怎么找到这篇文章的？

你在 Medium.com 找到的。这里 medium.com 是域名。你可以认为这是存储这篇文章的计算机的地址。如果你输入 www.medium.com/…和文章细节，你就会看到这篇文章。

在计算机之间的通信中发生了以下步骤。

1.  您在计算机浏览器(客户端)上输入了网址。
2.  当您的计算机连接到互联网时，调制解调器开始寻找具有[www.medium.com](http://www.medium.com)域信息的计算机
3.  您的计算机(客户端)连接到 medium.com 计算机(服务器)，并在您的浏览器中打开请求的页面。

现在，假设你正在编写一个调用网址的程序，不是在浏览器中，而是直接在程序中。假设，你有一个 python 程序，你写 http://www.medium.com，程序直接连接到服务器(步骤 3)。

这就是表述性状态转移(REST)的工作方式。在这种情况下，服务器将只允许某些页面(例如/thisarticle)给某些经过身份验证的用户，甚至允许特定的经过身份验证的用户修改某些页面上的某些信息。

REST API 中通信的一个关键方面是方法。该方法可以被认为是指示通信目的的参数。

*   GET:客户端正在从服务器获取信息。
*   POST:客户端正在向服务器添加新数据。
*   PUT:客户机正在修改服务器上的现有数据。
*   删除:客户端正在删除服务器上的现有数据。

## 让我们建造它！

既然您已经理解了基于令牌的身份验证和 REST API 的基础知识，那么是时候从 PLSQL 程序和 Python 开发 REST API 了。

如果你想了解 PLSQL、Oracle 数据库和 Python 是如何协同工作的，我推荐你阅读我下面的文章。

[Python 和 Oracle 数据库。没有数据科学家能做到的组合… |作者 Sanjay Singh |走向数据科学](/python-and-oracle-database-c7b5d4d7fa4c)

Python 有一个用于连接 Oracle 数据库的内置库 cx_Oracle。下面的代码片段显示了如何使用 cx_Oracle 库连接 Oracle 数据库。用您的数据库详细信息替换下面代码中的 userid、密码、主机、端口和服务名。

PLSQL 是 SQL 的过程扩展。我有一个简单的 PLSQL 函数 EMPFUNCT。它将年龄作为参数，并返回年龄大于所提供参数值的雇员人数。

可以通过数据库连接游标中的 callfunc 方法在 Python 应用程序中调用 PLSQL 程序，如下面的代码片段所示。

在上面的代码 PLSQL 函数中，使用参数值 20 调用 EMPFUNCT。它返回 21，表明 EMP 表包含 21 名年龄超过 20 岁的雇员。

![](img/2a05c3cd23557c8d2cc9a900ce730754.png)

如何发布这个和 REST API 一样的 PLSQL 函数？

cx_Oracle 库方便了 Python 程序与 Oracle 数据库及其对象如 PLSQL 程序的连接。然而，要将 PLSQL 程序发布为 REST API，需要将其公开为 web 服务器上的服务。

如果你想知道什么是网络服务，这里有一个来自[维基百科](https://en.wikipedia.org/wiki/Web_service)的简短解释

*web 服务是运行在计算机设备上的服务器，通过网络监听特定端口的请求，提供 Web 文档(HTML、JSON、XML、Images)，并创建 Web 应用服务，用于解决 web (www、internet、HTTP)上的特定领域问题。*

Python 有一个非常有用的框架 Flask，这是一个 web 服务开发框架。如果您使用的是 Anaconda 发行版，Flask 已经为您安装好了。

下面的代码片段展示了如何使用 Flask 框架将同一个 PLSQL 函数 EMPFUNCT 发布为 REST API。

执行上面的代码将显示下面的结果

![](img/783bf0884c5807aa8a932aa431468203.png)

如果你打开浏览器，输入[http://127 . 0 . 0 . 1:5002/empageenq？empage=20](http://127.0.0.1:5002/empageenq?empage=20) 您将获得与调用参数值为 20 的 EMPFUNCT 函数相同的结果。

![](img/4d23d553b5c53cdb38fb56186a4c4762.png)

调用 REST API get 方法

现在，PLSQL 函数 FUNCT 被公开为 REST API，任何其他系统都可以像其他 REST API 一样调用它，只要它可以访问发布该 API 的系统，并且提供访问的方法之一是通过 Python JWT 令牌。

## 结论

一个 PLSQL 程序可以通过 Python 库 cx_Oracle 和 Flask 框架发布为 REST API。在本文中，我已经向您介绍了将简单的 PLSQL 程序发布为 REST API 的步骤，可以通过 GET 方法访问 REST API。使用相同的步骤，您还可以将 PLSQL 函数增强为 POST 和其他 REST API 方法。

期待大家的建议，也很乐意回答大家的问题。

**参考文献:**

[Python+SQL+Oracle](https://www.udemy.com/course/python-with-oracle-developer-course/?referralCode=6F68B20A8FB16A20BFE1)

[PyJWT PyPI](https://pypi.org/project/PyJWT/)

[JSON Web 令牌— jwt.io](https://jwt.io/)

[JSON Web Token 教程及 Python-DEV 社区中的示例](https://dev.to/apcelent/json-web-token-tutorial-with-example-in-python-23kb)

[REST API 101:什么是 REST API，它是如何工作的？|邮差博客](https://blog.postman.com/rest-api-definition/)

[如何在 Python 中使用 API(初学者指南)【Python API 教程】(rapidapi.com)](https://rapidapi.com/blog/how-to-use-an-api-with-python/)

[通过四个简单步骤将机器学习模型部署为 REST API |迈向数据科学](/machine-learning-model-deployment-as-rest-api-in-four-easy-steps-e65764f7c5c)

[通过四个简单步骤将机器学习模型部署为 REST API |迈向数据科学](/machine-learning-model-deployment-as-rest-api-in-four-easy-steps-e65764f7c5c)