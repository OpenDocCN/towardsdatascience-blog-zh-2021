# 如何使团队的 Power BI 开发更容易

> 原文：<https://towardsdatascience.com/how-to-make-power-bi-development-for-teams-easier-c58d388b5e73?source=collection_archive---------28----------------------->

## 当你试图在一个团队中开发 Power BI 解决方案时，你会遇到同步变化的重大问题。让我们看看这个问题可能的解决方案是什么。

![](img/6410988e2f122eb10390447e157b125d.png)

由[马文·迈耶](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

我们都知道 Power BI 是一个单片软件，它创建一个包含解决方案所有数据的文件。这使得团队中的开发工作非常困难。

通常，你开始在一个文件中记录变更，有人负责将所有变更整合到一个“主文件”中。

这种方法效率不高，并且容易出错。

ALM 工具包是一个免费的工具，它承诺解决这些问题中的一些，如果不是全部的话。
它是一个独立的工具，用于在表格模型(Power BI 或 SSAS 模型)之间同步数据模型。

另一方面， [BISM 规格化器](http://bism-normalizer.com/)是一个免费的 Visual Studio 扩展，因此只能用于 Analysis Services 数据模型。

让我们看看这些工具以及如何使用它们。

# 面向 Power BI 的 ALM 工具包

用于 Power BI 的 ALM 工具包可以比较和同步表格模型。

表格模型可以是:

*   在 Power BI Desktop 中打开的 Power BI 文件
*   Power BI 服务中发布的数据集
*   model.bim 文件形式的表格模型
*   分析服务的数据模型

当您启动 ALM 工具包时，您必须选择源和目标。

当您在 Power BI Desktop 实例中打开两个 Power BI 文件时，您可以在下拉选择器中选择它们:

![](img/eb1e9378aa631f08f72dfec0694a6fe9.png)

图 1 —选择两个 Power BI 文件进行比较(图由作者提供)

点击 OK 按钮后，两个模型的比较开始。

一旦两个模型的比较完成，您就会收到所有对象的列表以及源模型和目标模型之间的差异。

当您从 Visual Studio 或表格编辑器中比较兼容级别高于 Analysis Services 服务器上的目标数据模型的 model.bim 文件时，系统会提示您将目标数据库的兼容级别升级到与源模型相同的级别:

![](img/ef7b474f5e80a9acd65c83040abbd82a.png)

图 2 —与 SSAS 目标比较时兼容性级别的升级(作者提供的图)

现在，您可以通过单击鼠标右键来选择不同之处，并选择工具必须执行的操作:

![](img/7b8fb6dbd556574eb70a58b765854c89.png)

图 3 —差异和要执行的操作(作者提供的图)

如上图所示，您可以看到每个对象的不同之处。

要仅显示有差异的对象，您必须单击功能区中的“选择操作”按钮，然后选择“隐藏跳过具有相同定义的对象”。

如下图所示，ALM 工具包在模型比较过程中考虑了注释、格式字符串和关系:

![](img/7de97a561fc19892792343944f1d90ad.png)

图 4 —现有对象的差异(按作者分类)

一旦您为每个变更的物件标记了动作，如果更新可行，您就可以验证您的选择:

![](img/05a6b44a6ab7c58fcee94d1f431d6574.png)

图 5 —验证结果(作者提供的图)

正如您在上面的图片中所看到的，该工具不能将所有的更改应用到目标模型。

以下更改不能应用于 Power BI 中的目标模型:

*   计算列
*   关系
*   新表
    ,因为这需要在超级查询中添加一个表

当您单击“应用”按钮时，该工具会自动跳过此类更改。

将 model.bim 文件与 SSAS 上的数据模型进行比较时，可以更新计算列和关系。

使用 Power BI，您可以将更改应用到以下对象:

*   措施
*   计算组

不幸的是，我们不能使用这个工具来缩放我们的模型到 SSAS。

当我尝试将 Power BI 文件与 SSAS 模型文件进行比较时，我得到以下错误信息:

![](img/90dd7012db95c7d5abb4ef3ce3808d75.png)

图 6 —与 SSAS 模型比较时的错误消息(作者提供的图片)

然后，我试图将 PBI 桌面上的一份 Power BI 报告与一份已发布的 PBI 服务报告进行比较。

事实证明，ALM 工具包访问 XMLA 端点，只有当您拥有每用户高级许可证或高级容量时，XMLA 端点才可用。您需要在激活高级功能的工作空间上发布报表。

当您有这些可能性之一时，您可以连接到您的工作区和报表:

![](img/a99ff9673dbc13d001bd5a59c1bd4ed1.png)

图 7 —连接到 PowerBI 服务(图由作者提供)

您可以通过以下 URL 访问您的工作区:power bi://API . power bi . com/v 1.0/my org/<workspace-name>。</workspace-name>

只要您输入 URL，该工具就会要求您提供 Power BI 凭据。

登录后，您可以从下拉列表中选择报告。

其余的过程与上面的例子相同。

# BISM 规格化器

BISM 规格化器是 Visual Studio for Analysis Services 的免费扩展。

该工具覆盖了与 ALM 工具包相同的特性，并且可以以相同的方式使用。唯一的区别是 BISM 规格化器不是一个独立的工具，就像 ALM 工具包一样，而且 GUI 略有不同。

![](img/c599c8c7428a066797ffbf63bccbc224.png)

图 8——BISM 规格化器(红色)和 ALM 工具包(蓝色)之间的比较(图由作者提供)

像 ALM 工具包一样，可以比较日期模型，验证变更，为差异创建 Excel 报告，直接部署变更，或者为变更的部署生成修改脚本。

您可以在本文末尾找到 BISM 规格化器的文档。

但是，BISM 规格化器有一些很酷的特性，可以帮助开发过程。

# 显色法

正如本文开头所描述的，当多个开发人员在一个表格模型上工作时，我们面临着巨大的挑战。无论您在 Visual Studio 中使用表格编辑器还是 Power BI Desktop 修改数据模型，都没有什么不同。

上面显示的两种工具都可以帮助您的团队解决这些挑战。

当一个开发人员完成了模型中的一个新特性时，他可以开始比较并更新一个目标模型，包括开发管道下一阶段的更新。

因为您可以有选择地更新变更，所以您不需要一直部署所有的变更。

这种可能性可以简化你的方法，节省大量时间。

另一种方法是使用分析服务服务器来支持开发人员。

想象以下场景:

Power BI 数据集中有一个复杂的数据模型。多个开发人员正在对数据模型进行修改。并且您希望随时记录您的更改。

现在，您可以使用 Analysis Services 作为桥梁来并行开发和测试多个更改。

不幸的是，如果不对 model.bim 文件进行许多修改，就不可能将 Power BI 数据模型直接迁移到 Analysis Services。
但是，当您从一开始就使用 Analysis Services 时，您可以使用下面描述的流程来支持您的团队:

1.  开发人员在 Visual Studio 中打开数据模型(model.bim 文件)的副本
2.  他更改模型，并将修改后的数据模型部署到服务器，成为新的数据模型
3.  测试人员可以使用 Excel 或 Power BI Desktop 来测试新特性
4.  现在，您可以使用 BISM 规范器来更新 Power BI Desktop 中的目标数据模型
    您需要从 DAX Studio 获得正确的端口，以便从 BISM 规范器连接到 Power BI Desktop:

![](img/edd0544fbb595c266df6e38bf2e7120b.png)

图 9 —将 BISM 归一化器连接到 Power BI Desktop)(作者提供的图片)

但是，为什么要用分析服务来管理 Power BI 的变化呢？

当您使用 Visual Studio 更新您的 Analysis Services 数据模型时，您可以保存 BISM 规范化器比较的结果，以便进行文档记录和版本控制。

正如您在 BISM 规范化器用户指南中所看到的，您可以将保存的比较文件添加到 Analysis Services 项目中。

当使用 ALM 工具包时，这种方法是不可能的。

如果您有 CI/CD 过程，那么您可以使用 BISMNormalizer.exe 来实现对您的数据模型的变更的自动部署。

不管您如何改变表格模型的开发过程，这两个工具都是对您的工具箱的一个有价值的补充，并且没有额外的成本。

# 结论

这两个工具都能以非常可靠的方式同步表格数据模型。

但是，您需要改变您的开发过程以及开发人员使用开发工具的方式，以便找到最有效的方式来使用您所掌握的工具。

据我所知，没有解决方案来同步电力查询代码或您的报告中的可视化。

当您想要在两个 Power BI 文件之间同步您的 Power Query 工作时，您必须手动将 M 代码从一个 Power BI 文件复制到另一个。

有趣的是，有一种方法可以在不打开 Power Query 的情况下从报告中提取整个 M 代码:

1.  转到帮助功能区
2.  点击关于
3.  点击复制按钮:

![](img/cb682ddd36df7ec579791869c76f8687.png)

图 10 —从 About 复制超级查询代码(作者提供的图)

当您将内容粘贴到文本编辑器时，您可以从所有表中获得完整的 M 代码。

为了观想；只要您有相同的数据模型，您就能够在 Power BI 报告之间复制/粘贴可视化效果。但是你也必须手动操作。

![](img/41eba769e5522235e6db88e0ba085952.png)

托尔加·乌尔坎在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 参考

这里是用于分析服务的模型比较和合并的 BISM 规范化器的用户指南的 URL:【Services.pdf 分析的 模型比较和合并

ALM 工具包网页的 URL:[https://www.sqlbi.com/tools/alm-toolkit/](https://www.sqlbi.com/tools/alm-toolkit/)

[2021 年表格模型的开发工具](https://www.sqlbi.com/articles/development-tools-for-tabular-models-in-2021/)关于 SQLBI 的文章

[](https://medium.com/@salvatorecagliari/membership) [## 通过我的推荐链接加入 Medium-Salvatore Cagliari

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

medium.com](https://medium.com/@salvatorecagliari/membership)