# 增强 Git 工作流的重要命令

> 原文：<https://towardsdatascience.com/vital-commands-to-empower-your-git-workflow-7e37d5bedf54?source=collection_archive---------28----------------------->

## 一些 git 提示和技巧来提高您的贡献和协作能力

![](img/2501ba4328634b23c754646b926142c5.png)

照片由[扎克·赖纳](https://unsplash.com/@_zachreiner_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/branches?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

作为一名软件工程师、设计师、开发人员，或者你选择的其他身份，你的工具包中最重要的一项是版本控制系统，特别是 git。

作为开发人员，git 是我们的保险单，也是协作的强大工具。但是，重要的是，除了简单的提交或推送之外，我们很多人都不知道自己到底在做什么。

但是请不要担心，我将带您快速浏览一些您在现实世界中会遇到的常见场景，以及一些帮助您应对这些场景的简单解决方案。

# 观众

对于那些已经理解 git 作为工作流的一部分的重要性并对提高他们的生产力感兴趣的用户来说，这篇文章将是最有帮助的。如果你对使用 Git 的好处感兴趣，我强烈建议[为什么使用 Git 以及如何作为数据科学家使用 Git](/why-git-and-how-to-use-git-as-a-data-scientist-4fa2d3bdc197)。

# 背景

我在这里分享的方法是在以下假设下运行的，但也可能适用于其他情况。

1.  你和其他人一起工作。
2.  您在一个从“主”存储库中派生出来的存储库上工作，而其他人从他们的派生中贡献出来。
3.  **你是唯一一个使用叉子的人**。*(极其重要:其中一些方法将改写历史。如果你在共享回购中搞乱了历史，这对你的合作伙伴来说可能是一件痛苦的事情。)*

![](img/55bd50fada12f87c871fbdd5466b54f2.png)

[Yancy Min](https://unsplash.com/@yancymin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/github?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

# 如何下载私人回购？(Github)

> 方案
> 
> 你刚刚开始与一个新的组织合作，并且你很兴奋开始建立一些令人惊奇的东西。您尝试克隆存储库，但是哦，不——您遇到了一个小问题
> 
> `fatal: repository 'https://github.com/user/project.git/' not found`

那么，我们该如何度过这一关呢？

首先，您需要为 Github 设置一个个人访问令牌。您可以使用下面的链接找到相关说明。

[https://docs . github . com/en/authentic ation/keeping-your-account-and-data-secure/creating-a-personal-access-token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

现在您已经有了令牌，您可以运行以下命令来克隆 repo。

```
git clone https://<TOKEN>@github.com/<USERNAME>/<REPO>.git
```

准备好开始贡献吧！

# 如何使分叉回购中的分支与原始回购中的分支完全匹配？

> 方案
> 
> 假设我们有一个名为 **dev 的分支。**在这个场景中， **dev** 是主要的分支，贡献者在这里合并来自原始 *repo* 的变更。因此，您希望将分叉回购中的本地 **dev** 分支与原始回购中的 **dev** 分支上的最新变更同步。

好的，这是一个相当简单的方法，如果你在一个快速发展的团队中，你会发现自己每天都在使用它。

首先，您需要确保获取最新的更改。

```
git fetch upstream/dev
```

此后，您可以硬重置本地分支，以匹配上游分支的状态。

```
git reset --hard upstream/dev
```

完全匹配。

# 怎么取别人的 PR(拉取请求)？

> 方案
> 
> 您的开发伙伴对代码做了一个小而重要的更改，从他们的 repo 向主 repo 提交了一个 PR，并希望您帮助测试这些更改。所以你需要在你的机器上获得他们的代码，这样你就可以在本地测试它。

这是你怎么做的！

```
git fetch upstream pull/ID/head:#username
```

这里的 *ID* 是拉请求的 ID，而#username 是您试图获取其代码的开发人员的用户名。例如，假设您的好友 JaneSmith 希望您查看他们的 PR，该命令如下所示:

```
git fetch upstream pull/1234/head:#JaneSmith
```

附加信息:在 [*上面的命令中，“上游”*](https://stackoverflow.com/questions/9257533/what-is-the-difference-between-origin-and-upstream-on-github#:~:text=When%20a%20repo%20is%20cloned,repo%20it%20was%20forked%20from.&text=You%20will%20use%20upstream%20to,you%20want%20to%20contribute%20to).) 是指分叉回购发起的回购。

这将使分支机构，他们的公关是提供给你本地，所有你要做的下一步是检查他们的分支机构。

```
git checkout #JaneSmith
```

瞧啊。现在，您可以在本地测试它们的更改。

***更新(2022 年 10 月):*** *不必在分支机构名称前加标签，实际上名称是任意的，你可以随意命名。*

# 如何从分支的历史记录中删除特定的提交？

> 方案
> 
> 您刚刚修复了一个 bug，发布了代码，创建了 PR。您去检查您的变更，但是，您注意到其中有一些来自您以前工作的分支的代码没有被批准，您需要将它从这个分支中取出来。

这是你需要做的。

首先，您需要包含不需要的更改的提交散列。您可以使用下面的命令找到它。

```
git log
```

找到散列后，可以运行下面的命令将其从分支的历史记录中删除。

```
git rebase --onto <commit-id>^ <commit-id>
```

太好了，现在你已经摆脱了那些讨厌的不想要的变化！

接下来，由于您删除了提交，本地分支的历史记录将不再与远程分支的历史记录相匹配。要解决此问题，您必须强制推送以覆盖遥控器的历史记录。

```
git push -f
```

太好了，现在你的公关准备好了！

如果你做到了这一步，首先，感谢你的阅读。

作为开发人员，git 是一个至关重要的工具，它使我们能够以很小的摩擦进行协作，并轻松地修复我们破坏的东西。我分享了这些具体的情况，因为它们是我经常遇到的问题，我发现自己在谷歌上搜索。我发现这些简单的命令对我的工作效率是不可或缺的，我希望你也能把它们添加到你的工具箱中！