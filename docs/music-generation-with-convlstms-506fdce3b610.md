# 使用 ConvLSTMs 生成音乐

> 原文：<https://towardsdatascience.com/music-generation-with-convlstms-506fdce3b610?source=collection_archive---------18----------------------->

## 用可播放的人工智能生成的音乐文件

![](img/dcbddfb88b5b9217fb01110497ceeea0.png)

Rajesh Kavasseri 在 [Unsplash](https://unsplash.com/s/photos/classical-music?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

这个项目是我的[另一个项目](/bachgan-using-gans-to-generate-original-baroque-music-10c521d39e52)的延续，因为我想到了一个产生更好结果的想法。我将使用 LSTMs，而不使用 GANs。这将使训练更快，并允许客观的评估函数来评估模型的性能。

在人类作曲家创作的音乐上训练，模型被训练使用过去的音符来预测下一个音符。这导致模型发现过去的音符和下一个音符之间的模式，这将允许原始音乐的生成

下面是我特别喜欢的一段摘录:

《LSTM 2》很有趣，因为它的结尾听起来像是要进入一个完美的节奏，和弦进行到 IV-V-I。在这个可能的序列之前的和弦也很有特点。

记住这段音乐，让我们开始构建程序:

# 数据预处理:

机器学习的第一步是数据预处理。对于这个项目，它包括两个步骤:

## 访问 Midi 文件:

我在网上找到了一个古典作品的数据集，是从一个在线网站上搜集来的。我把所有的 midi 文件提取出来，放在一个文件夹里。

## 将 Midi 文件转换成图像:

我找到了一个 github 页面,其中有两个程序使用 music21 库将 midi 文件转换成图像，然后再转换回来。

每个音符可以表示为一个白块。音块的高度决定了音高，长度决定了音符演奏的时长。

然后，我编写了一个脚本，将这两个程序与我的 midi 文件集成在一起，在不同的目录中创建新的图像:

```
import os
import numpy as nppath = 'XXXXXXXXX'os.chdir(path)
midiz = os.listdir()
midis = []
for midi in midiz:
    midis.append(path+'\\'+midi)
```

这个脚本转到 midi 目录，然后将所有 midi 文件路径添加到一个列表中，供以后访问。

```
from music21 import midimf = midi.MidiFile()
mf.open(midis[0]) 
mf.read()
mf.close()
s = midi.translate.midiFileToStream(mf)
s.show('midi')
```

这个脚本打开第一个 midi 文件，并播放它以确保程序正常工作。如果在非交互环境中运行，这可能不起作用。

```
import os
import numpy as np
import py_midicsv as pmos.chdir(path)
midiz = os.listdir()
midis = []
for midi in midiz:
    midis.append(path+'\\'+midi)

new_dir = 'XXXXXXXX'
for midi in midis:
    try:
        midi2image(midi)
        basewidth = 106
        img_path = midi.split('\\')[-1].replace(".mid",".png")
        img_path = new_dir+"\\"+img_path
        print(img_path)
        img = Image.open(img_path)
        hsize = 106
        img = img.resize((basewidth,hsize), Image.ANTIALIAS)
        img.save(img_path)
    except:
        pass
```

这个脚本使用 github 页面中的 midi2image 函数，并根据 midi 文件的路径转换所有的 midi 文件。它们也被重新成形为形状(106，106)。为什么？106 是程序的高度，因为这是 midi 文件中可能的音符数。此外，对于卷积转置，使用正方形要容易得多。

# 构建数据集:

```
import os
imgs = os.listdir()
pixels = []from PIL import Image
import numpy as npfor img in imgs:
  try:
    im = Image.open(img).rotate(90)
    data = np.array(im.getdata())/255
    pix = (data).reshape(106,106)
    pixels.append(pix)
  except:
    pass
```

这个脚本遍历目录，并记录所有的图像数据。请注意，所有图像都需要旋转 90 度，因为这将允许 getdata 函数按时间顺序而不是间距顺序访问数据。

```
def split_sequences(sequence, n_steps):
    X, y = list(), list()
    for i in range(len(sequence)):
        end_ix = i + n_steps
        if end_ix > len(sequence)-1:
            break
        seq_x, seq_y = sequence[i:end_ix-1], sequence[end_ix]
        X.append(seq_x)
        y.append(seq_y)
    return np.array(X), np.array(y)X = []
y = []
for i in range(len(pixels)):
    mini_x,mini_y = split_sequences(pixels[i],10)
    X.append(mini_x)
    y.append(mini_y)
```

这个脚本从时间序列数据中构造 X 和 y 列表。使用变量 mini_x，使得 X 列表由 9 个音符的单独集合组成，y 列表由映射到每个 9 个音符的集合的单独音符组成。

```
X = np.array(X)
y = np.array(y)
```

当数据被输入到模型中时，将 X 和 y 列表转换成 NumPy 数组将不会出现错误。

```
X = X.reshape(len(X),1,9,106)
y = y.reshape((y.shape[0]*y.shape[1],y.shape[2]))
```

这个脚本重塑了 X 和 y 数组，使其适合 ConvLSTM。y 值将是 106 个 1 和 0 的值。1 表示将在该音高播放一个音符，而 0 表示在该时间步长内不会播放该音高的任何音符。

# 构建 ConvLSTM:

```
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Flatten
from keras.layers import Dropout,BatchNormalization
from keras.layers import LSTM,TimeDistributed
from keras.layers.convolutional import Conv1D,MaxPooling1Dmodel = Sequential()
model.add(TimeDistributed(Conv1D(filters=128, kernel_size=1, activation='relu'), input_shape=(None, 9, 106)))
model.add(TimeDistributed(MaxPooling1D(pool_size=2, strides=None)))
model.add(TimeDistributed(Conv1D(filters=128, kernel_size=1, activation='relu')))
model.add(TimeDistributed(MaxPooling1D(pool_size=2, strides=None)))
model.add(TimeDistributed(Conv1D(filters=128, kernel_size=1, activation='relu')))
model.add(TimeDistributed(MaxPooling1D(pool_size=2, strides=None)))
model.add(TimeDistributed(Conv1D(filters=128, kernel_size=1, activation='relu')))
model.add(TimeDistributed(Flatten()))
model.add(LSTM(128,return_sequences = True))
model.add(LSTM(64))
model.add(BatchNormalization())
model.add(Dense(106,activation = 'sigmoid'))
model.compile(optimizer='adam', loss='mse')
```

这是使用的完整模型架构。我发现这种型号非常通用。这个模型和我根据历史数据预测股票价格的模型是一样的。主要区别在于，最后一层使用具有 106 个节点的 sigmoid 函数，因为每个时间步长必须被描述为 106 个音符，以及它们是否将被播放。

```
model.summary()
```

当调用这个函数来查看模型架构时，我们得到这样的结果:

```
Model: "sequential_4"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
time_distributed_25 (TimeDis (None, None, 9, 128)      13696     
_________________________________________________________________
time_distributed_26 (TimeDis (None, None, 4, 128)      0         
_________________________________________________________________
time_distributed_27 (TimeDis (None, None, 4, 128)      16512     
_________________________________________________________________
time_distributed_28 (TimeDis (None, None, 2, 128)      0         
_________________________________________________________________
time_distributed_29 (TimeDis (None, None, 2, 128)      16512     
_________________________________________________________________
time_distributed_30 (TimeDis (None, None, 1, 128)      0         
_________________________________________________________________
time_distributed_31 (TimeDis (None, None, 1, 128)      16512     
_________________________________________________________________
time_distributed_32 (TimeDis (None, None, 128)         0         
_________________________________________________________________
lstm_6 (LSTM)                (None, None, 128)         131584    
_________________________________________________________________
lstm_7 (LSTM)                (None, 64)                49408     
_________________________________________________________________
batch_normalization_2 (Batch (None, 64)                256       
_________________________________________________________________
dense_2 (Dense)              (None, 106)               6890      
=================================================================
Total params: 251,370
Trainable params: 251,242
Non-trainable params: 128
_________________________________________________________________
```

您可以摆弄模型中的超参数，看看它如何影响模型的结果。

```
model.fit(X,y,epochs = 100)
```

此脚本为 100 个时期训练模型。注意，不需要验证数据，因为模型实际上不需要 100%准确地预测下一个音符。它只是应该从源音乐中学习一种模式，以便创建一个似乎合理的下一个音符。

# 听证结果:

```
song_length = 106
data = X[np.random.randint(len(X))][0]
song = []
for i in range(song_length):
    pred = model.predict(data.reshape(1,1,9,106))[0]
    notes = (pred.astype(np.uint8)).reshape(106)
    print(notes)
    song.append(notes)
    data = list(data)
    data.append(notes)
    data.pop(0)
    data = np.array(data)
```

这个函数允许模型根据自己的音乐生成自己的音乐。它是这样工作的:

为了启动这一过程，模型需要从数据集中随机抽取一个实例来进行预测。数据列表的功能就像一个队列:每次做出一个新的预测，它都会被添加到列表的末尾。为了使输入数据的形状在每次迭代中都相同，有必要删除第一个实例。通过收集预测，它显示生成的片段完全是计算机生成的，不包含来自原始实例的任何信息。

```
new_image = Image.fromarray(np.array(song)).rotate(-90)
new_image.save('composition.png')
```

之后，我们会将图像向后旋转 90 度，以便 image2midi 功能可以正常工作。

```
image2midi('composition.png')
```

然后我们将图像转换成 midi 文件，我们可以使用下面的代码来听它(至少在 colab 笔记本中是这样的):

```
!apt install fluidsynth
!cp /usr/share/sounds/sf2/FluidR3_GM.sf2 ./font.sf2
!fluidsynth -ni font.sf2 composition.mid -F output.wav -r 44100
from IPython.display import Audio
Audio('output.wav')
```

让我们听听最初的结果:

![](img/91309438642dbc38d71f9888a816101e.png)

没什么。

没有结果。

# 人为改变结果:

检查结果时，模型无法得出大于 0.6 的值。当 NumPy 向下舍入任何低于 0.6 的值时，没有实际的音符被演奏。我到处寻找改变模型架构的方法，但是网上什么都没有。

我想到的解决方案相当不正统:手动改变结果。

网络的问题在于预测值太小。如果我们给预测加上一定的值，它将能够显示实际的音符。

这怎么能不干扰模型的预测呢？这是因为我们可以将模型的预测可视化，而不是播放或不播放，而是这个音符与当前时间步长的吻合程度。这一点得到了以下事实的支持:该模型是用 MSE 损失函数训练的:预测值和实际值之间的差异很重要。由于保留了模型预测的初始值，模型的预测被简单地放大了。

下面是改进的生成函数:

```
song_length = 106
data = X[np.random.randint(len(X))][0]
song = []
vanish_proof = 0.65
vanish_inc = 1.001
for i in range(song_length):
    pred = model.predict(data.reshape(1,1,9,106))[0]+vanish_proof
    vanish_proof *= vanish_inc
    notes = (pred.astype(np.uint8)).reshape(106)
    print(notes)
    song.append(notes)
    data = list(data)
    data.append(notes)
    data.pop(0)
    data = np.array(data)
```

如您所见，vanish_proof 和 vanish_inc 有两个新变量。Vanish_proof 是添加到所有预测中的值，vanish_inc 是 vanish_proof 增加的速率。这是必要的，因为网络的每个新预测将基于过去的预测。如果预测很小，这种效应会向前传播，使歌曲慢慢淡出。

这产生了更好的结果，就像本文开头显示的那样。这是我喜欢的另一段摘录:

# 结论:

我认为这篇文章最有见地的部分是人工改变结果，直接引出褪色的结果。我还没有真正在其他资源中看到过这种方法，我也不确定这种方法有多好或多有效。您可以尝试使用 vanish_proof 和 vanish_inc 参数。太高了，曲子被放错的音符打断了。

# 我的链接:

如果你想看更多我的内容，点击这个 [**链接**](https://linktr.ee/victorsi) 。