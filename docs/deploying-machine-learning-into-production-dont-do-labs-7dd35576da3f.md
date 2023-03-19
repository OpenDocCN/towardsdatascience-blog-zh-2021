# 将机器学习部署到生产中:不要做实验室。

> 原文：<https://towardsdatascience.com/deploying-machine-learning-into-production-dont-do-labs-7dd35576da3f?source=collection_archive---------33----------------------->

![](img/bae1e97d58a4324ec5020a2044b1a97f.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[路博宣礼塔](https://unsplash.com/@bubo?utm_source=medium&utm_medium=referral)拍摄的照片

众所周知 [87%的数据科学项目没有进入生产](https://venturebeat.com/2019/07/19/why-do-87-of-data-science-projects-never-make-it-into-production/)。问题有据可查(见[这里](/challenges-deploying-machine-learning-models-to-production-ded3f9009cb3)，这里[这里](https://medium.com/@ODSC/machine-learning-challenges-you-might-not-see-coming-9e3ed893491f)，这里[这里](https://venturebeat.com/2019/07/19/why-do-87-of-data-science-projects-never-make-it-into-production/))。

根据我在加拿大百思买构建分析产品的经验，应用数据科学项目很少因为科学而失败。他们之所以失败，是因为这种模式无法整合到现有的系统和业务运营中。我们已经有了显示良好准确性的可靠模型，甚至有了证明价值的概念证明，但仍然无法部署到生产中。挑战不在于概念验证，而在于扩展。

具体来说，开发模型的数据科学家和将其实施到系统中的工程师之间的差距是一个反复出现的挑战。数据科学家倾向于在一个孤立的环境中开发他们的 ML 模型，远离日常消防甚至客户互动。在“实验室”玩数据这意味着我们错过了关键问题的领域知识和背景，错过了与集成、支持和使用模型的工程师和业务用户意见一致的机会。

**在实验室里很少有机会感受到用户的痛点。**反过来，数据科学家解决错误问题、开发难以实施的解决方案或未能在用户眼中增加价值的几率也很高。实验室工作很难部署到生产中。

这个问题是复杂的，因为事实上接触百万美元的赚钱生产系统是高风险的。代码可能是创始人的代码和附加补丁的混合，与十几个其他系统集成在一起。有很多未知。所以集成模型输出太快，我们会冒导致不可预见的问题的风险。慢慢来，其他优先事项可能会接管。我们需要平衡展示 ML 模型价值的需求和发展软件特性和维持正常运行时间的需求。

一些同事提到，将模型投入生产是一个[创建 API](https://urldefense.com/v3/__https:/towardsdatascience.com/production-machine-learning-isnt-hard-anymore-932bd91e138f__%3B!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxcJZzVes$) 的例子。API 是很好的交付工具，但是认为它们解决了所有的生产挑战就有点简单化了。如果 API 在每小时赚 1000 万美元的生产软件上失败了怎么办？数据科学家会放下实验室的一切，与工程团队一起提供支持吗？谁负责设计迭代扩展的实验？如果数据有限，当前的开发环境甚至允许测试 ML 模型吗？我们如何监控不可预见的结果(想想[亚马逊的人工智能招聘工具对女性的偏见](https://www.theverge.com/2018/10/10/17958784/ai-recruiting-tool-bias-amazon-report))。我们数据科学家需要与生产软件团队携手应对这些挑战，而我们在实验室里做不到这一点。

## **所以我们百思买不再做实验室了。**

将数据科学产品部署到生产中的大多数尝试都集中在使集成更容易的[工具](https://urldefense.com/v3/__https:/stackoverflow.blog/2020/10/12/how-to-put-machine-learning-models-into-production/__%3B!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxp4UY1n0$__%3B!!KtbpKd1p8A!6im7EiEMNJvwS_HPtnROT5jfD6sP_klIeejq88U-UNxK_azpvI7kAPXAT2LpAKaQAMSJAD8$)上，或者有点令人困惑的[流程改进](https://urldefense.com/v3/__https:/www.mckinsey.com/business-functions/mckinsey-analytics/our-insights/executives-guide-to-developing-ai-at-scale*intro__%3BIw!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxrGRm06Q$__%3BKg!!KtbpKd1p8A!6im7EiEMNJvwS_HPtnROT5jfD6sP_klIeejq88U-UNxK_azpvI7kAPXAT2LpAKaQbRvf0ow$)(让顾问带头)。然而，他们未能弥合结构性差距:数据科学家在远离生产环境的实验室中开发算法的事实。反过来，未能部署到生产中的结果保持不变。([结构- >流程- >结果](https://urldefense.com/v3/__https:/en.wikipedia.org/wiki/Donabedian_model*Dimensions_of_Care__%3BIw!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxWRcv7WE$)是医疗保健行业中一个行之有效的质量改进框架，值得我们借鉴。)

为了从结构上提高我们的机会，我们的数据科学家现在与技术产品团队携手合作。

![](img/3de40341f5b9dbd6533f5e514ae3697a.png)

基思·约翰斯顿在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

我们已经将数据科学家嵌入到各自的产品团队中，该团队拥有我们想要颠覆的特定技术。我们在生产中发展权利。真实的世界。这符合我们的核心价值观之一，即作为一个团队比赛，作为一个团队输赢。我们认为，在实验室开发一个模型并传递给工程团队，就像一个四分卫把球传给了没有人准备好接球的区域。我们想确保团队准备好了。

例如，为了建立一个优化库存分配的模型，我们的分析团队与库存技术团队合作。我们吃他们吃的东西，看他们看的相同的系统和代码，最重要的是，对他们相同的指标和领导者负责。信任是在传统上来自两个不同世界的数据科学家和软件工程师之间有机建立的。

这种结构迫使我们从一开始就提出尖锐的问题。我们今年有资源交付解决方案吗？相对于其他产品功能，这款 ML 作品有什么价值？谁将在生产中帮助支持该系统？在我们扩大规模之前，需要进行哪些实验和测试？

符合每个人期望的一个关键问题是:**我们有能力做什么？**承认我们可以在生产中实际部署和支持的东西，可以避免开发无法集成的东西。例如，如果没有资源将所需的数据放入数据湖并创建自动化管道，那么更好的第一步可能是进行描述性分析并实现静态数据集，直到数据工程团队准备好支持我们的计划([有时来自描述性分析的结果也一样好](https://urldefense.com/v3/__https:/dspace.mit.edu/bitstream/handle/1721.1/121280/Jonquais_Krempl_2019.pdf?sequence=1&isAllowed=y__;!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxD8jbGkE$))。

嵌入式模型的另一个关键元素是让负责生产软件的产品经理也为 ML 模型设置开发优先级。他们可能没有机器学习或分析专业知识；他们可能需要分析技术产品经理的帮助来共同制定解决方案和路线图。但是他们需要成为领导者和主动性的代言人。如果产品经理主动要求分析解决方案来解决他们的问题，并且如果他们是向用户销售解决方案的人，那么部署到生产中自然是优先考虑的。这也有利于数据科学团队:他们可以专注于交付价值，即收集数据、测试模型、提炼、测试和扩展，而不是销售结果，向高管推销新的 ML 模型的价值。

应用数据科学家是为了解决问题而存在的。不只是做很酷的科学。嵌入式让我们能够直接感受到业务痛点，并设计出生产工程师和业务用户乐于实施的解决方案。

## **从生产团队的实验室工作中获益**

然而，在实验室中开发模型确实有一些好处。这里有三个我们正在努力复制到我们的嵌入式结构。

**#1:系统思维:**实验室为更多的系统思维提供了空间。我最喜欢的关于阿科夫博士的[系统思维的演讲](https://urldefense.com/v3/__https:/www.youtube.com/watch?v=OqEeIG8aPPk__;!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxefHAhzA$)强调了确保我们改善各个部分如何相互作用以改善系统整体的重要性。

![](img/c1e5ad074cc25efd397411c75becba16.png)

卡洛斯·阿兰达在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

嵌入一个团队有一个危险，那就是只关注对团队重要的事情，系统的一小部分。如果没有解决中间环节或库存方面的挑战，仅仅提高最后一英里的交付速度可能无法实现更快的履行。

为了避免目光短浅，我们发现在融入团队之前，给整体问题研究留出适当的时间是成功的。这使我们能够与涉及业务领域的任何人和每个人交谈，探索他们的挑战，并对问题和机会所在做出整体评估。在专注于一个部分之前，它给了好奇和探索的空间。这也确保了我们嵌入到我们预测会产生最大影响的业务领域。

**#2:理想设计:**恰好阿科夫有一本关于[理想设计](https://urldefense.com/v3/__https:/knowledge.wharton.upenn.edu/article/idealized-design-how-bell-labs-imagined-and-created-the-telephone-system-of-the-future/__%3B!!KtbpKd1p8A!4rKS7DwYaH7iAHyKWrnxQxZKw3x18zp50JhMst3pdQ5G7tI_TL-2dSVkXY1zPlbxPTH8b10$)的书，围绕解决根本问题的概念(如果你什么都不做，什么会扼杀你的生意，为什么为什么为什么……)。不要让今天的解决方案和约束限制了我们的想象力。换句话说，扰乱，不要调整。

![](img/2d8209727c7e8fac9c3f22c475d0c793.png)

照片由[爱丽丝·迪特里希](https://unsplash.com/@alicegrace?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

通过嵌入和只致力于我们今年能做的事情，我们冒着仅仅是调整而不是破坏的风险。例如，让当前的库存分配运行得更好 10%，而不是把它拆开，让它更好 100%。

在这方面，我们从理想的角度开始解决方案设计过程。设置北极星。只有当最终目标明确时，我们才承认团队的能力，并剔除版本 1。成功意味着逐步部署到产品中，然后让每个新版本让我们更接近北极星。这是一个双赢的局面，因为我们可以在现实世界中快速测试我们的模型，而业务团队可以立即看到业务成果。

**#3:不断创新:**在实验室工作的最大好处是有空间设计新工具，而不会陷入支持现有系统的困境。给探索、测试、迭代的空间。

![](img/13b6f4eca74d20ba14790710a50e5f6b.png)

照片由 [Goh Rhy Yan](https://unsplash.com/@gohrhyyan?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

在嵌入式模式下，部署到生产中的内容也需要得到数据科学家的支持。这花费了改进模型的时间和精力:探索新的分析、新的数据特征、新的算法等等。

我们仍在制定最佳实践，并真诚地欢迎对此的评论，但在一定程度上已经奏效的做法是将整个冲刺阶段(即 2 周)只专注于创新，而忽略了这段时间的支持工作。这可以防止上下文切换和分心。我们都知道，试图将每次冲刺的支持率保持在 30%以下，往往会适得其反。

## **通过合作增加成功几率**

祈祷我们的模型能够投入生产是不够的。我们需要认真解决这个问题。扭转对我们有利的局面。

将失败率从 87%降低到 50/50，需要团队结构、过程和工具的综合改进。数据科学团队可以比创业公司做得更好，成功的机会只有 1/10。通过与生产软件和业务团队携手构建他们想要的解决方案，更重要的是，根据他们可以部署的规范，产品与市场的契合度可以在第一天就存在。