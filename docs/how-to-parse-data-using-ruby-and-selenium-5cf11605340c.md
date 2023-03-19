# 如何使用 Ruby 和 Selenium 解析数据

> 原文：<https://towardsdatascience.com/how-to-parse-data-using-ruby-and-selenium-5cf11605340c?source=collection_archive---------19----------------------->

## 了解硒的基础知识

![](img/cedca91afe9dae3ea9bacfdde8af18b9.png)

[国立癌症研究所](https://unsplash.com/@nci?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

人工测试可能效率低下且容易出错。但是，它可以通过自动化消除。Selenium 是一个自动化测试的开源自动化工具。例如，我们可以用它来测试 web 应用程序。

在本文中，我们将使用 Selenium ( [Ruby](https://rubygems.org/gems/selenium-webdriver) 和 [Chrome](https://chromedriver.chromium.org/) 来解析数据。我们将使用一个名为`selenium-webdriver`的 ruby gem，它为 Selenium 和 ChromeDriver 提供 ruby 绑定来使用 Chrome web 浏览器。Selenium 拥有对 C#、Python、JavaScript 和 Java 的支持。而且支持的平台有 Firefox，Internet Explorer，Safari，Opera，Chrome，Edge。

# 设置

首先，我们需要安装一个 ruby gem 和 ChromeDriver。对于这个设置，我使用的是 macOS。如果你的机器上没有安装 Ruby，那么从[这里](https://www.ruby-lang.org/en/downloads/)获取。

在您的终端中运行以下命令。

```
gem install selenium-webdriver
```

现在，在您的终端中运行以下命令来安装 ChromeDriver。在这里，我使用[自制软件](https://brew.sh/)来安装驱动程序。

```
brew install chromedriver
```

如果你在初始设置时遇到任何问题，看看我的 [GitHub](https://github.com/lifeparticle/Ruby-Cheatsheet/tree/master/Scripts/web_scraper) 或者使用错误信息来解决问题。好了，我们可以走了。你可以从 [GitHub](https://raw.githubusercontent.com/lifeparticle/Ruby-Cheatsheet/master/Scripts/web_scraper/web_scraper.rb) 下载以下代码。让我们来分解一下`web_scraper.rb`文件的各个成分。在这篇文章中，我们将从名为 [toscrape](https://toscrape.com/) 的网站收集数据。这是一个网页抓取沙盒。

首先，我们用一个无头的 chrome 浏览器初始化我们的 selenium WebDriver。

```
def init_selenium_webdriver
 options = Selenium::WebDriver::Chrome::Options.new
 options.add_argument('--headless')
 return Selenium::WebDriver.for :chrome, options: options
end
```

现在，我们有了将用来解析数据的 URL。这里，我们从第一页开始。在循环内部，我们使用驱动程序、URL 和页码来导航网页。

```
def main
 driver = init_selenium_webdriver
 url = "http://quotes.toscrape.com/page/"
 page_no = 1
 loop do
  navigate(driver, url, page_no)
  data = get_data(driver, {class_name: 'quote'})
  parse_data(data, {text: {class_name: 'text'}, author: {class_name: 'author'}})
  page_no += 1
  break unless has_data?(data)
 end
 driver.quit
end
```

在`navigate`方法中，我们使用基本 URL 和页码构建最终的 URL。

```
def navigate(driver, url, page_no)
 driver.navigate.to URI.join(url, page_no.to_s)
end
```

我们使用`get_data`方法从`navigate`方法返回的数据中提取报价。`find_elements`方法将返回所有匹配给定参数的元素。

```
def get_data(driver, pattern)
 return driver.find_elements(pattern)
end
```

之后，我们遍历所有报价，使用`parse_data`方法在控制台中打印单个报价信息。`find_element`方法将返回匹配给定参数的第一个元素。你可以在这里阅读更多相关信息[。](https://www.selenium.dev/selenium/docs/api/rb/Selenium/WebDriver/SearchContext.html#find_element-instance_method)

```
def parse_data(data, pattern)
 data.each do |quote|
  text = quote.find_element(pattern[:text]).text
  author = quote.find_element(pattern[:author]).text
  puts "#{text} -- #{author}"
 end
end
```

完成此步骤后，我们将页码增加 1，并对页码 2 重复此过程。最后，我们将增加页码，直到没有需要解析的引号。

```
def has_data?(data)
 return data.size() > 0
end
```

太棒了，现在您知道如何使用 Selenium 解析数据了。继续尝试使用不同的导航技术解析数据，比如[滚动](https://quotes.toscrape.com/scroll)。我希望你从这篇文章中学到了一些新的东西。如果你有兴趣了解 Ruby，那么可以在 GitHub 上查看我的 Ruby Cheatsheet。编码快乐！

# 看看我关于 Ruby 的其他帖子

</useful-ruby-array-methods-to-manage-your-data-4d2813c63ccf>  </17-useful-ruby-string-methods-to-clean-and-format-your-data-9c9147ff87b9> 