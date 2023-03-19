# Jupyter 有一个完美的代码编辑器

> 原文：<https://towardsdatascience.com/jupyter-has-a-perfect-code-editor-62147cb9bf21?source=collection_archive---------5----------------------->

## 一切触手可及

![](img/29e38ebb9ed3867db261ccc102fba581.png)

由[克莱门特·赫拉尔多](https://unsplash.com/@clemhlrdt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/code-editor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

笔记本一直是软件思想增量开发的工具。数据科学家使用 Jupyter 来记录他们的工作，探索和实验新的算法，快速勾画新的方法，并立即观察结果。

而且， [JupyterLab](https://jupyter.org/) 正在走向成为一个成熟的 IDE 只是不是你习惯的 IDE。凭借其出色的扩展和库，如 [kale](https://github.com/kubeflow-kale/kale) 和 [nbdev](https://github.com/fastai/nbdev) ，它当然能够做的不仅仅是起草一个想法。查看下面的故事，了解更多信息。

</jupyter-is-now-a-full-fledged-ide-c99218d33095>  

但是，每隔一个月，我们可能要编辑一个`.py`文件。这个文件可能包含我们在笔记本中导入的一些实用函数，或者定义我们模型的类。这样工作是一个很好的实践，所以我们不会用许多定义污染笔记本。但是与 JupyterLab 捆绑在一起的文本编辑器仅仅是:一个简单的、没有特色的文本编辑器。

那么，我们能做什么呢？还有像[这种](https://github.com/jupyterlab/jupyterlab-monaco)的努力，它试图将[摩纳哥](https://github.com/Microsoft/monaco-editor/)编辑器(为 [VS 代码](https://github.com/Microsoft/vscode)提供动力的代码编辑器)集成到 JupyterLab 中。尽管如此，正如贡献者在 README 文件中明确提到的，“*这个扩展仅仅是一个‘概念验证’实现，与生产状态相去甚远。*“另外，上一次提交是在 3 年前(撰写本文时)，所以它看起来不像是一个非常活跃的项目。

但是我们不需要任何延期。我们有一个终端。因此，我们可以拥有 ViM。而 ViM 有我们需要的一切；只是需要一些时间去掌握。

如果你爱维姆，你知道我在说什么。如果没有，先不要回避！**这个故事向您展示了如何将 ViM 转换成 Python IDE，并与 Jupyter 一起使用。**

> [学习率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=jupyter-vim)是为那些对 AI 和 MLOps 的世界感到好奇的人准备的时事通讯。你会在每周五收到我关于最新人工智能新闻和文章的更新和想法。在这里订阅！

# Python 的 ViM

让我们言归正传。如果还没有 ViM，第一步实际上是安装它。在这个故事中，我假设我们在一个 hoster JupyterLab 环境中工作。如果您在本地工作，如果您更喜欢 VS 而不是 ViM，没有什么可以阻止您启动 VS 代码。

所以，我猜你是在云上的一个虚拟机中工作。这通常意味着 Linux 环境。要在 Debian 发行版上安装 ViM，运行以下命令:

```
sudo apt-get remove vim-tiny
sudo apt-get update
sudo apt-get install vim
```

注意，一些 Linux 发行版预装了`vim-tiny`。因此，我们需要先删除它，然后安装完整版本。

然后，让我们验证我们有我们需要的一切。运行`vim --version`并注意两件事:

1.  您应该已经安装了 VIM > 7.3
2.  检查`+python`或`+python3`功能

我们准备好了。接下来，我们需要一个好的插件管理器。

## 武德勒

我发现`[Vundle](https://github.com/VundleVim/Vundle.vim)`是一个优秀且简单易用的插件管理器。因此，我们将使用它来安装我们需要的一切。要安装`Vundle`，我们需要两样东西。首先，`git clone`将项目放在一个合适的目录下:

```
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

然后，我们需要在我们的`home`目录中创建一个`.vimrc`文件，并在上面添加一些行。因此，首先，创建文件:

```
touch ~/.vimrc
```

然后，添加以下几行:

```
set nocompatible              " required
filetype off                  " required

" set the runtime path to include Vundle
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'

" add all your plugins here between the vundle#begin() and "vundle#end() calls

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
```

看到那条叫`vundle#begin()`的线了吗？在这个调用和`vundle#end()`调用之间，我们可以添加任何我们想要的插件。事实上，我们已经这样做了:我们已经添加了`gmarik/Vundle.vim`插件。安装一个新的插件就像复制粘贴它唯一的 GitHub 路径一样简单。我们稍后会看到如何。

现在，启动 ViM，按下`:`进入命令模式，执行`PluginInstall`。我们走吧。

## Python IDE

我们将安装的第一个插件支持折叠。你见过 numpy 源代码吗？通常一个函数的`docstring`会占据整个空间。默认折叠吧。通过在我们之前看到的`begin`和`end`调用之间添加以下代码来安装插件:

```
Plugin 'tmhedberg/SimpylFold'
```

一个提醒:每次添加新插件的时候，别忘了用我们看到的`PluginInstall`命令安装。

要默认启用`docstring`折叠，在 begin-end 块外添加以下设置。

```
let g:SimpylFold_docstring_preview=1
```

接下来，让我们启用自动缩进。安装以下插件(现在你知道怎么做了):

```
Plugin 'vim-scripts/indentpython.vim'
```

此外，您可以通过在`vimrc`文件中添加以下选项来告诉 ViM 您希望它如何处理`.py`文件:

```
au BufNewFile,BufRead *.py
    \ set tabstop=4 |
    \ set softtabstop=4 |
    \ set shiftwidth=4 |
    \ set textwidth=79 |
    \ set expandtab |
    \ set autoindent |
    \ set fileformat=unix
```

这些设置的名称很容易理解。`autoindent`设置做了大部分正确的事情，但是安装`vim-scripts/indentpython.vim`插件，这是 python 特有的，让你安心。

最后但同样重要的是，你需要自动完成。这项工作的最佳工具是`[YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)`。然而，它的安装有点复杂。首先，用下面一行安装插件:

```
Bundle 'Valloric/YouCompleteMe'
```

它很可能会在最后向您显示一个错误。别担心。继续并安装必要的依赖项:

```
apt install build-essential cmake python3-dev
apt install mono-complete golang nodejs default-jdk npm
```

最后，编译插件:

```
cd ~/.vim/bundle/YouCompleteMe
python3 install.py --all
```

就是这样！您已经拥有了将 ViM 转变为 Python IDE 所需的大部分东西。您可能需要考虑的其他插件有:

*   [**vim-syntastic/syntastic**](https://github.com/vim-syntastic/syntastic)**:**Python 语法高亮显示
*   [**nvie/vim-flake 8**](https://github.com/nvie/vim-flake8)**:**pep 8 检查
*   [**scroolose/nerd tree**](https://github.com/preservim/nerdtree)**:**文件夹结构浏览器
*   [**tpope/vim-逃犯**](https://github.com/tpope/vim-fugitive) **:** git 集成

这里是一个完整但不详尽的`.vimrc`配置。如果你有更多的好东西要添加，请评论！

# 结论

笔记本一直是软件思想增量开发的工具。此外， [JupyterLab](https://jupyter.org/) 正在成为一个成熟的 IDE。但是，每隔一个月，我们可能要编辑一个`.py`文件，集成的文本编辑器只是一个文本编辑器。

这个故事研究了如何将 ViM 转换成 Python IDE，并通过终端将它用作我们的主要代码编辑器。如果您想了解更多关于使用 ViM 的信息，只需在您的终端中运行`vimtutor`并按照说明进行操作！

# 关于作者

我的名字是[迪米特里斯·波罗普洛斯](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=jupyter-vim)，我是一名为[阿里克托](https://www.arrikto.com/)工作的机器学习工程师。我曾为欧洲委员会、欧盟统计局、国际货币基金组织、欧洲央行、经合组织和宜家等主要客户设计和实施过人工智能和软件解决方案。

如果你有兴趣阅读更多关于机器学习、深度学习、数据科学和数据操作的帖子，请关注我的 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow) 、 [LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 Twitter 上的 [@james2pl](https://twitter.com/james2pl) 。此外，请访问我的网站上的[资源](https://www.dimpo.me/resources/?utm_source=medium&utm_medium=article&utm_campaign=jupyter-vim)页面，这里有很多好书和顶级课程，开始构建您自己的数据科学课程吧！