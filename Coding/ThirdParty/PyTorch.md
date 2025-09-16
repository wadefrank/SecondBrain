# 使用
## pytorch自定义层

在pytorch中，给我们提供了大量的预定义层，直接就能拿来用，但是对于科研小能手来说，肯定有各种关于神经网络的奇思妙想，这时候可能预定义层根本没法满足自己的需求，所以，需要我们能简单方便的实现自己的自定义层，并且能够与预定义层无缝结合在一个模型里。那应该怎么做呢？

总所周知，神经网络的任何操作都可以封装成pytorch的层，这样方便调用，所以比如relu等激活函数也是pytorch中的层，但是这种层并没有需要训练的参数。

所以自定义层可以分为两种，一种是带参数的，一种是不带参数的。

### 1.不带参数的

不带参数的非常简单，因为它没有自己的参数，所以它完成的工作肯定是对输入张量X进行各种乱七八糟的操作，最后返回一个张量。那么我们的实现方式如下：

```python
import torch
from torch import nn

class CenteredLayer(nn.Module):
    def __init__(self, **kwargs):
        super(CenteredLayer, self).__init__(**kwargs)
    def forward(self, x):
        return x - x.mean()

# 使用的时候跟自定义层的使用方法一样，例如：
layer = CenteredLayer()
layer(torch.tensor([1, 2, 3, 4, 5], dtype=torch.float))
# 或者作为更复杂模型的一部分：
net = nn.Sequential(nn.Linear(8, 128), CenteredLayer())
```

可以看到，只需要在forward函数中进行相应的变换就行了，这些变换都是pytorch中张量计算的基本操作，就不再赘述了。

# Python API(1.10.0)

[https://pytorch.org/docs/1.10/](https://pytorch.org/docs/1.10/)

## 函数

### torch.cat()

连接张量序列，所有张量必须具有相同的形状（连接维度除外）或为空。

```python
torch.cat(
    tensors,
    dim=0,
    *,
    out=None
) -> Tensor
```

### torch.cuda

#### torch.cuda.Event()

`torch.cuda.Event` 是 PyTorch 中用于在 GPU 上进行事件同步和性能测量的工具。它可以记录 CUDA 流中的特定事件，并用于测量在这些事件之间的时间间隔。

```python
import torch

# enable_timing：如果为 True，则启用事件的计时功能。默认是 True。
# blocking：如果为 True，则事件的记录是阻塞的。默认是 False。
# interprocess：如果为 True，则事件可以在多个进程间共享。默认是 False。

# 创建事件
start_event = torch.cuda.Event(enable_timing=True)
end_event = torch.cuda.Event(enable_timing=True)

# 在 CUDA 操作之前记录开始事件
start_event.record()

# TODO：需要记录时间的CUDA操作
a = torch.randn(10000, 10000, device='cuda')
b = torch.randn(10000, 10000, device='cuda')
c = a + b

# 在 CUDA 操作之后记录结束事件
end_event.record()

# 同步事件
end_event.synchronize()

# 计算并打印事件间的时间
elapsed_time = start_event.elapsed_time(end_event)
print(f"Elapsed time: {elapsed_time} ms")
```

### torch.flatten()

将输入张量reshape为一维张量。 如果传递了 start_dim 或 end_dim，则仅展平以 start_dim 开头并以 end_dim 结尾的维度。 输入中元素的顺序不变。

用法：

```python
torch.flatten(
    input,        # (Tensor) 输入的张量
    start_dim=0,  # (int) 第一个需要flatten的维度
    end_dim=-1    # (int) 最后一个需要flatten的维度
) -> Tensor
```

示例：

```python
>>> t = torch.tensor([[[1, 2],[3, 4]],[[5, 6],[7, 8]]])
>>> torch.flatten(t)
tensor([1, 2, 3, 4, 5, 6, 7, 8])
>>> torch.flatten(t, start_dim=1)
tensor([[1, 2, 3, 4],
        [5, 6, 7, 8]])
```

### torch.item()

用于将一个只有一个元素的张量转换为 Python 标准数据类型（如 `int`、`float` 等）。这个方法允许你从张量中提取单个标量值。

### torch.linspace()

创建大小为steps的一维张量，开始值为start，结束值为end，其值均匀分布。

用法：

```python
torch.linspace(
    start,  # 开始值 
    end,    # 结束值
    steps,  # 所构建的张量大小
    *,
    out=None,
    dtype=None,
    layout=torch.strided,
    device=None,
    requires_grad=False
) -> Tensor
```

示例代码：

```python

>>> torch.linspace(3, 10, steps=5)
tensor([  3.0000,   4.7500,   6.5000,   8.2500,  10.0000])
>>> torch.linspace(-10, 10, steps=5)
tensor([-10.,  -5.,   0.,   5.,  10.])
>>> torch.linspace(start=-10, end=10, steps=5)
tensor([-10.,  -5.,   0.,   5.,  10.])
>>> torch.linspace(start=-10, end=10, steps=1)
tensor([-10.])
```

### torch.meshgrid()

创建坐标网格，输入是一维张量。

用法：

```python
torch.meshgrid(
    *tensors,                  
    indexing=None
)
```

示例：

```python
>>> x = torch.tensor([1, 2, 3])
>>> y = torch.tensor([4, 5, 6])

Observe the element-wise pairings across the grid, (1, 4),
(1, 5), ..., (3, 6). This is the same thing as the
cartesian product.
>>> grid_x, grid_y = torch.meshgrid(x, y, indexing='ij')
>>> grid_x
tensor([[1, 1, 1],
        [2, 2, 2],
        [3, 3, 3]])
>>> grid_y
tensor([[4, 5, 6],
        [4, 5, 6],
        [4, 5, 6]])
```

### torch.permute()

按照给定顺序重新排列张量的各个维度。

用法：

```python
torch.permute(
    input,  # (Tensor) 输入的张量
    dims    # (tuple of python:ints) 期望的维度的顺序
) -> Tensor
```

示例：

```python
>>> x = torch.randn(2, 3, 5)
>>> x.size()
torch.Size([2, 3, 5])

>>> torch.permute(x, (2, 0, 1)).size()
torch.Size([5, 2, 3])
```

### torch.reshape()

返回一个新张量，其数据与自身张量相同，但形状(shape)不同。

单个维度可能为 -1，在这种情况下，它是根据剩余维度和输入中的元素数量推断出来的。

用法：

```python
torch.reshape(
    input,  # (Tensor) 输入的张量
    shape   # (tuple of python:ints) 新形状
) -> Tensor
```

示例：

```python
>>> a = torch.arange(4.)
>>> torch.reshape(a, (2, 2))
tensor([[ 0.,  1.],
        [ 2.,  3.]])
>>> b = torch.tensor([[0, 1], [2, 3]])
>>> torch.reshape(b, (-1,))
tensor([ 0,  1,  2,  3])
```

### torch.stack()

沿新维度连接（Concatenates）一个序列的张量

用法：

```python
torch.stack(
    tensors,  # (sequence of Tensors) 要连接的张量序列
    dim=0,    # (int) 要插入的尺寸，必须在0和连接张量的维数之间
    *,
    out=None
) -> Tensor
```

### torch.unsqueeze()

返回一个新的张量，其尺寸为 1 插入到指定位置。

用法：

```python
torch.unsqueeze(
    input,  # (Tensor) 输入的张量
    dim     # 插入位置索引
) -> Tensor
```

示例：

```python
>>> x = torch.tensor([1, 2, 3, 4])
>>> torch.unsqueeze(x, 0)
tensor([[ 1,  2,  3,  4]])
>>> torch.unsqueeze(x, 1)
tensor([[ 1],
        [ 2],
        [ 3],
        [ 4]])
```

### torch.Tensor

#### torch.Tensor.expand()

返回自张量的新视图，其中单例维度扩展到更大的尺寸。

用法：

```python
Tensor.expand(
    *sizes  # (torch.Size or int...)，预期扩展的张量形状
) -> Tensor
```

示例代码：

```python
>>> x = torch.tensor([[1], [2], [3]])
>>> x.size()
torch.Size([3, 1])
>>> x.expand(3, 4)
tensor([[ 1,  1,  1,  1],
        [ 2,  2,  2,  2],
        [ 3,  3,  3,  3]])

# -1 means not changing the size of that dimension
>>> x.expand(-1, 4)   
tensor([[ 1,  1,  1,  1],
        [ 2,  2,  2,  2],
        [ 3,  3,  3,  3]])
```

#### torch.Tensor.flatten()

详见torch.flatten()

用法：

```python
Tensor.flatten(
    start_dim=0,  # 
    end_dim=- 1   # 
) -> Tensor
```

#### torch.Tensor.permute()

详见torch.permute()

用法：

```python
Tensor.permute(
    *dims  # 期望的维度的顺序
) -> Tensor
```

#### torch.Tensor.repeat()

沿指定维度重复张量。

用法：

```python
Tensor.repeat(
    *sizes  # (torch.Size or int...) 沿每个维度重复该张量的次数
) -> Tensor
```

示例：

```python
>>> x = torch.tensor([1, 2, 3])
>>> x.repeat(4, 2)
tensor([[ 1,  2,  3,  1,  2,  3],
        [ 1,  2,  3,  1,  2,  3],
        [ 1,  2,  3,  1,  2,  3],
        [ 1,  2,  3,  1,  2,  3]])
>>> x.repeat(4, 2, 1).size()
torch.Size([4, 2, 3])
```

#### torch.Tensor.reshape()

返回一个新张量，其数据与自身张量相同，但形状(shape)不同。

用法：

```python
Tensor.reshape(
    *shape  # (torch.Size or int...)，预期的张量形状
) -> Tensor
```

#### torch.Tensor.view()

返回一个新张量，其数据与自身张量相同，但形状(shape)不同。

用法：

```python
Tensor.view(
    *shape  # (torch.Size or int...)，预期的张量形状
) -> Tensor
```

示例代码：

```python
>>> x = torch.randn(4, 4)
>>> x.size()
torch.Size([4, 4])
>>> y = x.view(16)
>>> y.size()
torch.Size([16])

# the size -1 is inferred from other dimensions
>>> z = x.view(-1, 8)  
>>> z.size()
torch.Size([2, 8])
```

## 类

### DataLoader

`DataLoader`是PyTorch中用于加载数据的工具。它可以帮助我们批量加载数据、打乱数据顺序、并行读取数据等。

通过调用 `Dataset` 的 `__getitem__` 方法来获取数据样本。

```bash
import torch
from torch.utils.data import DataLoader, Dataset

class MyDataset(Dataset):
    def __init__(self, data):
        self.data = data

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        return self.data[idx]

# 创建一个数据集
data = [i for i in range(100)]
dataset = MyDataset(data)

# 创建DataLoader
dataloader = DataLoader(dataset, batch_size=10, shuffle=True, num_workers=2)

# 迭代数据
for batch in dataloader:
    print(batch)
```

#### 参数说明

- `dataset`：数据集，必须是一个继承自`torch.utils.data.Dataset`的对象。
- `batch_size`：每个batch的数据量。
- `shuffle`：是否在每个epoch开始时打乱数据顺序。
- `sampler`：定义从数据集中提取样本的策略，如果设置了`sampler`，`shuffle`必须为`False`。
- `batch_sampler`：与`sampler`类似，但一次返回一个batch的索引。
- `num_workers`：加载数据时使用的子进程数量。`0`表示数据将在主进程中加载。
- `collate_fn`：如何将一个列表的样本组合成一个batch的数据。
- `pin_memory`：如果`True`，DataLoader将会在返回之前将tensors拷贝到CUDA的固定内存。
- `drop_last`：如果`True`，则丢弃不能完整形成一个batch的数据。
- `timeout`：如果大于`0`，则表示从worker中获取一个batch的最大时间（秒）。
- `worker_init_fn`：每个worker初始化时调用的函数。

# 其他

```python
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torchvision import datasets, transforms
from  torch.utils.data import DataLoader
import torch.optim.lr_scheduler as lr_scheduler

class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1_1 = nn.Conv2d(1, 32, kernel_size=5, padding=2)
        self.prelu1_1 = nn.PReLU()
        self.conv1_2 = nn.Conv2d(32, 32, kernel_size=5, padding=2)
        self.prelu1_2 = nn.PReLU()
        self.conv2_1 = nn.Conv2d(32, 64, kernel_size=5, padding=2)
        self.prelu2_1 = nn.PReLU()
        self.conv2_2 = nn.Conv2d(64, 64, kernel_size=5, padding=2)
        self.prelu2_2 = nn.PReLU()
        self.conv3_1 = nn.Conv2d(64, 128, kernel_size=5, padding=2)
        self.prelu3_1 = nn.PReLU()
        self.conv3_2 = nn.Conv2d(128, 128, kernel_size=5, padding=2)
        self.prelu3_2 = nn.PReLU()
        self.preluip1 = nn.PReLU()
        self.ip1 = nn.Linear(128*3*3, 2)
        self.ip2 = nn.Linear(2, 10, bias=False)

    def forward(self, x):
        x = self.prelu1_1(self.conv1_1(x))
        x = self.prelu1_2(self.conv1_2(x))
        x = F.max_pool2d(x,2)
        x = self.prelu2_1(self.conv2_1(x))
        x = self.prelu2_2(self.conv2_2(x))
        x = F.max_pool2d(x,2)
        x = self.prelu3_1(self.conv3_1(x))
        x = self.prelu3_2(self.conv3_2(x))
        x = F.max_pool2d(x,2)
        x = x.view(-1, 128*3*3)
        ip1 = self.preluip1(self.ip1(x))
        ip2 = self.ip2(ip1)
        return ip1, F.log_softmax(ip2, dim=1)

def train(epoch):
    print("Training... Epoch = %d" % epoch)
    ip1_loader = []
    idx_loader = []
    for i,(data, target) in enumerate(train_loader):

        ip1, pred = model(data)
        loss = nllloss(pred, target)

        optimizer4nn.zero_grad()

        loss.backward()

        optimizer4nn.step()
        ip1_loader.append(ip1)
        idx_loader.append((target))

    feat = torch.cat(ip1_loader, 0)
    labels = torch.cat(idx_loader, 0)
```

在pytorch里面自定义层也是通过继承自nn.Module类来实现的，我前面说过，pytorch里面一般是没有层的概念，层也是当成一个模型来处理的

# PyTorch_Tutorial

## Introduction

### What is PyTorch?

PyTorch is a Python-based scientific computing package serving two broad purposes:

- A replacement for NumPy to use the power of GPUs and other accelerators.
- An automatic differentiation library that is useful to implement neural networks.

### Goal of this tutorial

- Understand PyTorch’s Tensor library and neural networks at a high level.
- Train a small neural network to classify images

## Tensor

Tensors are a specialized data structure that are very similar to arrays and matrices. **In PyTorch, we use tensors to encode the inputs and outputs of a model, as well as the model’s parameters.**

Tensors are similar to NumPy’s ndarrays, except that tensors can run on GPUs or other specialized hardware to accelerate computing. If you’re familiar with ndarrays, you’ll be right at home with the Tensor API. If not, follow along in this quick API walkthrough.

```python
import torch
import numpy as np
```

### Tensor Initialization

Tensors can be initialized in various ways. Take a look at the following examples:

#### Directly from data

Tensors can be created directly from data. The data type is automatically inferred.

```python
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
```

### From a NumPy array

Tensors can be created from NumPy arrays (and vice versa).

```python
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
```

#### From another tensor:

The new tensor retains the properties (shape, datatype) of the argument tensor, unless explicitly overridden.

```python
x_ones = torch.ones_like(x_data) # retains the properties of x_data
print(f"Ones Tensor: \\n {x_ones} \\n")

x_rand = torch.rand_like(x_data, dtype=torch.float) # overrides the datatype of x_data
print(f"Random Tensor: \\n {x_rand} \\n")
```

#### With random or constant values:

**shape** is a tuple of tensor dimensions. In the functions below, it determines the dimensionality of the output tensor.

```python
shape = (2,3,)
rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

print(f"Random Tensor: \\n {rand_tensor} \\n")
print(f"Ones Tensor: \\n {ones_tensor} \\n")
print(f"Zeros Tensor: \\n {zeros_tensor}")
```

### Tensor Attributes

Tensor attributes describe their shape, datatype, and the device on which they are stored.

```python
tensor = torch.rand(3,4)

print(f"Shape of tensor: {tensor.shape}")
print(f"Datatype of tensor: {tensor.dtype}")
print(f"Device tensor is stored on: {tensor.device}")
```

### Tensor Operations

Over 100 tensor operations, including transposing, indexing, slicing, mathematical operations, linear algebra, random sampling, and more are comprehensively described here.([https://pytorch.org/docs/stable/torch.html](https://pytorch.org/docs/stable/torch.html))

Each of them can be run on the GPU (at typically higher speeds than on a CPU). If you’re using Colab, allocate a GPU by going to Edit > Notebook Settings.

```python
# We move our tensor to the GPU if available
if torch.cuda.is_available():
  tensor = tensor.to('cuda')
```

Try out some of the operations from the list. If you’re familiar with the NumPy API, you’ll find the Tensor API a breeze to use.

#### **Standard numpy-like indexing and slicing:**

```python
tensor = torch.ones(4, 4)
tensor[:,1] = 0
print(tensor)
```

#### Joining tensors

You can use **torch.cat** to concatenate a sequence of tensors along a given dimension. See also **torch.stack**, another tensor joining op that is subtly different from torch.cat.

```python
t1 = torch.cat([tensor, tensor, tensor], dim=1)
print(t1)
```

#### Multiplying tensors

```python
# This computes the element-wise product
print(f"tensor.mul(tensor) \\n {tensor.mul(tensor)} \\n")
# Alternative syntax:
print(f"tensor * tensor \\n {tensor * tensor}")
```

This computes the matrix multiplication between two tensors

```python
print(f"tensor.matmul(tensor.T) \\n {tensor.matmul(tensor.T)} \\n")
# Alternative syntax:
print(f"tensor @ tensor.T \\n {tensor @ tensor.T}")
```

#### In-place operations

Operations that have a _ suffix are in-place. For example: x.copy_(y), x.t_(), will change x.

```python
print(tensor, "\\n")
tensor.add_(5)
print(tensor)
```

In-place operations save some memory, but can be problematic when computing derivatives because of an immediate loss of history. Hence, their use is discouraged.

## Bridge with NumPy

Tensors on the CPU and NumPy arrays can share their underlying memory locations, and changing one will change the other.

#### Tensor to NumPy array

```python
t = torch.ones(5)
print(f"t: {t}")
n = t.numpy()
print(f"n: {n}")
```

A change in the tensor reflects in the NumPy array.

```python
t.add_(1)
print(f"t: {t}")
print(f"n: {n}")
```

#### NumPy array to Tensor

```python
n = np.ones(5)
t = torch.from_numpy(n)
```

Changes in the NumPy array reflects in the tensor.

```python
np.add(n, 1, out=n)
print(f"t: {t}")
print(f"n: {n}")
```

## A GENTLE INTRODUCTION TO TORCH.AUTOGRAD

**torch.autograd** is PyTorch’s automatic differentiation engine that powers neural network training. In this section, you will get a conceptual understanding of how autograd helps a neural network train.

### Background

Neural networks (NNs) are a collection of nested functions that are executed on some input data. These functions are defined by parameters (consisting of weights and biases), which in PyTorch are stored in tensors.

Training a NN happens in two steps:

**Forward Propagation**: In forward prop, the NN makes its best guess about the correct output. It runs the input data through each of its functions to make this guess.

**Backward Propagation**: In backprop, the NN adjusts its parameters proportionate to the error in its guess. It does this by traversing backwards from the output, collecting the derivatives of the error with respect to the parameters of the functions (gradients), and optimizing the parameters using gradient descent. For a more detailed walkthrough of backprop, check out this video from 3Blue1Brown.

### Usage in PyTorch

Let’s take a look at a single training step. For this example, we load a pretrained resnet18 model from torchvision. We create a random data tensor to represent a single image with 3 channels, and height & width of 64, and its corresponding label initialized to some random values.

```python
import torch, torchvision
model = torchvision.models.resnet18(pretrained=True)
data = torch.rand(1, 3, 64, 64)
labels = torch.rand(1, 1000)
```

Next, we run the input data through the model through each of its layers to make a prediction. This is the **forward pass**.

```python
prediction = model(data) # forward pass
```

We use the model’s prediction and the corresponding label to calculate the error (loss). The next step is to backpropagate this error through the network. Backward propagation is kicked off when we call .backward() on the error tensor. Autograd then calculates and stores the gradients for each model parameter in the parameter’s .grad attribute.

```python
loss = (prediction - labels).sum()
loss.backward() # backward pass
```

Next, we load an optimizer, in this case SGD with a learning rate of 0.01 and momentum of 0.9. We register all the parameters of the model in the optimizer.

```python
optim = torch.optim.SGD(model.parameters(), lr=1e-2, momentum=0.9)
```

Finally, we call .step() to initiate gradient descent. The optimizer adjusts each parameter by its gradient stored in .grad.

```python
optim.step() #gradient descent
```