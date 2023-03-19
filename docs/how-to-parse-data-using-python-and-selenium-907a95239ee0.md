# 如何使用 Python 和 Selenium 解析数据

> 原文：<https://towardsdatascience.com/how-to-parse-data-using-python-and-selenium-907a95239ee0?source=collection_archive---------14----------------------->

## 使用分页从网站解析数据

![](img/b2fc1bef33044a36f1815c791d83002a.png)

[博伊图梅洛·菲特拉](https://unsplash.com/@writecodenow?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

以前我谈到过硒以及如何在 Ruby 中使用它。今天，我将向您展示如何在 Python 中使用 Selenium。如果你想了解硒的基本知识，可以看看下面的帖子。在那篇文章中，我简要介绍了什么是硒以及我们何时可以使用它。

</how-to-parse-data-using-ruby-and-selenium-5cf11605340c>  

通过这篇文章，您将了解如何使用 Python、Selenium 和 ChromeDriver 解析一些数据。

# 设置

对于这个设置，我们需要安装一个 Python [包](https://pypi.org/project/selenium/)和 [ChromeDriver](https://chromedriver.chromium.org/) 。另外，确保你的机器上安装了 Python [和](https://www.python.org/downloads/)。我用的是 macOS。您可以使用以下命令快速检查您运行的 Python 版本。

```
python --version
```

在我的例子中，我运行的是 Python 版本 3.8.8。现在，在您的终端中运行以下命令。这里我们使用[pip](https://pip.pypa.io/en/latest/)(Python 的包安装程序)来安装这个包。

```
pip install selenium
```

安装 Selenium 之后，我们需要安装 ChromeDriver。我正在用自制软件安装驱动程序。

```
brew install chromedriver
```

如果您在驱动程序方面遇到任何问题，以下是 macOS 的一些基本故障诊断步骤。

## 解决纷争

1.  无法打开 Chromedriver，因为无法验证开发者。

```
open /opt/homebrew/Caskroom/chromedriver
```

首先，在 finder 中打开 chromedriver 文件夹。之后，右键点击 chromedriver 文件，选择打开。这应该可以解决问题。

2.该版本 ChromeDriver 仅支持 Chrome 版本`#`当前浏览器版本为二进制路径的`#`。

我们需要确保 ChromeDriver 和 Chrome 浏览器的版本是相同的。

```
brew upgrade --cask chromedriver
```

太好了，我们完成了初始设置。现在你可以从 [GitHub](https://github.com/lifeparticle/Python-Cheatsheet/blob/master/scripts/web_scraper/web_scraper.py) 下载 Python 脚本。让我们来分解一下`web_scraper.py`文件的各个组成部分。

`web_scraper.py`

首先，我们添加了一个`headless`选项。然后我们用选项初始化一个 Chrome webdirver。现在，我们沿着页码导航 URL。之后，我们使用`find_elements_by_class_name`方法从页面中提取所有元素，最后，我们遍历所有元素，使用 `find_element_by_class_name`方法提取单个元素。现在，我们可以将页码增加 1，并重复相同的过程，直到没有数据需要解析。你可以从[这里](https://selenium-python.readthedocs.io/api.html)阅读所有可用的 WebDriver API。

如果你不提供`headless`选项，它将打开一个 chrome 浏览器并运行剩下的代码。如果这是您想要的行为，删除第 23、24 行，并修改第 25 行，如下所示。

```
driver = webdriver.Chrome()
```

我希望这篇文章能帮助你开始使用 Python、Selenium 和 ChromeDriver。如果你访问 toscrape 网站，你可以找到不同的浏览数据的方法。例如，我们可以使用无限滚动分页，而不是使用分页来导航页面。如果你有兴趣学习 Python，那么可以在 GitHub[上查看我的 Python cheatsheet。编码快乐！](https://github.com/lifeparticle/Python-Cheatsheet)

# 相关帖子

</how-to-parse-data-using-ruby-and-selenium-5cf11605340c> 