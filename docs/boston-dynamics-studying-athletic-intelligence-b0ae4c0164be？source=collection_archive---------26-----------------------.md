# 波士顿动力学:研究运动智能

> 原文：<https://towardsdatascience.com/boston-dynamics-studying-athletic-intelligence-b0ae4c0164be?source=collection_archive---------26----------------------->

## 杂技舞蹈视频华而不实，但有哪些实际的技术突破？韩国机器人产业正在发生什么？

这家机器人公司擅长制作病毒式技术视频，展示机器人能做的[件小事](https://www.youtube.com/watch?v=VRm7oRCTkjE)、[跑酷](https://www.youtube.com/watch?v=_sBBaNYex3E)、[欺凌](https://www.youtube.com/watch?v=aFuA50H9uek)、[机器人](https://www.youtube.com/watch?v=aFuA50H9uek)，以及[更多](https://www.youtube.com/channel/UC7vVhkEfw4nOGp8TyDk7RcQ)。波士顿动力公司的一个核心原则是运动智能的理念——强健、灵活，甚至可能是人类的运动模式。这些视频和技术已经到了最受欢迎的技术艺人得到一份拷贝并评论它的地步，它们是待售的，并且是可访问的。最近的视频试图展示一种新的*人类*运动方式(下图)。

他们对运动智能的关注真的帮助我了解了这家公司，它在哪里适合他们的视频，以及为什么老板不留下来。波士顿动力公司使用机器学习和人工智能作为工程堆栈中的工具，而不是将其扔向他们遇到的每个子问题。

在本帖中，我们:

*   *理清波士顿动力公司的历史(它不是政府承包商)*，
*   *展示他们所谓的突破* ***竞技智能*** ，
*   和*讨论现代子公司*的下一步计划。

# 什么是波士顿动力

[Boston Dynamics 在 2014 年放弃了所有 DARPA 的合同(它们是由 DARPA 种子基金启动的，但我们使用的大量](https://cdn.substack.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fe011eff4-1982-4ca1-8941-c8a46582f92a_442x114.png) [技术是](https://www.darpa.mil/about-us/advancing-national-security-through-fundamental-research)！)他们有着有点复杂的买家和卖家历史，包括谷歌、软银和现在的现代——我认为这主要是他们的长期目标和市场的短期价值之间的脱节。波士顿动力公司的长期目标是创建一个运动智能的演示，并将其出售给能够解决其他认知问题(规划、后勤、互动)的人。

运动智能的目标是使机器人学中鲁棒性和速度的结合成为可能。如果这对你来说没有什么印象，看看一些[历史上最好的机器人团队参赛的例子](https://www.youtube.com/watch?v=g0TaYhjpOfo)。

![](img/6319537d49d7279ebb51c5a264d73f28.png)

来源——波士顿动力[博客](http://blog.bostondynamics.com/how-to-gather-better-data-and-reduce-dosage-in-nuclear-facilities)。

我经常听到(我也延续了这种想法的一部分)他们的技术部分是由军事应用资助的，完全是为军事应用设计的。虽然他们的机器人在自己的环境中似乎很健壮，但他们并没有满足 21 世纪相反战争的许多趋势:远程、隐形和受损时的健壮(他们的机器人很吵——那些大型液压致动器不会偷偷靠近你！).要了解完整的历史，请查看维基百科[、](https://en.wikipedia.org/wiki/Boston_Dynamics)[他们的关于页面](https://www.bostondynamics.com/about#Q2)，另一篇[更长的历史](https://techstory.in/the-boston-dynamics-story/)文章，或者波士顿报纸 [Raibert](https://www.boston.com/news/technology/2021/01/21/boston-dynamics-dancing-robots-video) 的更多个人评论。

总而言之，他们一直专注于通过联合开发硬件和软件能力来推动腿式运动极限的目标。他们一直在纠结如何将这种进步货币化。

## 运动智能与机器智能

在这一部分，我将重点介绍他们的神奇是如何发生的。他们所做的硬件开发是一流的，但是机器智能中的 [部分有可能扩展到更多的平台](https://democraticrobots.substack.com/p/robotics-take-two)，所以我关注它。简而言之，与机器人研究的状态相比，他们的学习和控制基础设施不是革命性的，但它是发达的和高度功能化的。让我们期待波士顿动力公司的未来，他们的控制方法不要太依赖于他们的硬件平台，他们可以重复控制工程。

广义而言，运动智能首先需要发展低水平的控制能力和运动灵活性。这种灵活的机器人运动**是公司价值的核心**。他们研究如何优化液压等机械部件，以提高控制性能。他们精确集成所需的传感器(激光雷达、电机编码器、IMU 等。)来解决任务和许多不同的计算。

在 NeurIPs 2020 [真实世界 RL 研讨会](https://sites.google.com/view/neurips2020rwrl)上，他们展示了他们的高水平方法(视频[此处](https://bostondynamics1.app.box.com/s/w4tpxw3rb0jx7co9lyn9clxz42rtx028))。关于控制方法的幻灯片总结了他们的工具集:基于模型、预计算和细致的工程设计(这是研究实验室所缺少的)。

[ [将](https://cdn.substack.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F5c6efa92-1391-4df6-bf3b-64007e7b6097_3360x2100.png)链接到我正在引用的幻灯片]

## 基于模型的

*所有的模型都是错的，但有些是有用的*【[乔治盒子](https://en.wikipedia.org/wiki/George_E._P._Box)这里是他们幻灯片的主旨。

事实上，这是一种有趣的相互作用，因为他们可以非常详细地模拟他们机器人的某些方面(例如，一种类型的液压致动器)，但当他们开始将碎片放在一起时，累积的模型有缺口——并且不可能知道确切的位置。

模型也是一个很好的工具，人类将参与其中(可以解释结果)，但没有法律说基于模型的方法更有效。他们将它与幻灯片上的第二点紧密结合。

## 导航预行计算

基于模型的是配对、预计算，它们共同构成了模拟的工具。模拟是机器人技术的未来(比运行硬件实验要便宜得多，尤其是在流行病中)，他们似乎已经充分利用了这一点。

现实世界 RL 研讨会上的对话强调了他们利用模型预测控制(MPC)的程度，这是目前已知的计算量最大的控制方法之一。不过，最近的工作已经转向离线求解 MPC，然后在线运行一个更简单的版本(他们没有证实这一点，但答案至少指向概念上类似的东西)。在实践中，这变成了:从真实的机器人试验中收集大量状态，然后离线求解每个可能状态下的最优动作，并运行读取这些动作的控制器(非常快)。

## 严格记录

他们在这里用数据驱动来表示一种不同的东西，“数据不会说谎”，这其实在 EE 和 ME 圈子里要常见得多(深度学习前的炒作，啾啾)。

我认为 RL 的许多领域可以从分析每一次试验中到底发生了什么中学到更多。知道代理是错的是一回事，但是弄清楚为什么它是错的以及错误如何随着时间传播是非常重要的。波士顿动力首先不是一家机器学习公司，这与他们的方法相匹配。

## 让新的机器人跳舞

值得说明的是，他们如何使用动作捕捉、模仿学习和丰富的模拟来制作他们最近的视频。为了更详细地阐述下图中的内容，他们让人类演员在录制环境中创建运动原语(或在物理模拟器中指定运动，这看起来不是很运动)，使用模仿学习的技术来生成与运动匹配的策略，然后正确设置模拟器，以便新策略可以轻松地转移到机器人。

[行为生成[幻灯片](https://cdn.substack.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F95c279b1-06e2-4ec5-9532-13bf05eb35c0_3360x2100.png)

[出自](https://cdn.substack.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F95c279b1-06e2-4ec5-9532-13bf05eb35c0_3360x2100.png) [IEEE 谱](https://spectrum.ieee.org/automaton/robotics/humanoids/how-boston-dynamics-taught-its-robots-to-dance):

> 最终，我们能够利用这个工具链在一天之内，也就是我们拍摄的前一天，创作出 Atlas 的一个芭蕾动作，并且成功了。所以这不是手写脚本或手写代码，而是有一个管道，让你可以采取一系列不同的运动，你可以通过各种不同的输入来描述，并推动它们通过机器人。

对于更大的图景，我只有一个想法:针对特定的行业约定，客户想要的预编程动作。你希望你的机器人能够拾取特定尺寸的包裹——让我们看看我们能否**验证**这个动作并更新你的机器人。有一个框架，设计者对输出足够有信心，他们可以通过软件更新来发布它，这听起来很像为什么这么多人喜欢他们的 Teslas(硬件上的软件更新实际上并不存在)。

他们在这些运动上投入了大量的工程时间。更值得思考的是更大的图片动机是什么(它不可能只是视频)。如果他们试图以任何人机交互的形式销售机器人，一套更丰富的动作可能非常有价值，但迄今为止没有任何迹象表明这一点。人类风格的舞蹈很可能会推动一个包的极限，也许是他们在 2 月 2 日宣布的新包。

## 艺术重点和恐怖谷

让机器人看起来像人类是一件值得警惕的事情。当然，人类的运动是动态的、优雅的、非常高效的，但这并不意味着它在不同的致动器下总是最优的。人类风格也会带来巨大的文化包袱和意想不到的影响。

这并不是说我反对具有人类**功能**的机器人——社会是围绕具有我们的腿和身高的生物构建的，这些生物可以轻松地在建筑物中导航并解决日常任务。我建议并希望机器人能够模仿人类的运动模式(比如使用两条腿，或者拥有带手和腕关节的手臂)，但不以完全模仿为目标。

如果你对此感到好奇，请阅读我在恐怖谷的文章。

![](img/622a336f4af60dc575aa1e57c687ef67.png)

照片由 Unsplash 提供。

# 利用更多的学习

有学习机会的 [**滑梯**](https://cdn.substack.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F5d2502b7-1d26-4012-bfd6-1f04004f044e_1654x932.png) 正在点上。持续学习、模型精度改进、基于学习的硬件优化和工程协助都是当今学习可以帮助解决的问题。

1.  持续学习非常适合无模型学习——这是一个旨在从稀疏回报中学习的系统，没有转移到其他任务的目标。
2.  现实世界中的模型精度归结为将数据拟合到扰动，或者他们已经优化以获得初始敏捷性的动力学模型与来自机载传感器的观测值之间的差异。模拟和物理来获得初始模型、数据，以及机器学习来更新它以匹配最近的现实。
3.  用 ML 进行硬件优化:人类真的不擅长拟合多目标函数。当你在制造物理设备时，成本会随着材料和时间的增加而增加，让机器为你做这件事([我正在做的事情](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2Fnatolambert%2FJNRgSdmxXP.pdf?alt=media&token=da95c4f0-ccf6-4d80-bf76-7d84dd11a71f))。
4.  工科出身。我喜欢他们的术语，我认为这只是填补裂缝的*可以这么说，也是对学习**可以**有用的认可，即使对一家如此注重实际交付成果的公司来说。*

# *波士顿(现代)动力公司的下一步是什么？*

*南韩有一套 [文化价值观](http://www.bbc.com/travel/story/20171205-why-south-korea-is-an-ideal-breeding-ground-for-robots)使得机器人代理在社会多个领域的扩散时机成熟。一份[对来自](https://internetofbusiness.com/south-korea-automated-nation-earth-says-report-uk-nowhere-robotics/#:~:text=South%20Korea%20has%20by%20far,eight%20times%20the%20global%20average.&text=The%20US%20has%20189%20robots%20per%2010%2C000%20employees%2C%20placing%20it%20seventh.)[国际机器人联合会](https://ifr.org/)的报告(我知道，双重引用，令人痛苦，但做机器人行业研究也是如此——这都是隐藏的)的分析得出结论:*

> *韩国是世界上自动化程度最高的国家。*

## *韩国精神&机器人*

*更有趣的是，正如一位韩国工程师[声称](http://www.bbc.com/travel/story/20171205-why-south-korea-is-an-ideal-breeding-ground-for-robots) s:*

> *任何一种非人类都可能有超越人类能力的精神或超能力。*

*这只是一种观点，但历史指向一套对其他存在更开放、更乐观的价值观。这种感觉肯定是基于动物的，但一小部分转化为机器人可以实质性地重构社会。*

*现代韩国的第一位国王丹根(Dangun)的故事，以及韩国对动物和潜在的非人类灵性生物的喜爱是奇妙的:*

> *一只老虎和一只熊向万隆祈祷，希望他们能变成人类。听到他们的祈祷，万隆给了他们二十瓣大蒜和一捆艾蒿，命令他们只吃这种神圣的食物，并且在 100 天内不要晒太阳。大约二十天后，老虎放弃了，离开了山洞。然而，熊坚持下来了，被变成了一个女人。据说熊和老虎代表了两个寻求天帝恩宠的部落。*
> 
> *The bear-woman (Ungnyeo; 웅녀/ Hanja: 熊女) was grateful and made offerings to Hwanung. However, she lacked a husband, and soon became sad and prayed beneath a “divine birch” tree (Korean: 신단수; Hanja: 神檀樹; RR: shindansu) to be blessed with a child. Hwanung, moved by her prayers, took her for his wife and soon she gave birth to a son named Dangun Wanggeom.*

*由此，韩国对非人类生物以及它们所能带来的价值有了更多的开放。这是一个多么伟大的变化——美国人无法摆脱社交媒体监管的泥潭，并认为他们的发言者在听。*

## *韩国实用性和机器人*

*[Hyuandai 最近收购了 Boston Dynamics](https://www.therobotreport.com/hyundai-acquires-boston-dynamics-for-921m/) (来自[《The Verge】](https://www.theverge.com/2020/12/11/22167835/hyundai-boston-dynamics-aquisition-consumer-robotics))和[《彭博》显示该公司并未持续盈利](https://www.bloomberg.com/news/articles/2020-11-17/boston-dynamics-needs-to-start-making-money-off-its-robots#lazy-img-366074900:~:text=It%E2%80%99s%20had%20bouts%20of%20profitability%20over,it%20did%20its%20previous%20owner%2C%20Google.)。重复销售和低盈利对一个公司来说不是一个好兆头(也许加入软银的基金对公司来说是一个坏兆头)，那么下一步是什么呢？*

*那么现代为什么要买它们呢？使用机器人进行生产。这是一个大转变，我认为大多数人都没有预见到(浮华、敏捷的机器人乍一看最适合高端消费者和利基产品)。[现代重型机械在 2020 年进入市场销售多种机器人](https://control.com/news/a-look-into-hyundai-robotics-latest-efforts-in-automation/)，他们自称[韩国头号机器人制造商](http://www.hyundai-holdings.com/hhiholdings)。一个重要的背景是，韩国已经拥有了高密度的工业机器人(见[关于韩国机器人产业的背景](https://roboticsandautomationnews.com/2020/02/03/south-korea-reaches-new-record-of-300000-industrial-robots-in-operation/29454/))。*

*波士顿动力公司在 Spot 和 Atlas 中创造的运动原型真的是人们可以建立的东西。制造业中的运动智能转化为强壮，这是非常有价值的。它只是没有敏捷性那么令人兴奋。*

## *新产品*

*2 月 2 日有一个新的[产品线公告](https://www.youtube.com/watch?v=WvTdNwyADZc)扩展 Spot 的产品。我预计这将是硬件的变体(想想不同的手臂附件，而不仅仅是视频中展示的单臂)和不同的软件包，可以解决探索或更复杂的运动挑战等任务。*

*在采访和产品中，波士顿动力公司看起来更像是一个研究实验室，在推动腿式运动的软硬件协同设计的极限。我喜欢他们为这个领域所做的工作，但实际上，我希望在几年内有更多的盈利，也许随着他们成为一个行业研究实验室，宣传会更少。*

*![](img/98d7a88a3295028291db72856ae453ce.png)*

*斑点在这种冰里会怎么样？来源—作者。*

## *包装*

*波士顿动力公司是一家推动机器人领域发展和投资的重要公司。不过，我不会投资它们，这没关系。作为一名运动员，我被他们关于运动智能的理想所吸引，但作为一名研究人员，我真的对实用智能更感兴趣。如果机器人不能捡起一个新的物体或制作咖啡，那么它是否能跑酷就无关紧要了。希望现代公司为他们闪亮的新研究机构增加一个翻译部门。*

*我也在考虑把这个博客重新命名为更时髦的东西。主题不会改变，但它可能有助于增长。如果你有任何意见，请告诉我。*

**这个原本出现在我的自由子栈* [***民主化自动化***](https://robotic.substack.com/) *。请查看或关注我的* [*推特*](https://twitter.com/natolambert) *！**