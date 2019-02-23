# PyTorch

## 介绍

- 2017 年 1 月，Facebook 开源了 PyTorch，PyTorch 的前身是 Torch，Torch 是一个科学计算框架，支持机器学习算法，易用而且提供高效的算法实现，这得益于 LuaJIT 和一个底层的 C 实现。由于 Torch 由 Lua 语言编写，Torch 在神经网络方面具有优势，但由于 Lua 语言并不流行，开发者就把 Torch 移植到 Python 语言中，形成了 PyTorch

## 说明

### CUDA 语义

- torch.cuda 用于设置和运行 CUDA 操作，它会记录当前选定的 GPU，并且所有 CUDA 张量会被默认地分配到该设备上创建。可以使用 torch.cuda.device 上下文管理器更改所选设备
- 但是，一旦张量被分配，就可以直接对其进行操作，而不需考虑所选择的设备，操作的结果将始终与张量放在同一设备上
- 默认情况下不允许跨 GPU 操作，但 copy_() 和其他具有类似复制功能的方法（如 to() 和 cuda()）除外，除非启用对等内存访问，否则尝试对不同设备上的张量任何启动操作都将会引发错误
- 一个小例子：

```python
cuda = torch.device('cuda')     # 默认 CUDA 设备
cuda0 = torch.device('cuda:0')
cuda2 = torch.device('cuda:2')  # GPU 2 (these are 0-indexed)

x = torch.tensor([1., 2.], device=cuda0)
# x.device is device(type='cuda', index=0)
y = torch.tensor([1., 2.]).cuda()
# y.device is device(type='cuda', index=0)

with torch.cuda.device(1):
    # 在 GPU 1 上分配一个张量
    a = torch.tensor([1., 2.], device=cuda)

    # transfers a tensor from CPU to GPU 1
    b = torch.tensor([1., 2.]).cuda()
    # a.device and b.device are device(type='cuda', index=1)

    # You can also use ``Tensor.to`` to transfer a tensor:
    b2 = torch.tensor([1., 2.]).to(device=cuda)
    # b.device and b2.device are device(type='cuda', index=1)

    c = a + b
    # c.device is device(type='cuda', index=1)

    z = x + y
    # z.device is device(type='cuda', index=0)

    # even within a context, you can specify the device
    # (or give a GPU index to the .cuda call)
    d = torch.randn(2, device=cuda2)
    e = torch.randn(2).to(cuda2)
    f = torch.randn(2).cuda(cuda2)
    # d.device, e.device, and f.device are all device(type='cuda', index=2)
```

#### 异步执行

- 默认情况下，GPU 操作是异步的。当调用 GPU 的函数时，操作将排入特定设备的队列中，但不一定要在以后执行。这允许我们并行执行更多计算，包括在 CPU 或其他 GPU 上的操作
- 通常情况，异步计算的效果对于调用者是不可见的，因为
    1. 每个设备按照它们队列的顺序执行操作
    2. PyTorch 只有在 CPU 和 GPU 之间或两个 GPU 之间复制数据时自动执行必要的同步
- 因此，程序运行将如同每个操作同步执行一样

## Tensor（张量）

- Tensor 是 PyTorch 中重要的数据结构，可认为是一个高维数组。它可以是一个数（标量）、一维数组（向量）、二维数组（矩阵）以及更高维的数组。Tensor 和 Numpy 的 ndarrays 类似，但 Tensor 可以使用 GPU 进行加速。Tensor 的使用和 Numpy 及 Matlab 的接口十分相似
- Tensor 的数据类型定义方式
- torch.FloatTensor：生成浮点型的 Tensor，参数可以是一个列表或一个维度值
- torch.IntTensor：生成整形的 Tensor
- torch.rand：生成指定维度的浮点型随机 Tensor，生成的数据在 [0,1] 区间均匀分布
- torch.randn：生成指定维度的浮点型随机 Tensor，生成的数据是均值为 0，方差为 1 的正态分布
- torch.range(beg, end, step)：生成自定义范围的浮点型 Tensor，参数分别为起始值、结束值和步长
- torch.zeros：生成维度指定的全为 0 的浮点型 Tensor
- torch.ones：全 1 Tensor
- torch.arange(beg, end step)：从 beg 到 end 步长为 step

```python
import torch

a = torch.FloatTensor(2,3)
b = torch.FloatTensor([2,3,4,5])

print(a)
print(b)

# 输出
tensor([[0.0000e+00, 0.0000e+00, 9.2526e+27],
        [8.8842e-43, 0.0000e+00, 0.0000e+00]])
tensor([2., 3., 4., 5.])
```

### 常用 Tensor 操作

- 通过 torch.view 方法可以调整 tensor 的形状，必须保证调整前后元素总数一致
- view 不会修改自身的数据，返回的新 tensor 与源 tensor 共享内存
- resize 是另一种调整 tensor 的方法，它可以修改 tensor 的尺寸，如果新尺寸超过了原尺寸，会自动分配新的内存空间

### Tensor 的运算

- torch.abs：返回参数的绝对值
- torch.add(input, value, out=None)：返回将两参数的求和，第一个参数是 Tensor，第二个参数可以是 Tensor，也可以是一个标量
$$ out = input +  value $$
- torch.clamp(input, min, max, out=None)：对输入的 Tensor 进行裁剪将大于 max 的值设为 max，将小于 min 的值 设为 min
- torch.div(input, value, out=None)：求商
- torch.mul：求积
- torch.pow：求幂
- torch.mm：矩阵求积
- torch.mv：矩阵与向量求积

### Torch Tensor to a NumPy Array

```python
a = torch.ones(5)
print(a)              # tensor([1., 1., 1., 1., 1.])
b = a.numpy()
print(b)              # [1. 1. 1. 1. 1.]
```

### NumPy Array to Torch Tensor

```python
import numpy as np
a = np.ones(5)
b = torch.from_numpy(a)
np.add(a, 1, out=a)
print(a)
print(b)
# 输出
[2. 2. 2. 2. 2.]
tensor([2., 2., 2., 2., 2.], dtype=torch.float64)
```

### CUDA Tensors

- 可以将 Tensors 的运算转移到GPU上

```python
# let us run this cell only if CUDA is available
# We will use ``torch.device`` objects to move tensors in and out of GPU
if torch.cuda.is_available():
    device = torch.device("cuda")          # a CUDA device object
    y = torch.ones_like(x, device=device)  # directly create a tensor on GPU
    x = x.to(device)                       # or just use strings ``.to("cuda")``
    z = x + y
    print(z)
    print(z.to("cpu", torch.double))       # ``.to`` can also change dtype together!
```

## autograd

### Variable

- autograd 中的核心数据结构是 Variable，Variable 封装了 tensor，并记录对 tensor 的操作记录用来构建计算图
- autograd.Variable 主要包含 data、grad、grad_fn 三个属性
- data：保存 variable 所包含的 tensor
- grad：保存 data 对应的梯度，grad 是 variable，形状与 data 一致
- grad_fn：指向一个 Function，记录 tensor 的操作历史，即它是什么操作的输出，用来构建计算图

## torch

### torch.nn

- torch.nn 是专门为神经网络设计的模块接口，nn 构建于 Autograd 之上，可以用来定义和运行神经网络

#### Module

- torch.nn.Module：所有神经网络模块的基类，可以通过继承此类来创建自己的神经网络
- nn.Module 是 nn 中最重要的类，可以把它看作一个网络的封装，包含网络各层定义及 forward 方法
- 调用 forward(input) 方法可以返回前向传播的结果

```python
import torch.nn as nn
import torch.nn.functional as F                 # 激励函数库

class Net(nn.Module):                                   # 继承 Module
    def __init__(self, n_feature, n_hidden, n_output):  # 重写 __init__ 函数
        super(Net, self).__init__()                     # 继承原 __init__ 功能（函数）
        # 定义每层的形式
        self.hidden = nn.Linear(n_feature, n_hidden)    # 隐藏层线性输出
        self.predict = nn.Linear(n_hidden, n_output)    # 输出层线性输出

    def forward(self, x):                               # 重写 forward 函数
       x = F.relu(self.hidden(x))                       # 激励函数
       x = self.predict(x)                              # 输出值
       return x                                         

net = Net(n_feature=1, n_hidden=10, n_output=1)
print(net)      # net 的结构
"""
Net(
  (hidden): Linear(in_features=1, out_features=10, bias=True)
  (predict): Linear(in_features=10, out_features=1, bias=True)
)
"""
```

- 只要在 nn.Module 的子类中定义了 forward 函数，backward 函数就会被自动实现（利用 Autograd）
- torch.nn.Sequential 类是 torch.nn 中的一种顺序容器，通过在容器中嵌套各种实现神经网络中具有具体功能相关的类，来快速搭建神经网络模型。参数会按照定义好的序列顺序自动传递下去
- 下列代码效果与上等同

```python
net = nn.Sequential(
    nn.Linear(1, 10),
    nn.ReLU(),
    nn.Linear(10, 1)
)
print(net)
"""
Sequential(
  (0): Linear(in_features=1, out_features=10, bias=True)
  (1): ReLU()
  (2): Linear(in_features=10, out_features=1, bias=True)
)
"""
```

#### Convolution layers（卷积层）

- 卷积是一种局部操作，通过一定大小的卷积核作用于局部图像区域，从而得到图像的局部信息
- 一维卷积层，输入的尺度是(N, C_in,L)，输出尺度（ N,C_out,L_out）的计算方式：
$$ out(N_i, C_{out_j})=bias(C {out_j})+\sum^{C{in}-1}{k=0}weight(C{out_j},k)\bigotimes input(N_i,k) $$
- bigotimes: 表示相关系数计算 
- stride: 控制相关系数的计算步长 
- dilation: 用于控制内核点之间的距离，详细描述在这里 
- groups: 控制输入和输出之间的连接， group=1，输出是所有的输入的卷积；group=2，此时相当于有并排的两个卷积层，每个卷积层计算输入通道的一半，并且产生的输出是输出通道的一半，随后将这两个输出连接起来
> torch.nn.Conv1d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True)
  - in_channels(int)：输入信号的通道
  - out_channels(int)：卷积产生的通道
  - kerner_size(int or tuple)：卷积核的尺寸
  - stride(int or tuple, optional)：卷积步长
  - padding (int or tuple, optional)：是否对输入数据填充 0，指定填充 0 的层数。Padding 可以将输入数据的区域改造成是卷积核大小的整数倍，避免不满足卷积核大小的部分数据被忽略
  - dilation(int or tuple, optional)：卷积核元素之间的间距
  - groups(int, optional)：将输入数据分成组，in_channels 应该被组数整除
  - bias(bool, optional)：如果 bias=True，添加偏置

```python
m = nn.Conv1d(16, 33, 3, stride=2)
input = autograd.Variable(torch.randn(20, 16, 50))
output = m(input)
```

- shape:
  - 输入: (N,C_in,L_in) 
  - 输出: (N,C_out,L_out) 
  - 输入输出的计算方式： 
$$L_{out}=floor((L_{in}+2padding-dilation(kernerl_size-1)-1)/stride+1)$$
- 变量
  - weight(tensor) - 卷积的权重，大小是(out_channels, in_channels, kernel_size) 
  - bias(tensor) - 卷积的偏置系数，大小是（out_channel）
- 在由多个输入平面组成的输入信号上应用2D卷积
- 二维卷积层, 输入的尺度是(N, C_in,H,W)，输出尺度（N,C_out,H_out,W_out）的计算方式：
$$out(N_i, C_{out_j})=bias(C_{out_j})+\sum^{C_{in}-1}{k=0}weight(C{out_j},k)\bigotimes input(N_i,k)$$ 
> torch.nn.Conv2d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True)
  - in_channels(int) – 输入信号的通道
  - out_channels(int) – 卷积产生的通道
  - kerner_size(int or tuple) - 卷积核的尺寸
  - stride(int or tuple, optional) - 卷积步长
  - padding(int or tuple, optional) - 输入的每一条边补充0的层数
  - dilation(int or tuple, optional) – 卷积核元素之间的间距
  - groups(int, optional) – 从输入通道到输出通道的阻塞连接数
  - bias(bool, optional) - 如果bias=True，添加偏置
- shape:
  - input: (N,C_in,H_in,W_in) 
  - output: (N,C_out,H_out,W_out)
$$H_{out}=floor((H_{in}+2padding[0]-dilation[0](kernerl_size[0]-1)-1)/stride[0]+1)$$ 
$$W_{out}=floor((W_{in}+2padding[1]-dilation[1](kernerl_size[1]-1)-1)/stride[1]+1)$$ 
- 变量:
  - weight(tensor) - 卷积的权重，大小是(out_channels, in_channels,kernel_size)
  - bias(tensor) - 卷积的偏置系数，大小是（out_channel）

```python
# With square kernels and equal stride
m = nn.Conv2d(16, 33, 3, stride=2)
# non-square kernels and unequal stride and with padding
m = nn.Conv2d(16, 33, (3, 5), stride=(2, 1), padding=(4, 2))
# non-square kernels and unequal stride and with padding and dilation
m = nn.Conv2d(16, 33, (3, 5), stride=(2, 1), padding=(4, 2), dilation=(3, 1))
input = autograd.Variable(torch.randn(20, 16, 50, 100))
output = m(input)
```

#### Pooling layers（池化层）

##### MaxPool2d

> torch.nn.MaxPool2d(kernel_size, stride=None, padding=0, dilation=1, return_indices=False, ceil_mode=False)
- 对于输入信号的输入通道，提供2维最大池化（max pooling）操作
- 如果输入的大小是(N,C,H,W)，那么输出的大小是(N,C,H_out,W_out)和池化窗口大小(kH,kW)的关系是： 
$$out(N_i, C_j,k)=max^{kH-1}{m=0}max^{kW-1}{m=0}input(N_{i},C_j,stride[0]h+m,stride[1]w+n)$$ 
- 如果padding不是0，会在输入的每一边添加相应数目0 
- dilation用于控制内核点之间的距离
- 参数 kernel_size，stride, padding，dilation数据类型： 可以是一个int类型的数据，此时卷积height和width值相同; 也可以是一个tuple数组（包含来两个int类型的数据），第一个int数据表示height的数值，tuple的第二个int类型的数据表示width的数值
- 参数：
  - kernel_size(int or tuple)：max pooling 的窗口大小
  - stride(int or tuple, optional)：max pooling 的窗口移动的步长，默认值是kernel_size
  - padding(int or tuple, optional)：输入的每一条边填充 0 的层数
  - dilation(int or tuple, optional)：一个控制窗口中元素步幅的参数
  - return_indices：如果等于True，会返回输出最大值的序号，对于上采样操作会有帮助
  - ceil_mode：如果等于 True，计算输出信号大小的时候，会使用向上取整，代替默认的向下取整的操作
- shape: 
  - 输入: (N,C,H_{in},W_in) 
  - 输出: (N,C,H_out,W_out) 
$$H_{out}=floor((H_{in} + 2padding[0] - dilation[0](kernel_size[0] - 1) - 1)/stride[0] + 1$$ 
$$W_{out}=floor((W_{in} + 2padding[1] - dilation[1](kernel_size[1] - 1) - 1)/stride[1] + 1$$ 

```python
# pool of square window of size=3, stride=2
m = nn.MaxPool2d(3, stride=2)
# pool of non-square window
m = nn.MaxPool2d((3, 2), stride=(2, 1))
input = torch.randn(20, 16, 50, 32)
output = m(input)
```

#### Non-linear activations（非线性激活函数）

- torch.nn.PReLU(num_parameters=1, init=0.25)
$$ PReLU =  $$
- torch.nn.ReLU(inplace-False)：定义时不需要传参
$$ ReLU(x) = max(0,x) $$
- torch.nn.Sigmoid
$$ Sigmoid = 1/(1+exp(-x)) $$
- torch.nn.Tanh

#### Linear layers

- torch.nn.Linear 该类由于定义模型的线性层，完成不同层之间的线性转换
> torch.nn.Linear(in_features, out_features, bias=True)
  - in_features：输入特征数
  - out_features：输出特征数
  - bias：是否使用偏置（默认使用）

#### Dropout layers

- torch.nn.Dropuot 类用于防止卷积神经网络在训练的过程中发生过拟合，工作原理是
- 在模型训练的过程中，以一定的随机概率将卷积神经网络模型的部分参数归零，以达到减少相邻两层神经连接的目的
> torch.nn.Dropout(p=0.5, inplace=False)
  - p - 将元素置0的概率。默认值：0.5
  - in-place - 若设置为True，会在原地执行操作。默认值：False
- 在训练期间，对于每次前向调用，使用来自伯努利分布的样本以概率 p 随机地将输入张量的一些元素归零

> torch.nn.Dropout2d(p=0.5, inplace=False)
- 通常输入来自Conv2d模块

#### Loss functions（损失函数）

- torch.nn.L1Loss 类使用平均绝对误差函数计算损失值，定义类的对象时不用传参，使用实例时传入两个维度一样的参数
- torch.nn.MSELoss 类使用均方误差函数计算损失值，定义类的对象时不用传参，使用实例时传入两个维度一样的参数
- torch.nn.CrossEntropyLoss 类用于计算交叉熵，定义类的对象时不用传参，使用实例时需要输入两个满足交叉熵的计算条件的参数

### torch.nn.functional

### torch.optim（神经网络模型参数优化）

- torch.optim 是一个实现了各种优化算法的库，比如 SGD、AdaGrad、RMSProp、Adam 等
- 使用 torch.optim 需要构建一个 optimizer 对象。这个对象能够保持当前参数状态并基于计算得到的梯度进行参数更新
- 例如：

```python
optimizer = optim.SGD(model.parameters(), lr = 0.01, momentum=0.9)
optimizer = optim.Adam([var1, var2], lr = 0.0001)
```

#### 为每个参数单独设置选项

- Optimizer 也支持为每个参数单独设置选项
- 不要直接传入 Variable 的 iterable，而是传入 dict 的 iterable。每一个dict都分别定 义了一组参数，并且包含一个 param 键，这个键对应参数的列表。其他的键应该 optimizer 所接受的其他参数的关键字相匹配，并且会被用于对这组参数的优化
- 例如，当我们想指定每一层的学习率：

```python
optim.SGD([
  {'params': model.base.parameters()},
  {'params': model.classifier.parameters(), 'lr': 1e-3}
  ], lr=1e-2, momentum=0.9)
```

- model.base 的参数将会使用 1e-2 的学习率
- model.classifier 的参数将会使用 1e-3 的学习率
- 0.9 的 momentum 将会被用于所有的参数

#### 进行单次优化

- 所有的 optimizer 都实现了 step() 方法，这个方法会更新所有的参数。它能按两种方式来使用：
> optimizer.step()
- 这是大多数 optimizer 所支持的简化版本。一旦梯度被如 backward() 之类的函数计算好后，就可以调用这个函数

```python
for input, target in dataset:
    optimizer.zero_grad()
    output = model(input)
    loss = loss_fn(output, target)
    loss.backward()
    optimizer.step()
```

### torch.autograd（自动梯度）

## torchvision

- torchvision 包的主要功能是数据处理、导入和预览等，在计算机视觉中有重要作用

### torchvision.datasets（数据集）

- torchvision.datasets 用于对数据集的训练集和测试集的下载
- 所有数据集都是 torch.utils.data.Dataset 的子类，它们实现了 __getitem__ 和 __len__ API，可以通过 torch.utils.data.DataLoader 使用多线程（python的多进程），它可以使用torch.multiprocessing workers 并行加载多个样本

```python
imagenet_data = torchvision.datasets.ImageFolder('path/to/imagenet_root/')
data_loader = torch.utils.data.DataLoader(imagenet_data, batch_size=4, shuffle=True, num_workers=args.nThreads)
```

- 目前包含了以下数据集：
  - MNIST：手写数字数据集
  - Fashion-MNIST
  - EMNIST
  - COCO：用于图像标注和目标检测（Captions and Detection）
  - LSUN
  - ImageFolder
  - DatasetFolder
  - Imagenet-12
  - CIFAR
  - STL10
  - SVHN
  - PhotoTour

- 所有的数据集都有相似的 API，它们都有两个共同的参数：transform 和 target_transform 分别转换输入和目标

#### MNIST

- [MNIST](http://yann.lecun.com/exdb/mnist/) Dataset
> torchvision.datasets.MNIST(root, train=True, transform=None, target_transform=None, download=False)
  - root(string)：数据集，存在于根目录 processed/training.pt 和 processed/test.pt 中
  - train：True = 训练集，False = 测试集
  - download：如果为 True 将从 Internet 下载数据集并将其放在根目录中，如果已下载数据集，则不会再次下载
  - transform：接收 PIL 映像并返回转换版本的函数/变换
  - target_transform：接收目标并对其进行转换的函数/转换

#### Fashion-MNIST

- [Fashion-MNIST](https://github.com/zalandoresearch/fashion-mnist)Dataset
> torchvision.datasets.FashionMNIST(root, train=True, transform=None, target_transform=None, download=False)

#### EMNIST

- [EMNIST](https://www.nist.gov/itl/iad/image-group/emnist-dataset) Dataset
> torchvision.datasets.EMNIST(root, split, **kwargs)
  - split(string)：数据集有6个不同的分割（byclass，bymerge，balanced，letters，digits和mnist），此参数指定要使用的分割方式

#### COCO

- 需要安装[COCO API](https://github.com/cocodataset/cocoapi/tree/master/PythonAPI)
- Captions（图像标注）
- [MS Coco Captions](http://cocodataset.org/) Dataset
> torchvision.datasets.CocoCaptions(root, annFile, transform=None, target_transform=None)
  - root：图像路径
  - annFile：json 注释文件的路径

- Detection（检测）
> torchvision.datasets.CocoDetection(root, annFile, transform=None, target_transform=None)

#### LSUN

- [LSUN](http://lsun.cs.princeton.edu) dataset
> torchvision.datasets.LSUN(root, classes='train', transform=None, target_transform=None)
  - classes(string or list)：{'train'(所有类别, 训练集)，'val'(所有类别, 验证集)，'test'(所有类别, 测试集)}之一或要加载的类别列表，例如 ['bedroom_train'，'church_train']

#### ImageFolder

- 一个通用的数据加载器，数据集中的数据以以下方式组织
```
root/dog/xxx.png
root/dog/xxy.png
root/dog/xxz.png

root/cat/123.png
root/cat/nsdf3.png
root/cat/asd932_.png
```
> torchvision.datasets.ImageFolder(root, transform=None, target_transform=None, loader=<function default_loader>)
  - loader：一个在给定路径的情况下加载图像的函数
- __getitem__(index) 返回：(sample，target) 其中target是目标类的class_index

#### DatasetFolder

- 通用数据加载器，其中样本以这种方式排列：
```
root/class_x/xxx.ext
root/class_x/xxy.ext
root/class_x/xxz.ext

root/class_y/123.ext
root/class_y/nsdf3.ext
root/class_y/asd932_.ext
```
> torchvision.datasets.DatasetFolder(root, loader, extensions, transform=None, target_transform=None)
  - loader(callable)：加载样本函数
  - extensions(list[string])：允许的扩展名列表

#### CIFAR

- [CIFAR10](https://www.cs.toronto.edu/~kriz/cifar.html) Dataset
> torchvision.datasets.CIFAR10(root, train=True, transform=None, target_transform=None, download=False)

- 这是CIFAR10数据集的子类
> torchvision.datasets.CIFAR100(root, train=True, transform=None, target_transform=None, download=False)

#### STL10

- [STL10](https://cs.stanford.edu/~acoates/stl10/) Dataset
> torchvision.datasets.STL10(root, split='train', transform=None, target_transform=None, download=False)
  - split(string)：选择数据集：{'train' = 训练集, 'test' = 测试集, 'unlabeled' = 无标签数据集, 'train+unlabeled' = 训练 + 无标签数据集 (没有标签的标记为-1)}

> torchvision.datasets.SVHN(root, split='train', transform=None, target_transform=None, download=False)

> torchvision.datasets.PhotoTour(root, name, train=True, transform=None, download=False)

### torch.utils.data（数据装载）

> torch.utils.data.DataLoader(dataset, batch_size=1, shuffle=False, sampler=None, batch_sampler=None, num_workers=0, collate_fn=<function default_collate>, pin_memory=False, drop_last=False, timeout=0, worker_init_fn=None)
  - dataset：载入的数据集名称
  - batch_size：每批打包加载的样本数（默认为1）
  - shuffle：如果为 True 在每个 eqoch 重新排列数据
  - sampler：从数据集中提取样本
  - batch_sampler：一次返回一批索引
  - num_workers：用于数据加载的子进程数（默认为0，表示数据将在主进程中加载）
  - collate_fn：合并样本列表以形成小批量
  - pin_memory：如果为 True，数据加载器在返回前将 Tensor（张量）复制到 CUDA 固定内存中
  - drop_last：如果数据集大小不能被 batch_size 整除，设置为 True 可删除最后一个不完整的批处理；设置为 False 则最后一个 batch 将更小

### torchvision.models

- torchvision.models 模块的子模块中包含以下预训练的模型结构：
  - [AlexNet](https://arxiv.org/abs/1404.5997)
  - [VGG](https://arxiv.org/abs/1409.1556)
  - [ResNet](https://arxiv.org/abs/1512.03385)
    - ResNet 引入了残差网络结构（Residual Network），通过残差网络，可以把网络层弄得很深
  - [SqueezeNet](https://arxiv.org/abs/1602.07360)
  - [DenseNet](https://arxiv.org/abs/1608.06993)
  - [Inception v3](https://arxiv.org/abs/1512.00567)

- 可以通过调用构造函数来构造具有随机权重的模型：

```python
import torchvision.models as models
resnet18 = models.resnet18()
alexnet = models.alexnet()
vgg16 = models.vgg16()
squeezenet = models.squeezenet1_0()
densenet = models.densenet161()
inception = models.inception_v3()
```

- PyTorch 提供了大量的预训练的模型，利用 PyTorch 的 torch.utils.model_zoo 来加载预训练模型。也可以通过传递 pretrained=True 来构造

```python
import torchvision.models as models
resnet18 = models.resnet18(pretrained=True)
alexnet = models.alexnet(pretrained=True)
squeezenet = models.squeezenet1_0(pretrained=True)
vgg16 = models.vgg16(pretrained=True)
densenet = models.densenet161(pretrained=True)
inception = models.inception_v3(pretrained=True)
```

- 对预训练的模型进行实例化将会下载它的权重到缓存目录，可以使用 TORCH_MODEL_ZOO 环境变量设置此目录。详情参阅：[torch.utils.model_zoo.load_url()](https://pytorch.org/docs/stable/model_zoo.html#torch.utils.model_zoo.load_url)
- 某些模型使用具有不同训练和行为评估的模块，例如批量标准化。要在这些模式之间切换，需要使用 model.train() 或 model.eval()
- 所有预训练模型都需要输入的图像使用相同的归一化方式，即小批量的 3 通道 RGB 图像格式（3×H×W），其中 H 和 W 至少为 224。图像必须加载到 [0,1] 的范围内，然后使用 mean = [0.485, 0.456, 0.406] 和 std = [0.229, 0.224, 0.225] 归一化
> normalize = transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])

### torchvision.transforms

- torchvision.transforms 中提供了丰富的类对载入的数据进行变换
- 数据增强：通过对有限的图片数据进行各种变换如放大缩小、水平垂直翻转等来生成新的训练集
- 将多个 transform 组合起来使用：
> torchvision.transforms.Compose(transforms)
  - transforms：要进行的变换的列表

```python
transforms.Compose([
    transforms.CenterCrop(10),
    transforms.ToTensor(),
  ])
```

#### 对 PIL 图像进行变换

##### 常用的数据变换操作

- 将输入 PIL 图像的大小调整为给定大小
> torchvision.transforms.Resize(size, interpolation=2)
- 对载入的图片进行缩放，用法和 Resize 类似
> torchvision.transforms.Scale(*args, **kwargs)
- 对载入的图像以图像的中心为参考点进行剪裁
> torchvision.transforms.CenterCrop(size)
  - size：可以是 tuple(target_height, target_width)，也可以是一个 Integer，在这种情况下，切出来的图片的形状是正方形
- 对载入的图像在随机位置裁剪：切割中心点的位置随机选取。size 可以是 tuple 也可以是 Integer
> torchvision.transforms.RandomCrop(size, padding=None, pad_if_needed=False, fill=0, padding_mode='constant')
- 以给定的概率随机垂直翻转 PIL 图像
> torchvision.transforms.RandomVerticalFlip(p=0.5)
- 以给定的概率随机水平翻转 PIL 图像
> torchvision.transforms.RandomHorizontalFlip(p=0.5)
- 把范围是[0,255]的 PIL.Image 或者 shape 为(H,W,C)的 numpy.ndarray，转换成形状为[C,H,W]，范围是[0,1.0]的torch.FloadTensor，让 PyTorch 能够对其进行计算、处理
> torchvision.transforms.ToTensor
- 将 shape 为(C,H,W)的 Tensor 或 shape 为(H,W,C)的 numpy.ndarray 转换成 PIL.Image，值不变。方便图片内容显示
> torchvision.transforms.ToPILImage(mode=None)

##### 其他操作

- 随机更改图像的亮度，对比度和饱和度
> torchvision.transforms.ColorJitter(brightness=0, contrast=0, saturation=0, hue=0)
- 将图像转换为灰度
> torchvision.transforms.Grayscale(num_output_channels=1)
- 用二维的变换矩阵处理一个张量（Tensor）图像
> torchvision.transforms.LinearTransformation(transformation_matrix)
- 将给定的PIL.Image的所有边用给定的pad value填充
> torchvision.transforms.Pad(padding, fill=0, padding_mode='constant')
  - padding：要填充多少像素
  - fill：用什么值填充
- 图像保持中心不变的随机仿射变换
> torchvision.transforms.RandomAffine(degrees, translate=None, scale=None, shear=None, resample=False, fillcolor=0)
- 随机应用给定概率的变换列表
> torchvision.transforms.RandomApply(transforms, p=0.5)
- 随机应用列表中的一个转换
> torchvision.transforms.RandomChoice(transforms)
- 在随机位置裁剪给定的PIL图像：切割中心点的位置随机选取。size 可以是 tuple 也可以是 Integer
> torchvision.transforms.RandomCrop(size, padding=None, pad_if_needed=False, fill=0, padding_mode='constant')
- 将图像随机转换为灰度，概率为p（默认值为0.1）
> torchvision.transforms.RandomGrayscale(p=0.1)
- 以随机顺序应用转换列表
> torchvision.transforms.RandomOrder(transforms)
- 将给定的PIL图像裁剪为随机大小和宽高比：先将给定的PIL.Image随机切，然后再resize成给定的size大小
> torchvision.transforms.RandomResizedCrop(size, scale=(0.08, 1.0), ratio=(0.75, 1.33333), interpolation=2)
  - 这通常用于训练 Inception 网络