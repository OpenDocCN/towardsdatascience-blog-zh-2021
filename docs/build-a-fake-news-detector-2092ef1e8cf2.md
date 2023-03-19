# 建立假新闻检测器

> 原文：<https://towardsdatascience.com/build-a-fake-news-detector-2092ef1e8cf2?source=collection_archive---------46----------------------->

## 是时候粉碎错误信息了

![](img/37dd489b40ee6490fc82d3af59d6cfb9.png)

作者图片

如果冠状病毒教会了我一件事，那就是没有人能幸免于这种危险的病毒。它在世界的某些地方成倍增长，让我们担心和害怕我们所爱的人和我们自己的身体健康。但是你有没有花一点时间停下来想想错误的信息会如何影响你？你根据这些数据做出的决定？事实上，虚假新闻传播或“病毒式传播”的可能性高达 70%[1]。想象一下，有些决策是基于这种虚假的病毒式新闻做出的。令人欣慰的是，人工智能在对抗错误信息方面取得了一些重大进展。

在本文中，我们将使用 python 构建我们自己的假新闻检测器。我们将利用一些测试数据和代码，使用被动积极分类器(PAC)用我们的数据训练模型，该分类器将确定一段文本是真还是假。PAC 背后的主要概念是，它占用的内存较少，并且使用每个训练样本来重新评估训练算法的权重。预测正确，保持不变，预测错误，调整算法。它是一种流行的机器学习算法，特别是在做假新闻检测等工作时。其背后的数学并不简单，所以我们将把它留给另一篇文章。在本文中，让我们构建一个实际的应用程序。准备好了吗？我们开始吧！

```
To see a video demonstration of this exercise check out the youtube video with code walkthrough at [https://youtu.be/z_mNVoBcMjM](https://youtu.be/z_mNVoBcMjM)
```

为了开发这个应用程序，我们将利用两个流行的 python 包，一个是 [pandas](https://pandas.pydata.org) ，另一个是 [sklearn](https://scikit-learn.org/stable/) 。完整的代码和数据可以在我的 g [ithub 链接](https://github.com/satssehgal/FakeNewsDetector)找到。所以让我们从导入我们的需求开始

```
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import PassiveAggressiveClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, confusion_matrix
from sklearn.model_selection import cross_val_score
```

一旦我们导入了需求，下一步就是导入数据集。我们从 kaggle 得到的数据和直接链接可以在上面链接的 github 链接上找到。

```
df=pd.read_csv('fake-news/train.csv')
conversion_dict = {0: 'Real', 1: 'Fake'}
df['label'] = df['label'].replace(conversion_dict)
df.label.value_counts()Fake    10413
Real    10387
Name: label, dtype: int64
```

一旦我们将数据导入 pandas 数据框架，我们将根据样本数据重新标记数据的真假。结果表明，我们有一个相当均匀和平衡的数据集，不需要在真实和虚假评论之间进行任何采样。所以我们不必担心不平衡的数据集。咻！少了一件需要担心的事情！

然后，我们获取数据集，创建一个训练测试分割，并保留一定百分比进行测试，在本例中为数据集的 25%。测试数据将由作为 X 变量的实际文本和作为 Y 值的标签组成。为了确保我们只使用关键字，我们还从算法中删除了所有停用词。

```
x_train,x_test,y_train,y_test=train_test_split(df['text'], df['label'], test_size=0.25, random_state=7, shuffle=True)
tfidf_vectorizer=TfidfVectorizer(stop_words='english', max_df=0.75)vec_train=tfidf_vectorizer.fit_transform(x_train.values.astype('U')) 
vec_test=tfidf_vectorizer.transform(x_test.values.astype('U'))pac=PassiveAggressiveClassifier(max_iter=50)
pac.fit(vec_train,y_train)PassiveAggressiveClassifier(C=1.0, average=False, class_weight=None,
                            early_stopping=False, fit_intercept=True,
                            loss='hinge', max_iter=50, n_iter_no_change=5,
                            n_jobs=None, random_state=None, shuffle=True,
                            tol=0.001, validation_fraction=0.1, verbose=0,
                            warm_start=False)
```

然后，我们用数据训练模型，运行 50 次迭代，并使其符合 PAC。太好了，现在我们有一个训练有素的分类器。接下来让我们测试我们的训练模型的准确性。我们运行 y 测试和 y_pred 准确度分数，看到我们得到了一个不错的分数。96.29%.太好了！

```
y_pred=pac.predict(vec_test)
score=accuracy_score(y_test,y_pred)
print(f'PAC Accuracy: {round(score*100,2)}%')PAC Accuracy: 96.29%
```

为了进一步确保我们的模型预测正确的行为，我们查看混淆矩阵，以帮助我们更好地了解我们在结果集中看到多少真阴性和假阳性。

```
confusion_matrix(y_test,y_pred, labels=['Real','Fake'])array([[2488,   98],
       [  95, 2519]])
```

基于上面的混淆矩阵，结果看起来相当不错。在大多数情况下，它准确地预测了正确的结果。好吧，但是我们也不要就此打住。让我们针对这些结果运行黄金标准测试，这是一个 K 倍精度测试。

```
X=tfidf_vectorizer.transform(df['text'].values.astype('U'))
scores = cross_val_score(pac, X, df['label'].values, cv=5)
print(f'K Fold Accuracy: {round(scores.mean()*100,2)}%')K Fold Accuracy: 96.27%
```

这导致了 96.27%的 k 倍测试分数。好的，我对这个结果很满意。好了，现在让我们向这个模型展示一些它从未见过的数据，看看它在预测正确标签方面表现如何。

```
df_true=pd.read_csv('True.csv')
df_true['label']='Real'
df_true_rep=[df_true['text'][i].replace('WASHINGTON (Reuters) - ','').replace('LONDON (Reuters) - ','').replace('(Reuters) - ','') for i in range(len(df_true['text']))]
df_true['text']=df_true_rep
df_fake=pd.read_csv('Fake.csv')
df_fake['label']='Fake'
df_final=pd.concat([df_true,df_fake])
df_final=df_final.drop(['subject','date'], axis=1)
df_fake
```

在上面的代码块中我们引入了两个新的数据集，df_true 表明我们都是真实的新文档，df_fake 表明我们都是虚假的文章。我想把这些文章放入我们训练好的分类器，看看它对 df_true 数据集预测为真的次数，以及对 df_fake 数据集预测为假的次数的百分比。

为此，让我们构建一个快速函数，该函数将返回模型的一个不可见数据集的标签

```
def findlabel(newtext):
    vec_newtest=tfidf_vectorizer.transform([newtext])
    y_pred1=pac.predict(vec_newtest)
    return y_pred1[0]findlabel((df_true['text'][0]))'Real'
```

在这个函数中，我们传递一些数据集，它将根据我们的 PAC 训练算法计算数据集是真是假。然后，我们运行 df_true 数据帧中的第一个文本文档，希望它能预测 real，在本例中就是这样。我想看到的是它准确预测正确数据集的时间百分比。

```
sum([1 if findlabel((df_true['text'][i]))=='Real' else 0 for i in range(len(df_true['text']))])/df_true['text'].size0.7151328383994023sum([1 if findlabel((df_fake['text'][i]))=='Fake' else 0 for i in range(len(df_fake['text']))])/df_fake['text'].size0.6975001064690601
```

好了，我们已经循环了 df_true 数据集中的数据，并注意到它在 71.5%的时间里准确地预测了“真实”。当我们查看 df_fake 数据集时，它准确预测 69.75%为“假”

对我来说，这实际上是一个非常有趣的项目，我也非常喜欢做这个项目。虽然训练结果看起来很惊人，但实际测试数据与看不见的数据相比并没有那么高，但是嘿，如果我能阻止来自互联网的错误信息大约 7/10 次，我认为这是一个胜利。

希望您喜欢这个代码演练。请务必登录 [levers.ai](https://levers.ai) 查看我们，了解更多关于我们服务的信息。

来源:[https://www . Reuters . com/article/us-USA-cyber-twitter/false-news-70%更有可能在 Twitter 上传播-study-iduscn1 gk2 QQ](https://www.reuters.com/article/us-usa-cyber-twitter/false-news-70-percent-more-likely-to-spread-on-twitter-study-idUSKCN1GK2QQ)