# 使用 Google Sheets 和 Apps 脚本将多个 CSV 文件自动转换为 BigQuery 管道

> 原文：<https://towardsdatascience.com/automate-multiple-csv-to-bigquery-pipelines-with-google-sheets-apps-script-3ff05f535a09?source=collection_archive---------29----------------------->

## 将一些简单的数据放入 BigQuery，而不必使用大量工具或复杂的工作流

![](img/7da1463b6cff2694778bf80cd8b70cde.png)

[https://unsplash.com/photos/JKUTrJ4vK00](https://unsplash.com/photos/JKUTrJ4vK00)

想要将 CSV 文件的多个管道自动加载到不同的 BigQuery 项目/数据集/表，并从单个 Google Sheet 控制它们吗？什么事？那么这篇文章就送给你了。

这个过程甚至允许您配置是否希望自动化在每次向 BigQuery 表中截断或追加数据。不错吧？

# 背景

不久前，我[写了一个使用 Google Apps 脚本](https://www.techupover.com/automatically-load-csv-files-from-google-drive-into-bigquery-using-appscript/)将 CSV 文件加载到 BigQuery 的过程。这个过程在代码中使用了检测到的 CSV 文件类型的定义、BigQuery 的加载模式以及其他一些东西。我意识到对于入门者来说，这可能是多余的，需要他们自己写太多的代码。因此，我重写了这个过程，使之更加简单，使用一个 Google Sheet 作为管道管理器。

# 先决条件

*   谷歌账户
*   谷歌应用程序脚本—【script.google.com 
*   谷歌大查询项目、数据集、表—[console.cloud.google.com](https://console.cloud.google.com)
*   此[谷歌表单的副本](https://docs.google.com/spreadsheets/d/1OOs3bEff7f6KOdMwFqwXlAXPCyB13RZ8e6oHcexAMZE/copy)
*   要加载到 BigQuery 的 CSV 文件
*   来自[这个](https://github.com/usaussie/appscript-multiple-bigquery-pipelines-google-sheet)仓库的代码

# Google 工作表和驱动设置

首先，将 google sheet 复制到你自己的 google 账户中。然后创建一些 Google Drive 文件夹来存放您的 CSV 文件。

您将需要一个谷歌驱动器“源”文件夹为每一个不同类型的 CSV。如果你愿意，你可以有多个“已处理”文件夹，或者如果你希望所有的东西都在同一个地方，你可以有同一个“已处理”文件夹。这真的取决于你。

然后，您将在适当的列中用 Google Drive ID 更新 Google 工作表。在浏览器中使用 Google Drive 时，ID 位于 URL 的末尾。例如:如果 Google Drive 文件夹的 URL 是*https://Drive . Google . com/Drive/u/0/folders/1-zfa 8 svucirp xf J2 Cun 31 a*，那么 Drive 文件夹的 ID 就是“*1-zfa 8 svucirp xf J2 Cun 31 a*”，这就是你要放入 Google Sheet 的列中的内容。

# BigQuery 设置

对于要加载的每种 CSV 类型，您都需要一个 BigQuery 表。如果你有 4 种不同的 CSV 文件，那么你需要 4 个不同的表格。这些可以跨任意数量的 BigQuery 项目和数据集进行拆分，但是最终每个 CSV 类型都需要一个表。

如果您已经设置了 BigQuery 表，那么您需要做的就是用适当的信息(项目 ID、数据集 ID、表 ID)更新 Google 工作表

如果您需要创建一个新的 BigQuery 表，最简单的方法(无需编写任何代码)是通过 BigQuery 控制台上传一个 CSV，并使用 Autodetect 为您设置模式。然后，该类型 CSV 的每个后续加载将保证工作。

**上传 CSV 以创建 BigQuery 表:**

1.  【https://console.cloud.google.com/bigquery 号
2.  单击左侧导航栏上的项目名称
3.  单击 3 点菜单并创建一个新数据集
4.  点击数据集名称，然后点击*创建表*

当您的表被创建时，将它的 ID 与项目 ID 和数据集 ID 一起放入 Google Sheet。

# 创建您的应用程序脚本项目并配置代码

到目前为止，您应该已经有了一个 Google Sheet，其中填充了许多关于您的 Google Drive 文件夹、您的 BigQuery 项目/数据集/表的详细信息，现在剩下要做的就是将代码放入适当的位置，使它们一起工作。

在 Google 表单中，打开脚本编辑器。在撰写本文时，如果你使用的是个人 gmail.com 账户，这将通过**工具- >脚本编辑器**菜单项实现。如果您使用的是组织/学校帐户，这将通过**扩展- >应用程序脚本**菜单项实现。

这将打开应用程序脚本编辑器。

从这个 github 存储库中复制 code.gs 文件内容，并粘贴到现有的 Code.gs 文件中。

单击+图标添加一个新的 HTML 文件，并将其命名为“email ”,这样您的文件列表如下所示:

从同一个 github 存储库中复制 email.html 文件内容，并将其粘贴到 Apps 脚本中的 email.html 文件中。

使用左侧服务菜单旁边的+图标添加 BigQuery 和 Drive 服务(因此您的服务列表看起来也像上面的截图)。

打开 Code.gs 文件并更新文件顶部的变量。您可以在这里设置诸如工作表标签名称(如果您已经从原始模板中更改了它们)以及该流程也可以发送的电子邮件通知中的信息。

就是这样。这就是你需要做的所有设置。

# 让这东西跑起来，好吗？

*   将适当类型的 CSV 文件放入 Google 工作表指定的 Google Drive 文件夹中
*   在应用程序脚本中，单击顶部的运行按钮，运行列出的唯一函数
*   当你第一次运行这个程序时，谷歌会询问你是否确定，以一些安全/批准提示的形式。这是正常的…这是您授权访问您的应用程序脚本项目，以便能够使用您的 Google Drive、BigQuery 和 Gmail 内容/服务。这是不允许谷歌或其他任何人对你的帐户做事情。只要确保你不与其他人分享谷歌表单，他们可能会用它来做一些邪恶的事情(因为他们也可以看到和编辑这些代码)。
*   这些提示看起来有点像这样:
*   之后…脚本将第一次运行。
*   检查 Google Sheet 的日志选项卡，看看它是否做了什么。
*   现在，为了好玩，再放几个 CSV 到文件夹中，然后再次运行该函数。看…成功了！
*   接下来，您将希望设置一个触发器来按计划自动运行该功能。只需使用应用程序脚本项目左侧的触发器菜单来设置触发器，如下所示:

现在…真的是这样。简单吧？

您可以在 BigQuery 控制台中看到自动化作业的结果——在 BigQuery 中查看作业，然后查看数据集和表，您应该会很快看到表中的数据。如果你改变了这个网址中的 id…这就是你应该去查看的地方:[https://console.cloud.google.com/bigquery?project=id&page =乔布斯](https://console.cloud.google.com/bigquery?project=id&page=jobs)(这也在谷歌表单日志中)。

# 万岁！

希望这比[的原始文章/方法](https://www.techupover.com/automatically-load-csv-files-from-google-drive-into-bigquery-using-appscript/)更容易理解，并且不需要使用很多工具或复杂的工作流程就可以将一些简单的数据导入 BigQuery。

我知道这个过程可以通过从 CSV 文件中自动检测模式来改进，以创建初始表，并且应该有更多的错误处理…但是…这是为了后面的提交和拉请求:-)

下面是代码:[https://github . com/usaussie/app script-multiple-big query-pipelines-Google-sheet](https://github.com/usaussie/appscript-multiple-bigquery-pipelines-google-sheet)

这篇文章大概贴在[techupover.com](https://www.techupover.com/)和[techupover.medium.com](http://techupover.medium.com)上。也可以在 Twitter 上关注我 [@techupover](http://twitter.com/techupover)

*原载于 2021 年 10 月 28 日*[*【https://www.techupover.com】*](https://www.techupover.com/manage-multiple-bigquery-csv-pipelines/)*。*