# 5 个用于网络安全的隐藏 Python 库

> 原文：<https://towardsdatascience.com/5-hidden-python-libraries-for-cyber-security-e83928777a95?source=collection_archive---------13----------------------->

## 这些图书馆会让你吃惊的

![](img/ae1edd1e9731553ac3cf468ddd557042.png)

来源:[像素](https://www.pexels.com/photo/bricks-wall-garden-door-1882/)

Python 现在是最流行和发展最快的编程语言之一。它的效用已经在人工智能和商业分析领域得到了证明。构建网络安全解决方案是技术的另一个重要应用。

Python 有一些惊人的库，可以用于网络安全。好的一面是，这些图书馆中的大部分目前都被用于网络安全领域。他们使用 python 是因为它简单易学且用户友好。

在他的文章中，我将分享一些使用 python 创建网络安全解决方案的有用库。

# 1. **Nmap**

Nmap 是一个开源工具分析器，广泛用于网络安全领域。该库使您能够将 Nmap 与您的 Python 脚本相集成，允许您利用 Nmap 的功能来扫描主机，然后与 Python 脚本中的结果进行交互。

Nmap 是系统管理员的绝佳工具，因为它专门通过修改 Nmap 扫描结果来自动执行扫描操作。Nmap 是 pen 测试人员用来分析扫描结果并针对主机发起定制攻击的工具。

## 如何安装

```
pip install python-nmap
```

你可以在 Nmap 的官方网站[这里](https://pypi.org/project/python-nmap/)了解更多。

# 2. **Scapy**

Scapy 是一个复杂的 Python 包，可以在渗透测试中用于扫描、探测、单元测试和跟踪路由。该计划的目的是嗅探，传输，分析和修改网络数据包。许多模块会忽略目标网络/主机没有响应的数据包。

另一方面，Scapy 通过生成一个额外的不匹配(未应答)数据包列表，向用户提供所有信息。除了探测数据包之外，Scapy 还可能向目标主机传输不正确的帧、注入 802.11 帧、解码 WEP 加密的 VOIP 数据包等等。

## 如何安装

```
pip install scapy
```

你可以在 Scapy 的官网[这里](https://scapy.readthedocs.io/en/latest/introduction.html)了解更多关于 Scapy 的信息。

# 3.**美汤**

数据收集是渗透测试的关键部分。渗透测试人员有时可能需要从 HTML/XML 站点提取数据。在大型项目中，从一开始就编写一个工具，甚至手动完成这个过程可能需要几个小时或几天。

Beautiful Soup 是一个 Python 模块，可用于自动化数据抓取操作。例如，库可以从 HTML 和 XML 文件中读取数据并解析它们。

## 如何安装

```
pip install beautifulsoup4
```

让我们来看看使用 Python 来使用漂亮的 Soup 的一瞥代码片段。

```
**from** **bs4** **import** BeautifulSoupsoup = BeautifulSoup(html_doc, 'html.parser')for tag in soup.find_all('b')
     print(tag.name)# b
```

你可以在美汤官网[这里](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)了解更多关于美汤的信息。

# 4.**密码术**

它包含高级配方和流行加密方法的简化网关，包括对称密码、消息摘要和密钥派生算法。底层的密码原语。这些通常是危险的，可能会被误用。

这一层被称为“危险材料”或“危险品”层，因为在这一层操作时存在潜在风险。这些可以在`cryptography.hazmat`包中找到，它们的解释总是包括一个警告。

## 如何安装

```
pip install cryptography
```

你可以在它的官网[这里](https://cryptography.io/en/latest/)了解更多关于密码学的知识。

# 5.亚拉

VirusTotal 的 Yara 是一个快速识别数据模式的工具。这就像一个增压版的 Ctrl+F。您可以提供字符串或正则表达式模式，以及是否应该满足一个条件或几个标准。

这个模块使得将 Yara 集成到你的脚本中变得简单。我们可以使用它从符合`yara`标准的 API 请求中提取数据。

## 如何安装

```
pip install yara-python
```

让我们来看一看使用 Python 来使用 Yara 的代码片段。

```
print(matches)
>>[foo]
```

你可以在雅拉官网[这里](https://yara.readthedocs.io/en/stable/yarapython.html)了解更多关于雅拉的信息。

# **结论**

给定的软件包对您的网络安全非常有用，将帮助您保护您的系统并有效地执行网络安全措施。此外，python 强大的库使它成为程序员的首选语言。

这些 Python 库/模块可以帮助开发人员创建最复杂的网络安全工具，而不必为产品的每个方面编写单独的代码。

> *在你走之前……*

如果你喜欢这篇文章，并且想继续关注更多关于 **Python &数据科学**的**精彩文章**——请点击这里[https://pranjalai.medium.com/membership](https://pranjalai.medium.com/membership)考虑成为一名中级会员。

请考虑使用[我的推荐链接](https://pranjalai.medium.com/membership)注册。通过这种方式，会员费的一部分归我，这激励我写更多关于 Python 和数据科学的令人兴奋的东西。

还有，可以随时订阅我的免费简讯: [**Pranjal 的简讯**](https://pranjalai.medium.com/subscribe) 。