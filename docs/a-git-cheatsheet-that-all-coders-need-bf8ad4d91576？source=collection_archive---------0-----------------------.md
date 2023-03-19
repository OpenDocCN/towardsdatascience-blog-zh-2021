# 所有编码人员都需要的 Git cheatsheet

> 原文：<https://towardsdatascience.com/a-git-cheatsheet-that-all-coders-need-bf8ad4d91576?source=collection_archive---------0----------------------->

## 曾经意外删除文件或必要的代码？或者您希望回顾一下您的代码的旧版本？

![](img/82090903b5007b6c7b5ac7b083c52492.png)

克里斯蒂娜·朗普夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

我对 Git 有一种正常的爱恨交加的关系，这意味着当只有我一个人在项目中工作时，我喜欢 Git，而当我不得不将自己的代码与其他人的代码合并时，我讨厌它。但是所有的婚姻都有起有落，对吧。事实是，作为一名程序员，在某些时候，你会需要 Git，一旦你开始使用它，你就不会停止，即使它有时会令人沮丧。所以这里有一个备忘单，当你很久以后重新访问 Git，或者想学习 Git 的更多应用，或者如果你需要时间旅行并取回旧代码时，你可以使用它。

警告:Git 无法显示您的未来代码；你必须做这件事😜。

在我们开始之前，让我解释一下 Git 的结构，这样您可以更好地理解这些命令。

*   存储库(简称 repos):它们是一个项目的不同版本的文件的集合。
*   远程存储库:远程/在线存储的当前存储库。因此，我们在 Github 或 Gitlab 网站上看到的回购是这些项目的远程回购。它们包含了每个人做出和推动的改变。
*   本地存储库:存储在本地设备上的当前存储库。它包含您所做的更改，也可以包含远程 repo 上的更改。
*   提交:它们本质上代表了代码库的版本。每次提交都包含有关回购最后状态的更改。
*   分支:一个分支代表一个独立的开发路线。当我们创建一个分支时，我们可以说是创建了一个全新的工作目录、临时区域和项目历史。新的提交记录在当前分支的历史中。

我们将在前面了解更多，所以让我们开始吧。

由[吉菲](https://giphy.com/)

## 1) git 配置

让我们从为 Git 设置环境开始。在您的终端上运行这些命令来设置 Git 的全局配置(对于所有未来的存储库)。

```
$ git config --global user.name "John Doe"
$ git config --global user.email "johndoe@email.com"
```

该命令的另一个有趣用途是设置别名。我们在下面的命令中为命令*git*commit 设置了一个别名，所以现在 *git co* 将实际运行 *git commit* 。这对于有很多标志的较长命令很有帮助。

```
$ git config --**global** **alias**.co commit
```

## 2) git 初始化

接下来，我们需要将一个文件夹初始化为 git 存储库。这个命令实际上在你的文件夹中创建了一个. git 隐藏文件夹。这个文件夹表示它是一个 git repo，存储 git 所需的元数据。

```
//Run the following command inside the folder
$ git init//Run the following command to create a new directory that is a git repo
$ git init DIRECTORY_PATH/DIRECTORY_NAME
```

## 3) git 克隆

如果您想要使用一个已经存在的 git repo(远程 repo)，您需要首先在您的本地设备上创建它的一个副本(本地 repo)。为此，我们使用克隆命令。首先，复制该存储库的克隆链接(这通常出现在存储远程存储库的位置)。

这个链接看起来会像这样:[https://github.com/harsh-99/SCL.git](https://github.com/harsh-99/SCL.git)。有了它之后，在您希望下载该存储库的文件夹中运行下面的命令。

```
$ git clone LINK
$ git clone [https://github.com/anveenaik99/onboardScripts.git](https://github.com/anveenaik99/onboardScripts.git)
```

## 4)获取 git

假设多人正在编辑您项目的远程存储库，这意味着你们都在协作编码，所以他们的更改对您来说是必不可少的。为了在需要时下载他们的更改，您需要您的本地 repo 了解这些更改，也就是说，您需要获取这些更改。

因此命令 *git fetch* 下载远程存储库的详细信息和设备上的更改。按如下方式使用它:

```
//download the state for all the branches
$ git fetch //fetch for just one branch
$ git fetch <remote> <local> 
//<remote> is the name of the remote branch
//<local> is the name of the local branch//an example of it is 
$ git fetch origin master
```

## 5) git pull

*git pull* 是两个命令的混合 *git fetch + git merge* 。当我们之前使用 Git fetch 时，它首先将远程存储库的当前状态下载到我们的本地设备上。但是我们的文件还没有改变。为了对我们的文件进行修改，我们需要 git merge，它基于远程版本更新我们的本地文件。

假设我有一个名为 test.py 的文件，在我的本地存储库中，它看起来像这样。

而文件的远程版本如下

我们希望带来这些变化，并把它们与我们的变化融合在一起。当更改彼此不冲突时，合并会顺利进行。但是通常情况下，我们会对别人也修改过的同一段代码进行修改。这就是 Git 抛出合并冲突的时候。至少可以说，我被这个词吓坏了，但我意识到这是有史以来最简单的事情来解决。让我们看看在前面的例子中是如何做到的。

有两种可能的方法来合并同一文件的上述版本。这些可能性如下:

选项 1

选项 2

选择哪个版本完全取决于您，取决于您想要实现的目标，但是让我向您展示如何锁定您的选择。当引发合并冲突时，它看起来像这样:

第 1、5 和 8 行表示您的文件有合并冲突，要消除冲突，您需要做的是选择第一个版本(第 2–4 行)、第二个版本(第 6–7 行)或两者的组合。然后删除第 1、5 和 8 行，继续您正在做的事情。

## 6) git 状态

现在运行这么多命令，我们显然想验证事情是否按计划进行。好的一面是 Git 允许我们随时检查状态。

```
$ git status //a sample output of this command is as follows
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your **local** commits)

Untracked files:
  (use "git add <file>..." to include **in** what will be committed)

	README.txt
	lab1
```

以上建议的命令都包含在这个博客中，所以为了更好的理解，可以跳到其中任何一个。

## 7) git 添加

我们的下一步是告诉 Git 我们希望它记住的变化。因此，对于每次提交，我们使用命令 git add 将更改添加到我们希望在提交中反映的文件夹中。

假设我在名为 GitTutorial 的 git repo 中的 test.py 文件中声明了一个新变量。我想通知 Git 这个更改，并让它保存这个更改。然后我将使用如下的 *Git add* 命令:

```
$ git add /path-to-test-py/test.py
```

现在，当您运行 *git status* 命令时，您会看到这个文件名用绿色书写，因为 git 对这个文件进行了更新。

如果你需要删除一个文件或文件夹，你可以使用 [git rm](https://www.atlassian.com/git/tutorials/undoing-changes/git-rm#:~:text=git%20rm%20is%20used%20to,event%20to%20the%20staging%20index.) 命令。

## 8) git 提交

现在我们已经添加或删除了需要通知 Git 的更改，我们提交这些更改。这在某种程度上最终确定了我们代码库的下一个版本。我们可以回到所有过去的提交来查看版本历史。该命令的工作方式如下。

```
$ git commit -m "The message you want to write to describe this commit"
```

-m 标志有助于编写描述提交的消息。

## 9) git 推送

到目前为止，我们所做的一切都发生在我们的本地存储库，但在某些时候，我们需要把它推到远程存储库，以便其他人可以看到和使用我们的代码。 *git push* 命令可以做到这一点。叫它如下:

```
$ git push <remote> <local> 
//<remote> is the name of the remote branch
//<local> is the name of the local branch//an example of it is
$ git push origin master
```

上述命令将我们的本地提交推送到主分支。

## 10) git 日志

当然，在执行多次提交之后，我们实际上想要看看代码是如何发展的。正如我们将在前面了解到的，也有可能许多人提交到他们的分支，并且在某些时候可能想要将他们的分支与不同的分支合并。我们的 repo 中已经完成的所有此类操作都可以使用 *git log* 命令来访问，如下所示:

```
$ git log --graph --oneline --decorate
//a sample output
*   0e25143 (HEAD, main) Merge branch 'feature'
|\  
| * 16b36c6 Fix a bug in the new feature
| * 23ad9ad Start a new feature
* | ad8621a Fix a critical security issue
|/  
* 400e4b7 Fix typos in the documentation
* 160e224 Add the initial code base
```

这里，我们在每行开头看到的字母数字代码代表每次提交，如果我们想要恢复或执行其他功能，将会使用这些代码。

另请参见 git shortlog。

## 11) git 还原

我们来到 Git 的一部分，当我们出错并想回到以前的代码时，我们将需要它。 *git revert* 可以被描述为撤销按钮，但是很聪明。它不只是及时返回，而是将过去的更改带入下一次提交，这样不需要的更改仍然是版本历史的一部分。

对于 *git revert，*我们需要之前在日志中看到的提交代码。

```
$ git log --oneline
86bb32e prepend content to demo file
3602d88 add new content to demo file
299b15f initial commit$ git reset --hard c14809fa
//this command will not changes files that you have not git added 
```

还有许多其他的方法可以恢复，所以，如果你需要的话，一定要检查一下。

## 12) git 分行

该命令允许我们创建、列出、重命名和删除分支。我们来看几个例子。

```
//this lists the name of the branches present
$ git branch 
main 
another_branch 
feature_inprogress_branch//delete a branch safely
$ git branch -d <branch>
$ git branch -d another_branch
```

## 13) git 结帐

*git checkout* 命令允许您在由 *git 分支*创建的分支之间导航。

```
//switch to a different branch
$ git checkout <branch_name>
$ git checkout another_branch//create a new branch
$ git checkout -b <new_branch_name>
$ git checkout -b new_feature_branch
```

## 14) git 差异

有时我们需要比较不同版本或者不同分支之间的代码；这就是我们使用 *git diff 的时候。*

```
//print any uncommitted changes since the last commit.
$ git diff//compare code between two branches
$ git diff branch1 branch2//print the uncommitted changes made in one file
$ git diff /filepath/filename
```

## 15) git rebase

到了我们的最后一个命令，也是我最害怕的一个命令:p。有时我们需要合并代码，或者将代码从主服务器拉到我们的分支服务器，或者许多其他情况。

告诉你一个要点:

Rebase 是专门用于将一个分支的变更集成到另一个分支的两个 Git 工具之一。另一个变更集成实用程序是`git merge`。合并总是一个向前移动的变更记录。或者，rebase 具有强大的历史重写功能。

让我们看看 git rebase 做了什么。

```
 B -- C (another_branch)
   /      
  A--------D (master)Rebasing another_branch onto master

             B -- C (another_branch)
            /      
  A--------D (master)
```

相应的代码

```
$ git pull origin master
$ git checkout another_branch
$ git rebase master
```

重新定基和合并可能是最复杂的命令，因为您必须解决大量的合并冲突才能恢复代码的顺序。但是它们对于你坚持基于版本控制的环境是至关重要的。我恳求你坚持完成这些，即使它们变得混乱，这样你的代码的结构完整性仍然是正确的。像平常一样解决合并冲突，把所有对你来说重要的代码都取出来。

另一个与 rebase 类似的功能是 git merge(正如我们上面看到的)。Git merge 主要用于合并两个分支，其工作方式类似于 rebase。

## 结论

我希望这篇博客对你有所帮助；请在评论中告诉我你的想法。尽管我取笑了 Git，但它绝对是任何开发人员最重要的工具之一，最好马上掌握它！所以要不断探索和尝试。

在[媒体](https://medium.com/@AnveeNaik)上关注我们，获取更多此类内容。

*成为* [*介质会员*](https://medium.com/@AnveeNaik/membership) *解锁并阅读介质上的许多其他故事。*