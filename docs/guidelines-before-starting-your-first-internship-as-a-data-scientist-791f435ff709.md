# 作为数据科学家开始第一次实习前的指导方针

> 原文：<https://towardsdatascience.com/guidelines-before-starting-your-first-internship-as-a-data-scientist-791f435ff709?source=collection_archive---------38----------------------->

## 在一家瑞士咨询公司做了一年的 ML 工程师

## 在这里你会找到一些我在开始实习前就知道的提示和建议！

![](img/72c6db936867bbef777bfa093127cf9d.png)

丹尼尔·麦金尼斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 帖子的目的:

这篇文章将引导你完成我的第一次实习之旅。您将找到数据科学家必备工程材料的相关信息，更重要的是，它将帮助您迅速成为同事的可靠伙伴。

在开始我作为一名 ML 工程师的第一次实习之前，我在网上查了一下哪些工具我可能会日常使用。事实证明，找到简明的信息比预期的要难。因此，我决定分享一些我希望在我出生前就知道的建议！

根据你将要工作的公司，你不一定会用到所有这些工具。但是，了解它们会让你成为更好的程序员！

## 指南:

*   *如何在远程服务器上工作？*用 **ssh** 协议对话，用 **tmux 工作。**
*   *如何和多个工程师一起开发代码？* **Github** 就是你的一切。
*   *如何部署你的项目？* **Docker** 是你部署的第一步。

# 如何在远程服务器上工作？

今天，许多公司都在云服务器上工作。了解远程访问协议的基础知识会给你的主管留下深刻印象，并在你实习的头几天为你节省宝贵的时间。你不需要掌握所有的东西，但是能够使用主要的命令会让你很快上手！

![](img/69ceac350e94cd45e0d950c3741bb647.png)

照片由 [Kvistholt 摄影](https://unsplash.com/@freeche?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

## 什么是宋承宪？

SSH(或安全外壳)是一种远程管理协议，允许用户控制和更改他们的远程服务器。赫尔辛基理工大学的研究员塔图·伊洛宁创建了这个协议，以确保所有与远程服务器的通信都以加密的方式进行。

## 你需要什么

*   服务器及其 IP 地址
*   使用 sudo 访问的服务器设置
*   本地计算机

## Mac/Linux 用户如何使用 SSH 协议

如果您只需要一个密码就可以进行基本的服务器访问，那么使用下面的命令和您的**用户名**以及 **IP** 地址。

```
ssh <user>@<IP-Address>
```

如果您需要在服务器上打开一个端口，您可以使用 **-p** 选项轻松完成:

```
ssh -p 24601 <user>@<IP-Address>
```

如果您需要使用一个 SSH 密钥(它是一个带有锁定语句的散列密钥)，您将需要首先生成一个 SSH 密钥:

```
ssh-keygen -t rsa
```

上面的命令为[非对称加密](https://en.wikipedia.org/wiki/Public-key_cryptography)创建了两个密钥——一个用于加密的公共密钥和一个用于描述的私有密钥，并要求您选择存储密钥的路径。我鼓励你保持预设的路径。然后，您将能够通过添加-i 选项和您的公钥的路径来访问您的远程服务器。

```
ssh -i path/id_rsa.pub <user>@<IP-Address>
```

对于 Windows 用户，可以使用 Putty。可以关注这个[网站](https://www.learnitguide.net/2019/03/access-linux-server-from-windows.html#:~:text=Enter%20the%20IP%20Address%20of,the%20correct%20username%20and%20password.)。

## 你知道 Tmux 吗？

它是一个终端管理器，允许你在后台运行程序。这是它最初的主要目的，但是随着我对它的特性了解越来越多，我开始在每次打开终端时使用它！这个终端管理器允许您以高效的方式同时设置许多终端。其结构类似于 [Vim](https://fr.wikipedia.org/wiki/Vim) 代码编辑器。因此，通过避免使用鼠标，你将比以往任何时候都编程更快！

它可以在 Mac OS 和 Linux OS 上使用。因此，即使你有 Windows，仍然可以使用虚拟机来设置 tmux！

## 安装:

对于 Mac 用户

```
brew install tmux
```

适用于 Linux Ubuntu 或 Debian

```
sudo apt install tmux
```

## 开始使用 tmux:

要开始你的第一个疗程，写下:

```
tmux
```

或者，如果您想用预定义的名称创建一个新的:

```
tmux new -s session_name
```

如你所见，它就像一个新的终端！但是它可以做得更多。例如，要获得所有命令的列表，您可以键入:`Ctrl+b` `?`

如果要离开当前会话:`Ctrl+b`

如果你需要继续治疗。首先，列出所有可用的会话:

```
tmux ls
```

然后，在会话名称后附加:

```
tmux a -t session_name
```

或者，如果您想终止一个会话:

```
tmux kill-session -t myname
```

## 为什么要深入？

如前所述，使用 *tmux* 作为终端管理器，您可以做的不仅仅是简单的终端界面！如果你看一看这个[网站](https://tmuxcheatsheet.com/)，你会发现你将能够同时管理不止一个而是多个终端(称为窗格)!

# 如何与多名工程师一起开发代码

Github 是最知名的源代码管理器。它为每个项目提供了代码历史、安全控制、更新跟踪、特性请求、任务管理、持续集成和 wikis。

你需要尽快了解如何使用 GitHub！这是你需要快速变得敏锐的最重要的技能之一。它的技术是独一无二的，它的结构使它非常坚固。它创造的真实故事是非凡的，如果你渴望了解它，我给你[链接](https://www.welcometothejungle.com/en/articles/btc-history-git)！

![](img/c6a0933d28a9a3cd0e17dcaffa80d2c8.png)

[扬西·敏](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

你需要意识到的主要是 **Git** 和 **Github** 的存在。仅仅放；Git 是一个分布式版本控制系统，可以让你管理和跟踪你的源代码历史。GitHub 是其基于云的托管服务，使您能够与其他人一起管理 Git 存储库。这个软件可能会在你作为工程师的整个职业生涯中跟随你！可以比作程序员的脸书，很多公司会尝试/要求访问你在 Github 上的项目。在这个网站上，招聘人员可以很容易地收集到很多信息，帮助他们建立你的个人资料。因此，不要忘记清理你的代码，如果你认为你的项目可以卖得更好，分享你的项目！

## 安装:

Linux:

```
sudo apt install git
```

Mac 用户:

```
brew install git
```

Windows:跟随[网页链接](https://gitforwindows.org/)

## 现在让我们试一试:

尝试下面的[步骤](https://guides.github.com/activities/hello-world/)来了解 Git/GitHub 中的主要流程！让我们从一个例子开始:

假设一个 python 代码在某个特定的功能上有一点小问题:您正在使用一个 python [包，它会向您发送关于代码进度的通知](/create-a-simple-bot-with-telegram-that-notifies-you-about-the-progress-of-your-code-69bab685b9db)。您意识到当代码失败时还不可能发送错误消息。因此，你肯定喜欢这个工具的想法，你想通过增加这个小特性来改进它！

由于它的源代码在 GitHub 上是开源的，您可以执行以下步骤:

1) **在本地机器上克隆**存储库:

```
git clone https://github.com/lolilol/package.git
```

2)然后，创建你的源代码的 [**分支**](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-branches) 并发送到你的个人 GitHub

```
git checkout -b issue123
git remote add upstream https://github.com/my_name/demo.git
```

3) **更新**丢失的代码并运行一些**测试**。

4) [**提交**](https://github.com/git-guides/git-commit) 和 [**推送**](https://docs.github.com/en/github/using-git/pushing-commits-to-a-remote-repository) 您的变更到您的新分支。

```
git commit -a -m 'Fixed issue 123'
git push -u origin issue123
```

5)打开一个 [**拉请求**](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) 给仓库的维护者:一旦你将变更推送到你的仓库，GitHub 中就会出现**比较&拉请求**按钮。点击**创建拉动请求**按钮，打开拉动请求。

6)如果资源库的维护人员欣赏该代码，就会将 [**合并**](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/merging-a-pull-request) 到主分支！

## 提示和警告:

正如您可能已经听说的那样，许多合并问题和 git 问题很难用很少的经验来解决。因此，我在这里分享一些技巧来避免 Git 可能产生的主要问题。

**提示 1:** 在每个 git 命令后使用以下命令行来检查您对 Git 行为的预期:

```
git status
```

**提示 2:** 如果您在一些文件之间有**合并问题**，请不要紧张！Git 将精确地指定由新版本的源代码更新的代码行。您只需要更改需要丢弃的代码部分，然后重新提交您的更改。那么，合并就成功了！这里有一个很好的[例子](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/resolving-a-merge-conflict-using-the-command-line)。

**提示 3:** 不要犯提交和推送超过 GitHub 限制 [100MB](/uploading-large-files-to-github-dbef518fa1a#:~:text=GitHub%20has%20a%20strict%20file,you%20might%20want%20to%20cross.) 的**大小**的文件的错误！下面两个命令中的一个应该就可以了，但是请记住，您已经在 GitHub 中重写了提交的故事。你需要了解发生了什么，以采取最佳解决方案。检查那两个帖子:[链接](https://thomas-cokelaer.info/blog/2018/02/git-how-to-remove-a-big-file-wrongly-committed/)1&链接 2

```
git filter-branch --tree-filter 'rm -rf path/to/your/file' HEADgit filter-branch --index-filter 'git rm -r --cached --ignore-unmatch <file/dir>' HEAD
```

**提示 4:** 使用 [gitignore](https://www.pluralsight.com/guides/how-to-use-gitignore-file) 文件管理不想推送到 GitHub 的文件夹/文件。这将避免与 3 号提示相关的任何问题！当你在 GitHub 上创建一个新的存储库时，你可以自动为特定的编程语言生成一个 gitignore 文件！

# 如何部署您的项目

在这个阶段，您能够在服务器上进行通信、工作和协作。您可以在远程机器上设置工作流。最后一步就是要知道怎么部署！

有多种选择，根据项目的重要性，你可能会比我在这篇文章中做得更深入。我主要讲包管理、bash 脚本和 docker。但是你可以自由地去追求你的知识，比如说 Kubernetes！

![](img/10229303181f12e2fe0ce35709fbd08b.png)

照片由[张秀坤·吕克曼](https://unsplash.com/@exdigy?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

## 为什么使用 bash 脚本？

Bash shell 脚本是可以在终端中执行的命令行。它们将允许你以一种用户友好的方式设置你的应用程序，因为所有奇怪的和不可理解的命令将被收集在一个文件中。

下面是一个下载数据并运行 python 应用程序的 bash 脚本示例。

```
# create directory.
mkdir models # Download data and deep learning model
wget https:nvjovt/model.zip
cd models && unzip model.zip && rm model.zip && cd ..wget https:nvjovt/data.zip
unzip data.zip && rm data.zip# Run python code
python3.7 app.py
```

一个简单的命令可以用一个*来运行这个脚本。sh* 扩展:

```
bash setup_app.sh
```

## 包管理和虚拟环境

打包和虚拟环境密切相关，因为它们都关注包的依赖性。因此，建立一个项目最简单的方法是创建一个虚拟环境并在其中安装软件包。但是你可以用不同的包装来简化你的生活。

下面的包管理器创建了一个虚拟环境，它们还负责包之间的依赖关系！它使用起来更舒服，但是它需要花费时间！下载软件包通常需要更多的时间，但是您可以相信它们会有更简单的部署。

python 最著名的管理器叫做 **pipenv。**因此**，**让我们以此为例向你展示他们的特色。你可以用一个 pip 命令安装它:

```
pip install pipenv
```

使用一个简单的命令，您将创建虚拟环境和包管理器:

```
pipenv shell
```

您将看到一个文件名为 Pipfile 的文件，包就保存在这里。这个管理器的一个很大的特点是可以设置一些开发包，这些开发包只能因为开发的原因而安装。

要下载软件包，请使用以下命令之一:

```
pipenv install package_name 
pipenv install "package_name~=2.2"
```

要在开发阶段下载它，请使用:

```
pipenv install package_name --dev
```

然后，当您想要使用正确的库和正确的版本部署应用程序时，您需要使用以下内容锁定包依赖关系:

```
pipenv lock
```

这将创建/更新您的`Pipfile.lock`。这个文件冻结了你当前的包和它们的依赖关系。现在任何人都可以访问这个文件，您可以运行`pipenv install --ignore-pipfile`来创建相同的环境！或者`pipenv install --dev`如果有人需要访问开发包。

**提示 1:** 尽管这需要一些时间，但是如果不使用这样的打包管理器，您很可能会遇到依赖问题。他们真让人头疼！

**提示 2:** 它们还有额外的功能，可以帮助你在 Heroku 或 Flask 上部署应用！检查下面的[连杆](https://realpython.com/pipenv-guide/)。

**提示 3:** 还有其他的打包管理器，你可以查看一下[poem](https://python-poetry.org/)，因为它可能有比 pipenv 更适合你的特性。

## 为什么应该使用 Docker？

Docker 是一个你可能已经听说过的开源软件。它使用其操作系统创建容器，以避免在其他机器上部署解决方案时出现软件依赖性问题。我鼓励你学习 Docker，因为如果你从事计算工程，你将会使用它或者其他类似的软件。

总结其复杂的层次，Docker 可以运行与虚拟机目标相同的容器。在每个容器中运行一个独特的操作系统及其环境。你可能会问，为什么大多数人更喜欢 Docker 而不是虚拟机？因为它设置起来容易多了，而且相比 VM 也优化的很好！

首先，你需要学习图像，我们将一起看一个例子！然后，您将了解 Docker Hub，它遵循与 Github 相同的理念。最终，您将能够使用一个命令在任何 Linux 操作系统上部署应用程序！干杯！

## 如何下载 docker:

*   对于 Mac 用户:跟随这个[链接](https://docs.docker.com/docker-for-mac/install/)。
*   对于 Linux 用户:跟随这个[链接](https://docs.docker.com/engine/install/ubuntu/)。
*   对于 Windows 用户:跟随这个[链接](https://docs.docker.com/docker-for-windows/install/)。

主要命令有:*构建*和*运行*。第一个命令创建图像。第二个在你的机器上运行。但是在玩命令之前，让我们先了解一下会做一切的 Dockerfile！

## 什么是 Dockerfile

当您的项目完成并准备部署时，您可以从在工作目录中创建 Dockerfil 开始。这个文件是特殊的，它将定义你的 docker 图像的工作方式！如果你的应用程序需要大量内存，你可以优化这个文件，以确保你没有任何无用的包或数据。

如上所述，这个文件生成操作系统，导入源代码，并设置容器动作。以下是 Dockerfile 文件的可能结构示例:

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

请记住:

1.  FROM:您将始终基于另一个图像创建一个**图像。**
2.  复制:Docker 从基础映像创建一个操作系统，你需要**将源代码复制到**中
3.  运行:Docker 允许你在操作系统内部运行命令来**设置你的容器**(例如，bash 脚本、包安装、导入数据集、数据库等。)
4.  CMD:您还需要**指定启动应用程序将执行的启动命令**。它将在您运行容器时使用。

如果你对一个你可以在家尝试的例子感兴趣，这里有一个[链接](https://docs.docker.com/develop/develop-images/baseimages/)。

当您的 docker 文件准备好时，您可以运行以下命令来构建映像:

```
docker build folder_path_of_dockerfile -t name_your_image
```

如果代码没有任何错误地完成了，这意味着您的第一个图像已经准备好了！因为您可能需要在不同的机器上部署解决方案，所以您应该将这个新的映像推送到 Docker Hub 存储库中。它将允许您仅通过互联网连接访问图像。因此，您需要:生成一个 Docker Hub 帐户，创建一个存储库，并将其链接到您的本地机器。

```
docker login --username=yourhubusername
```

然后，标记您的图像:

```
docker images
docker tag image_id username/reponame:tagname
```

并将其推送到您的 Docker Hub 存储库:

```
docker push account_name/repo_name:tag_name
```

现在，您可以在任何安装了 Docker 的机器上只运行一个命令来启动您的应用程序:

```
docker run account_name/repo_name:tag_name
```

## 维护您的图像/容器:

图像:

*   显示所有图像:`docker images ls`
*   擦除图像:`docker rmi -f image_id`
*   擦除所有图像:`docker rmi $(docker image -a -q)`

容器:

*   显示所有图像:`docker container ls -a`
*   擦除一个容器:`docker rmi -f container_id`
*   擦除所有容器:`docker rmi $(docker container -a -q)`

# 结论:

如果你已经到了终点，那就意味着你要为实习的第一周做好准备了！祝你好运，年轻的学徒。