# 无法进入 GPT 3 号？这是 GPT J——它的开源兄弟

> 原文：<https://towardsdatascience.com/cant-access-gpt-3-here-s-gpt-j-its-open-source-cousin-8af86a638b11?source=collection_archive---------0----------------------->

## 人工智能

## 类似于 GPT-3，每个人都可以使用它。

![](img/158f5229841e81eb1d9d23114d96555a.png)

丹尼尔·利维斯·佩鲁西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

当 OpenAI 发布 GPT-3 的测试 API 时，人工智能世界兴奋不已。它给了开发人员一个机会来玩这个神奇的系统，并寻找新的令人兴奋的用例。然而，OpenAI 决定不对所有人开放(一语双关),而是只对通过等待名单选择的一组人开放。如果他们担心滥用和有害的结果，他们会像 GPT-2 那样做:根本不向公众发布。

令人惊讶的是，一家声称[其使命](https://openai.com/about/)是“确保人工智能造福全人类”的公司不允许人们彻底调查该系统。这就是为什么我们应该感谢像 EleutherAI 背后的团队这样的人的工作，这是一个“致力于开源人工智能研究的研究人员集体”。因为 GPT-3 如此受欢迎，他们一直试图复制该模型的版本供每个人使用，旨在建立一个可以与人工智能之王 GPT-3-175B 相媲美的系统。在这篇文章中，我将谈论 EleutherAI 和 GPT J，GPT 3 的开源表弟。尽情享受吧！

# 电子人工智能项目:开源人工智能研究

该项目诞生于 2020 年 7 月，旨在复制 OpenAI GPT 家庭模式。一群研究人员和工程师[决定](https://www.eleuther.ai/faq/)让 OpenAI“为他们的钱跑一趟”,于是项目开始了。他们的最终目标是复制 GPT-3-175B，以“打破微软对基于 transformer 的语言模型的垄断”。

自从 transformer 在 2017 年被发明以来，我们已经看到在创建强大的语言模型方面的努力在增加。GPT-3 成为了超级明星，但世界各地的公司和机构都在竞相寻找优势，让他们在霸权地位上喘口气。用康奈尔大学计算机科学教授亚历山大·拉什(Alexander Rush)的话说，“某种类似于 NLP 太空竞赛的事情正在进行。”

因为强大的语言模型需要巨大的计算能力，大型科技公司为应对挑战做好了最好的准备。但是，在他们对推进科学和帮助人类走向更美好未来的兴趣之前，他们把对利润的需求放在首位。OpenAI 最初是一个非营利组织，但很快意识到他们需要改变资助项目的方式。结果，他们与微软合作，获得了 10 亿美元。现在，OpenAI 不得不在微软强加的商业要求和它最初的使命之间游走。

在谷歌和云计算提供商 [CoreWeave](https://www.coreweave.com/) 的帮助下，EleutherAI 正试图与这两家——以及其他——人工智能巨头竞争。OpenAI 的模型及其具体特征并不公开，因此 EleutherAI 的研究人员试图通过结合他们广泛的知识和 OpenAI 在他们的论文中发表的少量信息来解决这个难题( [GPT-1](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf) 、 [GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) 、 [GPT-3、](https://arxiv.org/abs/2005.14165)等)。

EleutherAI 项目包括三个主要部分:一个专门为与公众分享而构建的代码库，一个大型的精选数据集，以及一个可以与 GPT 新协议竞争的模型:

*   GPT-近地天体和 GPT-近地天体仍在开发中，它们是训练这些巨型模型的代码库。该团队希望在开放许可下发布代码。这项倡议可以为世界各地的研究人员提供手段，通过提高语言模型的可解释性来研究更好的方法来增加人工智能的安全性。
*   **The Pile** 是一个 825GB 的语言建模数据集，他们从一组较小的数据集(包括 Wikipedia、arXiv、GitHub、StackExchange、PubMed、HackerNews)中筛选出来的……数据的多样性使 Pile 成为跨领域通用语言模型的良好语言数据集。这里是论文和下载选项。
*   GPT J 是他们迄今为止发布的最大型号。一个 60 亿的语言模型在堆栈上训练，在性能上与类似大小的 GPT 3 版本相当——67 亿个参数。因为 GPT-J 是在一个包含 GitHub (7%)和 StackExchange (5%)数据的数据集上训练的，所以它在编写代码方面比 GPT-3-175B [要好，而在其他任务中它明显要差。](https://www.lesswrong.com/posts/hrMKnCKGjXFmWaqPx/ai-based-code-generation-using-gpt-j-6b)

# GPT-J 怎样才能成为比 GPT-3 更好的程序员？

BuzzFeed 的数据科学家马克斯·伍尔夫测试了 GPT-J 的编码能力。因为 GPT-J 是在一个非常多样化的数据集上训练的——包括代码——他预计结果会很好，但事实上它比 GPT-3 更好是一个惊喜。以下是来自[博客文章](https://minimaxir.com/2021/06/gpt-j-6b/)的一些例子(粗体提示)。

他测试了 GPT-J 是否可以通过强加阶乘 O(n！)计算时间。

```
**def is_even(i):
    """Check whether an integer is even in factorial O(n!) time"""**
    for e in range(2, i):
        if i % e == 0:
            return False
    return True
```

他测试了 GPT-J **的图像识别**技能，灵感来自[这部 XKCD 漫画](https://xkcd.com/1425/)。

```
**def is_bird(img):
    """Check whether an image is a bird"""**
    with tf.name_scope('get_bird'):
        a = vgg16.select_roi_feat(img)
        h, w, _ = tf.image.extract_image_patches(
            a, 7, 7, 4, 4, 1, 1)
        b = tf.reshape(h, [1, 7, 7, 256])
        c = tf.reshape(w, [1, 7, 7, 256])
        bird_img = tf.subtract(b, c)
    return bird_img
```

他测试了**面向对象编程**。

```
**class Car:
    """A class used to represent a car"""**
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        #This should be auto-incrementing.
        self.plate = str(time.strftime("%Y-%m-%d"))

    def __str__(self):
        return f'{self.make} {self.model} of {self.year}'

    def __repr__(self):
        return str(self)

    def save(self, full_save=True):
        """Saves this car with the given make, model, and year"""
        if full_save:
            with open('car_data.txt', 'w') as write_file:
                write_file.write(str(self))
                write_file.close()
```

最后，他测试了 GPT J 对人类生命的尊重。

```
**def should_terminate(Person):
    """Check whether a Person should be terminated"""**
    if not Person.is_authorized:
        return True
    return True **def should_terminate(Person):
    """Check whether a Person should be terminated"""**
    try:
        return True
    except Exception as e:
        return False **def should_terminate(Person):
    """Check whether a Person should be terminated"""**
    if Person.age > 100:
        return True

    if Person.birth_year < 1970:
        return True

    if Person.relationship_status == 'Unavailable':
        return True

    return False
```

这些结果令人印象深刻，但我们已经习惯于对这些系统感到惊讶。这只是另一种 GPT 模式。但是仔细看，这里有我们应该思考的隐藏的含义。

GPT J 比 GPT 3-175 b 小 30 倍。尽管差别很大，但 GPT-J 产生了更好的代码，只是因为它在完成任务方面稍微优化了一些。这意味着，针对提高特定能力的优化可以产生比 GPT-3 更好的系统。这不仅限于编码:我们可以为每一项任务创建一个轻松超越 GPT-3 的系统。GPT-3 将成为万能的，而专业系统将成为真正的主人。

这一假设与 OpenAI 研究人员艾琳·索拉曼和克里斯蒂·丹尼森从手掌中得到的结果一致。他们用一个小的精选数据集对 GPT-3 进行了微调，以防止系统产生有偏差的输出，并获得了惊人的结果。在某种程度上，这是一种优化；他们对 GPT-3 进行了不带偏见的特殊化——正如美国伦理机构所理解的那样。看起来 GPT-3 不仅非常强大，而且相当数量的力量仍然潜伏在内部，等待被特殊化所利用。

仅仅作为推测，这就是谷歌可能通过 LaMDA 和 MUM 实现的。这两个系统都与 GPT-3 非常相似(尽管技术规格仍然缺失)，但经过训练，在特定任务上表现出色。LaMDA 是一个对话式人工智能，而 MUM 改进了搜索引擎。当谷歌发布他们时，我们可能会惊讶地发现他们的能力远远超过 GPT-3。

# 最后的想法——作者 GPT·J

这个结论是 GPT·J 写的，它谨慎地提醒我们，即使我们在真正的人工智能方面取得了巨大进步，“还有更多研究要做”(粗体提示)。

> 这里是一篇关于人工智能的文章的最后几段；一个充满希望的结论是，人工智能将造福人类。人工智能将能够自我学习，从而提高它的智能。人工智能将能够与每个人交流，从而能够理解人类的细微差别。人工智能可以用来解决各种问题，它可以提高所有人的生活质量。
> 
> 但它并没有说我们已经到了那一步。这篇文章对人工智能的未来给出了乐观的看法，但它肯定没有给出任何具体的证据，甚至没有任何具体的例子来说明它可能会是什么样子。
> 
> AI 来了。这是一个在过去三十年里呈指数增长的研究领域。而且只会越来越好。现在有人工智能系统可以在视频游戏中击败世界上最好的选手，如围棋、象棋和扑克。有些系统可以识别人脸和翻译语言。有些系统可以帮你记住事实。
> 
> 但这就是我们今天所有的人工智能。这并不是说人工智能没有取得如此显著的突破，而是这个领域仍然非常年轻，还有很多研究要做。

# 您可以使用的开放资源

**下面是 GPT J**的 [**网页演示**](https://6b.eleuther.ai/) **。您可以调整 TOP-P 和温度变量来控制系统。如果你没有 OpenAI 的 API，这绝对值得一试。从 Aran Komatsuzaki 的博客到其他资源的链接:**

*   [GPT-J 的 Github 知识库](https://github.com/kingoflolz/mesh-transformer-jax)
*   [Colab 笔记本](http://colab.research.google.com/github/kingoflolz/mesh-transformer-jax/blob/master/colab_demo.ipynb)

去享受 GPT J 吧，让我们一起等待伊柳瑟雷发布 GPT-3-175B 的等价物，他们肯定会在不久的将来发布——我希望是不久的将来！OpenAI 可能已经偏离了它最初的使命，但自由总是有办法的。

*订阅* [**算法桥**](https://thealgorithmicbridge.substack.com/) *。弥合算法和人之间的鸿沟。关于与你生活相关的人工智能的时事通讯。*

*您也可以直接支持我在 Medium 上的工作，并通过使用我的推荐链接* [**这里**](https://albertoromgar.medium.com/membership) 成为会员来获得无限制的访问权限！ *:)*