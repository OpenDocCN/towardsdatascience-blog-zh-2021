# BigQuery 脚本入门

> 原文：<https://towardsdatascience.com/getting-started-with-bigquery-scripting-45bdd968010c?source=collection_archive---------18----------------------->

## 揭开 BigQuery 功能强大但可能难以理解的一面，一步一步来

![](img/00e9addf2d98b6383e09d8d7d6c495c5.png)

BigQuery 脚本有点像回到过去。照片由 [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

BigQuery 中的脚本功能非常强大，但语法复杂，尤其是对于习惯于使用简洁、描述性的、大部分是顺序的和人类可读的语言(如 Python)的人来说。

关于[脚本](https://cloud.google.com/bigquery/docs/reference/standard-sql/scripting)的官方文档是一个很好的参考，但我总是发现很难找到一本指南来帮助我一步一步地学习核心概念，从最简单的情况到更复杂的情况。

因为解释概念会让你更深入地学习它们，所以我尽最大努力学得更深一点。

所以让我们从头开始。

# 什么是 BigQuery 脚本？

BigQuery 中的脚本是做事情的语句序列。它们有基本的控制结构，如变量和循环，并使您能够自动化操作，否则将需要重复，复制/粘贴和潜在的人为错误，这总是伴随着这种方法。

通过在脚本中使用数据定义语言( [DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language) )，您可以实现大量的自动化任务，而无需离开您可信赖的 BigQuery 控制台。

BigQuery 脚本最有用的方面之一是能够将行为封装到一个**过程**(本质上是一个函数)中，然后用一组参数调用它来执行一个或一组任务。

# 程序存在于何处？

BigQuery 中的过程看起来与用户界面左侧资源树中的表或视图完全一样(具有不同的图标)，无论您使用的是哪个版本，资源树都在左侧。我不喜欢在新版本中启动额外的标签页，所以我坚持使用以前的功能强大的控制台。

# 你是如何创造它们的？

在 BigQuery 中定义一个函数(实际上它在技术上是一个过程。或者有时是例行公事。但我要称它们为函数，因为它们是函数)，至少你要命名函数，并在开头和结尾之间添加一些 SQL。开始由 BEGIN 语句标记，结束由 end 标记。

到目前为止一切顺利。实际的语法是:

```
CREATE OR REPLACE PROCEDURE project_id.dataset_name.function_name() 
BEGIN
# function body 
END;
```

实际上，在开头和结尾之间会有很多代码，在括号之间是变量定义，但这是代码的高级结构。

但是，上面的代码不会运行，因为函数体中没有要执行的内容。添加一个简单的 select 语句会在右下角给你一个方便的绿色勾号，这意味着你可以开始了。我将在 **flowfunction** 项目的**示例**数据集中创建一个真正的函数(注意，如果您的项目名称中没有任何下划线，您不需要用反斜杠将它们括起来，我认为这样更简洁):

```
CREATE OR REPLACE PROCEDURE 
flowfunctions.examples.hello_world() 
BEGIN
SELECT TRUE;
END;
```

太好了！运行后，我现在在**示例**数据集中有了一个名为 **hello_world** 的函数。我可以在我的结果窗格中看到对此的确认:

```
This statement replaced the procedure named flowfunctions.examples.hello_world.
```

但是我还没有实际运行它，所以我看不到输出。我将在脚本中添加另一行代码，重新定义函数，然后运行它:

```
CREATE OR REPLACE PROCEDURE 
flowfunctions.examples.hello_world() 
BEGIN
SELECT TRUE;
END;CALL flowfunctions.examples.hello_world()
```

现在，我应该在结果窗格中看到两个阶段。如果我第二次点击**查看结果**，我将看到一个结果为**真**。现在，由于我调用了函数 **hello_world** ，我最好更新函数体以与此保持一致。我将把代码改为:

```
CREATE OR REPLACE PROCEDURE 
flowfunctions.examples.hello_world() 
BEGIN
SELECT "Hello World!" AS response;
END;CALL flowfunctions.examples.hello_world()
```

现在我有了一个名为“你好，世界！”的专栏作为单行响应值。太好了！我们现在已经构建了最简单的 BigQuery 函数(与[发布的博客文章](https://cloud.google.com/blog/products/data-analytics/command-and-control-now-easier-in-bigquery-with-scripting-and-stored-procedures)相比，后者非常有趣，但肯定不是最简单的脚本介绍示例)。

# 参数呢？

还有一件事，因为您几乎总是会将函数参数传递给 BigQuery 过程，所以我将添加一个名为 **name** 的输入参数。这将是一个字符串，所以我需要预先定义它，我将把它变成一个新函数:

```
CREATE OR REPLACE PROCEDURE 
flowfunctions.examples.hello_name(name STRING) 
BEGIN
SELECT FORMAT("Hello %s!", name) AS response;
END;CALL flowfunctions.examples.hello_name("Dad")
```

现在，在执行之前，您可能会注意到函数调用带有红色下划线，这表明有问题。不要担心，这是因为在执行 CREATE 或 REPLACE 语句之前，该函数是不存在的。

执行这个脚本将创建函数，然后显示“Hello Dad！”作为单行响应，使用[格式的](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#format_string)字符串函数将变量注入字符串，这比使用[连接](https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#concat)更具可读性。

所以我们开始吧，最简单的可能的函数让你开始在 BigQuery 中编写函数。

如果您觉得这(以及其他相关材料)有用和/或有趣，请跟我来！

如果你还不是会员，[加入 Medium](https://jim-barlow.medium.com/membership) ，每月只需 5 美元，就能从这个活跃、充满活力和激情的数据人社区获得无限的故事。也有很多其他人，但是如果你对数据感兴趣，那么这里就是你要去的地方…