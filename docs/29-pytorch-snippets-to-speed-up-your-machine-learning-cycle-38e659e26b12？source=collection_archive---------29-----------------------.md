# 29 个 Pytorch 片段加速您的机器学习周期

> 原文：<https://towardsdatascience.com/29-pytorch-snippets-to-speed-up-your-machine-learning-cycle-38e659e26b12?source=collection_archive---------29----------------------->

![](img/5fd01fd8a827ab51bc2b91ed9c813f9e.png)

阿里·沙阿·拉哈尼在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 从创建张量到编写神经网络

# 为什么片段很重要

我非常支持代码片段来创建比传统机器学习管道更快的迭代周期。Pytorch 已经成为我的机器学习工具堆栈的重要组成部分有一段时间了，所以我决定将这篇文章写给 ***分享一些我最喜欢的代码片段，以帮助你建立 Pytorch 模型。***

## 1.打印 Pytorch 版本

```
import torch
print(torch.version.__version__)# Output1.7.1
```

## 2.从 n 数组的嵌套列表中创建张量

```
example_list = [1,2,3]
x = torch.tensor(example_list)
print(x)# Outputtensor([1, 2, 3])
```

## 3.克隆张量

```
y = x.clone()
print(y)# Outputtensor([1, 2, 3])
```

## 4.获取大小和形状

```
x = torch.randn((10,10))
print(x.size())
# or
print(x.shape)# Outputtorch.Size([10, 10])
torch.Size([10, 10])
```

## 5.沿着维度连接张量

```
tensor_seq = [torch.randn(1,2),torch.randn(1,2)]
x = torch.cat(tensor_seq, dim=0)
print(x)# Outputtensor([[-0.4298,  1.3190],
        [ 0.3904, -1.4962]])
```

## 6.将张量整形为维度:(1，任意)

```
x = torch.randn(10,2)
y = x.view(1,-1)
print(y.shape)# Outputtorch.Size([1, 20])
```

## 7.交换张量的特定维度

```
x = torch.randn(1,2)
print(x)
y = x.transpose(0,1)
print(y)# Outputtensor([[-0.3085,  0.9356]])

tensor([[-0.3085],
        [ 0.9356]])
```

## 8.向张量添加轴

```
x = torch.randn(2,2)
print(x.shape)
y = x.unsqueeze(dim=0)                      
print(y.shape)# Outputtorch.Size([2, 2])
torch.Size([1, 2, 2])
```

## 9.删除尺寸为 1 的所有尺寸

```
x = torch.randn(10,10,1)
print(x.shape)
y = x.squeeze()
print(y.shape)# Outputtorch.Size([10, 10, 1])torch.Size([10, 10])
```

## 10.矩阵乘法

```
A = torch.ones(2,2)
B = torch.randn(2,2)
print(A)
print(B)
print(A.mm(B))# Outputtensor([[1., 1.],
        [1., 1.]])
tensor([[ 0.5804,  1.2500],
        [-0.8334,  1.1711]])

tensor([[-0.2530,  2.4212],
        [-0.2530,  2.4212]])
```

## 11.矩阵向量乘法

```
A = torch.tensor([[1,2],[3,4]])
x = torch.tensor([1,2])
print(A.mv(x))# Outputtensor([ 7, 10])
```

## 12.矩阵转置

```
x = torch.randn(1,2)
print(x)
x = x.t()
print(x)tensor([[0.1167, 0.4135]])

tensor([[0.1167],
        [0.4135]])
```

## 13.检查 cuda 可用性

```
print(torch.cuda.is_available())# OutputTrue
```

## 14.将张量数据从 CPU 移动到 GPU 并返回新对象

```
x = torch.randn(2,2)
print(x)
x = x.cuda()
print(x)# Outputtensor([[-1.0331, -3.2458],
        [ 0.0226,  1.3091]])

tensor([[-1.0331, -3.2458],
        [ 0.0226,  1.3091]], device='cuda:0')
```

## 15.将张量数据从 GPU 移动到 CPU

```
x = torch.randn(2,2).cuda()
print(x)
x = x.cpu()
print(x)# Outputtensor([[ 0.4664, -1.7070],
        [ 1.7160,  0.0263]], device='cuda:0')

tensor([[ 0.4664, -1.7070],
        [ 1.7160,  0.0263]])
```

## 16.与设备无关的代码和模块化

```
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
print(device)# Outputdevice(type='cuda', index=0)
```

## 17.将张量复制到设备(gpu、cpu)

```
x = x.to(device)
print(x)# Outputtensor([[ 0.4664, -1.7070],
        [ 1.7160,  0.0263]], device='cuda:0')
```

## 18.检查它是否是 Pytorch 张量

```
print(torch.is_tensor(x))# OutputTrue
```

## 19.检查它是否是 Pytorch 存储对象

```
print(torch.is_storage(x))# OutputFalse
```

## 20.获取输入张量中的元素总数

```
x = torch.randn(2,2) # 4 elements
torch.numel(x)# Output4
```

## 21.获取给定大小的单位矩阵

```
size = 5
print(torch.eye(size))# Outputtensor([[1., 0., 0., 0., 0.],
        [0., 1., 0., 0., 0.],
        [0., 0., 1., 0., 0.],
        [0., 0., 0., 1., 0.],
        [0., 0., 0., 0., 1.]])
```

## 22.从 numpy 数组到 torch 张量的转换

```
x = np.random.rand(2,2)
print(torch.from_numpy(x))# Outputtensor([[0.7407, 0.8823],
        [0.0352, 0.5823]], dtype=torch.float64)
```

## 23.基本的线性间距(比如 numpy 的 np.linspace)

```
print(np.linspace(1,5,10))
print(torch.linspace(1, 5, steps=10))# Output[1\.         1.44444444 1.88888889 2.33333333 2.77777778 3.22222222
 3.66666667 4.11111111 4.55555556 5\.        ] # numpy

tensor([1.0000, 1.4444, 1.8889, 2.3333, 2.7778, 3.2222, 3.6667, 4.1111, 4.5556,
        5.0000]) # pytorch
```

## 24.对数间距

```
torch.logspace(start=-10, end=10, steps=15) #logarithmic spacing# Outputtensor([1.0000e-10, 2.6827e-09, 7.1969e-08, 1.9307e-06, 5.1795e-05, 1.3895e-03,
        3.7276e-02, 1.0000e+00, 2.6827e+01, 7.1969e+02, 1.9307e+04, 5.1795e+05,
        1.3895e+07, 3.7276e+08, 1.0000e+10])
```

## 25.将 Pytorch 张量分割成小块

```
x = torch.linspace(1,10,10)
print(torch.chunk(x,chunks=5))# Output(tensor([1., 2.]),
 tensor([3., 4.]),
 tensor([5., 6.]),
 tensor([7., 8.]),
 tensor([ 9., 10.]))
```

## 26.基本神经网络

```
import torch
import torch.nn as nn
import torch

class NeuralNet(nn.Module):
    def __init__(self):
        super(NeuralNet, self).__init__()
        self.fc1 = nn.Linear(1,1)
        self.relu = nn.ReLU()

    def forward(self,x):
        x = self.fc1(x)
        x = self.relu(x)

        return x
net = NeuralNet()
net# OutputNeuralNet(
  (fc1): Linear(in_features=1, out_features=1, bias=True)
  (relu): ReLU()
)
```

## 27.创建张量来保存用于训练神经网络的输入和输出

```
x = torch.linspace(-10, 10, 2000).view(-1,1)
y = torch.square(x)
```

## 28.加载神经网络并设置损失函数和优化器

```
model = NeuralNet()
criterion = torch.nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=1e-6)
```

## 29.训练循环，每 10 个时期打印一次输出

```
epochs = 50
for t in range(epochs):
    # Forward pass: computing prediction
    y_pred = model(x)

    # Compute the loss and printing ever 10 iterations
    loss = criterion(y_pred, y)
    if t % 10 == 9:
        print(t, loss.item())

    # Zero gradients, perform a backward pass, and update the weights.
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()# Output9 1987.289306640625
19 1986.6630859375
29 1986.0374755859375
39 1985.412353515625
49 1984.78759765625
```

# 结论

我认为这样的代码片段很容易获得，这很有帮助。这使得整个迭代周期更快，也更令人愉快，因为我们不必不断地寻找如何在我们选择的框架中执行我们想要的每个任务。

我想在这里提到两个我认为对 Pytorch 中的基本代码非常有用的资源:

 [## PyTorch 备忘单- PyTorch 教程 1.8.1+cu102 文档

### 编辑描述

pytorch.org](https://pytorch.org/tutorials/beginner/ptcheat.html) [](https://github.com/Apress/pytorch-recipes) [## apress/py torch-食谱

### 该知识库附有 Pradeepta Mishra 的 PyTorch 食谱(Apress，2019)。使用下载压缩文件…

github.com](https://github.com/Apress/pytorch-recipes) 

因此，请确保您的 Pytorch 代码片段随时可用，并享受编写代码的乐趣！

如果你喜欢这篇文章，请在 [Twitter](https://twitter.com/LucasEnkrateia) 、 [LinkedIn](https://www.linkedin.com/in/lucas-soares-969044167/) 上联系我，并在 [Medium](https://lucas-soares.medium.com) 上关注我。谢谢，下次再见！:)

# 参考

*   [Pytorch 备忘单](https://pytorch.org/tutorials/beginner/ptcheat.html)
*   [Pytorch 食谱](https://github.com/Apress/pytorch-recipes)