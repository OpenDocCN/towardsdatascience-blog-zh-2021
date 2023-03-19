# 解释 vim-sensible 的每一行

> 原文：<https://towardsdatascience.com/explaining-every-line-of-vim-sensible-b776affec51a?source=collection_archive---------7----------------------->

## 学习机

## 因为并非所有明智的 Vim 对你来说都是明智的

![](img/7d654fed998c4ce20301d49885644f3e.png)

由[特斯拉留·米海](https://unsplash.com/@mihaiteslariu0?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

以下是你点击这个故事的三个可能原因。首先，它作为一个推荐故事出现在你的屏幕上。你不是在用 Vim，而是对 Vim 和懂事的 Vim 很好奇。

第二个原因，你已经痴迷于 Vim 很多年了，你想，“我已经知道里面所有的行，但是这是一篇关于 Vim 的文章，所以我还是要读它。”

最后，你和我处于学习 Vim 的相同阶段，你已经搜索了 Sensible 的注释版本，但是还没有找到。

首先，我们需要理解 Vimrc 文件。Vimrc 文件是 Vim 的配置文件，允许您定制 Vim、安装插件等等。它通常在`~/.vimrc`中找到，但是如果它不在那里，你可以创建它。对于 Windows，您可以在这里找到关于查找 vimrc 文件的更多信息。

[](https://vim.fandom.com/wiki/Open_vimrc_file) [## 打开 vimrc 文件

### 1 什么是 vimrc？2 每个目录的 vimrc 3 位置 vimrc 4 打开 vimrc 4.1 来源 vimrc 4.2 从…恢复

vim.fandom.com](https://vim.fandom.com/wiki/Open_vimrc_file) 

我在寻找一个 vimrc 文件，其中充满了有用的缺省值，作为我未来的 vimrc 的基础。首先，我想分享我在多个帖子和讨论中发现的一个反复出现的答案。引用道格·布莱克的文章，

> 不要在你的 vimrc 中放任何你不理解的行。

我认为这是一个非常合理的建议。程序员不会在他们的代码中添加任何额外或不必要的行。vimrc 文件也应该如此。

在我的搜索过程中，我偶然发现了 Reddit 上的这篇文章，它指出 Sensible 是 vimrc 文件的一个很好的起点。

[](https://github.com/tpope/vim-sensible) [## t ope/vim-可感知

### 可以把 sensible.vim 看作是“不兼容”模式之上的一个步骤:一组通用的默认设置，希望每个人都可以…

github.com](https://github.com/tpope/vim-sensible) 

与我得到的其他选择(范围从完全相同的 Sensible 到超过 300 行)相比，我决定更深入地研究这一点作为起点。

这是 Sensible Vim 的完整代码。

tpope 对 vim 敏感的代码

有些最难理解的台词是有注释的，但是大部分根本没有任何解释。尽管这是可以理解的，但对于那些更熟悉 Vim 的人来说，在这些行上添加注释是没有意义的。

也许对他们来说会是这个样子。

```
if speed > 80.0:        # checks if speed is more than 80.0
    speeding = True     # set speeding flag to True
    n_violation += 1    # add 1 to variable n_violation
```

无论如何，我不是 Vim 专家，所以我想用过度注释的版本来更好地理解这些行实际上做了什么。

这是我发现的，随意跳过一些琐碎的解释。

# 逐行解释

```
if exists(‘g:loaded_sensible’) || &compatible
  finish
else
  let g:loaded_sensible = ‘yes’
endif
```

第一点是将 vim 变量`loaded_sensible`设置为‘yes ’,大概是为了与其他插件兼容，这些插件检查是否使用了 vim-sensible。

```
if has(‘autocmd’)
  filetype plugin indent on
endif
```

该行将检查`autocmd`是否启用。大多数 Vim 都会附带，但是一些发行版比如`vim-small`和`vim-tiny`会默认不带`autocmd`。

在这种情况下，需要检查`autocmd`，因为`filetype`检测系统需要它才能正常工作。

`filetype plugin indent on`是这些命令的简称。

```
filetype on
filetype plugin on
filetype indent on
```

第一个命令打开 Vim 的文件类型检测，以帮助设置语法高亮显示和其他选项。`plugin`部件将加载特定文件类型的插件，如果它们存在的话。最后一位将加载特定文件类型的缩进文件，如果它们也存在的话。

例如，如果你想只为 Python 语言激活某些插件，那么你可以创建一个文件`~/.vim/ftplugin/python.vim`。将所有专门针对 Python 的插件和命令放在该文件中。

一个好的做法是在另一个文件中分离缩进配置(`~/.vim/indent/python.vim`)。然而，我通常只是把缩进放在插件文件里。

```
if has(‘syntax’) && !exists(‘g:syntax_on’)
  syntax enable
endif
```

与前面的`if`命令相似，这个命令检查 Vim 是否用`syntax`编译，以及全局变量`syntax_on`是否已经存在。目标是避免多次打开`syntax`(如果它已经启用)。

```
set autoindent
```

`autoindent`选项将在插入模式下创建新行时使用当前缩进，通过正常返回或`o` / `O`。

```
set backspace=indent,eol,start
```

如果您使用 Vim 已经有一段时间了，您会注意到有时退格键在插入模式下不能正常工作。当您尝试在自动插入的缩进、换行符和插入开始处退格时，会发生这种情况。

设置这些退格选项将使您能够以特定的顺序正常地对它们执行退格操作。

```
set complete-=i
```

Vim 中的`-=`操作符意味着如果已经设置了选项，它将删除这些选项。在这种情况下，它将从 autocomplete 中删除`i`选项。

`i`选项声明它将“扫描当前和包含的文件”，如果当前文件包含许多其他文件，这可能会影响自动完成的结果。因此，禁用该选项是有意义的。

```
set smarttab
```

`smarttab`将根据`shiftwidth`、`tabstop`或`softtabstop`插入`n`空白(如果它们被配置的话),并且还将删除行首的一个`shiftwidth`空白，而不是默认的一个空白。

```
set nrformats-=octal
```

`nrformats`是数字格式的缩写，有助于定义哪种格式将被视为 Vim 的数字。如果您键入`13`，然后在正常模式下悬停在它上面并按 Ctrl+A，那么它会将它递增到`14`。或者可以用 Ctrl+X 递减到`12`。

由于使用基数 8，`octal`选项将导致`007`增加到`010`。在正常使用中，这不是预期的行为，因为没有多少人在日常工作中使用 base 8。禁用后，`007`将增加到`008`。

```
if !has(‘nvim’) && &ttimeoutlen == -1
  set ttimeout
  set ttimeoutlen=100
endif
```

`nvim`变量用于检查您运行的是 NeoVim 还是普通 Vim。`ttimeoutlen`设置 Escape、Ctrl、Shift 等组合键的超时时间。默认情况下，`ttimeoutlen`的值是-1，它将被改为 100，但这一行将其显式设置。

`ttimeoutlen`有什么用？假设您为 Cmd+Shift+Up 设置了一个映射。该命令的关键代码是`^[[1;6A`。另一方面，Esc 的关键代码是`^[`。如果将`ttimeoutlen`设置为 5000，则每次按下 Esc 键，都会等待 5 秒钟，然后才会注册 Esc 命令。这是因为 Vim 认为您有可能在 5 秒钟窗口内按下其余的按键代码。

可能有一种情况是将`ttimeoutlen`设置得更长是有用的，但是就我个人而言，我没有发现它们对我的用例有用。

```
set incsearch
```

增量搜索或`incsearch`允许 Vim 在您键入搜索关键字时直接进入下一个匹配结果。如果没有这个设置，您需要按 Enter 键让 Vim 转到搜索结果。

```
“ Use <C-L> to clear the highlighting of :set hlsearch.if maparg(‘<C-L>’, ’n’) ==# ‘’
 nnoremap <silent> <C-L> 
:nohlsearch<C-R>=has(‘diff’)?’<Bar>diffupdate’:’’<CR><CR><C-L>
endif
```

谢天谢地，这段代码附有解释，因为我肯定要花相当长时间才能弄明白它的意思。如果您启用了`hlsearch`，那么按 Ctrl+L 将会清除高亮显示，因为它不会自己消失。

```
set laststatus=2
```

`laststatus`的默认值是 1，这意味着只有当有 2 个或更多的 Vim 窗口打开时，Vim 的状态栏才会显示。通过将值设置为 2，现在状态栏将一直存在，即使你只打开 1 个 Vim 窗口。

```
set ruler
```

显示光标位置的行号和列号。有时候这有点多余，因为大多数状态栏插件都是内置的。

```
set wildmenu
```

当在命令行中调用自动完成时，打开`wildmenu`会在命令行上方显示可能的完成。查看效果的最简单方法是在正常模式下键入`:!`,然后按 Tab 键触发自动完成。

```
if !&scrolloff
  set scrolloff=1
endif
```

`scrolloff`值确保你的光标上方和/或下方始终至少有 n 行。这一行检查您是否设置了`scrolloff`值，如果没有，则将其设置为 1。

`scrolloff`的默认值是 5，所以这个设置实际上将它减少到 1。
有些人将该值设置为一个非常大的数字(999 ),以使光标保持在屏幕中间，但我发现每当我创建一个新行时，屏幕就会刷新并移动一行，这有点令人迷惑。也就是说，我也将我的设置为 1。

```
if !&sidescrolloff
  set sidescrolloff=5
endif
```

如果`scrolloff`是上下两行，`sidescrolloff`是光标左右两边的字符数。默认值为 0。您也可以将此值设置为 999，使光标保持在线的中间。

```
set display+=lastline
```

如果你打开一个很长的文件，它不适合屏幕，Vim 通常会用一串“@”来替换它。通过将其设置为`lastline`，Vim 将显示尽可能多的字符，然后在最后一列加上“@@@”。

该行使用`+=`来避免覆盖已设置为`truncate`的设置，这将在第一列显示“@@@”。

```
if &encoding ==# ‘latin1’ && has(‘gui_running’)
  set encoding=utf-8
endif
```

`==#`操作符表示区分大小写的比较，与`==`不同，因为它依赖于`:set ignorecase`。如果 Vim 运行 GUI，该行将把编码从`latin1`改为`utf-8`。

```
if &listchars ==# ‘eol:$’
 set listchars=tab:>\ ,trail:-,extends:>,precedes:<,nbsp:+
endif
```

`listchars`用于指定使用`:list`命令时，特定字符的显示方式。默认值是`'eol:$'`，所以如果您已经配置了自己的`listchars`，这一行不会覆盖您的设置。

制表符将被替换为`>`，后跟数字`\`，直到制表符结束。尾随空格将被替换为`-`。如果当前行不适合显示在屏幕上，扩展字符`>`将显示在最后一列，前面的字符`<`将显示在第一列。最后，`nbsp`代表不可破坏的空格字符，将被替换为`+`。

```
if v:version > 703 || v:version == 703 && has(“patch541”)
  set formatoptions+=j “ Delete comment character when joining commented lines
endif
```

还好这也是评论的台词。如果 Vim 版本比 7.3 或带有补丁 541 的 7.3 版本新，则为`formatoptions`添加`j`选项。

当您连接注释行时，该行将删除注释字符(例如“#”、“//”)。Vim 中`formatoptions`的默认值是`tcq`，但是这有点误导，因为它可能会根据打开的文件类型而改变。

```
if has(‘path_extra’)
  setglobal tags-=./tags tags-=./tags; tags^=./tags;
endif
```

`setglobal`命令与通常的`set`命令有点不同。通过使用`set`，我们设置了一个选项的局部和全局值，而`setglobal`只设置了全局值。

全局值是应用于所有 Vim 的默认值。局部值仅适用于当前窗口或缓冲区。

`tags`命令定义了 tag 命令的文件名。该行所做的是从当前的`tags`中删除`./tags`和`./tags;`。`^=`命令将值`./tags;`添加到当前的`tags`中。

例如，如果`tags`的当前值是`./a, ./b`并且我们执行了`setglobal tags ^= ./tags;`，那么它将变成`./tags;, ./a, ./b`。`;`表示如果已经在`./tags`中找到标记文件，Vim 将停止查找。

```
if &shell =~# ‘fish$’ && (v:version < 704 || v:version == 704 && !has(‘patch276’))
  set shell=/usr/bin/env\ bash
endif
```

`=~#`操作符意味着与正则表达式匹配，所以在这种情况下，它检查您是否正在使用`fish` shell。Vim 在`fish`上运行时有一些已知的问题，主要是因为`fish`不完全兼容 POSIX。

[](https://stackoverflow.com/questions/48732986/why-how-fish-does-not-support-posix) [## fish 为什么&如何不支持 POSIX？

### 感谢贡献一个堆栈溢出的答案！请务必回答问题。提供详细信息并分享…

stackoverflow.com](https://stackoverflow.com/questions/48732986/why-how-fish-does-not-support-posix) 

如果您正在使用 Vim 的旧版本(比 7.4 版本更早)或者使用没有补丁 276 的 7.4 版本，这一行将把您的 shell 设置为`bash`，以避免在运行一些不能用`fish` shell 执行的命令时出错。

```
set autoread
```

这将使 Vim 在检测到文件在 Vim 之外被更改时自动读取文件。但是，如果文件被删除，它将不会被重新读取，以保留删除前的内容。

```
if &history < 1000
  set history=1000
endif
```

将保留的历史数量设置为最小 1000。默认情况下，Vim 对于每种类型的历史只记住 50。在 Vim 中有五种类型的历史。

*   `:`命令
*   搜索字符串
*   公式
*   输入行(`input()`功能)
*   调试模式命令

```
if &tabpagemax < 50
  set tabpagemax=50
endif
```

使用`-p`命令行参数或`:tab all`命令将打开的最大选项卡页数从默认值 10 设置为至少 50。

```
if !empty(&viminfo)
  set viminfo^=!
endif
```

Viminfo 是一个由 Vim 自动编写的文件，作为一种“缓存”来存储信息，比如命令行历史、搜索字符串历史、最后一次搜索模式、全局变量等。

这一行将在前面加上`!`值，该值将保存和恢复以大写字母开头且没有任何小写字母的全局变量。

或者，如果您想在某种私有模式下运行 Vim，您可以使用命令`set viminfo=`禁用 viminfo 功能。

```
set sessionoptions-=options
```

当您使用`:mksession`进行会话时，从`sessionoptions`中删除`options`值将禁止保存选项、映射和全局值。

Tim Pope 自己解释说，他禁用它是因为记住全局选项会覆盖对 vimrc 所做的更改，这是他不想要的。

[](https://github.com/tpope/vim-sensible/issues/117) [## 为什么设置 session options-= options Issue # 117 tpope/vim-sensible

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/tpope/vim-sensible/issues/117) 

```
set viewoptions-=options
```

与上一行类似，这一行在使用`:mkview`制作视图时会禁用相同的东西。

我对视图不太熟悉，但是这一页解释了为什么不使用这一行会导致 Vim 在加载保存的视图时跳转到不同的工作目录。

[](https://vim.fandom.com/wiki/Make_views_automatic) [## 使视图自动化

### 当你关闭一个文件的时候，你可以使用:mkview 来保存文件夹，但是你必须在下次使用…

vim.fandom.com](https://vim.fandom.com/wiki/Make_views_automatic) 

```
“ Allow color schemes to do bright colors without forcing bold.if &t_Co == 8 && $TERM !~# ‘^Eterm’
  set t_Co=16
endif
```

`t_Co`是终端使用的颜色数量。当当前颜色数量设置为 8 且当前终端不固定时，这将使颜色增加到 16 种。

但是大多数现代终端不需要这个，因为通常默认设置为 256。您可以通过键入`:echo &t_Co`来检查这一点。

```
“ Load matchit.vim, but only if the user hasn’t installed a newer version.if !exists(‘g:loaded_matchit’) && findfile(‘plugin/matchit.vim’, &rtp) ==# ‘’
 runtime! macros/matchit.vim
endif
```

`matchit`是一个 Vim 特性，它使我们能够在左括号和右括号、HTML 标签等之间跳转。您可以尝试在正常模式下按下左括号顶部的`%`，它会将您的光标移动到匹配的右括号。

如果`matchit`尚未启用或者现有的仍然是旧版本，该行将启用。

```
if empty(mapcheck(‘<C-U>’, ‘i’))
  inoremap <C-U> <C-G>u<C-U>
endifif empty(mapcheck(‘<C-W>’, ‘i’))
 inoremap <C-W> <C-G>u<C-W>
endif
```

`mapcheck`命令检查您是否为特定模式下的特定键设置了自定义映射。在这种情况下，它会在为 Ctrl+U 和 Ctrl+W 创建新映射之前，检查您是否已在插入模式下映射了它们。

这两行将防止意外删除，而不可能同时使用 Ctrl+U 和 Ctrl+W 进行撤销。通过在实际的 Ctrl+U 或 Ctrl+W 之前执行`Ctrl+G u`，我们可以使用撤销操作(正常模式下的`u`)恢复我们删除的文本，如果没有这些重新映射，这是不可能的。

[](https://vim.fandom.com/wiki/Recover_from_accidental_Ctrl-U) [## 从意外 Ctrl-U 中恢复

### 您可能会意外丢失正在文本中键入的文本，并且无法使用“撤销”来恢复。这个小贴士可以让你…

vim.fandom.com](https://vim.fandom.com/wiki/Recover_from_accidental_Ctrl-U) 

# 最后的想法

在理解，或者至少试图理解所有这些行之后，我决定不把 Sensible 作为插件安装，而是偷一些有用的行。

我最喜欢的一个是与`set hlsearch`配对的`set incsearch`,它支持渐进式搜索，也可以渐进式地突出显示搜索结果。

有一些我省略了，比如如果使用 fish shell，将 shell 设置为 bash，因为我自己也在使用 zsh shell。

[](/maybe-its-time-to-upgrade-your-terminal-experience-a9e12b2af77) [## 也许是时候升级你的终端体验了

### 默认的 MacOS 终端不适合你，也不适合任何人。

towardsdatascience.com](/maybe-its-time-to-upgrade-your-terminal-experience-a9e12b2af77) 

首先，我开始寻找构建 vimrc 的最佳文件。在搜索过程中，我发现很多其他人也在搜索同样的东西。然而，在对 Vim 配置有了基本的了解之后，我决定从头开始构建一个，因为我使用 Vim 不仅是为了编码，也是为了写作。

如果任何解释是错误的或不够清楚，请随时发表评论给予纠正或要求澄清任何行。

最后一个提示，如果您不确定一行代码的作用，您可以通过键入`:h 'commandname'`来搜索 Vim 的手册，以显示特定命令的帮助。了解这些命令在 vimrc 文件中的作用和效果是一个很好的实践。

*学习机是一系列关于我所学到的，并且认为足够有趣可以分享的事情的故事。有时也是关于机器学习的基础。* [*获得定期更新*](https://chandraseta.medium.com/subscribe) *上新故事和* [*成为中等会员*](https://chandraseta.medium.com/membership) *阅读无限故事。*

[](https://chandraseta.medium.com/membership) [## 成为媒体成员—里奥纳尔迪·钱德拉塞塔

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事

chandraseta.medium.com](https://chandraseta.medium.com/membership)