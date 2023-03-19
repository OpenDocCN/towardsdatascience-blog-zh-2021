# SQL —在一个查询中更新选择

> 原文：<https://towardsdatascience.com/sql-update-select-in-one-query-b067a7e60136?source=collection_archive---------12----------------------->

## 选择一批记录，同时更新它们

![](img/31dd1c1cda1a5dec3f38bcd6efa005ba.png)

如何记录你选择了哪些记录？(图片由[像素](https://www.pexels.com/photo/white-sheep-on-farm-693776/)上的[凯拉什·库马尔](https://www.pexels.com/@kailashkumarphotography)拍摄)

本文将向您展示如何在一个查询中同时更新和**选择**记录。这种技术对于处理具有状态的成批记录特别实用，因为它确保您只选择每条记录一次。更新选择的优势包括:

*   Atomic:该语句要么成功执行 select 和 update，要么不执行任何操作。更新未处理的记录或选择具有错误状态的记录没有变化。
*   性能:您的数据库只需要执行一个操作，而不是多个独立的操作
*   冷静:用这个令人敬畏的查询给别人留下深刻印象

我们将通过一个实际例子来展示这个查询的强大功能。首先，我们将设置一些表，并用数据填充它们。

# 0.目标

你经营一家公司，允许你的用户写漫画并提交给出版物(例如杂志或报纸)。在漫画出版之前，出版商必须同意该漫画。因为他们是非常忙碌的人，一部漫画可能需要一段时间才能被批准。

我们决定在一个名为 ComicStatus 的表格中记录漫画的进度。该表包含漫画名称、出版物和漫画的状态。状态反映了漫画在处理、提交和被出版接受或拒绝的过程中的状态。让我们摆好桌子。

创建包含所有数据的 ComicStatus 表

每隔几分钟，我们就会运行一个执行以下操作的流程:

*   获取所有新漫画并提交
*   拿着提交的漫画，检查出版社是否已经处理了它们
*   如果漫画已处理:将状态更新为“已接受”或“已拒绝”

现在让我们看看 select-update 如何帮助我们改进流程。

# 2.更新状态时批量选择记录

出于开发目的，我们已经决定将这个过程限制在一次最多 5 幅漫画。我们将从处理这个问题的最明显的方法开始，然后慢慢地改进它。

![](img/656ad8a6a40ead2c0d7c80ff6d5250f5.png)

我们将所有记录集中在一批中(图片由 [Steven Lasry](https://unsplash.com/@stevenlasry) 在 [Unsplash](https://unsplash.com/photos/-DGfm-1JJ-k) 上提供)

## 1 .愚蠢的方式

假设我们以如下方式每分钟运行我们的流程:

1.  **选择**前 5 条记录新记录(状态为 0)
2.  遍历所有记录，并将每一条记录提交给他们的发布
3.  等待出版 API 批准漫画
4.  将漫画的状态更新为已接受或已拒绝

由于我们的流程每分钟都在运行，而批准漫画可能需要一分钟以上的时间，因此我们选择同一记录两次。这可以很容易地通过快速更新状态来解决。

## 2.更好的方法

在提交记录之前，我们会更新所选记录的状态

1.  **选择**前 5 条记录新记录(状态为 0)
2.  **将所选记录的**前 5 名更新为状态 1(处理中)
3.  遍历所有记录，并将每一条记录提交给他们的发布
4.  等待发布 API 返回结果
5.  将漫画的状态更新为已接受或已拒绝

这种方式仍然会产生错误。如果在第 1 步和第 2 步之间有新的记录插入到我们的表中会怎样？如果是这种情况，我们更新未处理的记录并错过更新已处理的记录。

## 3.更新-选择方式

我们选择我们想要的记录，并在同一个查询中更新它们的状态。

1.  **更新**前 5 条记录新记录(状态为 0)**选择**它们
2.  遍历所有记录，并将每一条记录提交给他们的发布
3.  等待发布 API 返回结果
4.  将漫画的状态更新为已接受或已拒绝

这样可以确保我们不会多次处理记录。我们的流程运行多长时间以及何时插入新记录都无关紧要。如何执行步骤 1？比你想象的容易:

就是这样！很短吧？我们首先更新状态为 0 的前 2 条记录，然后输出它们。该输出像常规选择一样工作。尝试连续多次执行此查询；您会注意到，您永远不会选择同一个记录两次，因为它首先更新状态。它将对我们的记录进行批处理，直到不再有状态为 0 的记录。

# 结论

使用这个 Update-Select 确保我们只更新我们选择处理的记录，并保证要么两者都成功，要么整个操作失败。它是你工具箱中的一个很棒的新工具，就像这些一样:

*   [删除到另一个表中](https://mikehuls.medium.com/sql-delete-into-another-table-b5b946a42299)
*   [更新到另一个标签页](https://mikehuls.medium.com/sql-update-into-another-table-bfc3dff79a66) le
*   [在一条语句中插入、删除和更新](https://mikehuls.medium.com/sql-insert-delete-and-update-in-one-statement-sync-your-tables-with-merge-14814215d32c)
*   [使用事务回滚查询](https://mikehuls.medium.com/sql-rolling-back-statements-with-transactions-81937811e7a7)
*   [版本控制你的数据库](https://mikehuls.medium.com/version-control-your-database-part-1-creating-migrations-and-seeding-992d86c90170)
*   [Docker 适合绝对初学者](https://mikehuls.medium.com/docker-for-absolute-beginners-what-is-docker-and-how-to-use-it-examples-3d3b11efd830)

编码快乐！

—迈克

页（page 的缩写）学生:比如我正在做的事情？[跟我来](https://mikehuls.medium.com/)！