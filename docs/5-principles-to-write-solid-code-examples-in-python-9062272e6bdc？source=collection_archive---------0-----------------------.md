# 编写可靠代码的 5 个原则

> 原文：<https://towardsdatascience.com/5-principles-to-write-solid-code-examples-in-python-9062272e6bdc?source=collection_archive---------0----------------------->

## [*小窍门*](https://towardsdatascience.com/tagged/tips-and-tricks)

## 借助于坚实的设计原则编写更好的代码的简短指南，用 Python 例子来说明。

作为一名刚刚开始工作的软件工程师，没有正式的计算机科学背景，我一直在努力提出合理的底层设计并以正确的方式构建代码。最初，它帮助我想出了一个需要遵循的 5 条原则的清单，我将在这篇文章中与你分享。

# 坚实的设计原则

SOLID 是 5 个面向对象设计原则集合的首字母缩写，大约 20 年前由 Robert C. Martin 首次提出概念，它们塑造了我们今天编写软件的方式。

![](img/d4246c039b5c2109517fed74fd420b8c.png)

照片由[在](https://unsplash.com/@thisisengineering?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) [Unsplash](/s/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上绘制

它们旨在帮助创建更简单、更容易理解、可维护和可扩展的代码。当一大群人在不断增长和发展的代码库上工作时，这变得非常重要，这些代码库通常由成千上万(如果不是数百万)行代码组成。这些原则是保持良好实践和编写更高质量代码的路标。

这些字母代表:

1.  单一责任原则
2.  开/关原则
3.  利斯科夫替代原理
4.  界面分离原理
5.  从属倒置原则

它们都是简单的概念，很容易掌握，但是在编写行业标准代码时非常有价值。

# 1.单一责任原则

> 一个类应该有且只有一个改变的理由。

这大概是最直观的原理，对于软件组件或者微服务也是如此。“只有一个改变的理由”可以被重新表述为“只有一个责任”。这使得代码更加健壮和灵活，对于其他人来说更容易理解，并且在更改现有代码时，您将避免一些意想不到的副作用。您还需要做更少的更改:一个类需要更改的独立原因越多，它需要更改的次数就越多。如果您有许多相互依赖的类，那么您需要做出的更改数量可能会呈指数级增长。你的类越复杂，就越难在不产生意外后果的情况下改变它们。

```
class Album:
    def __init__(self, name, artist, songs) -> None:
        self.name = name
        self.artist = artist
        self.songs = songs def add_song(self, song):
        self.songs.append(song) def remove_song(self, song):
        self.songs.remove(song)    def __str__(self) -> str:
        return f"Album {self.name} by {self.artist}\nTracklist:\n{self.songs}" # breaks the SRP
    def search_album_by_artist(self):
        """ Searching the database for other albums by same artist """
        pass
```

在上面的例子中，我创建了一个类`Album`。它存储专辑名称、艺术家和曲目列表，并可以操作专辑的内容，例如添加歌曲或删除。现在，如果我添加一个功能来搜索同一位艺术家的专辑，我就违反了单一责任原则。如果我决定以不同的方式存储专辑(例如，通过添加唱片标签或将曲目列表存储为曲目名称和长度的字典)，我的类将必须更改，如果我更改存储这些专辑的数据库(例如，我从 Excel 表移动到在线数据库)，我的类也需要更改。显然，这是两种不同的责任。相反，我应该创建一个与相册数据库交互的类。这可以通过按首字母、曲目数量等搜索专辑来扩展。(参见下一个原则)

```
# instead:
class AlbumBrowser:
    """ Class for browsing the Albums database"""
    def search_album_by_artist(self, albums, artist):
        pass

    def search_album_starting_with_letter(self, albums, letter):
        pass
```

一个警告:使类过于简单会使代码变得难以阅读，因为人们不得不遵循一长串相互传递的对象，并可能导致单方法类的代码库支离破碎。这个原则并不意味着每个类都应该像在一个方法中一样做*一件单一的事情*，而是*一个概念*。

# 2.开闭原理

> *软件实体(类、模块、功能等。)应该对扩展开放，但对修改关闭。*

这意味着我应该能够在不改变现有代码结构的情况下添加新功能，而是添加新代码。目标是尽可能少地更改现有的、经过测试的代码，以防止出现 bug，并且不得不重新测试所有东西。如果不遵循这个原则，结果可能是依赖类的一长串变化，现有特性的退化，以及不必要的测试时间。

下面的例子说明了这一点:

```
class Album:
    def __init__(self, name, artist, songs, genre):
        self.name = name
        self.artist = artist
        self.songs = songs
        self.genre = genre#before
class AlbumBrowser:
    def search_album_by_artist(self, albums, artist):
        return [album for album in albums if album.artist == artist] def search_album_by_genre(self, albums, genre):
        return [album for album in albums if album.genre == genre]
```

现在，如果我想按艺术家和流派搜索*，会发生什么？如果我加上*发布年份*会怎么样？我将不得不每次都编写新的函数(总共(准确地说是 2^n)-1)，而且数量呈指数增长。*

相反，我应该为我的规范定义一个具有公共接口的基类，然后为从基类继承该接口的每种类型的规范定义子类:

```
#after
class SearchBy:
    def is_matched(self, album):
        pass

class SearchByGenre(SearchBy):
    def __init__(self, genre):
        self.genre = genre def is_matched(self, album):
        return album.genre == self.genre

class SearchByArtist(SearchBy):
    def __init__(self, artist):
        self.artist = artist def is_matched(self, album):
        return album.artist == self.artist

class AlbumBrowser:
    def browse(self, albums, searchby):
        return [album for album in albums if searchby.is_matched(album)]
```

这允许我们在需要的时候用另一个类来扩展搜索(例如通过发布日期)。任何新的搜索类都需要满足 Searchby 定义的接口，所以当我们与现有代码交互时不会有意外。为了按条件浏览，我们现在需要首先创建一个 SearchBy 对象，并将其传递给 AlbumBrowser。

但是多重标准呢？我非常喜欢我在这个[设计模式 Udemy 课程](https://www.udemy.com/course/design-patterns-python/)中看到的这个解决方案。这允许用户通过`&`将浏览标准连接在一起:

```
#add __and__:
class SearchBy:
    def is_matched(self, album):
        pass

    def __and__(self, other):
        return AndSearchBy(self, other)class AndSearchBy(SearchBy):
    def __init__(self, searchby1, searchby2):
        self.searchby1 = searchby1
        self.searchby2 = searchby2 def is_matched(self, album):
        return self.searchby1.is_matched(album) and self.searchby2.is_matched(album)
```

这个`&`方法可能有点混乱，所以下面的例子演示了它的用法:

```
LAWoman = Album(
    name="L.A. Woman",
    artist="The Doors",
    songs=["Riders on the Storm"],
    genre="Rock",
)Trash = Album(
    name="Trash",
    artist="Alice Cooper",
    songs=["Poison"],
    genre="Rock",
)
albums = [LAWoman, Trash]# this creates the AndSearchBy object
my_search_criteria = SearchByGenre(genre="Rock") & SearchByArtist(
    artist="The Doors"
)browser = AlbumBrowser()
assert browser.browse(albums=albums, searchby=my_search_criteria) == [LAWoman]
# yay we found our album
```

# 3.利斯科夫替代原理

这个原则是由 Barbara Liskov 提出的，她非常正式地阐述了她的原则:

> *“设φ(x)是关于 t 类型的对象 x 的一个可证明的性质。那么φ(y)对于 S 类型的对象 y 应该是真的，其中 S 是 t 的子类型。”*

![](img/e997d8cf0902ead7800cad0e5b3615fe.png)

如果它看起来像鸭子，叫起来像鸭子，但它需要电池，你可能有错误的抽象。— 📸拉维·辛格

这意味着，如果我们有一个基类 T 和子类 S，您应该能够用子类 S 替换主类 T，而不会破坏代码。子类的接口应该和基类的接口一样，子类的行为应该和基类一样。

如果 T 中有一个方法在 S 中被覆盖，那么这两个方法应该接受相同的输入，并返回相同类型的输出。子类只能返回基类返回值的子集，但是它应该接受基类的所有输入。

在矩形和正方形的经典例子中，我们创建了一个 Rectangle 类，带有宽度和高度设置器。如果您有一个正方形，宽度设置器也需要调整高度，反之亦然，以保持正方形属性。这迫使我们做出选择:要么我们保留 Rectangle 类的实现，但是当您对它使用 setter 时 Square 不再是正方形，要么您更改 setter 以使正方形的高度和宽度相同。这可能会导致一些意想不到的行为，如果你有一个函数，调整你的形状的高度。

```
class Rectangle:
    def __init__(self, height, width):
        self._height = height
        self._width = width @property
    def width(self):
        return self._width @width.setter
    def width(self, value):
        self._width = value @property
    def height(self):
        return self._height @height.setter
    def height(self, value):
        self._height = value def get_area(self):
        return self._width * self._height class Square(Rectangle):
    def __init__(self, size):
        Rectangle.__init__(self, size, size) @Rectangle.width.setter
    def width(self, value):
        self._width = value
        self._height = value @Rectangle.height.setter
    def height(self, value):
        self._width = value
        self._height = value def get_squashed_height_area(Rectangle):
    Rectangle.height = 1
    area = Rectangle.get_area()
    return area rectangle = Rectangle(5, 5)
square = Square(5)assert get_squashed_height_area(rectangle) == 5  # expected 5
assert get_squashed_height_area(square) == 1  # expected 5
```

虽然这看起来没什么大不了的(当然你可以记住 sqaure 也改变宽度？！)，当函数更复杂时，或者当您使用其他代码时，这就成了一个更大的问题，只要假设子类的行为是相同的。

一个我非常喜欢的简短但直观的例子来自[圆-椭圆问题维基文章](https://en.wikipedia.org/wiki/Circle%E2%80%93ellipse_problem#Description):

```
class Person():
    def walkNorth(meters):
        pass
    def walkSouth(meters):
        passclass Prisoner(Person):
    def walkNorth(meters):
        pass 
    def walkSouth(meters):
        pass
```

显然，我们不能对囚犯实施步行方法，因为他们不能自由地向任意方向行走任意距离。我们不应该被允许在类上调用 walk 方法，接口是错误的。这就引出了我们的下一个原则…

# 4.界面分离原理

> “客户不应该被迫依赖他们不使用的接口。”

如果你有一个包含许多方法的基类，可能不是所有的子类都需要它们，可能只是少数。但是由于继承，您将能够在所有子类上调用这些方法，甚至在那些不需要它的子类上。这意味着大量未使用的、不需要的接口，当它们被意外调用时会导致错误。

这一原则旨在防止这种情况发生。我们应该把接口做得尽可能小，这样就不需要实现我们不需要的功能。我们应该把它们分成多个基类，而不是一个大的基类。它们应该只有对每个都有意义的方法，然后让我们的子类继承它们。

在下一个例子中，我们将使用抽象方法。抽象方法在基类中创建一个接口，该接口没有实现，但在从基类继承的每个子类中被强制实现。抽象方法本质上是加强一个接口。

```
class PlaySongs:
    def __init__(self, title):
        self.title = title def play_drums(self):
        print("Ba-dum ts") def play_guitar(self):
        print("*Soul-moving guitar solo*") def sing_lyrics(self):
        print("NaNaNaNa")# This class is fine, just changing the guitar and lyrics
class PlayRockSongs(PlaySongs): 
    def play_guitar(self):
        print("*Very metal guitar solo*") def sing_lyrics(self):
        print("I wanna rock and roll all night")# This breaks the ISP, we don't have lyrics 
class PlayInstrumentalSongs(PlaySongs):
    def sing_lyrics(self):
        raise Exception("No lyrics for instrumental songs")
```

相反，我们可以为歌唱和音乐分别开设一门课(假设在我们的例子中吉他和鼓总是一起出现，否则我们需要将它们分开更多，也许通过乐器。)这样，我们只有我们需要的接口，我们不能在器乐歌曲上调用 sing 歌词。

```
from abc import ABCMeta class PlaySongsLyrics:
    @abstractmethod
    def sing_lyrics(self, title):
        pass class PlaySongsMusic:
    @abstractmethod
    def play_guitar(self, title):
        pass @abstractmethod
    def play_drums(self, title):
        pass class PlayInstrumentalSong(PlaySongsMusic):
    def play_drums(self, title):
        print("Ba-dum ts") def play_guitar(self, title):
        print("*Soul-moving guitar solo*") class PlayRockSong(PlaySongsMusic, PlaySongsLyrics):
    def play_guitar(self):
        print("*Very metal guitar solo*") def sing_lyrics(self):
        print("I wanna rock and roll all night") def play_drums(self, title):
        print("Ba-dum ts")
```

# 5.从属倒置原则

最后一个原则是

> 高层模块不应该依赖低层模块。两者都应该依赖于抽象(例如接口)。
> 
> 抽象不应该依赖于细节。细节(具体实现)应该依赖于抽象

![](img/1da154a14970d19c6a82979bfcf5a3e6.png)

你会把灯直接焊在墙上的电线上吗？— 📸[亚什·帕特尔](https://unsplash.com/@yash44_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你的代码有定义良好的抽象接口，改变一个类的内部实现不会破坏你的代码。它与之交互的一个类不应该知道另一个类的内部工作方式，只要接口相同，它就不会受到影响。例如，更改您使用的数据库类型(SQL 或 NoSQL)或更改存储数据的数据结构(字典或列表)。

下面的例子说明了这一点，其中 ViewRockAlbums 明确地依赖于这样一个事实，即相册以一定的顺序存储在 AlbumStore 中的一个元组中。它应该不知道 Albumstore 的内部结构。现在，如果我们改变相册中元组的顺序，我们的代码就会中断。

```
class AlbumStore:
    albums = [] def add_album(self, name, artist, genre):
        self.albums.append((name, artist, genre)) class ViewRockAlbums:
    def __init__(self, album_store):
        for album in album_store.albums:
            if album[2] == "Rock":
                print(f"We have {album[0]} in store.")
```

相反，我们需要向 AlbumStore 添加一个抽象接口来隐藏细节，其他类可以调用这个接口。这应该像开闭原则中的例子那样完成，但是假设我们不关心通过任何其他方式过滤，我将只添加一个 filter_by_genre 方法。现在，如果我们有另一种类型的 AlbumStore，它决定以不同的方式存储相册，它需要为 filter_by_genre 实现相同的接口，以使 ViewRockAlbums 工作。

```
class GeneralAlbumStore:
    @abstractmethod
    def filter_by_genre(self, genre):
        passclass MyAlbumStore(GeneralAlbumStore):
    albums = [] def add_album(self, name, artist, genre):
        self.albums.append((name, artist, genre)) def filter_by_genre(self, genre):
        if album[2] == genre:
            yield album[0]class ViewRockAlbums:
    def __init__(self, album_store):
        for album_name in album_store.filter_by_genre("Rock"):
            print(f"We have {album_name} in store.")
```

# 结论

坚实的设计原则是编写可维护的、可扩展的和易于理解的代码的指南。下一次当你想到一个设计的时候，为了写出可靠的代码，把它们记在心里是值得的。在你的脑海中浏览这些字母，回忆每个字母的意思:

1.  单一责任原则
2.  开/关原则
3.  利斯科夫替代原理
4.  界面分离原理
5.  从属倒置原则

现在去一个接一个地让世界变得更美好吧！

![](img/24ab74f01ba00e92792ed57b75bd7b54.png)

马丁·w·柯斯特在 [Unsplash](/s/photos/happy-code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

**参考文献:**

[1]坚实的发展原则-在动机图片中—[https://los techies . com/derick Bailey/2009/02/11/SOLID-Development-Principles-In-motivative-Pictures/](https://lostechies.com/derickbailey/2009/02/11/solid-development-principles-in-motivational-pictures/)

[2] Udemy 设计模式课程—[https://www.udemy.com/course/design-patterns-python/](https://www.udemy.com/course/design-patterns-python/)

[3]坚实的设计原则—[https://ade vait . com/software/SOLID-Design-Principles-the-guide to-being-better-developers](https://adevait.com/software/solid-design-principles-the-guide-to-becoming-better-developers)