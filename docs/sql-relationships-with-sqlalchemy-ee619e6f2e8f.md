# SQL 与 SQLAlchemy 的关系

> 原文：<https://towardsdatascience.com/sql-relationships-with-sqlalchemy-ee619e6f2e8f?source=collection_archive---------15----------------------->

## 手动配置如何简化查询

![](img/979f390df4145bcc33acf6ddeef88236.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) 拍摄的照片

几周前，我写了一篇关于利用实体框架的力量来最小化复杂的 Linq 查询的[文章](https://medium.com/codex/something-i-learned-this-week-entity-framework-is-picky-about-primary-keys-b5d7642c9ab7?source=your_stories_page-------------------------------------)。本质上，解决方案是为数据库中的每个表创建配置，然后定义配置中表之间的关系。这并不是一件容易的事情。事实上，这有点令人困惑。但是这让我想到，是否可以使用 SQLAlchemy for Python 来完成类似的事情。

# 形势

在我们的场景中，我们有一个包含三个表的数据库，即 Application、ApplicationRunner 和 ApplicationLog。应用程序表包含一个名为 ApplicationRunnerId 的外键。该键在 Application 和 ApplicationRunner 表之间创建一对一的关系。至于 ApplicationLog 表，它保存了应用程序表的外键，指定了一对多关系。

看看我们当前的代码，所有确定表属性和关系的繁重工作都是由 SQLAlchemy 自动处理的。

至于获取数据，我们要么手动查询每个表，要么使用连接方法。为了简单起见，我们手动查询每个表。

# 有哪些不同的做法

与其偷懒让 SQLAlchemy 做所有艰苦的工作，我们可以采取代码优先的方法，为我们的表创建手动配置。

在我们开始修改表类之前，我们需要导入一些额外的东西。第一个将允许我们为表属性定义不同的类型，而第二个为我们提供了在表之间创建关系的功能。

```
from sqlalchemy import Column, ForeignKey, Integer, String, Numeric, DateTime, ForeignKey, CHAR, Table
from sqlalchemy.orm import relationship
```

导入后，我们可以通过定义所有属性和设置主键来开始处理应用程序表。属性类型将与数据库中的字段类型相匹配(INT 将是整数，VARCHAR 将是字符串，等等。).

```
class Application(Base):
     __tablename__ = "Application"
     ApplicationId = Column(Integer, primary_key=True)
     ApplicationName = Column(String(100), nullable=False)
```

接下来是 ApplicationRunner 表。

```
class ApplicationRunner(Base):
     __tablename__ = "ApplicationRunner"
     ApplicationRunnerId = Column(Integer, primary_key=True)
     ApplicationRunnerName = Column(String(50), nullable=False)
```

在继续之前，我们需要在这两个表之间创建一对一的关系。首先，我们需要向应用程序表添加一个外键。

```
ApplicationRunnerId = Column(Integer, ForeignKey("ApplicationRunner.ApplicationRunnerId"))
```

然后，我们将添加一个新的 class 属性来创建与 ApplicationRunner 表的关系。这种关系很特殊，因为我们将利用惰性联合加载。从本质上讲，延迟加载是指查询返回的对象没有加载相关的对象。一旦引用了相关对象，将运行另一个 SELECT 语句来加载所请求的对象。至于“joined ”,它向初始选择添加了一个连接，以保持所有内容都在同一个结果集中。

```
Runner = relationship("ApplicationRunner", lazy="joined")
```

我们需要做的下一件事是向 ApplicationRunner 表添加一个关系。这样做时，我们需要确保这个关系将反向引用 ApplicationRunner 表，并且它不需要对象列表，因为它是一对一的关系。

```
ApplicationRelationship = relationship("Application", backref="ApplicationRunner", uselist=False)
```

现在我们已经定义和配置了这些表，我们可以继续到 ApplicationLog 表。就像我们对前两个表所做的那样，首先需要定义属性和主键。

```
class ApplicationLog(Base):
     ApplicationLogId - Column(Integer, primary_key=True)
     ApplicationLogMessage - Column(String(250), nullable=False)
```

同样，为了创建关系，这个新表中需要一个外键。

```
ApplicationId = Column(Integer, ForeignKey("Application.ApplicationId"))
```

最后，ApplicationLog 表的一对多关系如下所示。

```
ApplicationRelationship = relationship("Application", backref="ApplicationLog", uselist=True)
```

但是对于应用程序表，它会是这样的。

```
Log = relationship(“ApplicationLog”, lazy=”joined”)
```

最终设置好一切后，我们可以运行一个查询来获取数据。

```
application = session.query(Application).filter(Application.ApplicationId == command.ApplicationId).one()
```

从应用程序表中获取数据很容易。

```
print(application.the_attribute)
```

但是，如果您希望从 ApplicationRunner 表中访问数据:

```
print(application.ApplicationRunner.the_attribute)
```

最后，要查看 ApplicationLog 表中的记录，需要一个 FOR 循环。

```
for item in application.ApplicationLog:
     print(item.the_attribute)
```

# 赞成还是反对

很快，您会注意到设置配置比让 SQLAlchemy 自动为您处理要花费更多的时间。就像在实体框架中一样，启动和运行东西也可能有点混乱/复杂。然而，这个过程并不全是坏事。通过使用代码优先的视图来配置表，它消除了 SQLAlchemy 的一些黑箱，将更多的权力交还给开发人员。尽管如此，它确实在查询端制造了更多的神秘，因为您看不到额外的连接或选择。

# 结束语

总而言之，我认为这是一次非常有启发性和有趣的经历。它明确回答了我关于 SQLAlchemy 能力的问题。然而，与此同时，它也带来了一些关于性能的新问题。作为编写大量需要快速请求的 API 的人，这种查询和配置是明智的吗？让我知道您对 SQLAlchemy 和在其中配置表关系的想法。在以后的文章中，我计划探索这些性能问题。直到下一次，编码快乐，干杯！

***用我的*** [***每周简讯***](https://crafty-leader-2062.ck.page/8f8bcfb181) ***免费阅读我的所有文章，谢谢！***

***想看完介质上的所有文章？成为中等*** [***成员***](https://miketechgame.medium.com/membership) ***今天！***

看看我最近的一些文章:

</mysql-vs-redis-def3287de41>  <https://python.plainenglish.io/searching-for-text-in-those-annoying-pdfs-d95b6dc7a055>  </daas-data-as-a-service-78494933253f>  <https://miketechgame.medium.com/one-year-of-writing-on-medium-d4d73366e297>  </deep-learning-in-data-science-f34b4b124580>  

参考资料:

  <https://stackoverflow.com/questions/16433338/inserting-new-records-with-one-to-many-relationship-in-sqlalchemy>  <https://blog.theodo.com/2020/03/sqlalchemy-relationship-performance/> 