---
layout: single
title: 'Boston Housing Dataset 모델'
typora-root-url: ../
categories: Deeplearning_Pytorch.08.Convolutional Neural Network 모델
tag: Pytorch
toc: true
---

# Convolutional Neural Network 모델 정의


```python
import torch
import torch.nn as nn

device = "cuda" if torch.cuda.is_available() else "cpu"
print(device)
```

    cpu
    


```python
layer = nn.Conv2d(in_channels = 4,   # 입력 데이터의 channel 수
                  out_channels = 2, # filter(kernel) 수
                  kernel_size = 3,  # filter 크기 (3: h, 3: w)
                  # stride = 1,       # 이동크기 => 좌우, 상하 1씩 이동하면서 계산 (Default: 1)
                  # padding = 0,      # 패딩 크기 (Default: None)
                  # padding='same' # same: filter 크기와 똑같이 맞춘다.
                 )
print(layer)
```

    Conv2d(4, 2, kernel_size=(3, 3), stride=(1, 1))
    


```python
input_data = torch.ones(1, 4, 3, 3) # (데이터개수, channel: Conv2d에서 지정한 channel, height, width)
feature_map = layer(input_data)
print(feature_map.shape)
# [1:데이터개수, 2:out_channels, 1:height, 1:width]
feature_map
```

    torch.Size([1, 2, 1, 1])
    




    tensor([[[[-0.4027]],
    
             [[-0.0480]]]], grad_fn=<ConvolutionBackward0>)




```python
# 파라미터 weight 
weight = layer.weight # conv2d의 weight 조회 => filter 조회
weight.shape
# [2: 필터개수, 4: 필터채널개수, 3: 필터 height, 3: 필터 width]
```




    torch.Size([2, 4, 3, 3])




```python
# 파라미터 bias
bias = layer.bias
print(bias.shape) # [2: 필터개수(필터당 1개)]
bias
```

    torch.Size([2])
    




    Parameter containing:
    tensor([-0.0722, -0.1063], requires_grad=True)




```python
# 첫번째 필터 계산.
input_data[0].shape, weight[0].shape
```




    (torch.Size([4, 3, 3]), torch.Size([4, 3, 3]))




```python
ch1 = torch.sum(input_data[0, 0] * weight[0, 0]) # [0: 첫번째필터, 0: 첫번째 채널]
ch2 = torch.sum(input_data[0, 1] * weight[0, 1]) # [0: 첫번째필터, 1: 첫번째 채널]
ch3 = torch.sum(input_data[0, 2] * weight[0, 2]) 
ch4 = torch.sum(input_data[0, 3] * weight[0, 3]) 

result = ch1 + ch2 + ch3 + ch4 + bias[0]
result
```




    tensor(-0.4027, grad_fn=<AddBackward0>)




```python
ch1 = torch.sum(input_data[0, 0] * weight[1, 0]) # [0: 첫번째필터, 0: 첫번째 채널]
ch2 = torch.sum(input_data[0, 1] * weight[1, 1]) # [0: 첫번째필터, 1: 첫번째 채널]
ch3 = torch.sum(input_data[0, 2] * weight[1, 2]) 
ch4 = torch.sum(input_data[0, 3] * weight[1, 3]) 

result = ch1 + ch2 + ch3 + ch4 + bias[1]
result
```




    tensor(-0.0480, grad_fn=<AddBackward0>)




```python
weight
```




    Parameter containing:
    tensor([[[[-0.0846,  0.0131,  0.0618],
              [ 0.1597,  0.1273, -0.1505],
              [ 0.0380,  0.0278, -0.1599]],
    
             [[-0.1334, -0.0556, -0.0557],
              [-0.0236, -0.0601,  0.0384],
              [-0.1193, -0.0783,  0.1356]],
    
             [[-0.0137, -0.1212,  0.0708],
              [ 0.0279, -0.1282, -0.0703],
              [ 0.1166,  0.0414,  0.1039]],
    
             [[-0.0181, -0.0360, -0.1220],
              [-0.0270,  0.1345,  0.0176],
              [-0.0263, -0.1170,  0.1559]]],
    
    
            [[[-0.0070,  0.0451, -0.0005],
              [-0.0211, -0.1611,  0.0137],
              [ 0.0635,  0.0836,  0.1595]],
    
             [[-0.0238,  0.0228,  0.0719],
              [ 0.1345, -0.0151,  0.0176],
              [-0.0486, -0.1316, -0.1033]],
    
             [[ 0.1064,  0.1398,  0.0237],
              [ 0.0939,  0.0375, -0.0088],
              [-0.0467, -0.0712, -0.1060]],
    
             [[ 0.0644, -0.0854, -0.0591],
              [-0.0328, -0.0936, -0.1588],
              [-0.0515,  0.0430,  0.1635]]]], requires_grad=True)




```python
m_layer = nn.MaxPool2d(kernel_size = 2, # 최대값을 추출할 영역크기 (2x2 영역의 max값 추출)
                       stride = 2,      # 이동 size => kernel_size 와 stride는 같은 값 지정해서 영역이 겹치지 않도록 한다.
                      )
input_data2 = torch.rand(1, 4, 4)
input_data2
```




    tensor([[[0.5776, 0.3370, 0.9638, 0.9961],
             [0.0690, 0.9467, 0.6114, 0.5507],
             [0.3081, 0.9029, 0.0378, 0.0651],
             [0.4741, 0.3923, 0.1740, 0.9599]]])




```python
result2 = m_layer(input_data2)
result2.shape
```




    torch.Size([1, 2, 2])




```python
result2
```




    tensor([[[0.9467, 0.9961],
             [0.9029, 0.9599]]])




```python
p_layer = nn.MaxPool2d(kernel_size = 2, # 최대값을 추출할 영역크기 (2x2 영역의 max값 추출)
                       stride = 2,      # 이동 size => kernel_size 와 stride는 같은 값 지정해서 영역이 겹치지 않도록 한다.
                      )
input_data3 = torch.rand(1, 5, 5)
input_data3

result3 = p_layer(input_data3)
```


```python
input_data3
```




    tensor([[[0.6394, 0.2826, 0.7214, 0.5893, 0.9619],
             [0.6056, 0.5662, 0.0959, 0.3648, 0.5662],
             [0.5551, 0.8238, 0.7387, 0.7218, 0.6925],
             [0.1631, 0.9418, 0.9644, 0.1815, 0.2035],
             [0.3455, 0.7655, 0.2107, 0.9767, 0.8003]]])




```python
result3
```




    tensor([[[0.6394, 0.7214],
             [0.9418, 0.9644]]])




```python
y_layer = nn.MaxPool2d(kernel_size = 2, stride = 2,     
                       padding=1)
# kennel_size보다 작은 영역에서는 최대값을 추출하지 않는다.  => padding 을 지정해서 zero_padding을 추가해서 작은 영역에서도 추출 될수 있도록한다.
input_data4 = torch.rand(1, 5, 5)
input_data4

result4 = y_layer(input_data3)
```


```python
input_data4
```




    tensor([[[0.0463, 0.1609, 0.6745, 0.1606, 0.4480],
             [0.6301, 0.8966, 0.3258, 0.5710, 0.4235],
             [0.9050, 0.7994, 0.6657, 0.7833, 0.0527],
             [0.7383, 0.8281, 0.4279, 0.6054, 0.3305],
             [0.4753, 0.1431, 0.3351, 0.6752, 0.6634]]])




```python
result4
```




    tensor([[[0.6394, 0.7214, 0.9619],
             [0.6056, 0.8238, 0.7218],
             [0.3455, 0.9644, 0.9767]]])




```python
input_data3
```




    tensor([[[0.6394, 0.2826, 0.7214, 0.5893, 0.9619],
             [0.6056, 0.5662, 0.0959, 0.3648, 0.5662],
             [0.5551, 0.8238, 0.7387, 0.7218, 0.6925],
             [0.1631, 0.9418, 0.9644, 0.1815, 0.2035],
             [0.3455, 0.7655, 0.2107, 0.9767, 0.8003]]])




```python
result3
```




    tensor([[[0.6394, 0.7214],
             [0.9418, 0.9644]]])



# MNIST


```python
import os

import torch
from torch import nn
import torch.nn.functional as F # Loss 함수
from torch.utils.data import DataLoader
from torchvision import models, datasets, transforms
from torchinfo import summary

import matplotlib.pyplot as plt
import numpy as np

from module.data import load_mnist_dataset, load_fashion_mnist_dataset
from module.train import fit

device = "cuda" if torch.cuda.is_available() else "cpu"

```


```python
# 하이퍼 파라미터
N_EPOCH = 1
BATCH_SIZE = 256
LR = 0.001
```

## Data 준비


```python
# MNIST
train_loader = load_mnist_dataset('datasets', BATCH_SIZE, True) # 저장경로, batch크기 Trainset여부
test_loader = load_mnist_dataset('datasets', BATCH_SIZE, False)
```


```python
# Fashion MNIST
train_loader = load_fashion_mnist_dataset('datasets', BATCH_SIZE, True) 
test_loader = load_fashion_mnist_dataset('datasets', BATCH_SIZE, False)
```


```python
train_loader.dataset
```




    Dataset MNIST
        Number of datapoints: 60000
        Root location: datasets
        Split: Train
        StandardTransform
    Transform: Compose(
                   ToTensor()
               )




```python
test_loader.dataset
```




    Dataset MNIST
        Number of datapoints: 10000
        Root location: datasets
        Split: Test
        StandardTransform
    Transform: Compose(
                   ToTensor()
               )



## CNN 모델 정의


```python
# CNN -> Convolution Layer: filter 개수(out_channel로 설정) 뒤로 갈수록 크게 잡는다.
         # Max Pooling layer를 이용해서 출력결과(Feature map)의 size(height, width)는 줄여나간다. (보통 절반씩 줄인다.)
# Conv block
## 1. Conv + ReLU + MaxPooling
## 2. Conv + BatchNorm + ReLU + MaxPooling
## 3. Conv + BatchNorm + ReLU + Dropout + MaxPooling

class MNISTCNNModel(nn.Module):

    def __init__(self):
        super().__init__()
        self.b1 = nn.Sequential(
            # Conv2(): 3 X 3 필터, stride=1, padding=1 => same padding(입력 size와 출력 size가 동일)
            nn.Conv2d(1, 32, kernel_size = 3, stride = 1, padding = 1),
            nn.BatchNorm2d(32), # Channel 기준으로 정규화 -> 입력 channel수 지정.
            nn.ReLU(),
            nn.Dropout2d(p = 0.3),
            nn.MaxPool2d(kernel_size = 2, stride = 2), # kernel_size와 stride가 같을 경우에는 stride 생략가능
            # MaxPool2d() 에서도 padding 지정
        )
        self.b2 = nn.Sequential(
            nn.Conv2d(32, 64, kernel_size=3, padding='same'),
            nn.BatchNorm2d(64),
            nn.ReLU(),
            nn.Dropout2d(p=0.3),
            nn.MaxPool2d(kernel_size=2) # kernel_size = stride -> stride 생략가능
        )
        self.b3 = nn.Sequential(
            nn.Conv2d(64, 128, kernel_size=3, padding='same'),
            nn.BatchNorm2d(128),
            nn.ReLU(),
            nn.Dropout2d(p=0.3),
            nn.MaxPool2d(kernel_size=2, padding=1) # 입력: 7 X 7 => 1/2 줄이면 -> 3.5 -> 0.5를 살리기 위해 padding 저장
        )
        
        # 결과 출력레이어 => Linear()
        self.output_block = nn.Sequential(
            # MaxPool2d() 출력결과 입력으로 받는다. => 4차원 (batch, ch, h, w)
            nn.Flatten(),
            nn.Linear(128*4*4, out_features=512),
            nn.ReLU(),
            nn.Dropout2d(p=0.3),
            nn.Linear(512, 10) # out = 클래스 개수
        )
        
    def forward(self, x):
        out = self.b1(x)
        out = self.b2(out)
        out = self.b3(out)
        out = self.output_block(out)
        
        return out
```


```python
model = MNISTCNNModel()
model.parameters()
```




    <generator object Module.parameters at 0x000001EACBEC18C0>




```python
model = MNISTCNNModel()
summary(model, (1, 1, 28, 28))
```

    C:\Users\world\anaconda3\envs\torch\lib\site-packages\torch\nn\functional.py:1345: UserWarning: dropout2d: Received a 2-D input to dropout2d, which is deprecated and will result in an error in a future release. To retain the behavior and silence this warning, please use dropout instead. Note that dropout2d exists to provide channel-wise dropout on inputs with 2 spatial dimensions, a channel dimension, and an optional batch dimension (i.e. 3D or 4D inputs).
      warnings.warn(warn_msg)
    




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    MNISTCNNModel                            [1, 10]                   --
    ├─Sequential: 1-1                        [1, 32, 14, 14]           --
    │    └─Conv2d: 2-1                       [1, 32, 28, 28]           320
    │    └─BatchNorm2d: 2-2                  [1, 32, 28, 28]           64
    │    └─ReLU: 2-3                         [1, 32, 28, 28]           --
    │    └─Dropout2d: 2-4                    [1, 32, 28, 28]           --
    │    └─MaxPool2d: 2-5                    [1, 32, 14, 14]           --
    ├─Sequential: 1-2                        [1, 64, 7, 7]             --
    │    └─Conv2d: 2-6                       [1, 64, 14, 14]           18,496
    │    └─BatchNorm2d: 2-7                  [1, 64, 14, 14]           128
    │    └─ReLU: 2-8                         [1, 64, 14, 14]           --
    │    └─Dropout2d: 2-9                    [1, 64, 14, 14]           --
    │    └─MaxPool2d: 2-10                   [1, 64, 7, 7]             --
    ├─Sequential: 1-3                        [1, 128, 4, 4]            --
    │    └─Conv2d: 2-11                      [1, 128, 7, 7]            73,856
    │    └─BatchNorm2d: 2-12                 [1, 128, 7, 7]            256
    │    └─ReLU: 2-13                        [1, 128, 7, 7]            --
    │    └─Dropout2d: 2-14                   [1, 128, 7, 7]            --
    │    └─MaxPool2d: 2-15                   [1, 128, 4, 4]            --
    ├─Sequential: 1-4                        [1, 10]                   --
    │    └─Flatten: 2-16                     [1, 2048]                 --
    │    └─Linear: 2-17                      [1, 512]                  1,049,088
    │    └─ReLU: 2-18                        [1, 512]                  --
    │    └─Dropout2d: 2-19                   [1, 512]                  --
    │    └─Linear: 2-20                      [1, 10]                   5,130
    ==========================================================================================
    Total params: 1,147,338
    Trainable params: 1,147,338
    Non-trainable params: 0
    Total mult-adds (M): 8.55
    ==========================================================================================
    Input size (MB): 0.00
    Forward/backward pass size (MB): 0.71
    Params size (MB): 4.59
    Estimated Total Size (MB): 5.30
    ==========================================================================================



## Train


```python
# MNIST
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=LR)

result = fit(train_loader, test_loader, model, loss_fn, optimizer, N_EPOCH, 
             save_best_model=False, early_stopping=True, device=device, mode='multi'
            )
```

    Epoch[1/1] - Train loss: 0.05659 Train Accucracy: 0.98155 || Validation Loss: 0.04868 Validation Accuracy: 0.98320
    ====================================================================================================
    204.90558528900146 초
    


```python
# Fashion MNIST
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=LR)

result = fit(train_loader, test_loader, model, loss_fn, optimizer, N_EPOCH, 
             save_best_model=False, early_stopping=True, device=device, mode='multi'
            )
```

    Epoch[1/1] - Train loss: 0.36133 Train Accucracy: 0.86838 || Validation Loss: 0.38384 Validation Accuracy: 0.86210
    ====================================================================================================
    209.18013167381287 초
    
