# 数据科学家的 6 个 GitHub 场景

> 原文：<https://towardsdatascience.com/7-github-scenarios-for-data-scientists-cb502dc54eb0?source=collection_archive---------27----------------------->

## 从第一天起就开始贡献你公司的代码库

当我刚开始作为一名数据科学家工作时，我被告知要将代码提交给 Github，尤其是提交给其他人正在管理的回购。一想到我笨拙的动作可能会破坏别人的代码，我就害怕。

现在我终于克服了恐惧，我想我会分享我的 GitHub 工作流程在我日常工作中的样子。

![](img/3be12caedd4b9d1a030af300d088f19b.png)

由 [Roman Synkevych](https://unsplash.com/@synkevych?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**场景 1** :你被分配到一个新任务，有一张吉拉或俱乐部会所的门票编号 DS-1234。

我从我的开发同事那里学到的一个好习惯是，总是用你的票号开始一个新的分支。这样，你将能够指出背景、故事和作品范围的血统。这不仅对你的代码审查者、你的项目经理(他们可能会友好地问你你发布了什么)非常有帮助，最重要的是，对你未来的自己非常有帮助。

```
# Create and switch to a branch named after the Jira ticket
$ git checkout -b DS-1234
```

您可以通过下面的命令检查您当前的分支

```
$ git branch
```

现在你可以开始你的分支工作了。你可以添加新的代码，编辑现有的代码，一旦你满意了，你可以通过这样做来`add`和`commit`你的工作:

```
$ git add new_algo.py # a new script called new_algo.py
$ git commit -m "added a new algo"
$ git push origin DS-1234
```

现在你的代码在本地`DS-1234`和 Github 远程分支`DS-1234`上都被更新了。

**场景 2** :在你第一次提交之后，创建一个拉请求(PR)并标记你的同事进行代码审查总是一个好主意。

您可以在 Github 上为分支`DS-1234`创建一个 PR，请访问:

[https://github.com/your-project-folder/pull/new/ds-1234](https://github.com/your-project-folder/pull/new/ds-1234)

你也可以在运行`git push origin DS-1234`时找到这个路径

假设你的同事给了你一些反馈和评论，你直接在 github 上对`readme`做了一些小的编辑，现在你想在本地进一步更新你的`new_algo.py`。

您可以首先在本地终端上运行下面的命令，从 github 获取`readme`变更

```
$ git pull origin DS-1234
```

然后在更新完`new_algo.py`之后，可以通过运行以下命令将更新后的代码推送到 github 远程分支

```
$ git push origin DS-1234
```

一旦你确认你的同事将你更新的分支`DS-1234`合并到`master`分支，你就可以安全地在本地和远程删除`DS-1234`。

```
# delete local branch
$ git branch -d DS-1234# or you can run
$ git branch -D DS-1234# delete remote branch using push --delete
$ git push origin --delete DS-1234
```

**场景三**:如果你的同事足够信任你，让你自己合并，或者你需要把别人的分支合并成 master，你可以这样做:

```
# assuming you are to merge DS-1234 to master branch
$ git merge --no-ff -m "merged DS-1234 into master"# or if you are to merge other people's bug fix branch bugfix-234
$ git merge --no-ff -m "merged bugfix-234 into master"
```

注意，`no-ff` 标志防止 git merge 执行“快进”,如果它检测到您当前的头是您试图合并的提交的祖先。

**场景 4** :你想删除一个文件。

```
$ git rm test.py # remove test.py file
# commit your change
$ git commit -m "remove test.py file"
```

如果你改变了主意或者删除了错误的文件怎么办？别担心，你能做到的

```
$ git checkout -- test.py
```

现在`test.py`回来了！

**场景 5** :删除未被跟踪的文件

有时你做了一堆你不想保留的更改，你可以运行下面的命令来一次清理它们。

```
# dry run to see which files will be removed
$ git clean -d -n# remove them
$ git clean -d -f
```

**场景 6** :【高级场景】我发现自己在创建`DS-1234`之前就开始对`master`分支做改动了。事实上，这种情况在我身上发生过很多次😂。

我们有两个解决方案。

**解决方案 1** : `git stash`

```
# step 1: save the changes you made on master branch
$ git stash # step 2: create and switch to DS-1234 branch
$ git checkout -b DS-1234# step 3: transfer the changes using stash pop
$ git stash pop
```

**解决方案二**:精选🍒

这是我从前任经理那里学来的一招。它类似于`git cherry-pick`，但在我看来更直观，因为它非常清楚地显示了路径。

现在，想象一下在你的 DS-1234 上，你处理了许多代码。明天是将你的代码合并到发布分支的最后期限。看起来只有一段代码准备合并。您可以通过运行以下命令来选择要合并的这个:

```
# step 1: pull all recent changes from remote
$ git pull# step 2: checkout your branch and release branch
$ git checkout DS-1234  # pick from this branch
$ git checkout release-2021-07-01 # to this target branch# step 3: checkout the code ready to merge
$ git checkout DS-1234 new_algo.py# step 4: add this code to release branch
$ git add new_algo.py# step 5: commit 
$ git commit -m "cherry pick changes from DS-1234 to release"
```

**关闭思路**

1.  Git 和 GitHub 是功能强大的协作工具，对于过去以笔记本方式工作的新数据科学家来说，一开始可能会感到害怕。
2.  对于数据科学家来说，你肯定不需要学习最奇怪的 GitHub 命令来开始与你的开发同事合作。
3.  通过熟悉常见的 GitHub 工作流环境，您可以从第一天(好吧，也许是第一周)就满怀信心地将您的代码推向您组织的代码库😊).