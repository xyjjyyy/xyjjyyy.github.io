## CNN

> 现在采用 pytorch 深度学习框架可以大大简化我我们深度学习的步骤并且大幅减少代码量，下面我们看看CNN利用 pytorch 框架的操作步骤

1. 引入需要的包

   > 主要看自己的需要引入需要的包

   其中 torch 和 d2l 几乎是必须要引入的

   ```python
   import torch
   from torch import nn
   from d2l import torch as d2l
   ```

2. 建立模型

   > 我们不需要深入底层去建立我们的模型，只需要在集成化层面去尝试对每一层的模型进行线性组合即可

   - `LeNet`网络

     

     <img src="https://lky-img.obs.cn-east-2.myhuaweicloud.com/image-20220522184703813.png" alt="image-20220522184703813" style="zoom: 33%;" />

     ```python
     net = nn.Sequential(
     nn.Conv2d(1, 6, kernel_size=5, padding=2), nn.Sigmoid(),
     nn.AvgPool2d(kernel_size=2, stride=2),
     nn.Conv2d(6, 16, kernel_size=5), nn.Sigmoid(),
     nn.AvgPool2d(kernel_size=2, stride=2),
     nn.Flatten(),
     nn.Linear(16 * 5 * 5, 120), nn.Sigmoid(),
     nn.Linear(120, 84), nn.Sigmoid(),
     nn.Linear(84, 10))
     ```

   - `VGG`网络

     ![image-20220522185504165](https://lky-img.obs.cn-east-2.myhuaweicloud.com/image-20220522185504165.png)

     ```python
     def vgg(conv_arch):
     conv_blks = []
     in_channels = 1
     # 卷积层部分
     for (num_convs, out_channels) in conv_arch:
     conv_blks.append(vgg_block(num_convs, in_channels, out_channels))
     in_channels = out_channels
     return nn.Sequential(
     *conv_blks, nn.Flatten(),
     # 全连接层部分
     nn.Linear(out_channels * 7 * 7, 4096), nn.ReLU(), nn.Dropout(0.5),
     nn.Linear(4096, 4096), nn.ReLU(), nn.Dropout(0.5),
     nn.Linear(4096, 10))
     net = vgg(conv_arch)
     ```

     

3. 之后确定我们的 `batch_size`(这个设的好几乎是个高手)，并引入数据集

   ```
   batch_size = 128
   train_iter, test_iter = d2l.load_data_fashion_mnist(batch_size, resize=224)
   ```

4. 确定学习率，和学习次数并进行训练

   小 慢  大 找不到

   ```
   lr, num_epochs = 0.01, 10
   d2l.train_ch6(net, train_iter, test_iter, num_epochs, lr, d2l.try_gpu())
   ```

---

之后可能进行模型判断之类的都是后话，主要是我们做的和图像不几乎不相关可能大概率用不上；

## RNN(循环神经网络)

