# 在 Faker 库中测试生日悖论(Python)

> 原文：<https://towardsdatascience.com/testing-birthday-paradox-in-faker-library-python-54907d724414?source=collection_archive---------42----------------------->

## 一个被程序化证明的著名统计现象

![](img/56192d5c533af0a33952eac57409027e.png)

[来自 Unsplash](https://unsplash.com/photos/pF0OE0JiF7w)

明天是 5 月 18 日，是我🥰的生日🎈。这启发我写了一篇关于概率论现象的文章，叫做[生日悖论](https://en.wikipedia.org/wiki/Birthday_problem)。

这个问题的本质是关于在一个随机的 n 人组中，至少有两个人的生日是同一天的概率。特别是，在 23 人的组中，这种概率略高于 50%，而在 70 人的情况下，这种概率增加到 99.9%。当人数达到 366(或 367，闰年)，共享生日的概率就变成 100%。前两种情况的概率值似乎出乎意料地高，而且违反直觉，可能是因为在日常生活中，我们不常遇到和自己同一天生日的人。因此，它被称为悖论，尽管它已经被统计学完美地证明了。

为了计算一组随机选择的 n 个人共享生日的概率，我们可以使用下面的公式:

![](img/b3b1cb404d36a2fab46e09d38c57475e.png)

其中 *P(365，n)* —一种排列，即从 365 天中无替换抽样的 n 个生日的有序排列。为了使该公式有效，我们做了以下假设:

*   我们不考虑闰年，
*   所有的 365 天都是同样可能的，不考虑季节变化和历史生日数据，
*   组里没有双胞胎。

该公式的指数近似值:

![](img/2eeff6e40b98638982eb0bce8c0acf8e.png)

计算任意数量的人共享生日的概率的最简单方法是使用可用的[生日悖论计算器](https://www.dcode.fr/birthday-problem)(你也可以找到许多类似的)。

有一些相邻的生日问题:

*   一个人出生在一年中某一天的概率有多大(比如我的生日)？这里的答案是 *1/365*100 = 0.27%* 。
*   有多少人出生在一年中的某一天(例如，我的生日)？
*   给定一个选定的概率，概率小于给定值的最大人数是多少(或者正好相反，概率大于给定值的最小人数是多少)？最后一个问题也被称为**反向生日悖论**。

现在让我们使用一个不太为人所知但非常有用的多功能 Python 库 Faker ( *安装:* `pip install Faker`)来编程证明生日悖论。如果你还不熟悉这个神奇的工具，现在是时候去发现它了。使用 Faker，我们可以创建大量的虚假数据，包括姓名、联系方式、地理信息、工作职位、公司名称、颜色等。例如:

```
from faker import Faker
fake = Faker()
fake.name()**Output:** 'Katherine Walker'
```

相同的语法可以用于创建任何其他类型的假数据。我们需要做的就是用一种合适的自解释方法来替代`name`:`address`、`email`、`phone_number`、`city`、`country`、`latitude`、`longitude`、`day_of_week`、`month_name`、`color`、`job`、`company`、`currency`、`language_name`、`word`、`boolean`、`file_extension`等。我们甚至可以创建假密码(使用`password`方法)、银行数据(`iban`、`swift`、`credit_card_number`、`credit_card_expire`、`credit_card_security_code`)和整个假文件(`csv`、`json`、`zip`)，如果可以的话，可以随意调整一些附加参数。此外，一些方法有更细粒度的版本。例如，我们可以使用`name_female`、`name_male`、`name_nonbinary`，类似于`first_name`、`last_name`、`prefix`和`suffix`，而不是应用`name`方法创建一个随机的名字(即名字+姓氏)。此外，可以选择输出的语言，确保输出的再现性或唯一性等。更多详情，请参考 [Faker 文档](https://faker.readthedocs.io/en/master/)。

然而，让我们回到我们的生日和他们的悖论。首先，我们需要收集假生日来工作。要创建假日期，本馆提供以下方法:

*   `date`—`'%Y-%m-%d'`格式的日期字符串，从 1970 年 1 月 1 日到现在(实际上，就我们的目的而言，年份并不重要)。我们可以使用`pattern`参数改变输出格式。
*   `date_between` —两个给定日期之间的随机日期。默认从 30 年前(`start_date='-30y'`)到现在(`end_date='today'`)。
*   `date_between_dates` —与上一个类似，但这里我们必须指定`date_start`和`date_end`参数。
*   `date_of_birth` —一个随机的出生日期，我们可以选择用`minimum_age`(默认为 0)和`maximum_age`(默认为 115)来约束。
*   `date_this_century` —当前世纪的任何日期。可以增加参数`before_today`和`after_today`；默认情况下，只考虑今天之前的日期。类似地，我们可以创建一个十年、一年或一月的随机日期。
*   `future_date` —从现在起 1 天到给定日期之间的随机日期。默认情况下，考虑一个月前的未来日期(`end_date='+30d'`)。

几乎所有这些方法都返回一个日期时间对象，而`date`返回一个字符串:

```
fake.date()**Output:**
'1979-09-04'
```

让我们用这个方法来测试一下生日悖论。我们将创建一个函数:

*   创建一个 n 个随机生日的列表，只从每个日期中提取月和日。
*   运行这个操作 1000 次，对于每个生日列表，检查列表中的所有生日是否都是唯一的(通常我们认为不是这样)。将结果(`True`或`False`)添加到列表(`shared_bday_test`)。
*   运行整个循环 100 次，计算在每个`shared_bday_test`中有一个共享生日的列表的概率(实际上意味着我们必须计算所有`False`值的百分比)。
*   为 100 个周期中的每一个周期创建所有概率的列表。
*   找到概率列表的平均值并打印出结果。

```
def test_bday_paradox(n):
    probabilities = []
    for _ in range(100):
        shared_bday_test = []
        for _ in range(1000):
            bdays=[]
            for _ in range(n):
                bdays.append(fake.date()[-5:])
            shared_bday_test.append(len(bdays)==len(set(bdays)))
        p = (1000 - sum(shared_bday_test))/1000*100
        probabilities.append(p)
    p_mean = round(sum(probabilities)/len(probabilities),1)
    print(f'The probability of a shared birthday among a group of {n} random people:'
          f'\n{p_mean}\n')
```

现在，我们准备检查本文开头提到的情况:23 人、70 人和 366 人组共享生日的概率。相应地，我们期望以下值:~50%、99.9%和 100%。

```
test_bday_paradox(23)
test_bday_paradox(70)
test_bday_paradox(366)**Output:** The probability of a shared birthday among a group of 23 random people:
50.5The probability of a shared birthday among a group of 70 random people:
99.9

The probability of a shared birthday among a group of 366 random people:
100.0
```

# 结论

总之，我们讨论了生日悖论这一奇怪的统计现象，计算共同生日概率的方法，公式有效的假设，以及一些有趣但似乎违反直觉的结果。此外，我们熟悉了一个很少使用但非常有用的库，用于在 Python 中创建假数据，探索了它的众多应用中的一些，最后，应用 Faker 来证实生日悖论。

感谢阅读，祝我生日快乐！🥳还有 0.27%的概率，也给你！😀

如果你喜欢这篇文章，你也可以发现下面这些有趣的:

[](https://medium.com/geekculture/creating-a-waterfall-chart-in-python-dc7bcddecb45) [## 用 Python 创建瀑布图

### 做这件事最简单的方法

medium.com](https://medium.com/geekculture/creating-a-waterfall-chart-in-python-dc7bcddecb45) [](https://python.plainenglish.io/the-little-prince-on-a-word-cloud-8c912b9e587e) [## 用 Python 为《小王子》生成单词云

### 文字云上的小王子:我们可以这样解开他的一些谜团吗？

python .平原英语. io](https://python.plainenglish.io/the-little-prince-on-a-word-cloud-8c912b9e587e) [](https://medium.com/mlearning-ai/11-cool-names-in-data-science-2b64ceb3b882) [## 数据科学中的 11 个酷名字

### 你可能不知道它们是什么意思

medium.com](https://medium.com/mlearning-ai/11-cool-names-in-data-science-2b64ceb3b882)