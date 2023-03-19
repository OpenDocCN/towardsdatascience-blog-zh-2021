# 创建快速自动记录、可维护且易于使用的 Python API——CRUD 路由和路由管理

> 原文：<https://towardsdatascience.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-crud-routes-and-routing-7e8f35ebda46?source=collection_archive---------28----------------------->

## 学习如何捕捉请求并获取它们所携带的信息

![](img/e5b8457ef4cab6f79ff02ff27e80c99b.png)

我们的 API 监听传入的请求(图片由唐纳德·詹纳蒂在 [Unsplash](https://unsplash.com/photos/4qk3nQI3WHY) 上提供)

在 [**上一篇文章**](https://mikehuls.medium.com/create-a-fast-auto-documented-maintainable-and-easy-to-use-python-api-in-5-lines-of-code-with-4e574c00f70e) 中，我们已经用 5 行代码建立了一个 API。我们已经安装了依赖项，并创建了一个工作 API，只有一个简单的路径。在本文中，我们将进一步构建，用多种不同的途径充实 API，向您展示使用 FastAPI 可以做的所有事情。然后，我们将关注如何以简洁明了的方式组织所有这些路线

# 安装

为了让这篇文章尽可能容易理解，我们将假设我们正在为我们的博客网站编写一个 API。目标是创建一些我们可以 CRUD(创建、读取、更新、删除)的公共路径:

*   添加新文章
*   从数据库中检索文章
*   更新文章以修复打字错误，例如
*   删除一篇文章

我们将专注于捕捉请求和提取信息。我们的 API 如何与我们的数据库通信是另一个主题，在本文的<https://mikehuls.medium.com/dramatically-improve-your-database-inserts-with-a-simple-upgrade-6dfa672f1424>**中有很好的描述。也可以查看本文 中的 [**来创建一个版本控制的数据库结构。**](https://mikehuls.medium.com/version-control-your-database-part-1-creating-migrations-and-seeding-992d86c90170)**

**由于我们的 API 将负责捕捉请求并获取它们携带的信息，所以让我们首先快速检查一下请求是如何工作的，以便我们可以准确地为我们的 API 创建端点来捕捉所有信息。**

**![](img/d2659596907944e4168273d08fba9260.png)**

**要求近距离拍摄(图片由 [Jassine Khalfalli](https://unsplash.com/@yassine_khalfalli) 在 [Unsplash](https://unsplash.com/photos/_c70Nhh6p44) 上拍摄)**

# **请求**

**让我们稍微简化一下，假设一个请求由一个`URL`、一个`HTTP method`和一个可选的`body`组成。在我们的 API 中实现它们之前，我们将快速浏览每一个。**

## **剖析 URL**

**我假设你对网址很熟悉，因为你在使用互联网时必须把它们输入浏览器。为了我们的 API，让我们把下面的 URL 分成与我们相关的 3 个部分。**

> **[https://www.github.com/mike-huls?tab=overview&郎=恩](https://www.github.com/mike-huls?tab=overview&lang=en)**

1.  **[https://www.github.com](https://github.com)
    这部分指定网站托管的**服务器**的位置。**
2.  **/mike-huls
    路径**路径**。这指定了我们想要在网站上访问的资源(文件)的确切位置。把它想象成你计算机上的一个文件系统；就像`c:/users/mike/mystuff`一样。**
3.  **？tab=overview&lang=en
    问号是查询字符串分隔符。它将到特定资源的路由与将被传递给资源的**查询参数**分开。参数成对传递，用“&”分隔。**

**总之:我们向`[https://www.github.com](https://www.github.com/mike-huls?tab=overview)`发送一个请求，在`/mike-huls`的服务器上搜索我们的资源，然后给资源传递一些查询参数，即`tab=overview`和`lang=en`。我们将在以后创建路线时使用这些术语。**

## **HTTP 方法**

**方法(也称为“动词”)用于在发送请求时区分不同的(HTTP)操作。虽然还有很多，但对于本文，我们将使用主要的 4:**

*   **获取—用于检索数据**
*   **发布-用于提交数据**
*   **上传—用于更新数据**
*   **删除—用于删除数据**

**我愿意把这些方法看作是区分你的目标的额外数据。**

## **请求正文**

**主体是我们想要发送的实际数据。在我们的例子中，我们可以发布一篇新文章，这样我们的 API 就可以将它插入到数据库中。**

**![](img/94cacade84c71cc4203f110cd062d30a.png)**

**在我们的 API 中路由不同类型的请求(图片由 [Javier Allegue Barros](https://unsplash.com/@soymeraki) 在 [Unsplash](https://unsplash.com/photos/C7B-ExXpOIE) 上提供)**

# **创建我们的 API**

**记住，请求由一个 URL 和一个方法组成。为了捕捉这些，我们需要为不同类型的请求创建特定的路由。在路线中，我们需要指定(至少)两件事:**

*   **路径(捕捉 URL 的路径**
*   **一个方法(捕捉请求的 HTTP 方法)**

**让我们来看看创建、读取、更新和删除文章(CRUD)所需的所有操作。我们将从最简单的开始:检索文章。**

## **简单阅读:检索所有文章的路径**

**最简单的途径是检索所有文章。看起来是这样的:**

```
@app.get("/all_articles")
def get_all_articles():
    return function_that_retrieves_all_articles()
```

**两件事很重要。首先注意我们使用了 GET 方法(用`@app.get`表示)。第二个是我们指定路径的地方:GET 方法中的`articles`部分。这定义了到此路由的路径。对这个路由的 get 请求可能类似于:`[www.ourbloggingwebsite.com/all_articles](http://mikehuls.medium.com/)`，它将从数据库中检索我们所有的文章。**

## **使用查询参数读取:检索一篇文章的路线**

**检索所有文章对于获得我们所有文章的概览来说是非常好的，但是如果用户只想阅读一篇文章呢？让我们创建另一个只检索一篇文章的路径:**

```
@app.get("/article")
def get_one_article(articleId:int):
    return function_that_retrieves_article(articleId=articleId)
```

**请注意，这条路线看起来与前一条非常相似。它仍然使用 get 方法，但是路径略有不同:函数现在需要一个参数；我们必须提供一个文章 Id。这称为查询参数。对该路由的 GET 请求如下所示:`[www.ourbloggingwebsite.com/article?articleId=1](https://mike-huls.github.io/)`。这将只返回一篇文章，即 articleId =1 的文章。**

**我们可以在这条路线上增加一点这样的东西**

```
@app.get("/article")
@app.get("/article/{articleId}")
def get_one_article(articleId:int):
    return dbConnection.get_one_article(articleId=articleId)
```

**请注意，我们在这里添加了第二行；它允许用户使用查询参数如`/article?articleId=1` *和路径如`/article/1`检索文章。我们可以使用两条线或只用一条线。***

## **删除:删除文章的途径**

**delete 语句的工作方式与带有查询参数的 GET 完全相同，它使用查询参数进行筛选。**

```
@app.delete("/article")
def delete_one_article(articleId:int):
    return dbConnection.delete_one_article(articleId=articleId)
```

**如您所见，它与前面的方法非常相似，唯一的例外是我们在第一行中使用了 DELETE 方法。**

## **创建:发布新文章的途径**

**让我们来看看现有的东西。插入新文章。路线看起来像这样:**

```
@app.post("/article")
def post_article(body:dict):
    return dbConnection.post_article(articleJson=body)
```

**再次注意，我们使用的是发送到/article 路径的帖子。FastAPI 从请求中获取主体数据，并将其传递给负责将其插入数据库的函数。**

## **更新:更新现有文章的途径**

**更新将来自 GET 的查询参数和来自 POST 请求的主体的查询参数组合在一起:**

```
@app.put("/article")
@app.put("/article/{articleId}")
def update_article(articleId:int, body:dict):
    return dbConnection.update_article(articleId=articleId, articleJson=body)
```

**注意，它接收一个 articleId 和一个 body。我们可以用这些来替换数据库中的记录；我们使用 articleId 查找它，然后用 articleJson 中的数据替换它。轻松点。**

# **组织路线**

**我们刚刚创建的所有路线都是为了一件事:修改文章。如果我们也有故事呢？有了所有这些途径，你可以想象你的项目会很快变得混乱。谢天谢地，组织这些路线很容易。**

**![](img/0ffceb16c56284ed30b011d223851c60.png)**

**我们的路线整洁有序(图片由 [Martin Lostak](https://unsplash.com/@martin_lostak) 在 [Unsplash](https://unsplash.com/photos/Gzu-sNr19TU) 上拍摄)**

**我们将创建一个名为`article_routes.py`的文件，并添加以下代码:**

```
from fastapi import APIRouter
router_articles = APIRouter()
```

**添加完这段代码后，我们可以使用路由器来捕捉请求。我们只需将前一章中编写的所有代码片段粘贴到`article_routes.py`中。**

```
@router_articles.get("/article")
def get_one_article(articleId:int):
    return function_that_retrieves_article(articleId=articleId)
```

**那些对细节有敏锐眼光的人注意到了一件事:在前一部分，我们用`@app.get(....)`修饰了函数。在上面的例子中，我们必须使用`@router_articles`，因为这是我们的新路由器。点击 查看此文件 [**的最终版本。**](https://github.com/mike-huls/fastapi_2/blob/main/routes/article_routes.py)**

**最后一步是转到我们的`main.py`文件，告诉它将某些请求重定向到我们的新文件。我们将所有与文章有关的请求重定向到我们的文章路由器，方法是导入它并告诉 FastAPI 重定向某些请求。我们的 main.py 文件现在简洁明了:**

```
from fastapi import FastAPI
from routes.article_routes import router_articles

app = FastAPI()
app.include_router(router=router_articles, prefix='/articles')
```

**当我们开始处理故事时，我们可以轻松地包括一个新的路由器，并在其中编写我们所需的所有代码！轻松又有条理。**

# **结论/TL；速度三角形定位法(dead reckoning)**

**[**在这里**](https://github.com/mike-huls/fastapi_2) **查看最终 api 的回购。****

**本文主要关注什么是请求，以及我们如何在路由中从这些请求中获取不同类型的信息。有了这些知识，你就可以创建一个非常好的 API 了。在下一篇文章中，我们将关注于为我们的路线定义**模型**。有了这些，我们就可以在将数据传递给数据库之前对其进行**清理和检查。此外，我们将重点关注**安全性**；例如，只有经过认证的访问，或者您必须提供密码。订阅敬请关注！****

**如果你有建议/澄清，请评论，以便我可以改进这篇文章。同时，查看我的其他关于各种编程相关主题的文章。**

**编码快乐！**

**—迈克**

**页（page 的缩写）学生:比如我正在做的事情？跟我来！**