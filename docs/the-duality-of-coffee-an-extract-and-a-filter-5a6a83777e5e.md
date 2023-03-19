# 咖啡的双重性:萃取物和过滤器

> 原文：<https://towardsdatascience.com/the-duality-of-coffee-an-extract-and-a-filter-5a6a83777e5e?source=collection_archive---------33----------------------->

## 咖啡数据科学

## 不能喝，但很有趣

之前，我尝试过通过将浓缩咖啡放回圆盘中提取更多的咖啡来给我的浓缩咖啡注入涡轮增压。相反，我发现用过或用过的咖啡渣会过滤咖啡。结果是浓度较低的浓缩咖啡。

> 我想更好地理解什么东西既可以用来提取浓缩咖啡，又可以用来过滤浓缩咖啡。

所以我设计了一个基于[断奏篡改](/staccato-tamping-improving-espresso-without-a-sifter-b22de5db28f6?source=your_stories_page-------------------------------------)镜头的实验。我一直在拉断奏捣实的镜头几乎有一年了，简单地说就是先捣实一半的咖啡渣，然后分别捣实另一半。我还在中间使用了[纸过滤器](/the-impact-of-paper-filters-on-espresso-cfaf6e047456?source=your_stories_page-------------------------------------)，这意味着拍摄完成后，冰球的顶部和底部很容易分开。

![](img/d09489ca80c096b8c8c0d2ff727a1a35.png)

不连续的篡改:通用流程图

## 试验设计

将咖啡渣放在用过的咖啡渣上，我们可以测量出有多少咖啡滞留在过滤中。

![](img/36cb7c044f0ec021d82b65339e2e5cfc.png)

# 制作用过的地面

这个实验的关键部分是喝没有什么可提取的咖啡。我拿了一个用过的冰球，用牙签混合，然后再次通过机器。

![](img/aff9303f617d2fd908248fb067df2694.png)![](img/cff1410681b43015d7a6a5389ff165bb.png)![](img/1e4828d0b354159170799391b33b94a7.png)

第二轮提取以取出剩余的所有东西。

然后我拿起那个冰球，在托盘上捣碎，放入烤箱低温烘干。

![](img/b6b5c64c7e6e2d0e8803f550adc081a7.png)![](img/a1dc20e9f33255a4438990b7d6477649.png)

我不得不用两个橡胶圆盘来得到 18 克用过的咖啡。对于这两个镜头，我提取了近 30%的干燥前。然后，它们在干燥过程中损失了 40%的重量，因此总重量损失了 50%。我不确定它是否有任何影响，因为每个冰球开始是 18 克干的，结果是 9 克废咖啡。

# 顶好

让我们拍些照片吧！我也拍了意大利香肠，因为我想知道 EY 的影响。这第一枪打得有些艰难，但我没有失去那么多液体。

![](img/2cdf3e752072b90576257c721b3e46e1.png)![](img/19f5125990ced5a9754e2ac297cb4cd0.png)![](img/64df093416df575fdae87a142bbd6451.png)

第一杯，向第二杯、第二杯过渡不良(第三杯未显示)

# 底部良好

底部变得更好，你可以看到初始液体变暗了。

![](img/41dfd83ee454ec3a6973371155a71a70.png)![](img/601d09f99cce50eb44920eb1fe5e9bd2.png)![](img/95d771255e009b945daee67aaae2589f.png)

第一杯，第一杯仍在运行，第二杯(第三杯未显示)

# 所有未使用的场地

全程拍摄进行得相当顺利。

![](img/4f0f1af3d425dbf5dfdf59ac56f51faa.png)![](img/ced68a4c4a0d3cf0d65d9574124918e0.png)![](img/9e0eaf36b8d6e7dfadb8f1cae577e3f7.png)

第一杯，第一杯仍在运行，第二杯(第三杯未显示)

# 实验数据

数据显示了一些主要问题，如地面在顶部还是在底部。由于溢出，最终的 EY 可能会低于顶部的货物，但对于第一杯来说肯定是准确的。我对两者使用了不同的 VST 篮筐，但我之前已经展示过[两个篮筐](https://link.medium.com/nMJEpnhLedb)在性能上没有区别(VST R 代表有脊，RL 代表无脊)。

![](img/3df9447ce0f0137e8e4214ee3e26c75a.png)

我绘制了数据并做出了最佳拟合曲线。提取是一个自然对数过程。底部的货物具有更高的提取率，表明正在进行过滤。

![](img/059a0d6609e9fab44597756d44a4a2e8.png)

我们可以结合这三者的最佳匹配，我估计如果这两个好的部分结合起来，完整的镜头应该是什么样的。事实证明，全杆趋势与底部杆的好。

![](img/1987722e14044e11be8be1b7e4b8c8f0.png)

一个挑战是，因为我每半场只使用 9g，很难控制镜头来决定何时使用另一个杯子。我甚至使用了精密秤，但是从一秒到下一秒的重量变化很难控制。出于这个原因，我宁愿在一台像样的浓缩咖啡机上做这个测试，而不是杠杆式咖啡机。

# 干燥圆盘

我还测量了干冰球。这只是测量最终萃取，但这样，我们可以看到是否有任何东西留在用过的咖啡中，或者是否有任何萃取物从顶部留在底部。

![](img/0fea644bb197b5de9ca5b67b541fc6a6.png)

我让所有两半分别干燥一周。底部的货物用的是顶部的地面，重量为 9.05 磅，所以有 0.56%的差异。他们应该没有保留任何可解物，所以我用这个小百分比来修正其他数据。

![](img/5e802ac8662c1d99cada2a6de6e39799.png)

然后我们可以看看一些趋势。完整的镜头比我想象的要均匀一半。

![](img/388a0844a7a6308b93edeb8921ea351a.png)

我本以为在一个完整的镜头会有一些过滤效果在下半部分，但它似乎没有这样。我还认为，如果下半部分是过滤咖啡，对于上面的好的情况，咖啡渣的下半部分应该在干燥后增加一些重量，但他们没有。

虽然我的许多实验看起来很奇怪，毫无关联，但我觉得它们已经汇聚到对浓缩咖啡内部运作的更深理解。

如果你愿意，可以在 Twitter 和 YouTube 上关注我，我会在那里发布不同机器上的浓缩咖啡视频和浓缩咖啡相关的东西。你也可以在 [LinkedIn](https://www.linkedin.com/in/robert-mckeon-aloe-01581595?source=post_page---------------------------) 上找到我。也可以关注我[中](https://towardsdatascience.com/@rmckeon/follow)。

# [我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):

[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)

[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)

[个人故事和关注点](https://rmckeon.medium.com/personal-stories-and-concerns-51bd8b3e63e6?source=your_stories_page-------------------------------------)

[乐高故事首页](https://rmckeon.medium.com/lego-story-splash-page-b91ba4f56bc7?source=your_stories_page-------------------------------------)

[摄影启动页面](https://rmckeon.medium.com/photography-splash-page-fe93297abc06?source=your_stories_page-------------------------------------)

[一款经济、简单的透明意式浓缩咖啡过滤器](https://medium.com/@rmckeon/a-cheap-transparent-portfilter-f60ee1824b52?source=your_stories_page-------------------------------------)

[浓缩咖啡过程中粉末不会迁移](/fines-dont-migrate-during-an-espresso-shot-1bb77e3252ca?source=your_stories_page-------------------------------------)

[浓缩咖啡捣固的收益递减](/the-diminishing-returns-of-tamping-for-espresso-cac289685059?source=your_stories_page-------------------------------------)

[浓缩咖啡可以涡轮增压吗？](/can-espresso-be-turbo-charged-2c5e619abdb8?source=your_stories_page-------------------------------------)

[浓缩咖啡浸泡测试](/espresso-soak-test-f73989d1faca?source=your_stories_page-------------------------------------)

[Kompresso 能得到 9 巴压力的浓缩咖啡吗？](/can-kompresso-get-9-bars-of-pressure-for-espresso-9aeff301b943?source=your_stories_page-------------------------------------)

[浓缩咖啡透明移动式过滤器实验](/experiments-with-a-transparent-portafilter-for-espresso-ad6b79fdd6b6?source=your_stories_page-------------------------------------)

[浓缩咖啡预湿，而非预浸](/espresso-pre-wetting-ecd9a895ed5f?source=your_stories_page-------------------------------------)