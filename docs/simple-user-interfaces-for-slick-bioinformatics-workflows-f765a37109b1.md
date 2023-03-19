# 流畅的生物信息学工作流程的简单用户界面

> 原文：<https://towardsdatascience.com/simple-user-interfaces-for-slick-bioinformatics-workflows-f765a37109b1?source=collection_archive---------34----------------------->

![](img/a322e7f5f85875385c9c6634f089f4e2.png)

螺旋艺术，[**braňo @ 3d 天堂**](https://unsplash.com/photos/Mm1VIPqd0OA) **，**在 [Unsplash 许可下免费使用](https://unsplash.com/license)

最近，我开始将我所有的生物信息学脚本和工作流程打包到一个简单的基于文本的用户界面中。不是为了我的利益，而是为了我的生命科学同事的利益，他们倾向于避免可怕的命令行。因此，我花在运行作业或解决工作流程问题上的时间大大减少了，而他们运行自己的分析的信心却大大增加了。在这里，我们将把一个示例分析管道打包到一个基于文本的用户界面中，同时还有一些用户提示，以确保正确的文件得到处理。

那么他们为什么这么喜欢界面呢？因为这让他们感到安全，而且使用起来也容易得多，不需要跳到可怕的脚本中。

还有为什么我们这么喜欢界面？因为当我们不在办公室时，它更容易确保他们以正确的顺序运行正确的工作流，让我们有更多的时间来编写错误的代码。

# 这是针对什么受众的？

我开始将我的生物信息学工作流和脚本放入一个复古的基于文本的用户界面(TUI)中的原因是，实验室中不太熟悉 Linux 和 shell 脚本的人可以自己执行我定期运行的分析。它面向的是那些更倾向于湿实验室的同事或实验室伙伴，当你外出徒步旅行、划独木舟或滑雪时，他们仍然需要运行偶尔的分析管道(这对创造性思维空间是必不可少的)。

# **他们的反馈如何？**

非常积极。他们不再需要考虑对某些样品进行何种类型的分析，但这也减少了错误设置参数或文件名的情况。当然，这一切都可以通过一系列彻底的检查和文档来完成，但是用户界面看起来很光滑，感觉很好，并且易于使用。

# 那么用户界面会是什么样子呢？

![](img/e1acae0b3091a451c9ff73e6469e9981.png)

简单的界面，图片由作者提供

我们将使用 *ncurses* [Dialog](https://invisible-island.net/dialog/) 和 shell 脚本来构建我们的界面，使用您通常使用的任何 yum/apt-get/conda 方法来安装 Dialog，并做一些修改，它看起来很容易让人想起某部科幻电影第 4 集的重启。这将比默认的蓝色和红色的配色方案少得多，我强烈建议改变它，以避免头痛。要更改我们的界面颜色，将我们的 [*profile.rc*](https://github.com/DaLizardWizard/BioInfTUI/blob/main/.dialogrc) 文件复制到我们将要运行 TUI.sh 的同一个目录中。忽略后果自负。

# **制作启动屏幕**

为了欢迎您的同事使用他们的新工具，我们可以显示一个闪屏，其中包括一些 ASCII 格式的欢迎说明或徽标。对话框有这个简洁的功能，你可以指向一个要在界面中显示的文本文件。为了正确显示，界面尺寸必须与文本文件的字符尺寸相匹配。我喜欢蜥蜴，所以我的欢迎屏幕将是一张使用[https://many tools . org/hacker-tools/convert-images-to-ASCII-art/](https://manytools.org/hacker-tools/convert-images-to-ascii-art/)转换成 ASCII 的 groovy 蜥蜴的图片，其尺寸与界面尺寸相匹配。我发现 60 x40 的效果很好。我们将把它保存为文本文件 *welcomemsg.txt* 。

然后，可以使用以下命令在 TUI 中显示该文本文件。

```
export NCURSES_NO_UTF8_ACS=1welcomemsg () {
 dialog — title “Bioinformatics Analysis Wizard” — ok-button “next” — msgbox “$(cat welcomemsg.txt)” 40 60
}
```

# **添加菜单**

一个很棒的功能是内置了交互式菜单，这些菜单(和可能的子菜单)可以引导用户进行特定类型的分析，例如 DNA 或 RNA。我们将为菜单创建三个选项，以代表通常分析的主要分子类别(RNA、DNA 和蛋白质)。

```
mainoption() {
 MAINOPTION=$(dialog — title “Bioinformatics Analysis Wizard” — menu “Please choose from the following\
 analysis options” 20 60 3 \
 “1” “Start DNA analysis” \
 “2” “Start RNA analysis” \
 “3” “Start Protein analysis” \
 3>&1 1>&2 2>&3)

 if [ $MAINOPTION = 1 ]; then
 DNA_analysis
 elif [ $MAINOPTION = 2 ]; then
 RNA_analysis
 elif [ $MAINOPTION = 3 ]; then
 Prot_analysis
 fi
}
```

# **菜单选项**

一旦用户选择了他们的分析选项，在我们开始潜在的冗长分析之前，显示正在处理的文件和通过的工作流总是一个好主意，只是为了确认已经上传了正确的数据集。为了显示输入文件的名称，我们将在 DNA 分析工作流程的描述旁边使用内置函数的对话框。这与我们显示闪屏的方式类似。

这里值得注意的是，进一步的子菜单可以放置在菜单选项中，以增加您的生物信息学工作流程的复杂性。

现在，用户已经手动验证了将要分析的文件以及如何分析它们，我们终于可以运行分析了。当分析正在运行时，我们通过显示“运行”屏幕让用户知道他们的分析正在运行。

```
DNA_analysis() {
 #Displays files listed in analysis folder
 if (dialog — title “Bioinformatics Analysis Wizard” — yesno \
 “Are these the samples you are expecting? \n\n$(ls [Your_Input_Directory.*])” 20 60)
 then
 #while the script is running display and information box
 $(yes | bash [YOUR_DNA_ANALYSIS_SCRIPT_HERE] 2>&1 out.log) | dialog — title “Bioinformatics Analysis Wizard” \
 — infobox “Performing DNA analysis, please wait” 8 60
 else
 mainoption
 fi
}
```

# **退出消息**

为了表明分析已经完成，我们可以显示最后的庆祝屏幕。

```
exitmsg () {
 dialog — title “Bioinformatics Analysis Wizard” — ok-button “exit” — msgbox “$(cat exitmsg.txt)” 20 60
}
```

为此，您可以选择尺寸为 60x40 的 ASCII 文件形式的个人风格。为了从图像中生成 ASCII 艺术，我使用了[https://many tools . org/hacker-tools/convert-images-to-ASCII-art/](https://manytools.org/hacker-tools/convert-images-to-ascii-art/)。

# 现在都在一起

```
export NCURSES_NO_UTF8_ACS=1welcomemsg () {
 dialog — title “Bioinformatics Analysis Wizard” — ok-button “next” — msgbox “$(cat welcome.txt)” 20 60
}mainoption() {
 MAINOPTION=$(dialog — title “Bioinformatics Analysis Wizard” — menu “Please choose from the following\
 analysis options” 20 60 3 \
 “1” “Start DNA analysis” \
 “2” “Start RNA analysis” \
 “3” “Start Protein analysis” \
 3>&1 1>&2 2>&3)

 if [ $MAINOPTION = 1 ]; then
 DNA_analysis
 elif [ $MAINOPTION = 2 ]; then
 RNA_analysis
 elif [ $MAINOPTION = 3 ]; then
 Prot_analysis
 fi
}DNA_analysis() {
 #Displays files listed in analysis folder
 if (dialog — title “Bioinformatics Analysis Wizard” — yesno \
 “Are these the samples you are expecting? \n\n$(ls [Your_Input_Directory.*])” 20 60)
 then
 $(yes | bash [YOUR_DNA_ANALYSIS_SCRIPT_HERE] 2>&1 out.log) | dialog — title “Bioinformatics Analysis Wizard” \
 — infobox “Performing DNA analysis, please wait” 8 60
 else
 mainoption
 fi
}exitmsg () {
 dialog — title “Bioinformatics Analysis Wizard” — ok-button “exit” — msgbox “$(cat exitmsg.txt)” 20 60
}welcomemsg
mainoption
exitmsg
```

# **重述**

总之，我们制作了一个基于文本的用户界面(TUI)来帮助指导我们潜在的不太自信的 Linux 同事运行常见的生物信息学工作流程。我们这样做的原因是通过限制可用的选项和命令，确保分析完成。添加了一个闪屏来欢迎用户，然后显示一个菜单来进一步指导用户执行他们想要的分析类型。然后在他们选择的分析运行时通知他们。一旦分析完成，就会显示完成通知。

通过以这种方式打包生物信息学脚本，您可以让同事们更加独立地执行常规分析流程。

如果不熟悉终端，它可能是一个可怕的地方，许多生物学家强烈地认为它是“非编码者”。将复杂的分析管道包装在安全舒适的 TUI 中可以弥补这一差距，并使我的一些同事能够更多地了解潜伏在终端矩阵阴影中的龙…