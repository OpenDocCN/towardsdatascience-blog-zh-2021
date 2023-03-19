# Doctor.ai，一个人工智能虚拟语音助手，用于医疗保健

> 原文：<https://towardsdatascience.com/doctor-ai-an-ai-powered-virtual-voice-assistant-for-health-care-8c09af65aabb?source=collection_archive---------2----------------------->

## 用 AWS Lex 和 Neo4j 构建一个聊天机器人

> 由黄思兴、德里克·丁、埃米尔·帕斯特、伊尔万·布塔尔、闪亮的朱组成。由 Neo4j 的 Maruthi Prithivirajan、Joshua Yu 和 Daniel Ng 提供支持。

> 我认为未来医疗保健的顶峰将是建立虚拟医疗教练来促进健康人的自我驾驶。我承认有很多障碍，但我仍然相信有一天它会建成并得到充分的临床验证。

上面这个未来派的说法是埃里克·托普在他的书《深度医学》中写的。根据上下文，托普所说的虚拟医疗教练实际上是一个语音人工智能助手。这位助理管理大量数据并从中学习，包括个人医疗记录、健康状况和科学文献。一方面，它可以提出健康建议，解释医学概念，并为患者创建警报。另一方面，它可以帮助医生做出更好的决定。

新冠肺炎全球疫情清楚地表明，我们需要让更多的人获得医疗保健。在这方面，语音助手提供了一些优于智能手机或电脑应用的优势。首先，它是免提的。手术室里的医生不会敲电话或敲键盘。其次，全球相当大一部分人口既不会写也不会写代码。我们不要忘记，许多人都有视力障碍。第三，语音输入比打字快。对于汉语等语言，语音可以比打字快一倍。最后但并非最不重要的一点是，当患者独自一人且无行为能力时，智能语音助手可以触发紧急警报。这最后一点对单身老人尤其重要。

![](img/27b00c24ca7d939c8f4aa04c6b19f2cd.png)

[国立癌症研究所](https://unsplash.com/@nci?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/doctor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

在我看来，由于自然语言理解和元学习仍处于初级阶段，一个真正具有人工一般智能的虚拟语音代理将是遥远的未来。但这并不意味着我们今天不能建立一个具有许多实用功能的语音代理。事实上，许多软件构建模块已经可用。我们只需要把这些碎片拼在一起。例如，通过 AWS Lex，我们可以快速构建一个理解自然语言的聊天机器人。

![](img/76dabeaef38dd1dbd53f5ea81325a347.png)

图一。健康医疗网的 Doctor.ai 概念。图片作者。

2021 年 12 月 3 日至 5 日，在 Neo4j 的支持下，我和四名 Neo4j 同事参加了 2021 年新加坡医疗保健 AI 数据大会和博览会。我们已经建立了一个名为 Doctor.ai 的虚拟语音助手。Doctor.ai 建立在 eICU 数据集的基础上。我们在 AWS 上运行 Neo4j 作为我们的后端数据库。Lex 充当我们的语音代理，它通过 Lambda 连接到 Neo4j。最后，我们基于 Lucas Bassetti 的简单聊天机器人组装了一个前端。

![](img/08ab69fe26737858d290ea9a7e66fa51.png)

图二。Doctor.ai 图片作者截图。

Doctor.ai 可以在英语对话中为病人和医生服务。一方面，患者可以查询自己的病历，但不能查询别人的。另一方面，医生可以询问患者的病史，如过去的 ICU 就诊、诊断和接受的治疗。此外，Doctor.ai 还可以通过 Neo4j Graph 数据科学库为某些患者做初步的治疗建议。当与 AWS Kendra 结合使用时，Doctor.ai 甚至可以解释医学术语，并从医学文献中获取答案。

在本文中，我将带您完成 Doctor.ai 的设置，并解释它的一些功能，以便您也可以拥有自己的 Doctor.ai 克隆。不过，在这个演示中，我确实去掉了一些高级功能，如用户验证。代码存放在我的 Github 存储库中:

[](https://github.com/dgg32/doctorai_walkthrough) [## GitHub—dgg 32/doctor ai _ walk through

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/dgg32/doctorai_walkthrough) 

# 1.eICU 数据

Doctor.ai 建立在 eICU 数据集之上。根据官方文档，该数据集“由来自整个美国大陆的许多重症监护病房的组合数据填充。协作数据库中的数据涵盖了 2014 年和 2015 年入住重症监护室的患者”。它包含超过 139，000 名患者的 200，000 次 ICU 访问的实验室结果、人口统计信息、诊断、治疗和其他相关信息。我们使用完整的数据集作为我们的替代病历来开发 Doctor.ai。您还可以预览一下[演示数据集](https://physionet.org/content/eicu-crd-demo/2.0.1/)。

如果你想使用完整的数据集，你应该首先申请对它的授权访问(按照这里[的指示](https://eicu-crd.mit.edu/gettingstarted/access/))。你需要完成 CITI 的“数据或标本只研究”课程，并获得完成报告。然后，在生理网上填写表格，他们会在几天内审批你的申请。最后，您将能够从 Google 云平台请求访问存储在 Big Query 中的数据。

对于这个项目，我们只需要从`eicu_crd`和`eicu_crd_derived`数据库下载六个表:

```
eicu_crd_derived diagnosis_categories icustay_detail pivoted_labeicu_crd diagnosis microlab treatment
```

![](img/6204741acc507425d8592fbf8cb99d3c.png)

图 3。如何从 eICU 下载用于 dr . ai 的表格。

下载需要通过谷歌云存储(GCS)。选择每张桌子，点击`EXPORT`和`Export to GCS`，使用`gz`格式并选择其中一个桶作为目的地。然后，您可以将数据从您的 bucket 下载到您的本地机器。

# 2.架构、SAM 和手动配置

Doctor.ai 由 EC2 实例上的后端 Neo4j 数据库、自然语言理解引擎 Lex 和 Amplify 托管的前端 web 应用组成(图 4)。在 datathon 中，我们使用了 Neo4j Enterprise，因为它允许我们通过基于角色的访问控制(RBAC)特性来管理医生/患者的权限。我们也将 Kendra 作为我们的 FAQ 引擎。

![](img/768caf932b8ae1c1b7d05166d4b9f02d.png)

图 4。艾医生的建筑形象作者。

## 2.1 SAM

在 datathon 之后，我已经将大部分基础设施编入了一个 AWS 无服务器应用程序模型(SAM)项目。从上面我的 Github 链接克隆项目。为您的 EC2 创建一个名为`cloudformation.pem`的密钥对，`chmod 400`并将其放入项目文件夹中。使用 SAM CLI，现在您只需要以下三个命令来操作基础架构:

选择一个区域，如`us-east-1`，其中 [Lex 可用](https://docs.aws.amazon.com/lex/latest/dg/supported-regions.html)。部署会很快。它将输出 Lex 的 ID、我们的 Neo4j 的 IP 地址和域名，以供后续步骤使用。

不幸的是，AWS SAM 到处都有一些 bug。例如，它不能设置 Lex 别名(此处读)。当在 SAM 中定义时，Amplify 不能自动构建，它也有环境变量的问题。当使用 Kendra FAQ 时，导入的 bot 会在 KendraSearchIntent 中出错。所以我们需要手动配置 Neo4j，Lex 和 Amplify，然后 Doctor.ai 才能上线。

## 2.2 EC2

首先，使用您的密钥对以用户“ubuntu”的身份登录您的 EC2:

```
ssh -i "cloudformation.pem" ubuntu@[your neo4j EC2 public domain name]
```

您需要将这六个表导入到 Neo4j 中。尽管可能存在其他选项，但我建议您首先将这六个文件转移到 EC2 中的`/var/lib/neo4j/import`文件夹中。然后通过以下 URL 在您的浏览器中登录 Neo4j:

```
[http://[your Neo4j EC IP address]:7474/browser/](http://44.201.23.14:7474/browser/)
```

输入初始用户名“`neo4j`”和密码“`s00pers3cret`”，迎接你的将是熟悉的 Neo4j 浏览器界面。从我的存储库中运行`neo4j_command.txt`中的命令来导入数据(如果需要，调整文件名)。

## 2.3 Lex

导入后，我们可以继续进行 Lex。首先，确保你在 Lex V2 控制台中(只要`Return to the V1 console`在左侧面板中可见)。

![](img/cbb5a484fd7d9074a72095ed680f36a1.png)![](img/63dd2fb83258d377227ef7ee0025cfe3.png)

图 5。Lex 的配置。图片作者。

点击`LexForDoctorai` ➡️ `Aliases` ➡️ `TestBotAlias` ➡️ `English (US)`进入`Lambda function`页面。选择`LambdaForLex`和`$LATEST`并点击`Save`按钮。

最后，让我们构建`LexForDoctorai`来测试 bot 是否功能正常。点击`Intents`并点击`Build`按钮。

![](img/4bcb74dfacbc00be7ce5a115c5e98fa9.png)

图 6。如何构建 Lex？图片作者。

构建完成后，您可以使用测试控制台测试`LexForDoctorai`。在这里，你可以看到医生。人工智能已经可以进行很好的对话。

![](img/7e71155f962c2209a090b8e7ee9e9f24.png)

图 7。在测试控制台中测试 Lex。图片作者。

## 2.4 前端

Lex 的测试控制台非常好，功能也非常强大。它既可以听用户说话，也可以向用户回话。然而，我们需要一个前端，以便我们可以将 Doctor.ai 部署为 web 或智能手机应用程序。我们将 Lucas Bassetti 的 React 前端放在一起，并在 Amplify 上托管。我试图在 SAM 中部署前端，但是遇到了错误。所以让我们手动部署放大器。

首先，将这个库分支到您的 Github 帐户，因为 Amplify 只能从您自己的帐户中检索代码。

[](https://github.com/dgg32/doctorai-ui) [## GitHub — dgg32/doctorai-ui

### 这是 Doctor.ai 的 React 前端应用程序，我们自豪地提交给新加坡医疗保健 ai 数据大会和博览会…

github.com](https://github.com/dgg32/doctorai-ui) 

一旦完成，前往 AWS 放大页面，点击`New app` ➡️ `Host web app`。然后选择`Github`, Amplify 将获取你账户下的所有存储库。在您的帐户下选择`doctorai-ui`。点击`Next`进入`Configure build settings`页面，点击打开`Advanced settings`，添加五个密钥对环境变量:`REACT_APP_AWS_ACCESS_KEY`、`REACT_APP_AWS_SECRET`、`REACT_APP_AWS_USERID`、`REACT_APP_LEX_botId`和`REACT_APP_AWS_REGION`。它们是您的 AWS 访问密钥、AWS 秘密访问密钥、您的 AWS 用户 id、BotID(您可以从`sam deploy`输出中获得该值)和您的 AWS 区域。

![](img/8e4eb2c78f3da856e285f2828df75d03.png)![](img/7b930188bd7772b6066a0c95894aac2b.png)

图 8。放大器的配置。图片作者。

然后点击`Next`和`Save and deploy`。

# 3.Doctor.ai 是如何工作的

在我们继续测试 Doctor.ai 之前，让我解释一下 Doctor.ai 是如何工作的。本质上，Doctor.ai 是一个信息检索系统。尽管它能掌握自然语言，能理解上下文，但它并不是一个真正的健谈者。它只理解一组预定义的查询。所以，我们需要有目的的和 dr . ai 说话，比如我们可以问一个病人去了多少次 ICU，是否曾经感染过*金黄色葡萄球菌*，接受过什么样的治疗。在莱克斯的行话中，这些“目的”被称为“意图”。

目前，Doctor.ai 可以实现以下意图:它检查这是否是第一次 ICU 就诊；它计算病人入院的次数；它显示过去的诊断、实验室结果和分离的微生物；它甚至可以推荐治疗方法。通过增加一些礼貌、命令和测试意图，Doctor.ai 可以像人类接待员一样进行简短的对话。由于上下文理解，Doctor.ai 还可以理解代词。我们用每个意图的样本话语训练 Lex，以便它能够理解生产中的类似话语。因此，当与 Doctor.ai 对话时，它会尝试将用户输入分类为十二种意图之一。如果分类失败，Doctor.ai 将退回到 FallbackIntent。

知道 AWS Kendra 和 Lex 的区别很有意思。Kendra 可以从其摘要文本语料库中以文本摘录的形式给出答案。本质上，它很像一个内部私有数据的搜索引擎。但是它不能聚合数字数据。例如，我们不能问病人去过 ICU 多少次，他的平均血糖水平是多少，或者某个病人的最后两次诊断是什么。相反，Lex 可以在 Lambda 函数的帮助下完成这些查询。这些函数通过 Neo4j 驱动程序查询后端 Neo4j 数据库。我们使用了图形数据库 Neo4j，因为它可以直观、轻松地对许多复杂的 eICU 维度进行建模。它使 Lex 能够汇总患者健康史许多方面的数据。莱克斯甚至可以建议在 GDS 的帮助下进行治疗。

Doctor.ai 中的治疗建议是基于用户相似度的。原则上，它的工作方式与电子商务网站中的产品推荐相同。具体来说，Doctor.ai 计算所有患者之间的成对余弦相似度。相同性别、年龄差异小于 10 且相似性得分高于 0.9 的患者被视为相似。我们设置这些严格的标准是因为我们想避免误报。当患者需要治疗建议时，Doctor.ai 首先返回他的类似患者已经接受的治疗，并考虑建议的治疗是否与患者的当前诊断兼容。如果治疗满足约束条件，Doctor.ai 会将它们推荐给医生。但是由于严格的标准和诊断及治疗数据的缺乏，目前只有少量的患者会得到治疗建议。

为了保护隐私，我们可以通过用户认证和授权来控制患者和医生可以看到谁的记录。借助 Neo4j Enterprise，我们甚至可以使用基于角色的访问控制(RBAC)来对某些维度进行保密。例如，我们可以让医生无法访问“种族”维度，但患者自己可以访问。

# 4.测试 Doctor.ai

理论了半天，辛苦了半天，还是来测试一下 Doctor.ai 吧，用 Chrome 打开 Amplify 里的`Production branch URL`。因为 eICU 数据是匿名的，所以我们使用`pid`作为患者的姓名。

## 4.1 诊断

我们可以一个一个的阅读或者键入下面的询问，看看 Doctor.ai 是怎么回复的。

```
Are you online?This is patient 002-43934How many times did he visit the ICU?What was the diagnosis?
```

![](img/142acfd23d74a8c646d4440118c5e2c6.png)

图 9。作者在.图像中的诊断检索。

正如你从上面的截图或你自己的测试中所看到的，Doctor.ai 能够告诉我们，患者 002–43934 因为心脏骤停已经两次进入 ICU。

## 4.2 实验室结果

假设患者 002–33870 在我们面前，我们想知道他的葡萄糖和血红蛋白水平:

```
Are you onlineThis is patient 002–33870What was his glucose level?What was his Hemoglobin level?
```

![](img/6825ae17fae156f941b117d14ad476ad.png)

图 10。作者在 dr . ai 图像中检索实验室结果。

艾医生很快检索了他最近两次重症监护室就诊时的血糖和血红蛋白读数。

## 4.3 治疗建议

最后，让我们来看看 Doctor.ai 会为 003–2482 号患者推荐哪些治疗方法。

```
This is patient 003–2482What was the diagnosis?treatment recommendation
```

![](img/64694c62573ce435ed32d377175b027b.png)

图 11。作者在 Doctor.ai 图片中的治疗建议。

有趣的是，艾医生建议对这位在最后一次重症监护室就诊时服药过量的患者进行会诊。这个建议乍一看很奇怪。但是药物过量可能会损害大脑功能，所以为了他的完全康复，神经科会诊可能是必要的。

# 结论

在这个项目中，我们将 Neo4j、AWS 和 eICU 数据集放在一起，构建了一个小型虚拟语音助手。尽管 Doctor.ai 目前只能完成有限的一组查询，但不难看出它在医疗保健方面的巨大潜力:我们可以在 ICU、精神病诊所和牙医中使用它。通过改变底层数据，我们甚至可以使其成为其他行业的通用问答聊天机器人。

Doctor.ai 还需要更多的打磨才能成为一款成熟的产品。首先，它的语音识别是由 Chrome 浏览器支持的，并不总是很精确。其次，在交谈中经常会混淆。这部分是由于它的上下文记忆仅持续五分钟。但更有可能的是，它的一些配置需要优化。第三，虽然 eICU 是一个大数据集，但许多患者的记录不完整。这使得信息检索和机器学习变得困难。我们还可以训练它理解更多的意图，提高它的情境意识。此外，您可以将 Kendra 添加到组合中。最后，虽然 Neo4j 社区版功能非常强大，可以有效处理这个 demo，但是并不是针对生产的。所以你应该考虑用[企业版或者 AuraDB](https://neo4j.com/pricing/#editions) 来代替。

所以请试试 Doctor.ai 并给我们你的反馈。

*更新:*

[*关于 Doctor.ai 的第二篇文章*](https://neo4j.com/blog/doctor-ai-a-voice-chatbot-for-healthcare-powered-by-neo4j-and-aws/) *已经发表在 Neo4j 的官方博客上。它深入到 Lambda 和 Lex 的实现。*

[*第三篇*](/transfer-knowledge-graphs-to-doctor-ai-cc21765fa8a6) *讲的是把三个知识图谱转移到 Doctor.ai 中，他们把 Doctor.ai 变成了一个更有知识的聊天机器人。*

[*第四篇*](https://dgg32.medium.com/from-symptoms-and-mutations-to-diagnoses-doctor-ai-as-a-diagnosis-tool-5b31ac7a16c3) *是根据第三篇的知识图而来。由于知识图表的数据，Doctor.ai 现在可以根据症状或突变的基因进行简单的诊断。*

[*第五篇*](https://medium.com/p/9e4b0b10a400) *是将图分发给 P2P 网络的尝试。*

[*第六篇*](https://medium.com/p/1396d1cd6fa5) *使用 GPT-3 作为 NLU，提高性能，减少开发时间，收缩代码。*

The seventh article [*Can Doctor.ai understand German, Chinese and Japanese? GPT-3 Answers: Ja, 一点点 and できます!*](https://dgg32.medium.com/can-doctor-ai-understand-german-chinese-and-japanese-gpt-3-answers-ja-%E5%8F%AF%E4%BB%A5-and-%E3%81%84%E3%81%84%E3%82%88-b63b10d67bf4)shows that Doctor.ai can understand German, Chinese and Japanese thanks to GPT-3.

[第八篇](https://medium.com/p/5f0b2f479cca)用 Alan AI 提高 Doctor.ai 的语音识别。

[第九篇](https://medium.com/p/5c2410b08d51)使用 Synthea 作为新的替身数据。

[第十篇](https://medium.com/geekculture/relationship-extraction-with-gpt-3-bb019dcf41e5)使用 GPT-3 从原始文本中提取主谓宾。

[第十一部](https://medium.com/p/a5c2c4580977)使用 GPT-3 来 ELI5 复杂的医学概念，使它们更容易理解。

[第 12 期](https://medium.com/p/f4cea26743a4)对比《ai 医生》中 GPT-J 和 GPT-3

[第 13 个](https://medium.com/geekculture/how-to-build-a-bayesian-knowledge-graph-dee1cc821d35)演示了一个贝叶斯知识图。

[第 14 篇](https://medium.com/p/8422c563b791)文章基于 Doctor.ai + GPT-3 + Kendra 构建了一个集合聊天机器人。

[](https://dgg32.medium.com/membership) [## 加入媒介与我的介绍链接-黄思兴

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

dgg32.medium.com](https://dgg32.medium.com/membership)