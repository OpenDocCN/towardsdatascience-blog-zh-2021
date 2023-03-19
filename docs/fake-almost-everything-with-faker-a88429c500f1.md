# 用 Faker 伪造(几乎)一切

> 原文：<https://towardsdatascience.com/fake-almost-everything-with-faker-a88429c500f1?source=collection_archive---------20----------------------->

![](img/d3e1553eaf31360182570d1543b11077.png)

dasha shchukova 在 [Unsplash](https://unsplash.com/s/photos/crowd?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

最近，我的任务是创建一些随机的客户数据，包括姓名、电话号码、地址和其他常见的东西。起初，我想我只是生成随机的字符串和数字(一些胡言乱语)，然后就到此为止。但是我记得我的同事们用了一个包。我知道，总有一个包裹可以装下所有的东西，嗯，几乎所有的东西。

不管怎样，我想我要试一试。这些天我开始写一些严肃的 Python 代码，并认为探索 Python 可用的各种包是个好主意。我执行了 pip 命令，下载了这个包，并开始在我的 CSV 文件中随机生成一些人。很有趣。所以我想我会记录这个过程，因为鉴于我的历史，我肯定会忘记 Faker。

# 安装 Faker

安装 Faker 与使用 pip 安装任何其他 Python 包没有什么不同。您可以使用以下任何一个命令来安装 Faker。

```
pip install faker 
pip3 install faker 
python -m pip install faker 
python3 -m pip install faker
```

根据您安装的 Python 版本，使用适当的命令来安装 Faker 包。不会超过几分钟。

# 将 Faker 导入您的代码并初始化它

将 Faker 包导入到您的代码中也没有什么不同。只需在 Python 文件的开头添加下面的 import 语句，就可以开始了。

```
from faker import Faker
```

一旦导入了包，就需要创建 Faker 类的对象。您可以使用以下命令来实现这一点。不过，locale 参数是可选的。你可以跳过它，你会完全没事的。

```
faker = Faker(locale='en_US')
```

# 让我们先看看它能做什么

在深入研究代码之前，让我们先看看它能为我们做什么。

```
My name is Mx. Linda Dunn III , I'm a gender neutral person. You can call me at 001-099-311-6470, or email me at caroljohnson@hotmail.com, or visit my home at 2703 Fitzpatrick Squares Suite 785 New Crystal, MN 18112 My name is Dr. John Harris MD , I'm a male. You can call me at (276)611-1727, or email me at combstiffany@brown-rivers.org, or visit my home at 7409 Peterson Locks Apt. 270 South Kimfurt, IL 79246 My name is Dr. Ann Huynh DVM , I'm a female. You can call me at 543.024.8936, or email me at timothy30@shea-poole.com, or visit my home at 5144 Rubio Island South Kenneth, WI 22855
```

这是我编写的一个简单 Python 脚本的输出，用来生成假的客户数据或假的人。看着这个，它看起来是如此的逼真。我用来得到这个输出的代码如下:

```
from faker import Fakerfaker = Faker(locale='en_US')print("My name is %s %s %s , I'm a gender neutral person. You can call me at %s, or email me at %s, or visit my home at %s" %(faker.prefix_nonbinary(), faker.name_nonbinary(), faker.suffix_nonbinary(), faker.phone_number(), faker.ascii_free_email(), faker.address()))print("My name is %s %s %s , I'm a male. You can call me at %s, or email me at %s, or visit my home at %s" %(faker.prefix_male(), faker.name_male(), faker.suffix_male(), faker.phone_number(), faker.ascii_company_email(), faker.address()))print("My name is %s %s %s , I'm a female. You can call me at %s, or email me at %s, or visit my home at %s" %(faker.prefix_female(), faker.name_female(), faker.suffix_female(), faker.phone_number(), faker.company_email(), faker.address()))
```

你现在可以看到产生大量的虚假客户是多么容易，当然是为了测试。乐趣并没有在这里结束。从那来的还有很多。您可以生成整个公司，例如:

```
The company I just created!David PLCProviders of Horizontal value-added knowledge userPhone: 001-891-255-4642x93803Email: ksanchez@cochran.com234 Torres PortsWest Rhonda, AL 96210
```

从上面的输出可以看出，我们为用户提供了一些很好的横向增值知识。那应该是公司的口头禅。

我不骗你，有一种方法叫做 **bs()** 。我不知道你什么时候用过，但是你可以随时调用 Faker 的 bs()。看到我做了什么吗？

# 这有什么帮助？

我以为你已经明白这一点了。无论如何，当你需要数据来测试，并且你需要这些数据尽可能真实(或者尽可能真实)的时候，你可以使用 Faker 来轻松快速的生成测试数据。

其实我对我最后一句话里的“迅速”部分不太确定。生成数据肯定很容易。而是生成一百万条包含名字、姓氏、电子邮件、电话等的客户记录。，在 2019 年的 16 英寸基本型号 MacBook Pro 上花费了近 350 秒。所以你想怎么做就怎么做。

# 摘要

尽管如此，这绝对是一个非常方便和有趣的软件包。你可以很容易地产生任意数量的客户或朋友(无论你如何摇摆),每个人都有完整的离线和在线资料。你可以生成家庭电话和电子邮件，工作电话和电子邮件，家庭住址，工作地址，兴趣，个人资料，信用卡，车牌号码，等等。所以一定要去看看[包的 Github repo](https://github.com/joke2k/faker?ref=pythonrepo.com) ，四处看看，然后带着它转一圈。源代码也很容易理解。

如果你喜欢你在这里看到的，或者在我的[个人博客](https://blog.contactsunny.com/)和 [Dev。要写博客](https://dev.to/contactsunny)，并希望在未来看到更多这样有用的技术帖子，请考虑在 [Github](https://github.com/sponsors/contactsunny) 上关注我。

*原载于 2021 年 9 月 30 日 https://blog.contactsunny.com**的* [*。*](https://blog.contactsunny.com/data-science/fake-almost-everything-with-faker)