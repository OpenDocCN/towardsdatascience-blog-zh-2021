# 如何撤销 Git 中的最后一次本地提交

> 原文：<https://towardsdatascience.com/undo-last-local-commit-git-5410a18f527?source=collection_archive---------31----------------------->

## 了解如何在 Git 中撤销最近的本地提交

![](img/985e0d7fa62381b55020a07e5aa5615d.png)

由 [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/git?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

## 介绍

Git 是帮助开发人员、数据科学家和工程师编写代码版本的最强大的工具之一。使用 Git 时，一个非常常见的场景是当您意外提交文件时，您需要撤销最近的提交，以避免将这些文件中所做的更改推送到远程主机。

在今天的文章中，假设您没有将最后一次提交**推送到远程主机**，我们将讨论几个可能的选项。具体来说，我们将探索如何

*   撤消最近的提交，并**清除对提交的文件**所做的所有更改
*   撤销最后一次提交，并**保留修改文件中的更改**
*   撤销最近的提交并且**保留文件和索引中的更改**

## 撤消上次提交并放弃对文件所做的所有更改

有时，除了撤销最近的提交，您可能还想放弃对该特定提交中包含的文件所做的所有更改。在这种情况下，你需要做一个**硬复位**。

```
**git reset** **--hard HEAD~1**
```

*   `--hard`标志表示`git reset`命令将重置机头、索引和工作树。这意味着提交将被撤销，此外，对文件所做的更改也将被丢弃。
*   `HEAD~1`翻译为“使用第一个父代从`HEAD`返回 1 次提交”。通常一个提交只有一个父级，所以在大多数情况下这应该可以解决问题。注意`HEAD~1`相当于`HEAD~`。

## 撤消上次提交，但保留对文件所做的更改

或者，您可能仍然希望进行最近的提交，但同时在本地保留对文件所做的更改。在这种情况下，您只需在运行`git reset`命令时指定`HEAD~1`:

```
**git reset HEAD~1**
```

该命令将指示 Git 将指针`HEAD`向后移动一次。但是，对文件所做的更改不会受到影响。现在，如果您运行`git status`，您应该仍然能够在本地看到对文件所做的更改。

## 撤消上次提交并保留索引和本地文件更改

最后，您可能想要撤销最后一次提交，保留对文件的更改，同时**保留索引，**也是如此。

索引(也称为暂存区)是准备新提交的地方，它包含新提交中要包含的所有内容。例如，每次你运行`git add <filename>`的时候，一个文件被从工作的三个文件中添加(或更新)到索引中。实际上，索引是一个存储在`<repo>/.git/index`下的二进制文件，包含。

```
**git reset --soft HEAD~1**
```

*   `--soft`标志表示`git reset`命令只会重置机头，而索引和工作树保持不变。

现在，如果你运行`git status`，你应该能够看到相同的文件仍然在索引中。

## 最后的想法

在今天的简短指南中，我们探索了几种不同的方法来撤销 Git 中最近的本地提交。有时，您可能想要撤消最近的提交，并放弃对已修改文件所做的所有更改，而在其他情况下，您可能只想撤消最近的提交，但同时保留更改。

但是请注意，只有当您尚未将提交推送到远程主机时，我们在本文中讨论的选项才适用。关于`git reset`命令的更多细节，您可以在命令行中运行`git reset -h`来显示使用说明。

[**成为会员**](https://gmyrianthous.medium.com/membership) **阅读介质上的每一个故事。你的会员费直接支持我和你看的其他作家。你也可以在媒体上看到所有的故事。**

<https://gmyrianthous.medium.com/membership>  

**你可能也会喜欢**

<https://betterprogramming.pub/how-to-merge-other-git-branches-into-your-own-2fe69f70a2b4>  </dynamic-typing-in-python-307f7c22b24e>  <https://betterprogramming.pub/11-python-one-liners-for-everyday-programming-f346a0a73f39> 