# Python 中的自动 HMM

> 原文：<https://towardsdatascience.com/auto-hmm-in-python-254bc937cbf6?source=collection_archive---------5----------------------->

## 隐马尔可夫模型的自动模型选择、训练和测试

![](img/d4e40bb81c0b6e06d472113e09275593.png)

马丁·桑切斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在加州大学洛杉矶分校攻读博士期间，我开发了各种序列和时间序列数据模型。隐马尔可夫模型(HMM)是我最早使用的模型之一，效果相当好。我在 Python/MATLAB/R 中找不到任何关于 HMM 的教程或工作代码。我正在发布 Auto-HMM，这是一个 Python 包，使用 AIC/BIC 为监督和非监督 HMM 执行自动模型选择。

该软件包使用 hmmlearn 进行隐马尔可夫模型训练和解码，并且它包括用于最佳参数数量(混合成分数量、隐藏状态数量等)的模型选择。我用 MATLAB 和 r 实现了一个类似的包。

如果你想看同一个模型的 R 和 MATLAB 实现，请在这篇文章下评论。

HMM 中的训练是通过 Baum-Welch 完成的，这是 EM 算法的特例。解码是通过维特比算法完成的。我猜除了 ML(维特比)解码器之外，hmmlearn 包还支持 MAP 解码器。

模型选择是通过 AIC 和 BIC 完成的，它们通过惩罚似然函数来运行。这是通过指定您喜欢的隐藏状态的最大数量来自动完成的，并且该算法为离散 HMM 找到隐藏状态的最佳数量、最佳数量混合分量以及为连续 HMM 找到隐藏状态的最佳数量。

要访问代码，请访问我的 GitHub

[](https://github.com/manitadayon/Auto_HMM) [## GitHub - manitadayon/Auto_HMM:隐马尔可夫模型

### 隐马尔可夫模型。在 GitHub 上创建一个帐户，为 manitadayon/Auto_HMM 开发做贡献。

github.com](https://github.com/manitadayon/Auto_HMM) 

请仔细阅读 DHMM 测试和嗯测试 Python 文件中的例子。它们要求您提供 CSV 文件地址、HMM 模型的迭代次数、训练规模、特征数量(时间序列的维度)。

例如，以下代码对 2000 个观察值执行离散 HMM，每个观察值有 50 个时间点，最多有 3 个隐藏状态。训练大小设置为 0.8 (0.8 * 2000 = 1600)。该功能决定了时间序列的维数(1 表示单变量时间序列)。该标志决定了您是希望按降序还是升序对隐藏状态进行排序。

```
from Hidden_Markov_Model import *
from Hidden_Markov_Model.DHMM import *
Train_ratio=0.8
Max_state=3
Iter=1000
Feat=1
N=2000
T=50
flag=0
N_symb=3
Path= 'Path to CSV file'
Data=pd.read_csv(Path)
Data=Data.astype(int)
First_DHMM=Supervised_DHMM(Train_ratio,Max_state,Iter,Feat,N,T,Data,N_symb)
First_DHMM.Best_States()
```

关于我最近的活动和任何问题的更多信息，请访问我的 YouTube 页面。我还编写了 tsBNgen，这是用于生成合成时间序列数据的 Python 包，可以用作 Auto-HMM 模型的输入。要了解更多关于 tsBNgen 的信息，请访问下面我的 YouTube 页面。

[](https://www.youtube.com/channel/UCjNDIlqrFdKUgSeN9wV-OzQ) [## 人工智能和 ML 基础

### 关于数值线性代数、最优化、统计学、机器学习、编码等的教育视频。专注于…

www.youtube.com](https://www.youtube.com/channel/UCjNDIlqrFdKUgSeN9wV-OzQ) 

如果您有任何问题或疑虑，可以访问我的个人页面

 [## 首页- Manie Tadayon

### 欢迎来到我的个人页面项目项目和出版物在机器学习，人工智能和信号处理。更多信息…

manietadayon.com](https://manietadayon.com/) 

要了解有关 HMM 建模和实现的更多信息，请参考以下视频: