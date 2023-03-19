# 浓缩咖啡圆盘分析

> 原文：<https://towardsdatascience.com/espresso-puck-analysis-9c35b102c4f8?source=collection_archive---------11----------------------->

## 咖啡数据科学

## 咖啡失效分析的可视化数据之旅

关于投篮后冰球分析或冰球学的有用性有一些争论。通常这需要看冰球的顶部，偶尔我会看到冰球底部的照片。通常，它们很难解码。

我注意到由于[断奏捣固](https://link.medium.com/FfQSXgCb3cb)和[纸过滤器](https://link.medium.com/BYukoXDb3cb)，我的镜头变得更加复杂，它们被用于更好的故障分析。我通常在照片的底部和中间使用纸质过滤器，如果有[残留的咖啡渍](https://link.medium.com/stXNMpIb3cb)，我可以更好地猜测通道是从哪里开始的。

![](img/da1445f56116731d0e269394d62adf8c.png)![](img/731f953fb5882d4a6ef69a9502a4d65d.png)

咖啡上或所用的纸上的咖啡污渍或黑斑是相对于托盘中的其余咖啡的低流动性的指示。高流量区域或通道应该有较少的污点。

我们可以看一些例子，对我的数据集来说，非常引人注目的是，我有一个所有浓缩咖啡照片的视频。我希望通过这些例子，其他人能够更好地利用拍摄后的视觉线索来帮助理解问题所在。

> 你不能修正你观察不到的东西。

# 顶部过滤器的第一个例子

这一针在预输注期间开始得不太好。还有一大块地方没有咖啡流出来。通常，如果在预输注期间发生这种情况，我发现的唯一恢复的好方法是尝试低压下的[压力脉动](https://link.medium.com/Lh19VVjc3cb)。我定期使用压力脉冲，但通过使用较低的压力，有时你可以让流量返回。

![](img/1185024fbace0b8ecef0b2bfd6854453.png)![](img/c04169f7836f10fe4468e5392d36c5b4.png)![](img/c200124e7980fe29feff4f4568fc4f2b.png)![](img/3a7db4a805ef260ff293660be4466d04.png)

在观察圆盘时，我使用了一个顶部滤镜，两个中间滤镜，没有底部滤镜。两个中间的过滤器使得没有咖啡渣的照片更清晰。

后来，我确定顶部过滤器增加了侧通道，因为它允许水比通常情况下更快地流向两侧。在查看过滤篮上的咖啡污渍时，一个大点没有太多的咖啡，如上图所示，当其余部分变成棕色时，咖啡颜色变深。

![](img/e7144f594085a1cf63bda7ea23853bb4.png)![](img/6f233509347528158a594e0dc1123fc7.png)![](img/afd70903016a5473149c09f74dbb548f.png)

冰球的底部也有咖啡渍，在中间的过滤器中，很明显，在水流到冰球底部之前很久就发生了沟流。大月牙标志着流量不足。

![](img/0234454819f7ead47fba42f0b6b0fde3.png)![](img/f835cb911736856e73c4d1e5d32649fc.png)![](img/c99166a68d93cfff9db40f103de6cdb4.png)

# 单面拍摄

我们可以从另一个角度来看如何在冰球分析中看到这些影响。

![](img/f4b5b1e7e6fcc205269f07a2037191e7.png)![](img/5fac9adcd5f0e0a5649f4e894680908a.png)![](img/4d97216ed607f65bfc82cb6b8f8ff5af.png)![](img/5426b9a937cfed79bb12a0db0a77552c.png)![](img/f7865cab9a5826af244b5a3baf99ce99.png)

这张照片使用了一个顶部滤镜，两个中间滤镜，没有底部滤镜。中间的过滤器显示水流从两边和中间流下，停在一个甜甜圈里。中间看起来正常，因为[注射准备被设计为在中间](https://medium.com/swlh/simplifying-coffee-distribution-for-espresso-e58ac4351ba)具有较低的密度，以调整杠杆机器圆环问题。

![](img/aec80cf59693a4ab86723cee075b588d.png)![](img/44fa9f90154c25e16335b9161a7dc40f.png)![](img/1571decc6994fb518ad59aef911922a1.png)![](img/69beb12cb38853e2fa18f073b82b5d00.png)![](img/a36b2239433a07e9b46c74d095b32398.png)![](img/8cfbac6c41e320fd14bde5e453e3fbd0.png)

两个滤纸中间的黑色圆圈是圆盘的两半，表明通道一直通向顶部。

![](img/d5b283f985882936d9ac68b928372564.png)![](img/491396cfb282cfd2881ce0d27ffab05c.png)![](img/7ba6c0ee5f00318a265452cc6c60229e.png)

这个分析让我更仔细地看了看移除顶部过滤器。当时，我试图看看顶部过滤器是否可以改善结果。

# 用顶部过滤器夯实

在这个例子中，我使用了一个更大的过滤器，并把过滤器压在上面。然而，一个大甜甜圈出现了。

![](img/4fc0a56eef85504bfc4ac2bf693cceaa.png)![](img/828f25fa2326d8fe1d76c2f03efab64f.png)![](img/baf6e5d68d9aad1ab0b1dbc9748d90c5.png)![](img/c5074ba4523975f5c89577fffae8ac32.png)![](img/69510b81a2a1c6887428335f68d686d4.png)

夯实没有帮助。我以为水可能在过滤器上流动，但看起来淋浴屏幕的封条正好在纸的上面。所以水穿过纸流到边缘。

![](img/f8d6f508df3ffb4857ff32d4cafd363f.png)![](img/107322395fca05c2f7f249b583e4bdec.png)![](img/bce7f0ac3352b2468faf02d89a0b53df.png)![](img/65379a9b67a46d2f3e23e88df23cb7d2.png)![](img/cce6aafae96449624171145c6a206515.png)![](img/14ec8e95e7b8c178c526e5b1ea4a9324.png)

# 较小的顶部过滤器

为了测试这个想法，我把顶部的过滤器做得更小，这样它就不会一直到边缘。通灵没有那么糟糕，但是仍然有问题。

![](img/6ec623e9f18e11e6aeaad7b537c001cc.png)![](img/d61b02f4a1242dfb50ec5264f71c6083.png)![](img/adc434bbe5e0b599a667d7ff384f8414.png)![](img/15a5b550b2f633522f9c4ffa9669af0e.png)![](img/c55ffb9074824d2cb741765ec6d83744.png)

滤纸和圆盘上的颜色不是黑色的。甜甜圈效应并不完全是由顶部滤镜造成的，但这并没有帮助。

![](img/4278cf5e67f0577f0b7196fcb875edeb.png)![](img/fae084370719f64d6f0fd740de6887df.png)![](img/60bd310d9fbbe79cb8294ce1a94be4f4.png)![](img/53271ce0423012da689cb41a9ee9d519.png)![](img/53edf44d019301cf5ae942df17f36cb2.png)![](img/8f2ffa779b9a017005da7d4797f85117.png)![](img/44b63c7b3d5b3973ec1e689d9ff2524f.png)![](img/2ae03dabd991480c6dedf7dd04e0e37f.png)

顶部有一个较小的纸质过滤器有助于侧面保持固化，这表明有一个完整的纸质过滤器会使侧面沟道效应变得更糟。这可能不是正常夯实镜头的情况，但对于断奏夯实，顶层有一个轻得多的夯实。

# 拆除顶部过滤器

对于这个镜头，顶部提取更均匀。这可以通过触摸圆盘并找到较硬的位置或较密集的位置来看到。

![](img/43f8288aebcaaf3a5f875bd3c6bee3a4.png)![](img/e4820feb57c5522da2a38e97201f7886.png)![](img/e0e16b3f83eec37f999f8c67a66b2c2d.png)![](img/47eb9c014593ee72be46dcafdc7830e9.png)

仍然有一个黑色的甜甜圈，但提取率更高，味道更好。

![](img/9dd6369c8efd82daa6d61a45eb1aebe3.png)![](img/a722818b2aa8d6c97bfc6f327e338c31.png)![](img/2ddd4d6a7c3136257a00bb233b9e6115.png)![](img/3bc5a8eb0fa87bfc7b2fedd5b4860707.png)![](img/f5e8668cb3f9fc90aeacf6c439bab84a.png)![](img/72cc4c755ba1ee09a124729d6cf2727e.png)

这个冰球的分布很奇怪。结果，一半的咖啡豆是错误的肯尼亚咖啡豆。我用两粒咖啡豆做典型的烘焙，我[将各层分开](/deconstructed-coffee-split-roasting-grinding-and-layering-for-better-espresso-fd408c1ac535)，我用肯亚豆做了两次烘焙。所以在这张照片中，肯尼亚咖啡豆比其他的要新鲜一周(一周而不是两周)。这导致了一些通灵问题。

# 缩小下半部分

![](img/6a7bc2d132c14ddca4583b188554f77b.png)![](img/5e264251f3fc49c58399ac7d91d03489.png)![](img/3e7ea9c6321536a5a47c4969fbd78e6e.png)![](img/903d0d0260e0d7946daba349c9996efd.png)

我在底部减少到 7 克，在顶部减少到 11 克，这似乎使过滤器的提取更加均匀，但喷雾更多了。

![](img/146896591003a32141ae3d6d40a1e428.png)![](img/2055645e3cb737d78b8461fe299f30f3.png)![](img/888f71e106f17df88548b09099f1318c.png)![](img/14b68b95e58e1bf6e1daab4b845d5fa2.png)![](img/869be78d9baf29f65509e2b30a868a52.png)![](img/811b74488e3857f9c6afa93c28bf5abc.png)![](img/434060c3c530da3c70fcda48b3325199.png)![](img/e267fa45b344bfcbe673d0e57f07243b.png)

# 在底部添加纸质过滤器

底部有纸过滤器，镜头提取时间长得多，但味道大大改善了提取。

![](img/9aff37163fd3620330c80741f32169f2.png)![](img/e8934685c52dd31910c79717e12b0e1a.png)![](img/a50c7218a3b34e87d36c63e2ab33753c.png)![](img/46cd63047a67412ae25cd6e65291abe4.png)![](img/50143736e06896633465d3df536c96b4.png)![](img/e7e1d5922650f6bb68b21a7dab39a719.png)

冰球分析显示更好的流动，但分析更难，因为冰球不容易出来。顶部没有大的标记，因为拍摄结束和大的沟道效应，这意味着顶部的沟道效应是由底部引起的

![](img/f2e85b5e25bbd969e56b3affce30a256.png)![](img/dbe0fdd7eda60dfd76700a3dfa6a9c90.png)![](img/d32ffa0fd01cf6420923c325c6560ab6.png)![](img/957957d6e763f3bea8ff175ebb05f198.png)![](img/d9eab8267b8235af16fc98a7f4283930.png)![](img/dbacd9cc9ff8353afdb0b3e24575cf57.png)![](img/b52c76e95b5bbe1b1d23358e6f1842d0.png)![](img/dfe0716c0714e8d1e3a9570b2b117bab.png)

这个镜头的流程也很有趣。似乎流量随着预输注和输注的时间而减少。流量的大峰值是由于压力脉动，这就是为什么流量被平均用于分析。

![](img/988cae573359370d158ed31fd50f284d.png)

# 重复使用的纸质过滤器

下一张照片在提取方面是相似的，但是现在，底部的纸质过滤器造成了一个反向的环形，因为水流通过中心非常集中。这是因为纸质过滤器只需用水清洗即可重复使用。

![](img/faa084a4f71645e6c59e4c9345a5e716.png)

于是，我决定在镜头的下半部分做一个平坦分布，结果真的很可怕。

![](img/79a2072f5e9c895e01d1dfedd33a8e90.png)![](img/bcad19d824658937f0e9949cab6c7213.png)![](img/e12a0c882e6e02501aa55c5d3a215363.png)![](img/bcb0c8371469a2dc9f6e882ec761c40d.png)![](img/947eaa123ba67e115b9b17026e68a811.png)

EY 接近 10%,正如你在杯子和过滤篮中看到的，大量未乳化的油被提取出来。击球后冰球很湿。篮子里到处都是油。太难看了。

![](img/cfbd4d4a83b595d5ef4a2cf069fee3d9.png)![](img/61f6a079a7beca9868bae4ea5593448e.png)![](img/96d00e911118c52c87785a0ba0a0bd40.png)![](img/2e88cea39aebbee7465e8afef2144603.png)![](img/8a30f5fa6649e6e37beb564dc883f6ee.png)![](img/7233b82fb2937baf83957df024c74030.png)

# 用力夯实

我回到了一个更密集的外环分布，并夯实底部一半稍微更难在 300 克，而不是 200 克。它回到了一个平均流量和 21% EY。

![](img/5166d5dbc80d3c7da83040fae8b5fcca.png)![](img/d38a3d50cd2c52626f12024ac4c2ddc8.png)![](img/14d0c02e875817fd903bb22462b5a3f8.png)![](img/31483e27f44c19000f137f65f8b04083.png)![](img/d33e6d6956a1b635023d6560949a6864.png)

底部纸质过滤器的外侧仍然有一个有点暗的环，但我不确定这是通道还是液体在拍摄结束时沉淀的地方。镜头的上半部分均匀地分开，没有任何主要通道。

![](img/cdc0621926ba661db8db0461249fec0f.png)![](img/d2da53ea03b8f6df1b2449a41f2cb640.png)![](img/f1dcb4ee005df85b34daba6d714bc2fd.png)

# 一个弱点

下一个镜头有 19%的 EY，这似乎很低。从底部看，边上有一些黑点，但在中间的过滤器上，外环没有那么暗。这意味着问题只是在下半部分。也许侧面密度不均匀。

![](img/12d0ba0ccefbdd4f41b64908f9bc9087.png)![](img/e8324dda0b2963481f0fdd141e11c1f4.png)![](img/24d04b4536db9aeb2d8f33a5b90f2531.png)

在视频中发现了一个缓慢的斑点，它靠近底部过滤器上暗咖啡污渍的位置。

![](img/a9f625837ecbe8d3c2d5e38e9d95ff10.png)![](img/f0ef851dc9ca2adccdc3c28a674f5734.png)![](img/e420d8ff044d665553c2d007c3421a8c.png)![](img/c3223c7fda5c4beede85635d77826a9c.png)![](img/459285d640e6e02d146fc4e4d7e23188.png)![](img/74e5dfe3ffd00bbb5e97b59aaa6638e2.png)![](img/ea853f8df1494b348bf37bba384db4b6.png)

# 布料过滤器

我开始看布过滤器，我注意到中间的布过滤器引起了极快的流动。我被吹走了。

![](img/63784ea7ef37cb204576fc9a31c1dd94.png)![](img/d325c7c76aee068661c9ee3eb9f480db.png)![](img/ccb235eac01c5cb8a708f3e6257083ee.png)![](img/53dc93a0371c9dfe8a48e972c4968cc1.png)

它们更干净，而且似乎比纸质过滤器更容易重复使用。纸质过滤器上的一个黑点可能是一些过滤孔，咖啡无法通过。

![](img/feac5303d9b01c73effb8e78fcf2dccc.png)

我在拍摄前/后的电子表格、拍摄期间的视频和拍摄后的冰球图像中收集了大量数据。他们很好地指导我如何制作更好的浓缩咖啡，因为我对浓缩咖啡的功能有了更深的理解。这就是为什么当人们在网上成群结队地询问如何修复他们的镜头时，我总是要求提供视频和图片。

> 数据越多越好。

我希望这给感兴趣的人一个起点，帮助他们学习如何提高他们的投篮。

![](img/e46d6c8e80468910152a0db3b3000311.png)![](img/4b1d790ffd6c2629b2b706e20c1f5e46.png)![](img/fafc9b2de80176b7041ce51247e57e4e.png)![](img/404c81a230ebb2d0892d3a185890b060.png)![](img/2e3b0e0c236d7eed67ff7775685fe924.png)![](img/1c6a26a84e369220332ddb55ef214c68.png)![](img/167f186cc97eab78bf227df3c3ee3720.png)![](img/97b58f4d0be8e2edeabb7017c3b30476.png)![](img/abd85368c2eac2bd7c319d56a8b4fe4c.png)![](img/025921ef72a41a95e7f2f2975f11a33c.png)![](img/04097ad94ebe499d280e599b2d0962cb.png)![](img/12d0ba0ccefbdd4f41b64908f9bc9087.png)![](img/c1857ca61f7d99513b28af15d96015bb.png)![](img/04d32a86abec4648ef37069d331d811a.png)![](img/fc93688987897a88458b732320271dd2.png)![](img/5aca21b75747304aff4db4554507c2dc.png)![](img/5bce3eba6b60213504a0cbc31e5a3abe.png)![](img/6e6c460b421f34567269b07a880c2e76.png)![](img/d11aeebd0d4cd962f161088975887e0b.png)![](img/a3a02e09a2e4a157189c000559cebda1.png)![](img/c6eae3899fd848972bf09cf3e22762a8.png)![](img/fb04d4d3f0a859463410a3d93fa6f9cb.png)![](img/1d777ef149b10b974f2ecae9491c749f.png)![](img/6b9d13188eb926b455b136426a8bbbd6.png)![](img/26934452c7b2f531ff7c6444c3e66edd.png)![](img/1110291c5642d6c331088327649d0098.png)![](img/2c2f4f54083301ddae6e5d69e52b89cf.png)![](img/f35118eaff9b18d39bb5c739c37c530b.png)![](img/79b3485b04540aba0767ac01a132fc7c.png)![](img/e7ad6090aae5db46d941b7bb2b256f6d.png)

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事启动页面](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影启动页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[使用模式识别比较咖啡](/comparing-coffee-using-pattern-recognition-35b92cca4502)

[咖啡数据回顾:等级和口味](https://link.medium.com/1lDMQUH0Hbb)

[家庭烘焙咖啡的经济学](/the-economics-of-home-roasting-coffee-93003ea31ee8)

[咖啡豆脱气](/coffee-bean-degassing-d747c8a9d4c9)

[解构咖啡:分割烘焙、研磨、分层以获得更好的浓缩咖啡](/deconstructed-coffee-split-roasting-grinding-and-layering-for-better-espresso-fd408c1ac535)

[浓缩咖啡的预浸:更好的浓缩咖啡的视觉提示](/pre-infusion-for-espresso-visual-cues-for-better-espresso-c23b2542152e)

[咖啡的形状](/the-shape-of-coffee-fa87d3a67752)

[香辣浓缩咖啡:热磨，冷捣以获得更好的咖啡](/spicy-espresso-grind-hot-tamp-cold-36bb547211ef)

[断续浓缩咖啡:提升浓缩咖啡](https://link.medium.com/vmI2zVeQabb)

[用纸质过滤器改进浓缩咖啡](/the-impact-of-paper-filters-on-espresso-cfaf6e047456)

[浓缩咖啡中咖啡溶解度的初步研究](/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)

[断奏捣固:不用筛子改进浓缩咖啡](/staccato-tamping-improving-espresso-without-a-sifter-b22de5db28f6)

[更好的浓缩咖啡压力脉动](/pressure-pulsing-for-better-espresso-62f09362211d)

[咖啡数据表](https://towardsdatascience.com/@rmckeon/coffee-data-sheet-d95fd241e7f6)