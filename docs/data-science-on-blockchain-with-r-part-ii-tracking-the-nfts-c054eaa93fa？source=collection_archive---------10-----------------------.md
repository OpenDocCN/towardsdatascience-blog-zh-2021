# r .区块链上的数据科学第二部分:跟踪 NFT

> 原文：<https://towardsdatascience.com/data-science-on-blockchain-with-r-part-ii-tracking-the-nfts-c054eaa93fa?source=collection_archive---------10----------------------->

## 关于节点和顶点的故事

![](img/6b3964f85d153380d3caee74161582b8.png)

怪异鲸鱼 NFT 的例子。这些 NFT(令牌 ids 525、564、618、645、816、1109、1523 和 2968)属于集合的创建者便雅悯·艾哈迈德(贝诺尼)，他允许我们在本文中展示它们。

托马斯·德·马尔钦和米拉娜·菲拉滕科娃

*Thomas 是 Pharmalex 的高级数据科学家。他对区块链技术让世界变得更美好的不可思议的可能性充满热情。你可以在* [*Linkedin*](https://www.linkedin.com/in/tdemarchin/) *或者*[*Twitter*](https://twitter.com/tdemarchin)*上联系他。*

*Milana 是 Pharmalex 的数据科学家。她对分析工具发现我们周围世界的真相和指导决策的力量充满热情。你可以在*[*Linkedin*](https://www.linkedin.com/in/mfilatenkova/)*上联系她。*

**此处** **提供这篇文章的 HTML 版本** [**。用来生成它的代码在我的**](https://tdemarchin.github.io/DataScienceOnBlockchainWithR-PartII//DataScienceOnBlockchainWithR-PartII.html)[**Github**](https://github.com/tdemarchin/DataScienceOnBlockchainWithR-PartII)**上有。**

# 1.介绍

***什么是区块链:*** 区块链是一个不断增长的记录列表，称为块，它们使用加密技术链接在一起。它用于记录交易、跟踪资产以及在参与方之间建立信任。区块链主要以比特币和加密货币应用而闻名，现已用于几乎所有领域，包括供应链、医疗保健、物流、身份管理……一些区块链是公共的，任何人都可以访问，而一些是私有的。数以百计的区块链都有自己的规范和应用:比特币、以太坊、Tezos……

***什么是 NFT:***不可替换的令牌用来表示唯一物品的所有权。他们让我们将艺术品、收藏品、专利甚至房地产等事物符号化。他们一次只能有一个正式的所有者，他们的所有权记录在区块链上是安全的，没有人可以修改或复制/粘贴一个新的 NFT。

***什么是 R:*** R 语言在统计学家和数据挖掘者中被广泛用于开发数据分析软件。

***什么是智能合约:*** 智能合约就是存储在区块链上的程序，当满足预定条件时运行。它们通常用于自动执行协议，以便所有参与者可以立即确定结果，而无需任何中介的参与或时间损失。他们还可以自动化工作流，在满足条件时触发下一个操作。智能合约用例的一个例子是彩票:人们在预定义的时间窗口内购买彩票，一旦到期，就会自动选出一名中奖者，并将钱转移到他的账户上，所有这一切都没有第三方的参与。

这是关于使用 r 与区块链进行交互的系列文章的第二篇。第一部分重点介绍与区块链相关的一些基本概念，包括如何阅读区块链数据。如果您还没有阅读，我强烈建议您阅读一下，以便熟悉我们在第二篇文章中使用的工具和术语:[第一部分](/data-science-on-blockchain-with-r-afaf09f7578c)。

听说加密货币受黑手党欢迎并不罕见，因为它是匿名和保密的。这只是部分正确。虽然我们不知道某个地址背后到底是谁，但每个人都可以看到该地址进行的交易。除非你非常小心，否则几乎有可能通过跨数据库来确定谁是该地址的幕后操纵者。现在有公司专门从事这项工作。这是通向一个更加透明和公平的世界的突破性道路。区块链有可能解决世界面临的与缺乏可追溯性和问责制相关的主要问题。以象牙海岸的可可文化为例。在过去的 25 年里，尽管农业综合企业正式参与到与森林砍伐的斗争中，该国已经失去了超过 60%的“受保护”森林。当前保护野生森林的策略效率低下的主要原因是追溯可可豆的原产地极其困难，现有的追溯解决方案很容易被操纵。区块链可以帮助解决这个问题。在制药业，区块链也有潜力改善药品生产行业的某些方面。例如，这项技术将提高制造过程和供应链的透明度，以及临床数据的管理。

本文涉及的主题是如何提取和可视化区块链事务。我们将探索 R 上的一些可用工具来做到这一点。为了避免与黑手党搅在一起，让我们把注意力集中在区块链技术的一个更良性但目前非常流行的应用上:艺术 NFTs。我们将更具体地观察怪异的鲸鱼。这里描述的方法当然可以扩展到存储在区块链上的任何东西。

怪异鲸鱼项目是一个由 3350 只鲸鱼组成的集合，这些鲸鱼是通过编程从组合的海洋中产生的，每只鲸鱼都有其独特的特征和特性:[https://weirdwhalesnft.com/](https://weirdwhalesnft.com/)。这个项目是由 12 岁的程序员便雅悯·阿迈德创建的，他在著名的 NFT 市场 OpenSea 上出售。根据这个感人的故事，3350 只计算机生成的怪异鲸鱼几乎立即销售一空，便雅悯在两个月内赚了 40 多万美元。鲸鱼最初以大约 60 美元的价格出售，但从那以后，它们的价格已经翻了 100 倍……阅读[这篇](https://www.cnbc.com/2021/08/25/12-year-old-coder-made-6-figures-selling-weird-whales-nfts.html)来了解这个令人难以置信的故事。

**这篇文章的 HTML 版本以及用来生成它的代码可以在我的** [**Github**](https://github.com/tdemarchin/DataScienceOnBlockchainWithR-PartII) **上找到。**

# 2 数据

该部分专用于下载销售数据，包括哪些代币被转移到哪个地址以及销售价格。**这是一个非常有趣的话题，但也有点技术性，因此如果您仅对数据分析感兴趣，可以跳过这一部分，直接进入第 3 部分。**

# 2.1 转让

所有智能合约都是不同的，理解它们以解码信息是很重要的。通过阅读源代码和使用像 EtherScan 这样的块探索者进行逆向工程通常是一个好的开始。这些怪异的鲸鱼由以太坊区块链的一个特殊的智能合同管理。这份合同存放在一个特定的地址，你可以在这里阅读它的代码[。](https://etherscan.io/address/0x96ed81c7f4406eff359e27bff6325dc3c9e042bd#code)

为了更容易地从区块链中提取信息，由于它在分类账中的存储方式，这可能相当复杂，我们可以读取事件。在 Solidity 中，用于在 Ehtereum 上编写智能合约的编程语言，事件被分派为智能合约可以触发的信号。任何连接到以太坊网络的 app 都可以监听这些事件，并据此采取行动。以下是最近的怪鲸事件列表:[https://ethers can . io/address/0x 96 ed 81 C7 f 4406 eff 359 e 27 BFF 6325 DC 3c 9 e 042 BD #事件](https://etherscan.io/address/0x96ed81c7f4406eff359e27bff6325dc3c9e042bd#events)

我们最感兴趣的是一种特定类型的事件:转移。每次发生令牌传输时，事件被写入区块链，其结构为:Transfer (index_topic_1 *地址从*，index_topic_2 *地址到*，index _ topic _ 3 uint 256*token id*)。正如其名称所示，该事件记录了令牌传输的起始地址、目的地址和令牌 ID，从 1 到 3350(因为生成了 3350 条怪异的鲸鱼)。

因此，我们将提取所有与怪异鲸鱼有关的转移事件。为此，我们过滤该事件的散列签名(也称为主题 0)。通过对 ethers can([https://ethers can . io/tx/0xa 677 CFC 3 b 4084 f 7 a 5 f 2e 5 db 5344720 bb 2c a2 c 0 Fe 8 f 29 c 26 b 2324 ad 8 c 8d 6 c 2 ba 3 # event log](https://etherscan.io/tx/0xa677cfc3b4084f7a5f2e5db5344720bb2ca2c0fe8f29c26b2324ad8c8d6c2ba3#eventlog))做一点逆向工程，我们看到这个事件的主题 0 是“0x ddf 252 ad 1 be 2c 89 b 69 C2 b 068 fc 378 DAA 952 ba 7 f 163 C4 a 11628 f 5

下面，我们概述了创建包含怪异鲸鱼贸易数据的数据库的过程。我们使用 EtherScan API 下载数据，更多详情参见[第一部分](/data-science-on-blockchain-with-r-afaf09f7578c)。EtherScan 的 API 被限制为每次调用 1000 个结果。这不足以分析怪异的鲸鱼交易，因为只有铸造(在区块链上创建令牌的过程)生成 3350 个交易(每个铸造的 NFT 一个交易)。而且这还不包括所有后续的转账！这就是为什么我们必须使用一个肮脏的 while 循环。请注意，如果您准备支付一点，有其他区块链数据库没有限制。例如，以太坊数据库可以在 Google BigQuery 上找到。

这是传输数据集的外观:

![](img/9d3833bc9e3e927ae342d9e1928c4d90.png)

# 2.2 销售价格

虽然转让和销售都可以由同一个合同管理，但在 OpenSea 上是以不同的方式完成的。销售由主要的 OpenSea 合同管理，如果它被批准(达到要求的价格)，第一个合同调用第二个 NFT 合同，这里是怪异鲸，然后触发转让。如果我们想知道 NFT 的销售价格(除了上面讨论的转让)，我们需要从主合同中提取数据([https://ethers can . io/address/0x7be 8076 f 4 ea 4a 4 ad 08075 c 2508 e 481d 6 c 946d 12 b](https://etherscan.io/address/0x7be8076f4ea4a4ad08075c2508e481d6c946d12b))。销售额由一个名为 *OrderMatch* 的事件记录。

请注意，这个循环可能需要一段时间来运行，因为我们下载了 OpenSea 上所有 NFT 销售的所有销售价格，而不仅仅是怪异的鲸鱼。鉴于在撰写本文时，OpenSea 上平均每分钟有 30 个事务，下载可能需要一段时间…这个代码块与上面的代码块非常相似，所以如果您不确定一行代码具体做什么，请阅读上面的代码注释。

如果我们查看 orderMatch 事件结构，我们会看到价格在数据字段中以 uint256 类型编码。它前面是另外两个字段，buyHash 和 sellHash，都是 bytes32 类型。uint256 和 bytes32 类型的长度都是 32 个字节，因此有 64 个十六进制字符。我们对 buyHash 和 sellHash 数据不感兴趣，只对价格销售感兴趣。因此，我们必须检索最后 64 个字符，并将它们转换成十进制，以获得销售价格。

![](img/1ed2e47e8e11eea01a9f4ed8654b1e19.png)

图 orderMatch 事件的结构

# 2.3 将两个事件结合起来

现在让我们通过怪异鲸鱼转账的交易散列来合并这两个数据集。

# 2.4 将 ETH 价格换算成美元

因为我们在区块链以太坊工作，所以交易价格是以以太坊给出的。以太币/美元汇率波动很大。如果我们试图将 ETH 转换为 USD，我们不能只应用乘法因子。因此，我们必须下载历史 ETH USD 价格。这一次，我们将从 Poloniex 交换下载数据，而不是 EtherScan(你需要一个专业帐户)。Poloniex 交易所提供免费的 ETH-USD 兑换服务。

我们将使用样条函数近似来平滑和插值转换率。这是因为交易事件的时间戳以秒为单位，而历史价格数据集的分辨率要低得多，它们平均每 30 分钟记录一次。因此，我们必须插入历史价格来涵盖交易事件。

![](img/815588f655d349494a56dc11168e0a72.png)

图 2:历史汇率。红线:样条。

现在，让我们使用交易时价格的内插值，将怪异鲸交易的 ETH 转换为美元。

# 2.5 最终数据集

请注意，如果您无法从 EtherScan 下载所有数据，您可以加载 github 上的数据集。

这是最终数据集的样子:

![](img/5798fcbddf2d7a40a227faea4c4f2a68.png)

# 3 分析

# 3.1 描述性统计

下面我们通过计算一些描述性统计数据对数据集进行总结:

![](img/bf70d25d1216aa99c2958bcaaf11b31f.png)

表 1:数据集内容的汇总统计。

另一个有趣的练习—我们可以确定每个地址的事务数量。第一个地址(0x00..)不是一个真实的地址—它指的是 NFT 的铸造。其中的交易数量就是在区块链上注册的 NFT 的总数。此外，我们看到一些地址在交易怪异鲸鱼方面相当活跃，因为它们已经参与了数百次交易！

![](img/55fe2fac4caef3ea4eed30af489298d8.png)

现在让我们把交易价格想象成时间的函数，而不考虑令牌 ID。在最初的几天，我们看到价格的高度可变性。接下来是一段相对平静的时期，我们在八月的最后几天看到了上升趋势的开始。随着怪异鲸鱼创造者的故事成为病毒，价格上涨恰逢社交媒体上的密集活动。一笔交易飙升至 25000 美元，与最初的 45 美元相比，这相当于增加了约 55555%!

![](img/5f7066ff773ef59002812637cd1e5e76.png)

图 3:怪异鲸 NFTs 交易的销售价格(美元)演变。

# 3.2 可视化网络

到目前为止，我们已经查看了交易价格摘要。现在，知道每个 NFT 都是独一无二的，需要与其他的区分开来，如何可视化事务呢？我们需要的工具是一个网络。我们收集的数据集非常适合绘制成网络。网络由顶点(或节点)和边(或链接)来描述。这里我们将对钱包地址使用节点表示。我们要构建的网络会显示所有曾经交易过怪鲸的钱包地址。顶点之间的连接，即所谓的边，将代表事务。

在 R 中有几个类和相应的包可以用来绘制网络，最著名的是*网络*和 *igraph* 。请查看参考资料部分，了解一些关于该主题的精彩教程。我个人偏好的是*网络*包，因为它提供了通过*网络动态*和 *ndtv* 包创建互动剧情的可能性。除此之外，还开发了其他的包来方便对这些对象的操作和绘制，例如*gg graph*包，它将熟悉的 *ggplot2* 框架引入到网络类中。

让我们试一试！我们将首先创建一个简单的静态(即没有时间维度)网络，并使用 *ggraph* 包绘制它。在一张图中显示的数据太多，因此我们将通过仅绘制涉及 7 个以上交易的 NFT 来划分数据子集。

我们现在可以绘制我们的网络。我们看到所有代币的所有交易都源自单个铸造地址(1)。一些地址涉及多个交易，这就是为什么我们看到这些交易有几个(弯曲的)边。我们还看到，一些令牌被传输到一个地址，但却被发送回发送方。

![](img/6749176af9e4645703c180a32413b953.png)

图 WheirdWhales NFTs 事务的静态网络。每个地址由一个节点(圆圈)表示，事务由边(线)表示。边缘颜色指的是令牌 ID。**这里有一个高分辨率的图可用**[](https://tdemarchin.github.io/DataScienceOnBlockchainWithR-PartII//DataScienceOnBlockchainWithR-PartII.html)****。****

**现在让我们使用事务的时间戳为我们的网络添加一个时间维度。这将允许我们可视化网络随时间的演变。为此，我们将使用令人惊叹的 *networkDynamic* 包。**

**我们可以创建一个时间线图来显示事务活动的频率。我们看到大多数(约 2/3)的交易发生在 NFT 诞生后不久。接下来是一个相对平静的时期，然后在 1000 点左右出现一个活动高峰。**

**![](img/ad6db19048a03332d199765ba9e52bca.png)**

**图 5:显示事务频率的时间线图。**

**下面，我们创建一个可以直接从浏览器启动的动画。可以点击边和顶点来显示关于相关地址和交易令牌的更多信息。**

**![](img/34fc46fb3a2beadc5436bee7f8f0f6ca.png)**

**图 6:whe ird wheels NFTs 交易的动画网络。每个地址由一个节点(圆圈)表示，事务由边(线)表示。边缘颜色指的是令牌 ID。可以点击边和顶点来显示关于相关地址和交易令牌的更多信息。**互动版可用** [**此处**](https://tdemarchin.github.io/DataScienceOnBlockchainWithR-PartII//DataScienceOnBlockchainWithR-PartII.html) **。****

# **4 结论**

**希望您喜欢阅读这篇文章，并且现在对如何可视化区块链交易有了更好的理解。这里，我们展示了一个如何下载和绘制与 NFTs 相关的交易网络的例子。我们看到，一旦有了数据，分析和绘制交易就很容易了。另一方面，以适当的格式获取数据需要对区块链有深刻的理解，尤其是在使用智能合同时。**

**在接下来的文章中，我们将探索泰佐斯区块链，一个新的进行非功能性交易的地方。我们还可以挖掘氦区块链，这是一种物理分散的无线区块链供电网络，用于物联网(IoT)设备。如果你想了解更多，请在 [Medium](https://tdemarchin.medium.com/) 、 [Linkedin](https://www.linkedin.com/in/tdemarchin/) 和/或 [Twitter](https://twitter.com/tdemarchin) 上关注我，这样你就会得到新文章发布的提醒。感谢您的阅读，如果您有任何问题或意见，请随时联系我们。**

**这篇文章的 HTML 版本带有高分辨率的数字和表格，可在[这里](https://tdemarchin.github.io/DataScienceOnBlockchainWithR-PartII//DataScienceOnBlockchainWithR-PartII.html)获得。用于生成它的代码可以在我的 [Github](https://github.com/tdemarchin/DataScienceOnBlockchainWithR-PartII) 上获得。**

**如果您希望帮助我们继续研究和撰写关于区块链的数据科学，请不要犹豫，向我们的以太坊地址(0 xf 5 fc 137 e 7428519969 a 52 c 710d 64406038319169)或 Tezos 地址(tz 1 ffzlhbu 9 adcobxmd 411 ufbdcvgrw 14 mbd)捐款。**

**敬请期待！**

# **5 参考文献**

**由 Etherscan.io 和 Poloniex APIs 支持:**

**[https://etherscan.io/](https://etherscan.io/)**

**[https://docs.poloniex.com/](https://docs.poloniex.com/)**

**常规:**

**[https://ethereum.org/en](https://ethereum.org/en)**

**[https://www.r-bloggers.com/](https://www.r-bloggers.com/)**

**网络:**

**[https://kateto.net/network-visualization](https://kateto.net/network-visualization)**

**[https://www.jessesadler.com/post/network-analysis-with-r/](https://www.jessesadler.com/post/network-analysis-with-r/)**

**[https://programminghistorian . org/en/lessons/temporal-network-analysis-with-r](https://programminghistorian.org/en/lessons/temporal-network-analysis-with-r)**

**[https://ggraph.data-imaginist.com/index.html](https://ggraph.data-imaginist.com/index.html)**