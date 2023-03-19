# 使用 Python 构建您自己的自定义数据集

> 原文：<https://towardsdatascience.com/build-a-your-own-custom-dataset-using-python-9296540a0178?source=collection_archive---------8----------------------->

## 我是如何从零开始构建数千行数据点的

![](img/eef3d4444197022fbb57418ffab8b83a.png)

肖恩·托马斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

D ata 是数据科学和机器学习的基础。为了分析、可视化、得出结论和建立 ML 模型，需要成千上万的数据点。在某些情况下，数据是现成的，可以免费下载。其他时候，数据无处可寻。

在数据不容易获得但需要的情况下，您将不得不求助于自己构建数据。您可以使用许多方法从 webscraping 到 API 获取这些数据。但是有时，您最终需要创建假的或“虚假的”数据。当您知道将要使用的确切功能和包含的数据类型，但是您没有数据本身时，虚拟数据会很有用。

在这里，我将向您展示我是如何创建 100，000 行虚拟数据的。这个数据也不完全是随机的。如果是的话，建造它会容易得多。有时，当从头开始创建虚拟数据时，您需要在数据中开发可能反映真实世界行为的趋势或模式。你以后会明白我的意思。

> [在这里注册一个中级会员，可以无限制地访问和支持像我这样的内容！在你的支持下，我赚了一小部分会费。谢谢！](https://marco-santos.medium.com/membership)

# 构建数据集的需求

假设你正在从头开始构建一个应用程序，需要建立一个庞大的用户群来进行测试。为您提供了一个要素及其各自数据类型的列表。

这个用户群也需要在一定程度上准确地反映真实世界的用户和趋势，所以它不能完全随机。例如，您不希望一个用户拥有大学学历，但也只有 10 岁。或者，您可能不希望某个特定数据点的代表性过高，例如男性多于女性。这些都是创建数据集时需要注意的事项。

由于真实世界的数据很少是真正随机的，所以像这样的数据集将是对您将来要处理的其他数据集的一个很好的模拟。它还可以作为你希望训练的任何机器学习模型的试验场。

现在让我们开始吧，请随意跟进，我将向您展示我是如何构建这个数据集的…

# 构建数据集

要进行编码，首先要导入以下库:

```
import pandas as pd
import uuid
import random
from faker import Faker
import datetime
```

## 大小

数据集大小将为 100，000 个数据点(*您可以做得更多，但可能需要更长时间来处理*)。我将这个数量分配给一个常量变量，我在整个过程中都使用这个变量:

```
num_users = 100000
```

# 特征

我挑选了我认为在常规用户数据集中最常见的 10 个特征。这些功能和各自的数据类型是:

*   **ID** —标识每个用户的唯一字符串。
*   **性别** —三种选择的字符串数据类型。
*   **订阅者** —他们订阅状态的二元真/假选择。
*   **Name** —用户的名和姓的字符串数据类型。
*   **Email**—用户电子邮件地址的字符串数据类型。
*   **上次登录** —上次登录时间的字符串数据类型。
*   **出生日期** —年-月-日的字符串格式。
*   **教育** —字符串数据类型的当前教育水平。
*   **Bio** —随机词的短字符串描述。
*   **评分**——对某事物从 1 到 5 的整数型评分。

我输入上面的内容作为初始化熊猫数据框架的特征列表:

```
# A list of 10 features
features = [
    "id",
    "gender",
    "subscriber",
    "name",
    "email",
    "last_login",
    "dob",
    "education",
    "bio",
    "rating"
]# Creating a DF for these features
df = pd.DataFrame(columns=features)
```

## 创建不平衡的数据

上面的一些属性通常应该包含不平衡的数据。通过一些快速的研究，可以有把握地假设，有些选择不会被同等地代表。对于更真实的数据集，需要反映这些趋势。

# 本能冲动

对于 ID 属性，我使用`uuid`库生成了 10 万次随机字符串。然后，我将它赋给数据帧中的 ID 属性。

```
df['id'] = [uuid.uuid4().hex for i in range(num_users)]
```

UUID 是一个很好的库，可以为每个用户生成唯一的 ID，因为它复制 ID 的几率非常低。在生成独特的识别字符集时，这是一个很好的选择。但是，如果您希望确保没有重复的 id，那么您可以使用以下命令对数据帧进行简单的检查:

```
print(df['id'].nunique()==num_users)
```

如果数据集中的所有 id 都是唯一的，这将返回 **True** 。

# 性别

![](img/fbf764f120dd4cb12a85983515e04c82.png)

照片由 [Dainis Graveris](https://unsplash.com/@dainisgraveris?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

该属性是不应该使用同样随机的选择的实例之一。因为，可以有把握地假设，每一种选择发生的可能性并不相等。

对于性别，我提供了三个选项:男性、女性和 na。然而，如果我使用 Python 的`random`库，那么每个选择可能会平等地显示在数据集中。事实上，男性和女性的选择要比北美多得多。所以我决定在数据中展示这种不平衡:

```
genders = ["male", "female", "na"]df['gender'] = random.choices(
    genders, 
    weights=(47,47,6), 
    k=num_users
)
```

通过使用`random`库，我向`choices()`函数提供了性别选项列表、每个选项的权重，以及由“ *k* 表示的要做出多少个选择。然后将它分配给 dataframe 的“ *gender* ”属性。我之前描述的不平衡，在权重部分表示为大约 6%的时间出现“na”选择。

# 订户

对于这个属性，选项可以在真和假之间随机选择。因为可以合理地预期大约一半的用户将是订户。

```
choice = [True, False]df['subscriber'] = random.choices(
    choice, 
    k=num_users
)
```

就像之前的“*性别*”一样，我使用了`random.choices()`，但是没有权重，因为这个属性可以在两个选项之间随机拆分。

# 名字

在这里，我使用 Faker 库为所有这些用户创建了数千个名字。Faker 库在这种情况下很棒，因为它有一个男性和女性名字的选项。为了处理性别化的名字，我创建了一个函数来根据给定的性别分配名字。

```
# Instantiating faker
faker = Faker()def name_gen(gender):
    """
    Quickly generates a name based on gender
    """
    if gender=='male':
        return faker.name_male()
    elif gender=='female':
        return faker.name_female()

    return faker.name()# Generating names for each user
df['name'] = [name_gen(i) for i in df['gender']]
```

我使用我的简单函数根据之前的“性别”属性数据快速生成一个姓名列表，并将其分配给 dataframe。

# 电子邮件

![](img/d6e13b9475a408b0fa1b5d242d7ddefc.png)

照片由 [Daria Nepriakhina](https://unsplash.com/@epicantus?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

电子邮件被证明是更棘手的属性之一。我想创建与生成的名称相关的电子邮件地址。然而，可能会有重复的机会，因为人们可以共享相同的名称，但不是相同的电子邮件。

首先，我创建了一个新函数，它可以将名字格式化成带有默认域名的电子邮件地址。它还可以通过在格式化名称的末尾添加一个随机数来处理重复的地址:

现在，为了恰当地利用这个函数的目的，我创建了一个循环，当遍历“ *Name* 属性时，这个循环将在必要时重新运行这个函数。该循环将继续运行该函数，直到创建一个唯一的电子邮件名称。

```
emails = []for name in df['name']:

    # Generating the email
    email = emailGen(name)

    # Looping until a unique email is generated
    while email in emails:

        # Creating an email with a random number
        email = emailGen(name, duplicateFound=True)

    # Attaching the new email to the list
    emails.append(email)

df['email'] = emails
```

在所有电子邮件生成之后，我将它们分配给 dataframe 的“ *Email* ”属性。您还可以使用与 IDs 相同的方法进行可选的检查，以查看每封电子邮件是否是唯一的。

# 上次登录

这个属性现在需要特定的格式，通过使用`datetime`库，这变得更加容易。在这里，我希望用户有过去一个月左右的登录历史。我使用了另一个自定义函数来帮助:

该函数主要生成两个给定时间之间的时间戳列表。它生成了一个随机时间戳列表来分配给数据帧。

# 出生日期

这个属性很简单，因为它类似于“*上次登录*”。我所做的只是通过删除小时、分钟和秒来改变时间格式。我再次使用`datetime`来帮助为每个用户随机选择一个日期，但是这次时间范围从 1980 年到 2006 年，以获得一个很好的年龄随机分布。下面的代码与前面的代码基本相同，但格式和日期范围不同:

# 教育

![](img/0d001acc0c113b5029e042ecc6a78579.png)

马特·拉格兰在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

“*教育*”属性依赖于“*出生日期*”。在这些情况下，教育水平是基于使用者的年龄，而不是他们达到的最高教育水平。这是随机选择教育水平不能反映真实世界趋势的另一个属性。

我创建了另一个简单的函数，它根据今天的日期检查用户的年龄，并返回他们当前的教育水平。

生成教育水平列表后，我将它分配到数据框架中。

# 个人简历

对于这个属性，我想根据用户的订阅状态来改变简历的长度。如果用户是订阅者，那么我会假设他们的 bios 比非订阅者的要长。

为了适应这种情况，我构建了一个函数来检查他们的订阅状态，并从 Faker 返回一个长度不等的随机句子。

在上面的函数中，我根据订阅状态随机选择了假句子的长度。如果他们是订阅者，那么他们的简历会比平时长，反之亦然。

# 评级

![](img/4d8a846657f557c07967ca95154a4c73.png)

乔治·帕甘三世在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

为了完善这个数据集，我想包含一个数字数据类型。我选择“ *rating* ”为整数类型。1 到 5 的评级代表任何东西，它只是为了任何自由裁量的目的而存在。

对于评分本身，我选择将 1 到 5 的分布向极端倾斜，以反映用户似乎对他们的评分更加绝对的倾向。

```
# The different ratings available
ratings = [1,2,3,4,5]# Weighted ratings with a skew towards the ends
df['rating'] = random.choices(
    ratings, 
    weights=(30,10,10,10,30), 
    k=num_users
)
```

我再次使用了`random.choices()`函数，但是权重偏向 1 和 5。

# 保存数据集

既然数据已经完成，如果您正在编写代码，那么在决定保存它之前，可以随意查看数据帧。如果一切正常，那么用这个简单的命令将数据帧保存为一个`.csv`文件:

```
df.to_csv('dataset.csv')
```

这将数据集作为一个相当大的 CSV 文件保存在本地目录中。如果您想检查保存的数据集，请使用以下命令查看它:

```
pd.read_csv('dataset.csv', index_col=0)
```

一切都应该看起来不错，现在，如果你愿意，你可以执行一些基本的数据可视化。如果您想了解更多关于数据可视化的知识，请查看我的以下文章:

</how-to-use-plotly-for-data-visualization-f3d62bbcfd92> [## 为什么我使用 Plotly 进行数据可视化

towardsdatascience.com](/how-to-use-plotly-for-data-visualization-f3d62bbcfd92) 

# 结束语

我希望您能从我构建这个数据集的经历中学到一些新东西。如果有的话，构建它提供了一点创建 Python 函数和算法的实践。此外，我相信构建自己的数据集可以让你对数据集有更好的理解。它也许能让你准备好处理任何你需要的大量数据。

如果您愿意，您可以通过向数据分布添加更多的限制或依赖性，甚至向数据帧添加更多的属性来改进它。请随意探索这些数据或对其进行扩展！

> [**不是中等会员？点击这里支持 Medium 和我！**](https://medium.com/@marco_santos/membership)

## Github 上的代码

<https://github.com/marcosan93/Medium-Misc-Tutorials/blob/main/Build-a-Dataset.ipynb> 