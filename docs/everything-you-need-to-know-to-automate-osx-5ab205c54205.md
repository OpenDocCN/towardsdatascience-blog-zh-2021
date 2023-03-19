# 自动化 OSX 所需的一切

> 原文：<https://towardsdatascience.com/everything-you-need-to-know-to-automate-osx-5ab205c54205?source=collection_archive---------31----------------------->

![](img/9794b04fe13316e22d1cd022af0a228c.png)

照片由[芙罗莉丝·安德烈亚](https://unsplash.com/@florisand?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

当我们想到自动化工作负载、可重复流程、“一键式”部署或类似的东西时，我们通常会想到笔记本电脑。我们在做任何事情的时候都在练习 DevOps，为什么我们的笔记本电脑会被忽视呢？毕竟，我们都同意“宠物”是坏的，“羊”是好的。如果您的笔记本电脑现在没电了，您需要多长时间才能将一台新电脑完全设置到正常工作状态？

对我来说，答案大概是 10-15 分钟的工作和 30 分钟的等待(咖啡休息时间！万岁。).

除非你运气特别差，否则你不需要经常更换你的工作笔记本电脑。但是如果你的笔记本电脑升级到新的型号呢？如果你的笔记本电脑坏了怎么办？如果你买了一台新的个人笔记本电脑会怎么样？如果你不得不做一件事不止一次，为什么不自动化呢？

这是我大约两年前经历的思考过程，从那以后，设置一台新的笔记本电脑就不再是一个问题。让我向您介绍我的过程，并向您展示如何自动化您的 OSX 设置。

## 先决条件

要使这一过程成功，需要做好几件事情:

*   您需要在笔记本电脑上登录您的 iCloud 帐户
*   你需要一个云存储服务(例如 Dropbox、iCloud、Google Drive)
*   您的引导脚本(我们将一起创建)和配置文件应该存储在这个云存储服务中。

就是这样！这就是使用此流程所需的全部内容。

## 过程

在我开始向您展示代码片段之前，我想先介绍一下我们正在努力实现的目标，以及我们将如何实现它。

我们将编写并运行一个 bash 脚本，它将:

*   检查`xcode`、&、`brew`是否安装，如果没有安装。
*   使用`brew`，我们还将安装一个名为`mas`的应用。此应用程序允许您通过 CLI 从 App Store 安装应用程序。
*   使用`brew`和`mas`安装我们想要的任何东西。
*   使用云存储服务上的配置文件配置笔记本电脑。

当我们完成时，我们所有的键绑定、别名、AWS 配置文件等…都将自动设置在我们的笔记本电脑上。新的笔记本电脑应该处于与之前的硬件相同的状态。

## 让我们开始编码吧！

现在你知道我们在做什么，编码将会很容易。我们从以下内容开始:

```
**#!/usr/bin/env bash** if [ -f ~/.osx-bootstrapped.txt ]; then
  cat << **EOF** ~/.osx-bootstrapped.txt FOUND!
This laptop has already been bootstrapped
Exiting. No changes were made.
**EOF** exit 0
fi

CURRDIR=`pwd`
BREWINSTALLED=`which brew`
XCODEINSTALLED=`which xcode-select`

# Install Xcode
if [[ ${XCODEINSTALLED} == "" ]]; then
  echo "Installing Xcode"
  xcode-select --install
fi

# Install Brew
if [[ ${BREWINSTALLED} == "" ]]; then
  echo "Installing Brew"
  ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
fi
```

我们做的第一件事是检查这个脚本以前是否被执行过，如果是，我们不希望它再次运行，并将优雅地退出。检查完成后，我们开始工作。

接下来，我们检查是否使用`which`命令安装了`xcode`和`brew`。如果在本地系统上找不到它们，我们将安装它们。

下一部分将因人而异，因为并非所有人都使用相同的应用程序。

```
#Required App
brew tap homebrew/cask
brew install mas#List your preferred applicationsbrew install --cask intellij-idea
brew install --cask google-chrome
brew install --cask slack
brew install --cask spotify
brew install --cask spectacle
brew install --cask karabiner-elementsbrew install jq
brew install awscli
brew install terraform
brew install packer
brew install docker-compose
brew install mysql-clientmas install 1295203466 #Remote Desktop
```

以上大部分都很容易理解。我们设置 brew tap/repo，并安装`mas`。除此之外，我正在安装我在笔记本电脑上使用的所有应用程序。我的清单要长得多，但是为了这个例子，上面的应该足够了。

## 节日

我想扩展的一件事是`mas`,因为这可能不是主流的工具。如前所述，mas 允许您使用 CLI 与 Apple App store 进行交互。这是我们将自动安装任何应用商店应用的机制。

一旦你在当前的笔记本电脑上安装了这个工具，你应该运行`mas list`。它将输出一个列表，列出所有安装在你的笔记本电脑上的应用程序及其 id。

```
➜ mas list | egrep 'Numbers|Key|Remote'
1295203466 Microsoft Remote Desktop (10.5.1)
409183694 Keynote (10.3.9)
409203825 Numbers (10.3.9)
```

有了这些 id，您现在可以通过运行`mas install {IDNUMBER}`来安装它们。关于 mas 的更多信息，请查看他们的 [Github repo](https://github.com/mas-cli/mas) 。

## 编码继续

既然我们已经讨论了 mas，并且安装了我们的应用程序，那么我们继续配置。和以前一样，这一步对每个人来说都是不同的，但是这应该会让你知道我们在努力做什么以及如何做。

```
# Symlink my configs
ln -s $CURRDIR/aws/ ~/.aws
ln -s $CURDDIR/bash_profile-config ~/.bash_profile

#Remove default karabiner dir since we are providing our own
rm -rf ~/.config/karabiner
ln -s $CURRDIR/karabiner-config ~/.config/karabiner# ln -s $CURRDIR/karabiner-config/karabiner.json ~/.config/karabiner/karabiner.json

# Remove Spectable default shorcuts json since we are providing our own
rm -f ~/Library/Application\ Support/Spectacle/Shortcuts.json
ln -s $CURRDIR/spectacle/Shortcuts.json ~/Library/Application\ Support/Spectacle/Shortcuts.json
```

因为所有的配置文件都在云中，所以这一步就像创建一个符号链接一样简单。您可以复制文件，而不是创建符号链接。然而，我更喜欢符号链接，因为这意味着我所有的笔记本电脑都使用相同的配置。如果我在我的一台笔记本电脑上添加别名，它将在我所有的笔记本电脑上可用。

我们的最后一步是安装‘哦，我的 Zsh’并创建我们的`osx-bootstrapped.txt`文件。

```
if [ ! -d ~/.oh-my-zsh ]; then
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    rm ~/.zshrc
    ln -s $CURRDIR/zshrc-config ~/.zshrc
fi

touch ~/.osx-bootstrapped.txt
```

你可以传递“哦，我的 Zsh ”,但是如果你还没有使用它，我强烈建议你去看看。

## 结论

我们都同意“宠物”是坏的，“羊”是好的。我们的笔记本电脑不应该有任何不同。鉴于笔记本电脑的重要性，这一点尤为重要。如果您按照上面的代码片段创建了自己的脚本，您就再也不用担心您的笔记本电脑会跑到天上的农场了。您还可以获得在所有笔记本电脑上获得一致体验的额外好处。只要 10 到 20 分钟的工作，你就能得到所有这些。非常值得投资回报！

为了获得无限的故事，你还可以考虑注册<https://blog.rhel.solutions/membership>**成为中等会员，只需 5 美元。如果您使用* [*我的链接*](https://blog.rhel.solutions/membership) *注册，我会收到一小笔佣金(无需您额外付费)。**