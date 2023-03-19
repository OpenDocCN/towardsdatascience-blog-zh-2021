# Gallia:数据转换库

> 原文：<https://towardsdatascience.com/gallia-a-library-for-data-transformation-3fafaaa2d8b9?source=collection_archive---------27----------------------->

## 用于实际数据转换的模式感知 Scala 库:ETL、特性工程、HTTP 响应等

![](img/c757eb59e9c37060da944f1b2627661a.png)

由[约书亚·索蒂诺](https://unsplash.com/@sortino?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

[Gallia](https://github.com/galliaproject/gallia-core/blob/master/README.md) 是一个用于通用数据转换的 Scala 库，重点关注**实用性**、**可读性**和**可伸缩性**(如果需要的话)。

这是一个个人项目，是我在对现有工具失望多年后开始的。它旨在帮助数据工程师完成工作，而不必在需要时放弃可伸缩性。它还旨在填补像 *pandas* 这样的库(对于那些重视 Scala 这样的强大类型系统的人)和 *Spark SQL* 这样的库(对于那些发现 SQL 很难理解特定查询复杂性的人)之间的空白。更一般地说，它是为应用程序中的大多数或所有数据转换需求提供一站式范例而创建的。

它的执行发生在**两个阶段**，每个阶段遍历一个专用的执行[有向无环图(DAG)](https://en.wikipedia.org/wiki/Directed_acyclic_graph) :

1.  初始的**元**阶段完全忽略数据，并确保转换步骤是一致的(模式方面)。
2.  随后的**数据**阶段，在此阶段数据被实际处理。

![](img/707ab08769a6b88387c240e4b25ec1a5.png)

图片通过 LogoGround 授权给 Anthony Cros。

# 微不足道的例子

下面是一个非常基本的用法示例:

这样就会成功打印: *{"* ***姓名*** *":"托尼】，* ***年龄*** *":40}*

但是，由于 Gallia 支持模式，因此在实际处理任何数据之前，以下操作将会失败:

因为递增字符串(通常)是无意义的，初始的“元”阶段将返回一个错误，抱怨字符串不能递增。

注意事项:

*   *JSON* 因其作为对象符号无处不在而被用于所有示例，然而 Gallia 并不是 *JSON* 特定的数据序列化或内存表示(见 [JSON 缺陷](http://github.com/galliaproject/gallia-docs/blob/master/json.md)
*   这里实际上推断出了模式，这更简洁，但通常并不理想(参见[改为提供模式](https://github.com/galliaproject/gallia-core/blob/master/README.md#schema-metadata))

# 更复杂的例子

让我们来看一个更复杂的用法例子。您的老板比尔向您提供了以下电子表格(优雅地被甩为 TSV):

雇员

项目

问题

他希望您为公司( *Initech* )的每位员工创建一份报告，并基于以下模板对“W2K”和“W3K”进行项目设计:

# 代码和数据

您可以使用 Gallia 和以下代码实现上述结果:

这个例子的可运行代码可以在 github 上找到[。存储库还包含输入和输出数据，以及所有中间顶级模式/数据对的转储，这将有助于澄清任何不明确之处。例如，如果我们考虑上面的第 31 行(生成 *is_manager* 字段)，我们可以找到所有的中间体文件，如下所示:](https://github.com/galliaproject/gallia-medium-article)

*   **输入**模式:[。/data/intermediate/30 . meta . txt](https://github.com/galliaproject/gallia-medium-article/blob/master/data/intermediate/30.meta.txt)
*   **输入**数据:[。/data/intermediate/30 . data . JSON](https://github.com/galliaproject/gallia-medium-article/blob/master/data/intermediate/30.data.json)
*   **输出**模式:[。/data/intermediate/31 . meta . txt](https://github.com/galliaproject/gallia-medium-article/blob/master/data/intermediate/31.meta.txt)
*   **输出**数据:[。/data/intermediate/31 . data . JSON](https://github.com/galliaproject/gallia-medium-article/blob/master/data/intermediate/31.data.json)

请注意，这不是一个标准的 Gallia 特性，只是为了本文的方便而提供的。还要注意，每个步骤只提供第一个数据实体。

**EDIT:** 我还增加了一个[手动对应物](https://github.com/galliaproject/gallia-medium-article/blob/master/src/main/scala/galliamedium/initech/InitechManual.scala)，可以很容易的和 [Gallia 加工](https://github.com/galliaproject/gallia-medium-article/blob/master/src/main/scala/galliamedium/initech/InitechGallia.scala)相比。手动对应物专门使用标准库。

# 游戏攻略

上面的转换步骤尽可能不言自明。例如,`.remove(“Employee ID”)`就是这个意思:“雇员 ID”字段将不再存在。

相反，我们将在下面详述一些更复杂的操作。

## 嵌套操作

第一个嵌套操作很简单:

```
.nest(“first”, “middle”, “last”).under(“name”)
```

它基本上把一个实体:

*{****【第一个*** *”:【彼得】、* ***中间*** *:【罗恩】、* ***最后*** *:【长臂猿】、…}*

到…里面

*{****姓名****:{***【彼得】，* ***中间*** *【罗恩】，* ***最后*** *【长臂猿】}，… }**

*然而，第二个嵌套操作(有意地)更加复杂:*

```
*.renest{ **_.filterKeys(_.startsWith(“Address”))** }.usingSeparator(“ “)*
```

*它利用了两种机制:*

*   *通过谓词选择一个键的子集，即这里的`startsWith(“Address”)`*
*   *使用一个普通的分隔符重新嵌套，即这里的空格字符*

*目标选择集中在仅有的四个带有“地址”前缀的字段上。同时，重定机制使用提供的分隔符来重建隐含的嵌套:*

*因此我们从:*

```
*{
  ...
  "Address number":  9,
  "Address street": "Channel Street",
  "Address city"  : "Houston",
  "Address zip"   :  77001,
  ...
}*
```

*收件人:*

```
*{
  ...
  "Address": {
    "number":  9,
    "street": "Channel Street",
    "city"  : "Houston",
    "zip"   :  77001
  },
  ...
}*
```

*请注意，这是处理已被“展平”以适合矩形格式的数据的典型方式。*

*更多关于 Gallia 重新嵌套的细节可以在[这里](https://github.com/galliaproject/gallia-core/blob/master/README.md#renesting-tables)找到*

## *进行操作*

```
*issues.bring(projects, target = "Name", via = "Project ID" <~> "ID")*
```

*该语句可以用简单的英语理解为“通过匹配字段将字段*名称*从*项目*带入*问题*”。这里，我们必须显式地命名匹配的字段，因为它们的键不同(*"项目 ID"* vs *"ID"* )，否则它们可能会被猜到。*

*"*▲*"是一种特殊类型的左连接。当一个人只想用来自另一个数据源的几个字段来扩充数据时，可以方便地使用它。这与以下情况形成对比:*

*   *Gallia *join，*对应于同名的 *SQL* 操作，因此意味着潜在的反规范化。*
*   *A Gallia *co-group* ，对应于同名的 *Spark* 操作:没有反规格化，但是分组的边分别嵌套在 *_left* 和 *_right* 字段下。*

## *透视操作(嵌套)*

```
*.transformObjects(“issues”).using {
  _ .countBy(“Status”) // defaults to “_count”
    .pivot(_count).column(“Status”)
      .asNewKeys(“OPEN”, "IN_PROGRESS", “RESOLVED”) }*
```

*这里要注意的第一件事是，我们将转换应用于嵌套元素，而不是每个实体根处的数据元素。下划线在 Scala 中有特殊的含义，因此对应于嵌套在 *issues* 字段下的每个实体(在前面的 group-by Employee ID 操作中，参见[中间元数据](https://github.com/galliaproject/gallia-medium-article/blob/master/data/intermediate/50.meta.txt)和[数据](https://github.com/galliaproject/gallia-medium-article/blob/master/data/intermediate/50.data.json))。*

*从这里开始，对于所有这样的“发布”实体，我们按状态进行计数(*打开*、*进行中*或*已解决*)，然后对该计数进行透视。也就是说，如果这些是给定受让人的问题:*

*那么计数操作导致:*

*该支点导致:*

*应该注意的是，`.asNewKeys(“OPEN”, “IN_PROGRESS”, “RESOLVED”`部分是必需的，因为——非常重要 Gallia 在整个转换过程中维护一个模式。在目前的情况下，*状态*的值只能在查看整个数据集后**才能知道，因此必须提前提供。关于这个问题的更多细节也可以在文档的[模式](https://github.com/galliaproject/gallia-core/blob/master/README.md#schema-metadata)和【T21(DAG)】标题部分找到。***

# *未来文章*

*在以后的文章中，我想讨论使用替代技术实现相同结果的方法，如 *SQL、Spark sql、pandas、*等(与[这个想法](https://github.com/galliaproject/gallia-docs/blob/master/bounties.md)相关)*

*我还想讨论如何使用内置 Gallia 的 *Spark RDDs* 扩展上述转换(参见[文档](https://github.com/galliaproject/gallia-core/blob/master/README.md#spark))。上面的例子是有意忽略的，但是如果数据包含数十亿行，代码将不能像现在这样处理它。*

# *结论*

*对于现实生活中更复杂的例子，[参见这个](https://github.com/galliaproject/gallia-dbnsfp)知识库。另请参见[示例的完整列表](https://github.com/galliaproject/gallia-core/blob/master/README.md#examples)。*

*Gallia 的主要**优势**可以总结如下:*

*   *提供最常见/有用的数据操作，或者至少[预定](https://github.com/galliaproject/gallia-docs/blob/master/tasks.md)。*
*   *领域专家至少应该能够部分理解的可读 DSL。*
*   *扩展不是事后的想法，需要时可以利用*Spark rdd*。*
*   *元感知，意味着不一致的转换会尽可能地被拒绝(例如，不能使用已经被移除的字段)。*
*   *可以处理单个实体，而不仅仅是其集合；也就是说，没有必要创建一个实体的“虚拟”集合来操作那个实体。*
*   *可以以自然的方式处理任何多重性的嵌套实体。*
*   *宏[可用于](https://github.com/galliaproject/gallia-macros)与 case 类层次的平滑集成。*
*   *提供灵活的目标选择，即作用于哪个(哪些)字段，范围从显式引用到实际查询，包括涉及嵌套时。*
*   *执行 DAG 是充分抽象的，它的优化是一个很好分离的关注点(例如谓词下推、剪枝等)；但是请注意，目前很少有这样的优化。*

*Gallia 仍然是一个相当新的项目，因此我期待着它的任何反馈！主存储库可以在 github 上找到[，而](https://github.com/galliaproject/gallia-core)[父组织](https://github.com/galliaproject)包含了所有相关的存储库。*

*![](img/707ab08769a6b88387c240e4b25ec1a5.png)*

**根据 Anthony Cros 的许可，图片来自 LogoGround。**

# *脚注*

*[1]:如果提供了类之间的关系(例如通过*隐含*)，则不会出现这种情况，参见两个相关任务:*

*   *[提供关系( *t210426094707* )](https://github.com/galliaproject/gallia-docs/blob/master/tasks.md#t210426094707)*
*   *更一般的:[语义( *t210124100546* )](https://github.com/galliaproject/gallia-docs/blob/master/tasks.md#t210124100546)*

*还要注意的是，*带来* / *加入* / *协同组*对多个按键的操作还不可用:参见任务[多个按键( *t210304115501* )](https://github.com/galliaproject/gallia-docs/blob/master/tasks.md#t210304115501) 。解决方法是生成一个临时字段。*

*[2]: *左连接*和*左连接*功能相同，如果右侧没有多个匹配的话。*

*[3]:如果该字段被显式设置为一个*枚举，*就可以避免这种情况，因为我们使用了模式推断。Gallia 将来也可能提供“不透明物体”:参见相应的[任务( *t210202172304* )](https://github.com/galliaproject/gallia-docs/blob/master/tasks.md#t210202172304)*