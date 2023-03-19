# 基本思想重新映射

> 原文：<https://towardsdatascience.com/the-essential-ideavim-remaps-291d4cd3971b?source=collection_archive---------1----------------------->

## 到达无鼠生产力乐土

![](img/8aa51347efc58cee72862aa8b0b089bd.png)

照片由 Amine Elhattami 拍摄

我使用 IdeaVim 进行机器学习和通用软件项目已经有一段时间了。在我的旅程中，我编制了一个基本重映射列表，将您的无鼠标开发提升到一个新的水平，而不会牺牲 Jetbrains IDEs 提供的健壮特性。因此，如果你更关注你正在编写的代码，而不是你应该使用哪个航空公司的主题，那么这篇博文就是为你准备的。这也是为了机器学习人群，他们仍然使用 Jupyter 笔记本电脑，并希望加强他们的开发环境游戏，但被 Vim 陡峭的学习曲线所阻碍。

几年前，我使用 Vim/NeoVim 作为我的日常驱动程序，我决定改用 IdeaVim，因为我意识到我把宝贵的时间花在了我的`.vimrc`上。正如你将在这篇文章中看到的，你仍然需要做一些配置。然而，与在 Vim 上构建“IDE”相比，它主要是设置快捷方式。IdeaVim 并不完美，但它为我提供了我需要的灵活性，尤其是因为我不是 Vim 超级用户。如果你对我为什么转而使用 IdeaVim 感兴趣，请查看这篇文章。

# 开始之前

如果你是 IdeaVim 的新手，看看我之前的[帖子](https://amine-elhattami.medium.com/destination-ideavim-day-1-the-basics-793a514af57f)，看看如何设置它，并了解 IdeaVim 快捷方式是如何工作的。此外，我理解记住所有的快捷方式可能会让人不知所措。我刚开始的时候也是这样。我尝试了多种方法来记忆它们，但最有效的是:

*   让按键绑定成为你自己的。在完善配置的过程中，您最终会从多个来源收集重映射，就像这个一样，有时键绑定对您来说没有意义，或者它可能已经被使用了。所以去改变它吧。
*   按照相同的方法创建快捷方式。例如，你可以选择使用`CTRL`和快捷键动作的第一个字母。例如，映射`CTRL t`来启动终端。偶尔一些动作会以同一个字母开始，这时你可以使用`<leader>`键或任何类似`gd`的键来定义。
*   把你经常忘记的快捷方式列表放在显眼的地方。我在我的显示器上用了一张便利贴，它真的有效！

记住，学习 Vim 风格的键绑定总是很有价值的。因为即使您选择使用另一个 IDE，它也可能有某种 vim 模拟。例如，我使用 PyCharm 和 CLion 进行开发，使用 NeoVim 动态编辑配置文件，我在这三个版本中使用了大致相同的键绑定。所以我只需要记住一套快捷键，这是一大优势。此外，下次当你不得不编辑一个服务器配置，而 vi 是唯一的选择时，你会感谢你自己。

最后，我在这里分享一些屏幕截图[因为眼见为实。](https://youtube.com/playlist?list=PLYDrCnplQfmG2aoNeu5_RP3GfcBiD1wl7)

# 基本重映射列表

我已经决定将列表分成几个部分，以提供每个部分的更多细节，如果你只是在寻找特定的重映射。

作为参考，我在帖子底部附上了完整的配置。但是，我建议您复制/粘贴每个部分，并尝试每个重新映射，看看它做了什么。

## 编辑和重新加载配置

在这篇博文中，你会看到需要添加到`.ideavimrc`文件中的片段。保存后，需要重新加载以应用更改。下面定义了两个重映射:`\e`打开文件，`\r`重新加载文件。此外，`set clipboard`命令支持从 IdeaVim 使用系统剪贴板，反之亦然。

```
set clipboard+=unnamed
set clipboard+=ideaputnnoremap \e :e ~/.ideavimrc<CR>
nnoremap \r :action IdeaVim.ReloadVimRc.reload<CR>
```

请注意，对于我在编码时不使用的重映射，如上图所示。我尝试使用相同的前缀(在本例中是`\`，因为我不想浪费有用的键绑定。

## 退出按钮

或者更确切地说是缺少退出按钮。对于那些被老款 MacBook touch bar 诅咒的人来说，这个很方便。因为你几乎总是需要点击`<ESC>`，所以拥有一个物理按钮的触觉反馈是必须的(至少对我来说)。下面重新映射`CTRL c`到`<ESC>`。

```
map <C-c> <Esc>
```

此外，我从系统偏好设置中将`CAPSLOCK`设置为`CTRL`键。我发现它非常有用，因为你的手指不需要离开主行。您甚至可以将`CAPSLOCK`键设置为按一次充当`<ESC>`，按住则充当`CTRL`。

## 前导键

如果您不熟悉 Vim，可以将 leader 键看作一个曾经设置过的映射前缀。假设您的配置中有`<leader>c`，并且您已经将 leader 映射到`SPACE`，如下所示，那么您的映射是`SPACE c`。

```
let mapleader=" "
```

使用 leader 键的好处是，如果你决定`SPACE`不再实用，只需要在一个地方进行改变。

## 无分心模式

正如我在[之前的一篇文章](https://amine-elhattami.medium.com/dear-vim-i-q-5df03b763ae4)中提到的，我是 Vim 干净界面的忠实粉丝。然而，我一直被问到为什么有人会隐藏 ide 提供的所有有用的小部件。我的回应是，我喜欢把我的开发界面当成我的办公桌。我尽量把最少的放在最上面，因为每个项目都在不断地吸引我的注意力。此外，可见的小部件可以“欺骗”你使用鼠标而不是使用快捷方式。永远记住:

> 看不到的不能点。

```
nnoremap <c-z> :action ToggleDistractionFreeMode<CR>
```

## 末端的

在我日常使用的所有命令行工具中，拥有一个终端快捷方式是必须的。

```
nnoremap <c-t> :action ActivateTerminalToolWindow<CR>
nnoremap <leader>t :action Terminal.OpenInTerminal<CR>
```

`Terminal.OpenInTerminal`动作允许您在编辑器中当前文件的父文件夹中直接打开一个新的终端。这对于深度嵌套的文件夹结构非常有用，因为它避免了必须 *cd* 到正确的文件夹。

## 窗口导航

在编程时，我非常依赖导航重映射，因为我不断地创建分割，并从一个缓冲区(或选项卡)移动到另一个。

```
nnoremap <c-\> :action SplitVertically<CR>
nnoremap <c--> :action SplitHorizontally<CR>
nnoremap <c-=> :action Unsplit<CR>
nnoremap <c-m> :action MoveEditorToOppositeTabGroup<CR>sethandler <c-j> a:vim
sethandler <c-k> a:vim
nnoremap <c-h> <c-w>h
nnoremap <c-l> <c-w>l
nnoremap <c-j> <c-w>j
nnoremap <c-k> <c-w>knnoremap <TAB> :action PreviousTab<CR>
nnoremap <s-TAB> :action NextTab<CR>nnoremap <Leader>en :action EditSourceInNewWindow<CR>
nnoremap <Leader>n :action NextWindow<CR>nnoremap <Leader>q :action CloseContent<CR>
nnoremap <Leader>qa :action CloseAllEditors<CR>
```

`sethandler`命令将 IdeaVim 设置为指定快捷方式的处理程序。这可以通过设置 UI 来完成。然而，我喜欢尽可能把所有东西都放在一个地方。

## 编辑源代码

如果基本的重映射列表不包括编辑源代码，它将是不相关的。看着我的配置，我可以数出 30 多个我在这里和那里找到的地图。然而，在写这篇文章的时候，我选择了我最常用的。这表明你不需要一个详尽的清单。你只需要保留你使用的那些，所以请随意挑选。

```
set ideajoin
set idearefactormode=keepvnoremap < <gv
vnoremap > >gvnnoremap [[ :action MethodUp<CR>
nnoremap ]] :action MethodDown<CR>nnoremap zc :action CollapseRegion<CR>
nnoremap zo :action ExpandRegion<CR>
nnoremap <leader>zc :action CollapseAllRegions<CR>
nnoremap <leader>zo :action ExpandAllRegions<CR>nnoremap <leader>c :action CommentByLineComment<CR>nnoremap <leader>r :action Refactorings.QuickListPopupAction<CR>nnoremap <Leader>=  :action ReformatCode<CR>
nnoremap <leader>o :action OptimizeImports<CR>nnoremap <c-r> :action RecentFiles<CR>
nnoremap <leader>l :action RecentLocations<CR>
nnoremap <leader>h  :action LocalHistory.ShowHistory<CR>nnoremap ge :action GotoNextError<CR>
nnoremap gE :action GotoPreviousError<CR>
```

对于重构，我选择了快速列表，因为我发现这样更容易。但是，您可以使用适当的 ID 来映射每一个特定的重构操作，比如`ExtractMethod`、`RenameElement`、`ChangeSignature`等等。使用`:actionlist`命令查找完整的动作 ID 列表。

## 搜索和源代码导航

我喜欢 Jetbrains IDEs 的搜索功能。它只是工作，并有一个接近完美的智能感知。尤其是在处理一个新的大型代码库时，我仍然在学习东西在哪里。

```
set incsearchnnoremap <c-/> :action FindInPath<CR>
nnoremap <c-a> :action GotoAction<CR>
nnoremap <c-f> :action GotoFile<CR>
nnoremap <leader>u :action FindUsages<CR>nnoremap <leader>s :action GotoRelated<CR>
nnoremap <leader>h :action CallHierarchy<CR>
nnoremap <leader>b :action ShowNavBar<CR>
nnoremap <c-s> :action FileStructurePopup<CR>
nnoremap <c-o> :action GotoSymbol<CR>
nnoremap gc :action GotoClass<CR>
nnoremap gi :action GotoImplementation<CR>
nnoremap gd :action GotToDeclaration<CR>
nnoremap gp :action GotToSuperMethod<CR>
nnoremap gt :action GotoTest<CR>
nnoremap gb :action Back<CR>
nnoremap gf :action Forward<CR>
```

对于文件内搜索，我喜欢使用 Vim 的默认搜索命令`/`。因为它提供了我所需要的一切。但是，你总是可以映射`Find`动作。

## 工具窗口

在任何 Jetbrains IDEs 中，几乎所有不是编辑器的东西都是工具窗口。所以有一套好的快捷方式来管理它们非常方便。

首先在你的`.ideavimrc`中，你需要设置从编辑器中触发的快捷键。

```
nnoremap <c-p> :action JumpToLastWindow<CR>
nnoremap <c-x> :action HideAllWindows<CR>
```

其次，从 IDE 设置中设置从工具窗口触发的设置:

*   转到“键盘映射”并找到“其他”，然后是“工具窗口视图模式”并映射“固定停靠”、“浮动”和“窗口”。在我的例子中，我分别使用了`ALT d`、`ALT f`和`ALT w`。请注意，这些映射不会干扰您的`.ideavimrc`中的映射。
*   仍然在“键盘映射”中，找到“主菜单”，然后是“窗口”，然后是“隐藏活动工具窗口”和映射“隐藏活动工具窗口”。在我的例子中，我使用了`ALT h`。
*   在“工具”中找到“终端”并取消选中“覆盖 ide 快捷键”

最后一个评论，无论何时你想回到编辑器，只要点击`<ESC>`。然而，由于 IdeaVim 中的一个错误，你不能使用`<ESC>`从**运行控制台**返回到编辑器。一个解决方法是从 IDE 设置中为“焦点编辑器”设置第二个快捷方式，而不是`<ESC>`，在我的例子中，我使用的是`F1`。

## 运行和调试

我的运行和调试配置非常简单，如下图所示。

```
nnoremap ,r :action ContextRun<CR>
nnoremap ,c :action RunClass<CR>
nnoremap ,f :action ChooseRunConfiguration<CR>
nnoremap ,t :action ActivateRunToolWindow<CR>
nnoremap ,u :action Rerun<CR>
```

我选择映射`ContextRun`而不是`Run`动作，因为后者运行之前选择的配置，而不是运行光标处的代码。`RunClass`动作名容易引起误解，但它是用来运行当前文件的。此外，由于在使用上述定义的`ActivateRunConfiguration`时，您可以通过按住`SHIFT`来够到`ActivateDebugConfiguration`。我决定不绘制它。

以上所有命令都适用于运行测试。只需将光标放在您想要运行的测试上，然后点击快捷键。我也喜欢使用`RerunFailedTest`来做它所说的事情。

```
nnoremap ,f :action RerunFailedTests<CR>
```

至于调试，可以重新映射多个动作。但是，我只在基本情况下使用快捷键。对于任何更高级的东西，我现在坚持用鼠标。主要依靠快捷键感觉比较慢或者可能我在键盘上没那么快。还有，如果你看一下 [Vimspector](https://github.com/puremourning/vimspector) 项目(最有前途的调试插件之一)，你可以看到他们在 Vim 的界面上增加了按钮。所以我可能不是唯一有这种想法的人。

```
nnoremap ,b :action ToggleLineBreakpoint<CR>
nnoremap ,d :action ContextDebug<CR>
nnoremap ,n :action ActivateDebugToolWindow<CR>
```

使用 ide 运行和调试的一个缺点是，如果您使用一些高级设置，如环境变量或解释器选项，您将需要使用鼠标来创建配置。您仍然可以从终端运行您的代码，但是您将失去一些不错的功能，比如测试运行器和调试器。这对我来说不是一个交易破坏者，因为这是一个设置和忘记设置。

## 完整的配置

```
""" Editing and Reloading the Config
set clipboard+=unnamed
set clipboard+=ideaput
nnoremap \e :e ~/.ideavimrc<CR>
nnoremap \r :action IdeaVim.ReloadVimRc.reload<CR>""" The Escape button
map <C-c> <Esc>""" The Leader Key
let mapleader=" """" Distraction Free Mode
nnoremap <c-z> :action ToggleDistractionFreeMode<CR>""" Terminal
nnoremap <c-t> :action ActivateTerminalToolWindow<CR>
nnoremap <leader>t :action Terminal.OpenInTerminal<CR>""" Navigation
nnoremap <c-\> :action SplitVertically<CR>
nnoremap <c--> :action SplitHorizontally<CR>
nnoremap <c-=> :action Unsplit<CR>
nnoremap <c-m> :action MoveEditorToOppositeTabGroup<CR>sethandler <c-j> a:vim
sethandler <c-k> a:vim
nnoremap <c-h> <c-w>h
nnoremap <c-l> <c-w>l
nnoremap <c-j> <c-w>j
nnoremap <c-k> <c-w>knnoremap <TAB> :action PreviousTab<CR>
nnoremap <s-TAB> :action NextTab<CR>nnoremap <Leader>en :action EditSourceInNewWindow<CR>
nnoremap <Leader>n :action NextWindow<CR>nnoremap <Leader>q :action CloseContent<CR>
nnoremap <Leader>qa :action CloseAllEditors<CR>""" Editing source code
set ideajoin
set idearefactormode=keepvnoremap < <gv
vnoremap > >gvnnoremap [[ :action MethodUp<CR>
nnoremap ]] :action MethodDown<CR>nnoremap zc :action CollapseRegion<CR>
nnoremap zo :action ExpandRegion<CR>
nnoremap <leader>zc :action CollapseAllRegions<CR>
nnoremap <leader>zo :action ExpandAllRegions<CR>nnoremap <leader>c :action CommentByLineComment<CR>nnoremap <leader>r :action Refactorings.QuickListPopupAction<CR>
nnoremap <Leader>=  :action ReformatCode<CR>
nnoremap <leader>o :action OptimizeImports<CR>nnoremap <c-r> :action RecentFiles<CR>
nnoremap <leader>l :action RecentLocations<CR>
nnoremap <leader>h  :action LocalHistory.ShowHistory<CR>nnoremap ge :action GotoNextError<CR>
nnoremap gE :action GotoPreviousError<CR>nnoremap <leader>s :write<CR>""" Searching and Source Code Navigation
set incsearchnnoremap <c-/> :action FindInPath<CR>
nnoremap <c-a> :action GotoAction<CR>
nnoremap <c-f> :action GotoFile<CR>
nnoremap <leader>u :action FindUsages<CR>nnoremap <leader>s :action GotoRelated<CR>
nnoremap <leader>h :action CallHierarchy<CR>
nnoremap <leader>b :action ShowNavBar<CR>
nnoremap <c-s> :action FileStructurePopup<CR>
nnoremap <c-o> :action GotoSymbol<CR>
nnoremap gc :action GotoClass<CR>
nnoremap gi :action GotoImplementation<CR>
nnoremap gd :action GotToDeclaration<CR>
nnoremap gp :action GotToSuperMethod<CR>
nnoremap gt :action GotoTest<CR>
nnoremap gb :action Back<CR>
nnoremap gf :action Forward<CR>""" Tool windows
nnoremap <c-p> :action JumpToLastWindow<CR>
nnoremap <c-x> :action HideAllWindows<CR>""" Running and Debugging
nnoremap ,r :action ContextRun<CR>
nnoremap ,c :action RunClass<CR>
nnoremap ,f :action ChooseRunConfiguration<CR>
nnoremap ,t :action ActivateRunToolWindow<CR>
nnoremap ,u :action Rerun<CR>nnoremap ,f :action RerunFailedTests<CR>nnoremap ,b :action ToggleLineBreakpoint<CR>
nnoremap ,d :action ContextDebug<CR>
nnoremap ,n :action ActivateDebugToolWindow<CR>
```

# 结论

我再次重申，我不是由 Jetbrains 赞助的；然而，这是我分享我的旅程的愿望的一部分，希望它可以帮助别人。

我仍然相信 IdeaVim 并不完美，但直到现在它给我提供了足够的灵活性，我很乐意继续使用它。

最后，我目前依靠一组插件来增强我的配置，并将很快分享它们，所以请务必关注我！

# 在你走之前

在 [Twitter](https://twitter.com/amine_elhattami) 上关注我，我经常在那里发关于软件开发和机器学习的微博。