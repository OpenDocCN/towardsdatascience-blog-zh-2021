# 如何以编程方式从 PubChem 获取化学数据

> 原文：<https://towardsdatascience.com/how-to-programmatically-obtain-chemical-data-from-pubchem-59762810eb1?source=collection_archive---------14----------------------->

## PUG-REST 与 PUG-VIEW:简介

你有没有想过在互联网不存在的时候，科学家们是如何交流的？我也没有想过这个问题，直到我从一位和我一起工作的教授那里听到这个故事。显然，有机化学家过去常常制造新的分子，但并没有努力告诉任何人。那时候，科学家的跨学科程度比现在低得多。因此，他偶然遇到了一个想制造特殊分子的生物化学家。这也是偶然的，这位教授制造了这种类型的分子。

现在，制药公司有意花费数十亿美元[1]来制造和测试寻找新药的化学物质。自然地，机器学习被用在这个过程的每一步[2]。当然，没有大量的好数据，机器学习方法是没有用的。这就是世界上最大的免费化学数据库 PubChem 的用武之地。在本教程中，我想回顾一下以编程方式获取 PubChem 数据的方法，以防您想尝试一下。

![](img/cc63bfa75ce050fbbcb286a1319415dc.png)

照片由[卡琳·海斯留斯](https://unsplash.com/@silverkakan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

有两种方法可以通过编程从 PubChem 获取数据:PUG-REST [3]和 PUG-View [4]。选择哪一个取决于您想要的数据类型。下面是如何使用这两个服务的简短介绍。请参考底部的资源，以获得这些服务所能做的更详尽的列表。

## **目录:**

*   [泥料架](#6a81)
*   [PUG-View](#ef16)
*   [有什么区别？](#3e9b)
*   [使用政策](#07a3)
*   [请求&美汤](#8a41)

# 帕格托

PUG-REST 提供了一种通过 URL 访问数据的方法，它看起来像这样(<prolog>是“https://pub chem . NCBI . NLM . NIH . gov/REST/PUG”):</prolog>

> <prolog>/  / <operation> /</operation></prolog>

您使用 *<输入>* 来告诉服务要寻找哪种化合物或物质。例如，如果我想查找阿司匹林，我可以像这样构造*<prolog>/<input spec>*:

> [<序言>/化合物/名称/阿司匹林](https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/aspirin)

这里， *<输入>* 被分解成 3 个部分。首先，我告诉服务记录的类型，即“复合”然后，我想按“姓名”进行搜索这个名字就是“阿司匹林”这个链接给了我一个 XML 文件，其中包含了 PUG-REST 关于阿司匹林的所有信息。假设我只想要阿司匹林的分子量。我可以这样做:

> [<序言>/化合物/名称/阿司匹林/性质/分子量](https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/aspirin/property/MolecularWeight)

这里的<operation>是研究阿司匹林的“属性”并检索“分子量”然后，假设我不喜欢 XML，我想用 JSON 来代替它:</operation>

> <prolog>[/化合物/名称/阿司匹林/属性/分子量/json](https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/aspirin/property/MolecularWeight/json)</prolog>

# PUG-VIEW

通过 PUG-View 检索数据的方式与 PUG-REST 非常相似。在本例中，序言是“https://pub chem . NCBI . NLM . NIH . gov/rest/pug _ view”。假设我想知道帕格-维尤对阿司匹林有什么看法:

> [<序言>/数据/复合/2244](https://pubchem.ncbi.nlm.nih.gov/rest/pug_view/data/compound/2244)

在这里，2244 是阿司匹林的 CID，代表化合物 ID。这个链接给了我 PUG-View 所有关于阿司匹林的信息。为了缩小范围，我这样做

> [<序言>/数据/复合/2244？标题=熔点](https://pubchem.ncbi.nlm.nih.gov/rest/pug_view/data/compound/2244?heading=Melting-Point)

询问阿司匹林的熔点。如果你看看这个链接，你会发现它不仅仅是一个数字，而是多个数字和注释。这就引出了我的下一点。

# 有什么区别？

首先，PUG-REST 检索由 PubChem 计算的信息，而 PUG-View 为您提供从其他来源收集的信息以及注释。这意味着如果你想要实验数据，你应该首先查看 PUG-View。有关允许标题的列表，请参见[5]。如果你点击“化学和物理属性”，然后点击“实验属性”，你会看到“熔点。”因此，使用上面的模板，你可以获得这个列表中关于阿司匹林(和其他化合物)的其他类型的信息，如沸点或密度。

第二，我没有使用“阿司匹林”这个名字在 PUG-View 中查找它的信息。这是因为，与 PUG-REST 不同，PUG-View 只将任务 CID 作为其输入。关于标识符的完整列表，您可以使用 PUG-REST，参见[3]。

第三，使用 PUG-REST，您可以检索多个标识符的信息。例如，我可以获得 cid 为 1 和 2 的化合物的信息，如下所示

> [https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/cid/1,2](https://pubchem.ncbi.nlm.nih.gov/rest/pug/compound/cid/1,2)

另一方面，PUG-View 一次只允许一个标识符。

# 使用策略

PUG-REST 和 PUG-View 都不是为大量(数百万)请求设计的。PubChem 有以下限制:

*   每秒不超过 5 个请求。
*   每分钟不超过 400 个请求。
*   每分钟运行时间不超过 300 秒。

如果你的要求超过了这些限制，你可以联系他们来解决。

# 请求&美味的汤

这里有一个关于如何与 PubChem 互动的 Google Colab 笔记本。我不能保证它对所有化学品都有效。我只在阿司匹林上试过。希望这有所帮助！

关于如何与 PubChem 互动的 Google Colab 笔记本

# 我的更多文章

</what-has-machine-learning-been-up-to-in-healthcare-a137db866f05>  </a-checklist-of-basic-statistics-24b1d671d52>  </6-common-metrics-for-your-next-regression-project-4667cbc534a7>  

# 来源

[1][https://www . biopharmadive . com/news/new-drug-cost-research-development-market-JAMA-study/573381/](https://www.biopharmadive.com/news/new-drug-cost-research-development-market-jama-study/573381/)

[2]https://www.nature.com/articles/s41573-019-0024-5

[3]https://pubchemdocs.ncbi.nlm.nih.gov/pug-rest

[https://pubchemdocs.ncbi.nlm.nih.gov/pug-view](https://pubchemdocs.ncbi.nlm.nih.gov/pug-view)

[https://pubchem.ncbi.nlm.nih.gov/classification/#hid=72](https://pubchem.ncbi.nlm.nih.gov/classification/#hid=72)