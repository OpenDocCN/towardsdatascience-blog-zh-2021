# 使用 PyTorch 在 40 行代码中训练神经网络来预测图像的连续属性。

> 原文：<https://towardsdatascience.com/train-neural-net-to-predict-continuous-properties-from-an-image-in-40-lines-of-code-with-pytorch-c3a408a02b52?source=collection_archive---------6----------------------->

本教程的目标是如何使用 Pytorch 以最短的方式预测图像的连续属性，如颜色、填充级别。我们将学习加载现有的网络，修改它以预测特定的属性，并在不到 40 行代码(不包括空格)内训练它。

支持本教程的完整代码可以在这里找到:

<https://github.com/sagieppel/Train-neural-net-to-predict-continuous-property-from-an-image-in-40-lines-of-code-with-PyTorch>  

标准神经网络通常专注于分类问题，如识别猫和狗。然而，这些网络可以很容易地修改，以预测图像的连续属性，如年龄，大小或价格。

首先，让我们导入包并定义主要的训练参数:

```
import numpy as np
import torchvision.models.segmentation
import torch
import torchvision.transforms as tfLearning_Rate=1e-5
width=height=900
batchSize=1
```

*Learning_Rate* :训练时梯度下降的步长。

*宽度*和*高度*是用于训练的图像尺寸。训练过程中的所有图像都将调整到这个大小。

*batchSize: i* s 将用于每次训练迭代的图像数量。

*批次大小*宽度*高度*将与训练的内存需求成比例。根据您的硬件，可能有必要使用较小的批处理大小来避免内存不足的问题。

请注意，由于我们只使用单一的图像大小进行训练，因此训练后的网络很可能仅限于使用这种大小的图像。

接下来，让我们创建我们的训练数据。我们想做一个简单的演示，所以我们将创建一个图像，用白色填充到一定高度。网络的目标是预测图像中被白色覆盖的部分。这可以很容易地用于从真实图像中预测更复杂的属性，正如在[其他教程中演示的那样。](https://medium.com/@sagieppel/predict-material-properties-from-an-image-using-a-neural-net-in-50-lines-of-code-with-pytorch-4d60965b58c0)

例如:

![](img/bdbe7268e02fd70513bee7c314ef7c5a.png)

47%填充白色的图像。

![](img/3283c099bb730ebc09f160f6d80afde5.png)

76%填充白色的图像。

在上面的图像中，我们希望网络预测 0.47，因为图像的 47%填充为白色。在底部的图片中，我们希望网络预测 0.76，因为 76%的图像填充为白色。

在实际设置中，您可能会从文件中加载数据。在这里，我们将动态创建它:

```
def ReadRandomImage(): 
   FillLevel=np.random.random() # Set random fill level
   Img=np.zeros([900,900,3],np.uint8) # Create black image 
   Img[0:int(FillLevel*900),:]=255 # Fill the image  

   transformImg=tf.Compose([tf.ToPILImage(),  
tf.Resize((height,width)),tf.ToTensor(),tf.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))]) # Set image transformation

 Img=transformImg(Img) # Transform to pytorch
 return Img,FillLevel
```

在第一部分，我们创建图像:

```
FillLevel=np.random.random() # Set random fill level
Img=np.zeros([900,900,3],np.uint8) # Create black image
Img[0:int(FillLevel*900),:]=255 # Fill the image
```

第一行选择 0-1 之间的一个随机数作为填充级别。

*Img=np.zeros([900，900，3])* 创建一个大小为 900X900 的矩阵，用零填充该矩阵作为图像，这相当于一个高度和宽度为 900 的黑色图像。该图像有 3 个对应于 RGB 的通道。

接下来，我们用白色填充图像的顶部直到填充线。

```
Img[0:int(FillLevel*900),:]=255
```

现在我们已经创建了图像，我们对其进行处理并将其转换为 Pytorch 格式:

```
transformImg=tf.Compose([tf.ToPILImage(),  
tf.Resize((height,width)),tf.ToTensor(),tf.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))]) # Set image transformation
```

这定义了一组将应用于图像的变换。这包括转换为 PIL 格式，这是转换的标准格式，以及调整大小和转换为 PyTorch 格式。对于图像，我们还通过减去平均值并除以像素强度的标准偏差来归一化图像中的像素强度。对于我们的简单图像，并不真正需要标准化和调整大小，但是这些转换对于真实图像是重要的。

接下来，我们将变换应用于图像:

```
Img=transformImg(Img)
```

为了训练，我们需要使用一批图像。这意味着几幅图像以 4D 矩阵的形式堆叠在一起。我们使用以下函数创建批处理:

```
def LoadBatch(): # Load batch of images
    images = torch.zeros([batchSize,3,height,width])
    FillLevel = torch.zeros([batchSize])
    for i in range(batchSize):
        images[i],FillLevel[i]=ReadRandomImage()
    return images,FillLevel
```

第一行创建了一个空的 4d 矩阵，它将存储图像的维度:[batchSize，channels，height，width]，其中 channels 是图像的层数；这是 RGB 图像的 3。下一行创建了一个数组，用来存储填充的级别。这将作为我们培训的目标(基本事实)。

下一部分使用我们之前定义的 ReadRandomImage()函数将一组图像和 FillLevels 加载到空矩阵中:

```
for i in range(batchSize):
  images[i],FillLevel[i]=ReadRandomImage()
```

现在我们可以加载数据了，是时候加载神经网络了:

```
device = torch.device('cuda') if torch.cuda.is_available() else torch.device('cpu')Net = torchvision.models.resnet18(pretrained=True) # Load netNet.fc = torch.nn.Linear(in_features=512, out_features=1, bias=True)Net = Net.to(device)optimizer = torch.optim.Adam(params=Net.parameters(),lr=Learning_Rate)
```

第一部分是识别计算机是否有 GPU 或 CPU。如果有 Cuda GPU，培训将在 GPU 上进行:

```
device = torch.device(‘cuda’) if torch.cuda.is_available() else torch.device(‘cpu’)
```

对于任何实际的数据集，使用 CPU 进行训练都是极其缓慢的。

接下来，我们加载网络进行图像分类:

```
Net = torchvision.models.resnet18(pretrained=True)
```

*torchvision.models.* 包含许多用于图像分类的有用模型*reset 18*是一个轻量级分类模型，适用于低资源训练或简单数据集。对于较难的问题，最好用 *resenet50* (注意数字是指网的层数)。

通过设置 *pretrained=True* ，我们在 Imagenet 数据集上加载预训练权重的网络。当学习一个新问题时，从预先训练的模型开始总是更好，因为它允许网络使用以前的经验并更快地收敛。

我们可以看到我们刚刚加载的网络的所有结构和所有层，通过编写:

```
*print(Net)*
```

> …
> 
> …
> 
> (avgpool):AdaptiveAvgPool2d(output _ size =(1，1))
> (fc):Linear(in _ features = 512，out_features=1000，bias=True)

这将按使用顺序打印层的网络。网络的最后一层是具有 512 层输入和 1000 层输出的线性变换。1000 代表输出类别的数量(该网络在图像网络上训练，图像网络将图像分类为 1000 个类别中的一个)。因为我们只想预测一个值，所以我们想用一个具有一个输出的新线性图层来替换它:

```
Net.fc = torch.nn.Linear(in_features=512, out_features=1, bias=True)
```

公平地说，这部分是可选的，因为具有 1000 个输出通道的网络可以简单地通过忽略剩余的 999 个通道来预测一个值。但这样更优雅。

接下来，我们将网络加载到我们的 GPU 或 CPU 设备中:

```
Net=Net.to(device)
```

最后，我们加载一个优化器:

```
optimizer=torch.optim.Adam(params=Net.parameters(),lr=Learning_Rate) # Create adam optimizer
```

优化器将在训练的反向传播步骤中控制梯度率。亚当优化器是最快的优化器之一。

最后，我们通过加载数据开始训练循环，使用网络进行预测:

```
AverageLoss=np.zeros([50]) # Save average loss for display
for itr in range(2001): # Training loop
    images,GTFillLevel=LoadBatch() # Load taining batch
    images=torch.autograd.Variable(images,requires_grad=False).
    to(device) 
    GTFillLevel = torch.autograd.Variable(GTFillLevel,
    requires_grad=False).to(device)

    PredLevel=Net(images) # make prediction
```

第一，我们想挽回我们训练时的平均损失；我们创建一个数组来存储最后 50 步的丢失。

```
AverageLoss=np.zeros([50])
```

这将允许我们跟踪网络学习的情况。

我们将训练 2000 步:

```
for itr in range(2000):
```

*LoadBatch* 是之前定义的，加载一批图像及其填充级别。

*torch . autogradated . variable*:将数据转换成网络可以使用的梯度变量。我们设置 *Requires_grad=False* ,因为我们不想将渐变只应用于网络的图层。 *to(device)* 将张量复制到与网络相同的设备(GPU/CPU)中。

最后，我们将图像输入网络，得到预测。

```
PredLevel=Net(images)
```

一旦我们做出预测，我们可以将其与真实(地面真实)填充水平进行比较，并计算损失。损失将仅仅是图像的预测和真实填充水平之间的绝对差(L1 ):

```
Loss=torch.abs(PredLevel-GTFillLevel).mean()
```

请注意，我们不是将损失应用于一个图像，而是应用于批中的几个图像，因此我们需要取平均值，将损失作为单个数字。

一旦我们计算出损失，我们就可以应用反向传播并改变净重。

```
Loss.backward() # Backpropogate loss
Optimizer.step() # Apply gradient descent change to wei
```

在训练过程中，我们想看看我们的平均损失是否减少，看看网络是否真正学到了什么。因此，我们将最后 50 个损失值存储在一个数组中，并显示每一步的平均值:

```
AverageLoss[itr%50]=Loss.data.cpu().numpy() # Save loss average
print(itr,") Loss=",Loss.data.cpu().numpy(),
'AverageLoss',AverageLoss.mean())
```

这涵盖了整个训练阶段，但是我们还需要保存训练好的模型。否则，一旦程序停止，它就会丢失。
存钱非常耗时，所以我们希望每 200 步只存一次:

```
if itr % 200 == 0: 
   print(“Saving Model” +str(itr) + “.torch”)
   torch.save(Net.state_dict(), str(itr) + “.torch”)
```

在运行这个脚本大约 200 步之后，网络应该会给出好的结果。

总共 40 行代码，不包括空格。

完整代码可在此处找到:

<https://github.com/sagieppel/Train-neural-net-to-predict-continuous-property-from-an-image-in-40-lines-of-code-with-PyTorch>  

在网络被训练和保存之后，它可以被加载用于进行预测:

<https://github.com/sagieppel/Train-neural-net-to-predict-continuous-property-from-an-image-in-40-lines-of-code-with-PyTorch/blob/main/Infer.py>  

该脚本加载您之前训练和保存的网络，并使用它进行预测。

这里的大部分代码与训练脚本相同，只有几处不同:

```
Net.load_state_dict(torch.load(modelPath)) # Load trained model
```

从*模型路径*中的文件加载我们之前训练并保存的网络

```
#Net.eval()
```

将网络从培训模式转换为评估模式。这主要意味着不会计算批量标准化统计数据。虽然使用它通常是一个好主意，但在我们的情况下，它实际上降低了准确性，所以我们将使用没有它的网络。

```
with torch.no_grad():
```

这意味着网络运行时不收集梯度。梯度仅与训练相关，并且收集它们是资源密集型的。

这就是它如果你想知道如何将它应用到真实的图像和真实的属性，查看下一个教程:

[https://medium . com/@ sagieppel/predict-material-properties-from-an-image-using-a-neural-net-in-50-line of-code-with-py torch-4d 60965 b58c 0](https://medium.com/@sagieppel/predict-material-properties-from-an-image-using-a-neural-net-in-50-lines-of-code-with-pytorch-4d60965b58c0)