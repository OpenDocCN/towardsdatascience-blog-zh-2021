# 如何在 Python 中用 Click 制作漂亮的命令行界面

> 原文：<https://towardsdatascience.com/how-to-make-beautiful-command-line-interfaces-in-python-with-click-d6d744aad3eb?source=collection_archive---------18----------------------->

## 使用 Python 的 Click 制作令人惊叹的命令行界面概述

![](img/e0b13bddcf3d61795bd27d5b408fa169.png)

(图片由 [Pixabay](http://Pixabay.com) 上的 [OpenClipArtVectors](https://pixabay.com/images/id-148836/) 提供)

# 介绍

命令行界面，或 CLI，是程序员经常用来从终端或 REPL 控制他们的软件的一个重要工具。命令行界面的问题是，虽然它们并不特别难制作，但制作起来可能会有点乏味和烦人。此外，它们很难做得很好或完美。命令行界面经常会变得杂乱无章，这对于可能有用户也在与 CLI 交互的应用程序来说是个问题。不好看的东西只会更令人困惑，我们如此习惯于以一种方式消费内容、阅读或解释事物，以至于如果它们不符合那些规格，就会导致我们大脑中的小短路。

幸运的是，对于 Python 程序员来说，有一些非常棒的工具可以用来在 Python 内部创建增强的 CLI。这类工具的一个例子是名为 Click 的 Python 模块。我在另一篇关于 Python 中令人敬畏的装饰者的文章中列出了 Click，如果你也想阅读那篇文章，你可以在这里阅读:

</10-of-my-favorite-python-decorators-9f05c72d9e33>  

Click 模块是一个用于 Python 的可组合命令行工具包，由 Pallets 团队开发。如果您想了解有关托盘团队和 Click apparatus 的更多信息，您可以在此处查看 Click Github 页面:

<https://github.com/pallets/click>  

您可能不知道，但很可能您实际上也使用了托盘团队的另一个模块。他们还负责 Flask WSGI 模块，这是 Python 程序员的行业标准。不用说，这个团队对软件的贡献是显著的，我感谢他们对开源软件的承诺。

今天，我想浏览一下 click，看看该模块提供了什么。该模块功能齐全，非常健壮，所以这里需要解释一下。我们还将参与另一篇文章中的一个项目，以展示该包的功能。

# 开始一个项目

点击模块的应用可能适用于任何类型的用 Python 构建的应用。这是一件很棒的事情，因为在很多情况下，点击确实可以增加趣味。首先，我创建了一个漂亮的小目录来存储我们的 Python 项目。因为这里没有太多内容，而且我已经在 Python 的全局环境中安装了 click 模块，所以我不想为这个项目创建一个虚拟环境。

让我们编写文件的基础:

```
import click as clkdef main(): if __name__ == "main":
    main()
```

我们在这里所做的就是编写一个主函数，导入 click，然后做 if __name… Python 的事情来命名我们的主函数，因为我们可能会通过 CLI 调用这段代码。现在让我们来看看 click 模块实际包含了什么，这样我们就可以构建一些漂亮的命令行界面了！

## 为点击创造一些东西

为了利用 click，我们需要做的第一件事是制定一个明确的目标，并确定将通过 CLI 提供的用于控制该应用程序的参数。我想到了一个好主意

> 战斗模拟器

这将把随机性和力量考虑在内，并对战斗做出裁决。我认为这将是一个有趣的项目，因为可能会有很多这种类型的东西，也许我甚至可以为它开辟一个新的存储库，并在未来的文章中创建一些更酷的东西。假设我们正在创建一个战斗模拟器，我在这里要做的第一件事就是定义

> "顶级的等级制度。"

听我说，朱莉娅程序员和所有人。我的意思是我们需要一个

> “抽象类型…”

我们需要一个类，它将是其他类的父类。在这种时候，从一个范例到另一个范例真的会让你的大脑很难受。首先，让我们在我的游戏中加入一个视觉元素，一个 REPL 装订的印刷品和清晰的循环。我之所以要先这么做，是想看看这个想法是否行得通，这有助于我意识到自己是否需要一直贯彻下去。

> 我是说，这整件事毕竟是自发的。

## 我们的项目

虽然我不会详细介绍我是如何制作这个小游戏的，但您可能会在这篇文章中发现这一点:

</writing-a-command-line-interface-simulation-game-in-under-30-minutes-using-python-239934f34365>  

在本文中，我们主要评估点击模块的应用。也就是说，为了更好地理解这个项目，我还会展示一些类，做一些基本的解释来揭示游戏是如何工作的。要全面、深入地了解这款游戏以及我对其组件的编程，我当然会建议阅读上面的文章！此外，这里还有一个我今天要使用的 Github 库分支的链接:

<https://github.com/emmettgb/characterclash/tree/0.0.3-CLI>  

总之，这个程序的类结构是这样的:

*   play grid([players])
    play-grid 将获取一个玩家，并在上面打印一个玩家的网格。它接受玩家的完整列表，并管理视觉方面以及与环境中其他人物的交互。
*   玩家(位置，

我想我应该写几篇文章来介绍这个整洁的小项目的创建，因为我创建了一个完整的东西来把 CLI 放入其中。在未来，我们将训练一个模型来控制我们的玩家角色，但现在我们只是简单地添加一些命令行参数并稍微改变这个项目的代码，以便接受这些新数据并对其做一些事情。鉴于这基本上是一个我们编写的基于角色的 2d 游戏引擎，有一些全局定义的变量，并且肯定有很多我们可能希望能够从命令行改变的变量。我希望能够改变的主要论点是

*   网格宽度
*   网格高度
*   随机字符数
*   在未来，我想考虑让最终用户来设置角色。

事不宜迟，让我们试着用点击模块来完成这些目标吧！

# 使用点击

首先，我将调用 click @click.option()装饰器。我们将把下面所有的装饰器放在我们的主函数之上。

```
# CLIS
@clk.command()
@clk.option('--width', default = 30, help = 'Width of the draw grid.')
@click.option('--height', default = 100, help =' Height of draw grid.')
@clk.option('--players', prompt='Number to simulate',
              help='The number of randomly generated players to include.')
```

这可以采用各种关键字参数，但是位置参数当然是最重要的，它只是一个参数名。接下来，提供帮助信息和这种性质的东西。我们还可以提示用户输入一些值。我还需要删除将全局定义这些别名的以下代码:

```
CHAR_W = 30
CHAR_H = 100
rplayers = 20
```

我们需要将这些作为变量填充到整个包中，这有点麻烦，但是真的不会那么糟糕。我们的主要功能现在看起来像这样:

```
click.command()
[@click](http://twitter.com/click).option('--width', default = 30, help = 'Width of the draw grid.')
[@click](http://twitter.com/click).option('--height', default = 100, help =' Height of draw grid.')
[@click](http://twitter.com/click).option('--players', prompt='Number to simulate',
              help='The number of randomly generated players to include.')
def main():
    players = []
    players.append(Player([11, 20], 0))
    players.append(Player([5, 6], 1))
    players.append(Player([20, 10], 2))
    players.append(Player([11, 20], 0))
    players.append(Player([11, 20], 0))
    players.append(Player([11, 20], 0))
    players.append(Player([11, 20], 0))
    players.append(Player([11, 20], 0))
    players.append(Player([11, 20], 0))
    game = PlayGrid(players)
    for i in range(1, 50):
        sleep(.5)
        game.update("".join(["Iteration: ", str(i)]))
```

## 带着我们的 CLI

这里需要的是一个全面的 while 循环，它开始游戏并继续循环，直到所有的玩家都死了。我将继续这样做，尽管我们专注于 CLI，因为这将是我们最后一次需要调整这些参数，此外还有一些我希望在未来进行的升级。我们需要将所有命令行参数作为参数添加到主函数中:

```
def main(players, width, height):
```

我要做的第一件事是去掉这个大的 players.append()函数，它将被提取到一个迭代循环中，而不是在一个我还没有定义的新函数中，

```
players = random_players(players)
```

现在 main()函数看起来有点像这样:

```
def main(players, width, height):
    players = random_players()
    game = PlayGrid(players)
    for i in range(1, 50):
        sleep(.5)
        game.update("".join(["Iteration: ", str(i)]))
```

我们将继续下去，去掉这个 for 循环，只关注初始化。我们还需要改变在 update()函数中处理宽度/高度的方式。下面是这个函数的一个例子:

```
def update(self, message):
        clear()
        self.empty_grid()
        self.draw_players(self.grid)
        self.make_moves()
        print(self.draw_grid())
        print("\n" + message)
```

我们可以在这里添加网格的宽度和高度，因为每次绘制网格时都会调用这个函数。然而，我想我会将网格尺寸定义为全局变量。这将意味着即使是玩家也可以知道网格的大小，这可能是保持工作的关键。另一个很好的方法可能是把它放在类中，但是如果是这样的话，那么每当调用播放器的任何方法时，类的初始化版本都需要作为 self 传递。这当然是有意义的，但是考虑到这更多的是关于网格是窗口的维度的讨论，保持这样的东西是全局的可能是一个好主意，这样我们总是知道它的大小。鉴于这些是 CLI，它们将先于我们是全球性的，因此我们可以忽略这些参数。

```
players = random_players()
    game = PlayGrid(players)
    for i in range(1, 50):
        sleep(.5)
        game.update("".join(["Iteration: ", str(i)]))
```

现在这三件事情中的两件已经处理好了，我们唯一要做的事情就是创建一个基本的 while 循环，它将在玩家> 1 时执行所有操作。这样游戏就不会结束，直到一个玩家消灭了所有其他人，顺便说一下，这还没有实现——所以我们将永远看着这些无意义的战斗。

> 永远。

```
round_counter = 0
    while len(players) > 1:
        round_counter += 1
        sleep(.5)
        game.update("".join(["Iteration: ", round_counter]))
```

现在我们只需要编写我们的随机玩家函数:

```
def random_players(n_p):
    players = []
    for p in range(0, n_p):
        pos = [random.randrange(1, width), random.randrange(1, height)]
        player = Player(pos, p)
        players.append(player)
    return(players)
```

Player()构造函数需要一个位置和一个 ID。ID 是它在玩家列表中的索引位置。

现在让我们试着运行这个:

```
players
    for p in range(0, n_p):
TypeError: 'str' object cannot be interpreted as an integer
```

我们可以通过在传递该类型时简单地添加一个整数转换来解决这个问题。

```
def main(players, width, height):
    players = random_players(int(players))
```

> 打字强很棒。

我还将 width 和 height 重命名为 w 和 h，这样我们可以独立于 CLI 调整全局定义。这是整个包装的外观:

```
import click as clk
import random
from time import sleep
from os import system, name
width = 30
height = 100
# CLIS
[@clk](http://twitter.com/clk).command()
[@clk](http://twitter.com/clk).option('--w', default = 30, help = 'Width of the draw grid.')
[@clk](http://twitter.com/clk).option('--h', default = 100, help =' Height of draw grid.')
[@clk](http://twitter.com/clk).option('--players', prompt='Number to simulate',
              help='The number of randomly generated players to include.')
def main(players, w, h):
    players = random_players(int(players))
    game = PlayGrid(players)
    round_counter = 0
    width = w
    height = h
    while len(players) > 1:
        round_counter += 1
        sleep(.5)
        game.update("".join(["Iteration: ", round_counter]))
```

好吧，让我们试试这个！：

```
[emmac@fedora CharacterClash]$ python3 character_clash.py.......yers
    current = list(grid[newpos[1]])
KeyError: 40
[emmac@fedora CharacterClash]$
```

对于为什么现在会发生这种情况，我的最佳猜测是我们的范围混合了 x 和 y 值。有趣的是，我遇到了一个大问题，因为它们被翻转到绘制网格的函数内部:

```
def empty_grid(self):
        self.grid = dict()
        str = ""
        for row in range(1, height):
            str = ""
            for i in range(1, width):
                str += "`"
            self.grid[row] = str
```

现在我得到另一个类似的错误:

```
File "/home/emmac/dev/CharacterClash/character_clash.py", line 55, in draw_players
    current[newpos[1]] = player.symbol[0]IndexError: list assignment index out of range
```

我认为这里的问题可能与我在这里做的索引有关。应该是 newpos[0]而不是 newpos[1]。这一次我们错误地调用了 x，而我们应该调用 y。请允许我用代码中的一点引用来更好地解释一下:

```
current = list(grid[newpos[1]])
current[newpos[0]] = player.symbol[0]
current[newpos[0] + modifier] = player.symbol[1]
```

在这个函数中，current 是该列字符串中的一组字符。我们通过用 y 值调用网格字典来选择该列。现在，我们调用当前索引中的 x 值，以便获得位置，并用我们的字符符号替换它，希望得到合适的结果。

最后一件事，我们时常会遇到这个关键错误:

```
File "/home/emmac/dev/CharacterClash/character_clash.py", line 54, in draw_players
    current = list(grid[newpos[1]])
KeyError: 0
```

这个关键错误可能会让人认为 draw_players 函数没有正常工作，但实际上数据的格式不适合该函数——没有我们需要的网格的 0 索引。我们只需要改变填充这个字典的 for 循环:

```
def empty_grid(self):
        self.grid = dict()
        str = ""
        for row in range(1, height):
            str = ""
            for i in range(1, width):
                str += "`"
            self.grid[row] = str
```

用 0 替换 1，

```
for row in range(0, height):
            str = ""
            for i in range(0, width):
```

我们还需要在高度上加+ 1，这样所有的网格都会被填充。每当一个位置通过球员移动函数出现时，我们都需要检查并确保它不在界内。然而，我认为一个正确的方法是编写一些冲突，因为我们现在可以测试我们的 CLI，我认为我们应该这样做。每当我们运行新文件时，都会得到以下结果:

```
[emmac@fedora core]$ python3 character_clash.py
Number to simulate:
```

我现在会尽我所能得到每个循环的截图。为了帮助我做到这一点，我将把 sleep()计时器设置为 2 秒。接下来是屏幕上播放的动画 GIF:

```
[emmac@fedora core]$ python3 character_clash.py
Number to simulate: 5
```

![](img/bdfebecef2c9fbcd13b563f2dc9ae019.png)

(图片由作者提供)

# 我们拥有的

到目前为止，所有这些工作的结果都很酷，正如你在上面的动画中看到的。此外，打电话时，我们有调整高度和宽度的选项:

```
[emmac@fedora core]$ python3 character_clash.py --help
Usage: character_clash.py [OPTIONS]Options:
  --w INTEGER     Width of the draw grid.
  --h INTEGER     Height of draw grid.
  --players TEXT  The number of randomly generated players to include.
  --help          Show this message and exit.
[emmac@fedora core]$
```

如果玩家参数没有提供给我们，我们会提示用户。这非常简单，输出非常简洁。显然，还有很多事情要做。翻转并不完全正确，并且在某些区域的某些协调是错误的。此外，目前还没有任何人工智能暴力，但我很快会带来关于这个项目的另一篇文章，我们将完成这一部分，之后会有更多关于这个有趣的小项目的文章。现在，让我们再多谈谈点击。

# 为什么不是 ArgParse 或其他选项？

Click 的伟大之处在于，它为您提供了一个完整的数据模板打印输出，它会根据您的论点数据发出回声。此外，Click 包括所有不同类型的参数，甚至支持各种不同的调用。当然，我们可以用一些输入来启动 main()函数，但是将它解析为传入的 CLI 要酷得多。

不仅如此，这个项目的实际点击部分非常容易使用。这是另一个在 Python 中散布装饰魔法的模块，这是我非常喜欢的。所以，是的，我相信 Click 更容易一些，并且包含了很多非常好的组件，它允许你写密码，提示参数，提供选择，各种各样的其他选项所没有的东西。也就是说，它也是来自托盘，他们的软件可能值得一试。

# 结论

这是一个非常酷的项目，我很高兴看到它继续发展。这可能不是世界上最简单的选择，但它非常有趣，我认为我的方法非常棒。在做这个项目的过程中，我一直在想，如果有一个基于 REPL 的小引擎来为你处理所有的显示，那么你就可以编写具有视觉反馈等功能的真正交互式的 REPL，那该有多酷。这似乎是一个非常好的主意，我确信也许以前有人尝试过。

然而，老实说，如果我要写这样的东西，我可能会使用更多的类型，也用 Julia 来写。有时候项目抓住了我，我觉得我应该试着让它们变得很棒。非常感谢你的阅读，它对我来说意味着整个世界！希望这篇文章有助于让您的 CLI 变得生动，并且通过这个基于真实项目的示例，可能对如何在一些软件中实现这个模块有更多的理解。快乐编程，

> 还有(差不多)新年快乐！