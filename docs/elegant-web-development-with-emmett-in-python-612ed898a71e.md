# 用 Python 语言用 Emmett 进行优雅的 Web 开发

> 原文：<https://towardsdatascience.com/elegant-web-development-with-emmett-in-python-612ed898a71e?source=collection_archive---------24----------------------->

## 概述如何使用 Emmett Web 框架，并使用它在 Python 中创建 Web 应用程序

![](img/69d25afaaa40e8688b14f8369b74cacb.png)

(src =[https://pixabay.com/images/id-1209008/](https://pixabay.com/images/id-1209008/)

Python web 开发是 Python 编程的一部分，多年来一直由 Flask 和 Django 等流行选项主导。那些解决方案当然很棒，我自己也用过很多。也就是说，我认为任何软件都有缺点，因为必须采取某些方向，并且有些软件在某些方面比其他方面更好。在寻找我写的一篇关于机器学习验证的文章时，我偶然发现了埃米特项目。

谁会想到我会有一个以自己名字命名的 Python web-framework 呢？当然，这只是一个笑话，这个框架实际上是以《回到未来》中的埃米特·布朗博士命名的。框架的标志是他戴着标志性护目镜的脸。事实上，我是那部电影的粉丝，但不是第二部(那是什么？)鉴于这个名字，这个包激发了我的好奇心，我决定尝试一下。如果你想更深入地了解这个包，你可以在 Github 上查看:

<https://github.com/emmett-framework/emmett>  

首先，我应该说，我通常对这类事情不抱太大期望，因为有这么多 web 框架的好例子，有更好的文档和更多的用户。然而，在阅读了特性列表并查看了方法之后，我对这个奇怪的包很感兴趣。虽然我不能说谎，但每当我尝试的时候，对自己的事情感到奇怪和好奇。但是我很高兴展示这个框架和它的能力！除了 Emmett、Django 和 Flask 之外，我过去还写过两篇讨论其他解决方案的文章，您可以在这里阅读:

</5-smooth-python-web-frameworks-for-the-modern-developer-47db692dfd52>  

或者这里:

</5-cool-alternatives-to-django-and-flask-for-deploying-endpoints-5c99a066696>  

# 功能概述

任何 web 框架实现的例子都会产生一个问题，为什么选择 Emmett 而不是 Flask 或 Django 等更受欢迎的解决方案。当我们在这两种解决方案之间比较 Emmett 时，我们应该注意到 Emmett 在很多方面是这两者的结合。Django 更常用于 fullstack 应用程序，而 Flask 更常用于更多数据驱动和简单的应用程序。两者都有不同的方法和语法。埃米特在很多方面是他们之间的一个伟大的中间地带。

语法和方法非常接近 Flask。就我个人而言，我更喜欢这样。我从来都不喜欢 Django 压倒一切的模板性质。虽然它肯定没有任何问题，但它会立即生成大量目录和文件，而不是让用户自己创建。Emmett 介于这两者之间，但它不依赖于目录和文件，而是使用模块调用，我认为这要优雅得多。如果你想了解更多关于 Flask 和 Django 之间的区别，我有另一篇文章会详细介绍:

</django-or-flask-recommendation-from-my-experience-60257b6b7ca6>  

首先也是最重要的，埃米特有一套平易近人、易于使用的方法。使用与 Flask 类似的语法，大多数用户会发现自己在使用框架时如鱼得水。然而，这个框架并不像 Flask 那么轻量级，这意味着 Flask 内置了比我们预期的更多的优秀特性。它写起来像 Flask，但工作起来更像 Django。

该模块带有数据库类，使查询事物、任务甚至身份验证变得更加容易。根据我使用该模块的经验，所有这些都比 Django 的实现更容易使用。该项目还声称是生产就绪，这对于那些想在他们的技术堆栈中使用该模块的人来说当然是很好的。

Emmett 的另一个很酷的地方是它是异步的。Websockets 太牛了！埃米特被设计在阿辛西奥的顶部，这是一个为这类事情而建的古老的图书馆。这种方法使 Emmett 成为非阻塞的，并且对许多设计模式更加有用。另一个很酷的事情是这个包自带了自己的查询接口。

埃米特最酷的一点是模板。在大多数 web 框架中，模板通常是一团乱麻。有时他们有自己的模板框架，需要学习一门新的语言，有时你可能需要写一些 HTML 和/或 Javascript 来完成工作。Emmett 有一个非常强大的模板系统，没有这些传统系统的所有问题。模板是用纯 Python 编写的！

# 利用埃米特

根据我对该软件包的体验，使用 Emmett 非常简单，非常平易近人。文档也很特别。您可以安装软件包

```
pip3 install emmett
```

当然，我不是网络开发人员。我在 web 框架中寻找的可能与传统上使用 Python 进行全栈的框架不同。我所缺乏的恰好也是埃米特所拥有的，所以那绝对是非常酷的！Emmett 实际上包含了一个我们可以使用的非常棒且易于使用的查询框架，我认为它确实令人印象深刻。查看文档中的以下示例，我们只用几行代码就创建了一个包含用户、帖子和评论的模式！

```
from emmett import session, now
from emmett.orm import Model, Field, belongs_to, has_many
from emmett.tools.auth import AuthUserclass User(AuthUser):
    # will create "users" table and groups/permissions ones
    has_many('posts', 'comments')class Post(Model):
    belongs_to('user')
    has_many('comments')title = Field()
    text = Field.text()
    date = Field.datetime()default_values = {
        'user': lambda: session.auth.user.id,
        'date': lambda: now
    }
    validation = {
        'title': {'presence': True},
        'text': {'presence': True}
    }
    fields_rw = {
        'user': False,
        'date': False
    }class Comment(Model):
    belongs_to('user', 'post')text = Field.text()
    date = Field.datetime()default_values = {
        'user': lambda: session.auth.user.id,
        'date': lambda: now
    }
    validation = {
        'text': {'presence': True}
    }
    fields_rw = {
        'user': False,
        'post': False,
        'date': False
    }
```

我们可以使用@app 语法来路由应用程序。在我看来，这是非常独特和酷的，我们使用@app.command()方法，然后我们可以调用全局定义的函数供我们的应用程序使用！一个非常流行的数据科学和数据工程任务是创建一个简单的端点，下面是使用 Emmett 完成该任务的代码:

```
from emmett import App  
app = App(__name__)  
@app.route("/") 
async def hello():
     return "Here is a simple endpoint!"
```

正如您可能知道的，这个端点与 Flask 中的端点非常相似！事实上，代码几乎是相同的。然而，很容易看出这个框架不是烧瓶，它带来了一大堆特性。在我看来，它真的很酷，很高级，而且声明性很强，有时似乎太容易使用了。我并不经常发现自己爱上我找到的像这样的随机包，但它确实值得一试！我知道我将来会用到它！如果你对 Flask 更感兴趣，我确实有另一篇真的很老的文章(2019 年的！)，您可以阅读以了解有关在 it 中部署端点的更多信息！

</constructing-http-data-pipelines-with-flask-27fba04fbeed>  

# 结论

虽然有时用一个有我名字的包工作有点令人不安，但这个包肯定值得处理那种恐怖谷的感觉。我真的很喜欢使用这个包，至少可以说，一次又一次地意识到它有多酷是令人震惊的。当然，当我使用这样的包时，我总是将它与我已经使用的包进行比较。在这方面，我最常用的选择可能是 Flask，我真的很喜欢 Flask 既小又大的设计。然而，这个网络框架让我对采用同样的方法并深入其中感到完全无语。如果我必须用 Python 程序员能理解的一句话来描述它，我会说“它就像 Flask，只是有很多令人惊奇和有用的特性。”我当然推荐这个模块，不仅仅是因为我们有相同的名字。感谢您的阅读，我希望您考虑试用这个软件包——它太棒了！