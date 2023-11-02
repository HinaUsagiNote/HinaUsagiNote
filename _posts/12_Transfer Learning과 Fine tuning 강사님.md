---
layout: single
title: 'Boston Housing Dataset 모델'
typora-root-url: ../
categories: Deeplearning_Pytorch.10.Transfer Learning과 Fine Tuning 강사님
tag: Pytorch
toc: true
---

# Pretrained Model

- 다른 목적을 위해 미리 학습된 모델.
- Pretrained model을 현재 해결하려는 문제에 이용한다.
- 대부분 내가 만들려는 네트워크 모델에 pretrained model을 포함시켜 사용한다.
    - 이런 방식을 Transfer Learning (전이 학습)이라고 한다.
    - 보통 Feature Extractor block을 재사용한다.

## Pytorch에서 제공하는 Pretrained Model
- 분야별 라이브러리에서 제공
    - torchvision: https://pytorch.org/vision/stable/models.html
- torch hub 를 이용해 모델과 학습된 parameter를 사용할 수 있다.
    - https://pytorch.org/hub/
- 이외에도 많은 모델과 학습된 paramter가 인터넷상에 공개되 있다.    
    - 딥러닝 모델기반 application을 개발 할 때는 대부분 Transfer Learning을 한다.  
    - 다양한 분야에서 연구된 많은 딥러닝 모델들이 구현되어 공개 되어 있으며 학습된 Parameter들도 제공되고 있다.  
    - [paperswithcode](https://paperswithcode.com/)에서 State Of The Art(SOTA) 논문들과 그 구현된 모델을 확인할 수 있다.
    
>  **State Of The Art(SOTA)**: 특정 시점에 특정 분야에서 가장 성능이 좋은 모델을 말한다.

## VGGNet Pretrained 모델을 이용해 이미지 분류

- Pytorch가 제공하는 VGG 모델은 ImageNet dataset으로 학습시킨 weight를 제공한다.
    - 120만장의 transet, 1000개의 class로 구성된 데이터셋.
    - Output으로 1000개의 카테고리에 대한 확률을 출력한다.


```python
# ImageNet 1000개의 class 목록
# !pip install wget
import wget
url = 'https://gist.github.com/yrevar/942d3a0ac09ec9e5eb3a/raw/238f720ff059c1f82f368259d1ca4ffa5dd8f9f5/imagenet1000_clsidx_to_labels.txt'
imagenet_filepath = wget.download(url) # 다운로드 받고 받은 경로를 반환.
```

    100% [..............................................................................] 30564 / 30564


```python
imagenet_filepath  # 다운 받은 파일의 경로
```




    'imagenet1000_clsidx_to_labels (1).txt'




```python
# eval("파이썬코드")  # 파이썬 코드를 문자열로 넣으면 알아서 해석해서 실행
r = eval("1 + 3")
print(type(r))
r
```

    <class 'int'>
    




    4




```python
r = eval("[1, 2, 3]")
print(r, type(r))
```

    [1, 2, 3] <class 'list'>
    


```python
r = eval("{0:'사람', 1:'물통'}")
print(type(r), r)
```

    <class 'dict'> {0: '사람', 1: '물통'}
    


```python
# with open(imagenet_filepath, "rt") as fr:
#     a = fr.read()
#     print(type(a))
#     print(a)
```


```python
with open(imagenet_filepath, "rt") as fr:
    index_to_class = eval(fr.read())

print(type(index_to_class))
```

    <class 'dict'>
    


```python
index_to_class[1]
```




    'goldfish, Carassius auratus'




```python
# 추론할 이미지 다운로드
import requests
from io import BytesIO
from PIL import Image
```


```python
## import

import torch
from torchvision import models, transforms
from torchinfo import summary

device = "cuda" if torch.cuda.is_available() else "cpu"
device
```




    'cpu'




```python
## torchvision에서 제공하는 Pretrained 모델을 다운로드
### VGG19 모델.
### weights: 학습된 파라미터들을 같이 다운로드 받는다. => Image Net 데이터셋으로 학습한 파라미터를 받는다.
##### IMAGENET1K_V1 : Image net 버전 1로 학습된 파라미터
##### IMAGENET1K_V2 : 버전 2로 학습된 파라미터
####### 모델에 따라서 V2는 없을 수도 있다.
######   models.VGG19_Weights.DEFAULT - 둘중에 기본 데이터셋으로 설정된 것을 받는다.
load_model_vgg = models.vgg19(weights=models.VGG19_Weights.IMAGENET1K_V1)
```


```python
load_model_alex = models.alexnet(weights=models.AlexNet_Weights.DEFAULT)
```


```python
summary(load_model_vgg, (100, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    VGG                                      [100, 1000]               --
    ├─Sequential: 1-1                        [100, 512, 7, 7]          --
    │    └─Conv2d: 2-1                       [100, 64, 224, 224]       1,792
    │    └─ReLU: 2-2                         [100, 64, 224, 224]       --
    │    └─Conv2d: 2-3                       [100, 64, 224, 224]       36,928
    │    └─ReLU: 2-4                         [100, 64, 224, 224]       --
    │    └─MaxPool2d: 2-5                    [100, 64, 112, 112]       --
    │    └─Conv2d: 2-6                       [100, 128, 112, 112]      73,856
    │    └─ReLU: 2-7                         [100, 128, 112, 112]      --
    │    └─Conv2d: 2-8                       [100, 128, 112, 112]      147,584
    │    └─ReLU: 2-9                         [100, 128, 112, 112]      --
    │    └─MaxPool2d: 2-10                   [100, 128, 56, 56]        --
    │    └─Conv2d: 2-11                      [100, 256, 56, 56]        295,168
    │    └─ReLU: 2-12                        [100, 256, 56, 56]        --
    │    └─Conv2d: 2-13                      [100, 256, 56, 56]        590,080
    │    └─ReLU: 2-14                        [100, 256, 56, 56]        --
    │    └─Conv2d: 2-15                      [100, 256, 56, 56]        590,080
    │    └─ReLU: 2-16                        [100, 256, 56, 56]        --
    │    └─Conv2d: 2-17                      [100, 256, 56, 56]        590,080
    │    └─ReLU: 2-18                        [100, 256, 56, 56]        --
    │    └─MaxPool2d: 2-19                   [100, 256, 28, 28]        --
    │    └─Conv2d: 2-20                      [100, 512, 28, 28]        1,180,160
    │    └─ReLU: 2-21                        [100, 512, 28, 28]        --
    │    └─Conv2d: 2-22                      [100, 512, 28, 28]        2,359,808
    │    └─ReLU: 2-23                        [100, 512, 28, 28]        --
    │    └─Conv2d: 2-24                      [100, 512, 28, 28]        2,359,808
    │    └─ReLU: 2-25                        [100, 512, 28, 28]        --
    │    └─Conv2d: 2-26                      [100, 512, 28, 28]        2,359,808
    │    └─ReLU: 2-27                        [100, 512, 28, 28]        --
    │    └─MaxPool2d: 2-28                   [100, 512, 14, 14]        --
    │    └─Conv2d: 2-29                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-30                        [100, 512, 14, 14]        --
    │    └─Conv2d: 2-31                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-32                        [100, 512, 14, 14]        --
    │    └─Conv2d: 2-33                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-34                        [100, 512, 14, 14]        --
    │    └─Conv2d: 2-35                      [100, 512, 14, 14]        2,359,808
    │    └─ReLU: 2-36                        [100, 512, 14, 14]        --
    │    └─MaxPool2d: 2-37                   [100, 512, 7, 7]          --
    ├─AdaptiveAvgPool2d: 1-2                 [100, 512, 7, 7]          --
    ├─Sequential: 1-3                        [100, 1000]               --
    │    └─Linear: 2-38                      [100, 4096]               102,764,544
    │    └─ReLU: 2-39                        [100, 4096]               --
    │    └─Dropout: 2-40                     [100, 4096]               --
    │    └─Linear: 2-41                      [100, 4096]               16,781,312
    │    └─ReLU: 2-42                        [100, 4096]               --
    │    └─Dropout: 2-43                     [100, 4096]               --
    │    └─Linear: 2-44                      [100, 1000]               4,097,000
    ==========================================================================================
    Total params: 143,667,240
    Trainable params: 143,667,240
    Non-trainable params: 0
    Total mult-adds (T): 1.96
    ==========================================================================================
    Input size (MB): 60.21
    Forward/backward pass size (MB): 11889.03
    Params size (MB): 574.67
    Estimated Total Size (MB): 12523.91
    ==========================================================================================




```python
summary(load_model_alex, (1, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    AlexNet                                  [1, 1000]                 --
    ├─Sequential: 1-1                        [1, 256, 6, 6]            --
    │    └─Conv2d: 2-1                       [1, 64, 55, 55]           23,296
    │    └─ReLU: 2-2                         [1, 64, 55, 55]           --
    │    └─MaxPool2d: 2-3                    [1, 64, 27, 27]           --
    │    └─Conv2d: 2-4                       [1, 192, 27, 27]          307,392
    │    └─ReLU: 2-5                         [1, 192, 27, 27]          --
    │    └─MaxPool2d: 2-6                    [1, 192, 13, 13]          --
    │    └─Conv2d: 2-7                       [1, 384, 13, 13]          663,936
    │    └─ReLU: 2-8                         [1, 384, 13, 13]          --
    │    └─Conv2d: 2-9                       [1, 256, 13, 13]          884,992
    │    └─ReLU: 2-10                        [1, 256, 13, 13]          --
    │    └─Conv2d: 2-11                      [1, 256, 13, 13]          590,080
    │    └─ReLU: 2-12                        [1, 256, 13, 13]          --
    │    └─MaxPool2d: 2-13                   [1, 256, 6, 6]            --
    ├─AdaptiveAvgPool2d: 1-2                 [1, 256, 6, 6]            --
    ├─Sequential: 1-3                        [1, 1000]                 --
    │    └─Dropout: 2-14                     [1, 9216]                 --
    │    └─Linear: 2-15                      [1, 4096]                 37,752,832
    │    └─ReLU: 2-16                        [1, 4096]                 --
    │    └─Dropout: 2-17                     [1, 4096]                 --
    │    └─Linear: 2-18                      [1, 4096]                 16,781,312
    │    └─ReLU: 2-19                        [1, 4096]                 --
    │    └─Linear: 2-20                      [1, 1000]                 4,097,000
    ==========================================================================================
    Total params: 61,100,840
    Trainable params: 61,100,840
    Non-trainable params: 0
    Total mult-adds (M): 714.68
    ==========================================================================================
    Input size (MB): 0.60
    Forward/backward pass size (MB): 3.95
    Params size (MB): 244.40
    Estimated Total Size (MB): 248.96
    ==========================================================================================




```python
model = load_model_alex.to(device)
i = torch.randn(1, 3, 224, 224).to(device)
p = model(i)
p.shape
```




    torch.Size([1, 1000])




```python
label = p[0].argmax(dim=-1).item()
index_to_class[label]
```




    'poncho'




```python
torch.nn.Softmax(dim=-1)(p[0]).max(dim=-1).values
```




    tensor(0.0938, grad_fn=<MaxBackward0>)




```python
# img_url = 'https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Common_goldfish.JPG/800px-Common_goldfish.JPG'
# img_url = 'https://cdn.download.ams.birds.cornell.edu/api/v1/asset/169231441/1800'
# img_url = 'https://blogs.ifas.ufl.edu/news/files/2021/10/anole-FB.jpg'
# img_url = 'https://i.namu.wiki/i/uk5VsDTA_sXLvqe0Rwj8E678mIj4SkhbUqRyS27TEvVsFVliMwtuO6BpLe9N65IYG5c2XgWndG13sQiReZoz1A.webp'
img_url = "https://i.pinimg.com/736x/1b/42/50/1b42503a3593d50bec4da04ee69a1b4f.jpg"


res = requests.get(img_url)  # url로 요청-> 응답 받기.
test_img = Image.open(BytesIO(res.content))
# 응답받은 것이 binary 파일일 경우. BytesIO(binary) => bytes타입 입출력이 가능한 객체.
test_img
```




    
![png](output_20_0.png)
    




```python
# 전처리
test_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor()
    # PIL.Image -> torch.Tensor, 0 ~ 1 scaling, channel first처리
])
```


```python
type(test_img), test_img.size
```




    (PIL.JpegImagePlugin.JpegImageFile, (735, 498))




```python
input_data = test_transform(test_img)
print(type(input_data), input_data.shape, input_data.min(), input_data.max())
```

    <class 'torch.Tensor'> torch.Size([3, 224, 224]) tensor(0.) tensor(1.)
    


```python
# batch 축을 추가.
input_data = input_data.unsqueeze(dim=0)
input_data.shape
```




    torch.Size([1, 3, 224, 224])




```python
# load_model_vgg = models.vgg19(weights=models.VGG19_Weights.IMAGENET1K_V1)
```


```python
# 모델을 device로 이동
model = load_model_vgg.to(device)
```


```python
# 추론
pred1 = model(input_data.to(device))
```


```python
pred1.shape
```




    torch.Size([1, 1000])




```python
# 추론결과를 확률로 변환
pred_prob1 = torch.nn.Softmax(dim=-1)(pred1)
# 추론 label, 확률값을 조회
label1 = pred_prob1.max(dim=-1).indices
prob1 = pred_prob1.max(dim=-1).values
# 추론 label을 이용해서 label name을 조회
labelname1 = index_to_class[label1.item()]
```


```python
print(label1.item(), labelname1)
print("확률:", prob1.item())
```

    283 Persian cat
    확률: 0.3343072235584259
    

# Transfer learning (전이학습)
- 사전에 학습된 신경망의 구조와 파라미터를 재사용해서 새로운 모델(우리가 만드는 모델)의 시작점으로 삼고 해결하려는 문제를 위해 다시 학습시킨다.
- 전이 학습을 통해 다음을 해결할 수 있다.
    1. 데이터 부족문제
        - 딥러닝은 대용량의 학습데이터가 필요하다.
        - 충분한 데이터를 수집하는 것은 항상 어렵다.
    2. 과다한 계산량
        - 신경망 학습에는 엄청난 양의 계산 자원이 필요하다.

![transfer_learning01](figures/09_transfer_01.png)

- 미리 학습된(pre-trained) Model을 이용하여 모델을 구성한 뒤 현재 하려는 예측 문제를 해결한다.
- 보통 Pretrained Model에서 Feature Extraction 부분을 사용한다.
    - Computer Vision 문제의 경우 Bottom 쪽의 Convolution Layer(Feature Extractor)들은 이미지에 나타나는 일반적인 특성을 추출하므로 **다른 대상을 가지고 학습했다고 하더라도 재사용할 수 있다.**
    - Top 부분 Layer 부분은 특히 출력 Layer의 경우 대상 데이터셋의 목적에 맞게 변경 해야 하므로 재사용할 수 없다.

![transfer_learning02](figures/09_transfer_02.png)

> **Frozon**: Training시 parameter가 update 되지 않도록 하는 것을 말한다.

### Feature extraction 재사용
- Pretrained Model에서 Feature Extractor 만 가져오고 추론기(Fully connected layer)만 새로 정의한 뒤 그 둘을 합쳐서 모델을 만든다.
- 학습시 직접 구성한 추론기만 학습되도록 한다.
    - Feature Extractor는 추론을 위한 Feature 추출을 하는 역할만 하고 그 parameter(weight)가 학습되지 않도록 한다.
- 모델/레이어의 parameter trainable 여부 속성 변경
    - model/layer 의 `parameters()` 메소드를 이용해 weight와 bias를 조회한 뒤 `requires_grad` 속성을 `False`로 변경한다.
        
#### Backbone, Base network
전체 네트워크에서 Feature Extraction의 역할을 담당하는 부분을 backbone/base network라고 한다.

## Fine-tuning(미세조정)
- Transfer Learning을 위한 Pretrained 모델을 내가 학습시켜야 하는 데이터셋(Custom Dataset)으로 **재학습**시키는 것을 fine tunning 이라고 한다.
- 주어진 문제에 더 적합하도록 Feature Extractor의 가중치들도 조정 한다.

### Fine tuning 전략
![transfer02](figures/09_transfer_03.png)

- **세 전략 모두 추론기는 trainable로 한다.**

**<font size='5'>1. 전체 모델을 전부 학습시킨다.(1번)</font>**    
- Pretrained 모델의 weight는 Feature extraction 의 초기 weight 역할을 한다.
- **Train dataset의 양이 많고** Pretrained 모델이 학습했던 dataset과 Custom dataset의 class간의 유사성이 **낮은 경우** 적용.
- 학습에 시간이 많이 걸린다.
    
    
**<font size='5'>2. Pretrained 모델 Bottom layer들(Input과 가까운 Layer들)은 고정시키고 Top layer의 일부를 재학습시킨다.(2번)</font>**     
- **Train dataset의 양이 많고** Pretrained 모델이 학습했던 dataset과 Custom dataset의 class간의 유사성이 **높은 경우** 적용.
- **Train dataset의 양이 적고** Pretained 모델이 학습했던 dataset과 custom dataset의 class간의 유사성이 **낮은 경우** 적용
    
    
**<font size='5'>3. Pretrained 모델 전체를 고정시키고 classifier layer들만 학습시킨다.(3번)</font>**      
- **Train dataset의 양이 적고** Pretrained 모델이 학습했던 dataset과 Custom dataset의 class간의 유사성이 **높은 경우** 적용.
  
  
> **Custom dataset:** 내가 학습시키고자 하는 dataset
> 1번 2번 전략을 Fine tuning 이라고 한다.

![fine tuning](figures/09_finetuning.png)


```python
%%writefile module/utils.py

import matplotlib.pyplot as plt

def plot_fit_result(train_loss_list, train_acc_list, val_loss_list, val_acc_list):
    plt.figure(figsize=(12, 5))
    plt.subplot(1, 2, 1)
    plt.title("Loss")
    plt.plot(train_loss_list, label="Train")
    plt.plot(val_loss_list, label="Validation")
    plt.xlabel("Epoch")
    plt.ylabel("Loss")
    plt.legend()

    plt.subplot(1, 2, 2)
    plt.title("Accuracy")
    plt.plot(train_acc_list, label="Train")
    plt.plot(val_acc_list, label="Validation")
    plt.xlabel("Epoch")
    plt.ylabel("Accuracy")
    plt.legend()

    plt.tight_layout()
    plt.show()
```

    Writing module/utils.py
    


```python
!pip install torchinfo
```

    Collecting torchinfo
      Downloading torchinfo-1.8.0-py3-none-any.whl (23 kB)
    Installing collected packages: torchinfo
    Successfully installed torchinfo-1.8.0
    


```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader

from torchvision import models, datasets, transforms
from torchinfo import summary

from module.train import fit
from module.utils import plot_fit_result

import os
from zipfile import ZipFile

!pip install gdown -U
import gdown

device = 'cuda' if torch.cuda.is_available() else "cpu"
device
```

    Requirement already satisfied: gdown in /usr/local/lib/python3.10/dist-packages (4.6.6)
    Collecting gdown
      Downloading gdown-4.7.1-py3-none-any.whl (15 kB)
    Requirement already satisfied: filelock in /usr/local/lib/python3.10/dist-packages (from gdown) (3.12.4)
    Requirement already satisfied: requests[socks] in /usr/local/lib/python3.10/dist-packages (from gdown) (2.31.0)
    Requirement already satisfied: six in /usr/local/lib/python3.10/dist-packages (from gdown) (1.16.0)
    Requirement already satisfied: tqdm in /usr/local/lib/python3.10/dist-packages (from gdown) (4.66.1)
    Requirement already satisfied: beautifulsoup4 in /usr/local/lib/python3.10/dist-packages (from gdown) (4.11.2)
    Requirement already satisfied: soupsieve>1.2 in /usr/local/lib/python3.10/dist-packages (from beautifulsoup4->gdown) (2.5)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (3.3.1)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (3.4)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (2.0.7)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (2023.7.22)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (1.7.1)
    Installing collected packages: gdown
      Attempting uninstall: gdown
        Found existing installation: gdown 4.6.6
        Uninstalling gdown-4.6.6:
          Successfully uninstalled gdown-4.6.6
    Successfully installed gdown-4.7.1
    




    'cuda'




```python
os.makedirs("data", exist_ok=True)
os.makedirs("datasets", exist_ok=True)
```


```python
# download
url = 'https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV'
path = r'data/cats_and_dogs_small.zip'
gdown.download(url, path, quiet=False)
```

    Downloading...
    From (uriginal): https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV
    From (redirected): https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV&confirm=t&uuid=96112698-c170-4650-8392-668f032079de
    To: /content/data/cats_and_dogs_small.zip
    100%|██████████| 90.8M/90.8M [00:01<00:00, 60.0MB/s]
    




    'data/cats_and_dogs_small.zip'




```python
# 압축 풀기 - zip
image_path = os.path.join("datasets", "cats_and_dogs_small") # 압축 풀 경로
with ZipFile(path) as zfile:  # ZipFile(압축파일경로)
    zfile.extractall(image_path)
    # extractall(압축풀 디렉토리 경로) #경로생략-현재 DIR에 푼다.
```

## Dataset, DataLoader를 생성


```python
#### transform 정의
###### Trainset: Image augmentation + Resize + ToTensor
train_transform = transforms.Compose([
    transforms.Resize((224, 224)), # Image Net 학습 모델=>input size: 224, 224
    # 상수 -> 1-상수 ~ 1+상수 범위 에서 변환.
    transforms.ColorJitter(brightness=0.3, contrast=0.3, saturation=0.5, hue=0.15),
    transforms.RandomHorizontalFlip(), #p=0.5(default)
    transforms.RandomVerticalFlip(),
    transforms.RandomRotation(degrees=90), # -90 ~ +90 범위에서 회전.
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
    # 채널별: (평균), (표준편차)  # (각픽셀값-평균)/표준편차
])
```


```python
##### validation/test set ==> Image augmentation은 정의하지 않는다.
##########  Resize, ToTensor, Normalize
test_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
])
```

##### Dataset


```python
image_path+"\train"
```




    'datasets\\cats_and_dogs_small\train'




```python
train_set = datasets.ImageFolder(os.path.join(image_path, "train"),
                                 transform=train_transform)
valid_set = datasets.ImageFolder(os.path.join(image_path, "validation"),
                                 transform=test_transform)
test_set = datasets.ImageFolder(os.path.join(image_path, "test"),
                                transform=test_transform)
```


```python
train_set.classes
```




    ['cats', 'dogs']




```python
train_set.class_to_idx
```




    {'cats': 0, 'dogs': 1}




```python
len(train_set), len(valid_set), len(test_set)
```




    (2000, 1000, 1000)




```python
##### 하이퍼파라미터 정의
BATCH_SIZE = 256
N_EPOCH = 20
LR = 0.001
```


```python
os.cpu_count()
```




    2




```python
###### DataLoader
train_loader = DataLoader(train_set, batch_size=BATCH_SIZE, drop_last=True,
                          shuffle=True,
                          num_workers=os.cpu_count())
                          # Data를 load 할때 병렬처리하겠다. => 한번에 몇개씩 동시에 loading할지.
                          # os.cpu_count() -> cpu 프로세서 개수
valid_loader = DataLoader(valid_set, batch_size=BATCH_SIZE, num_workers=os.cpu_count())
test_loader = DataLoader(test_set, batch_size=BATCH_SIZE, num_workers=os.cpu_count())
```


```python
# step수
len(train_loader), len(test_loader), len(valid_loader)
```




    (7, 4, 4)




```python
# 한 배치만 조회
batch_one = next(iter(train_loader))  # (X, y)
batch_one[0].shape, batch_one[1].shape
```




    (torch.Size([256, 3, 224, 224]), torch.Size([256]))




```python
import matplotlib.pyplot as plt
plt.imshow(batch_one[0][30].permute(1,2,0))
```

    Clipping input data to the valid range for imshow with RGB data ([0..1] for floats or [0..255] for integers).
    




    <matplotlib.image.AxesImage at 0x20d5d3688b0>




    
![png](output_59_2.png)
    



```python
### 모델의 모든 파라미터들을 조회 -> model.parameters()
#      : 모델의 모든 layer들의 모든 파라미터(weight, bias)를 순서대로 반환하는 generator
### Layer의 파라미터 -> Layer객체.weight, layer객체.bias

## 파라미터(Tensor)의 train가능 여부 ->
#        Tensor객체.requres_grad: True - trainable, False: Non-Trainable(Frozen)
```


```python
# pretrained VGG19 모델
model = models.vgg19(weights=models.VGG19_Weights.DEFAULT)
```

    Downloading: "https://download.pytorch.org/models/vgg19-dcbb9e9d.pth" to /root/.cache/torch/hub/checkpoints/vgg19-dcbb9e9d.pth
    100%|██████████| 548M/548M [00:05<00:00, 96.4MB/s]
    


```python
#### 파라미터 확인 - trainable 상태를 확인
for param in model.parameters():
    print(param.shape, param.requires_grad)
#     break
```

    torch.Size([64, 3, 3, 3]) True
    torch.Size([64]) True
    torch.Size([64, 64, 3, 3]) True
    torch.Size([64]) True
    torch.Size([128, 64, 3, 3]) True
    torch.Size([128]) True
    torch.Size([128, 128, 3, 3]) True
    torch.Size([128]) True
    torch.Size([256, 128, 3, 3]) True
    torch.Size([256]) True
    torch.Size([256, 256, 3, 3]) True
    torch.Size([256]) True
    torch.Size([256, 256, 3, 3]) True
    torch.Size([256]) True
    torch.Size([256, 256, 3, 3]) True
    torch.Size([256]) True
    torch.Size([512, 256, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([512, 512, 3, 3]) True
    torch.Size([512]) True
    torch.Size([4096, 25088]) True
    torch.Size([4096]) True
    torch.Size([4096, 4096]) True
    torch.Size([4096]) True
    torch.Size([1000, 4096]) True
    torch.Size([1000]) True
    


```python
## VGG19의 parameter들을 frozen (학습이 안되도록 처리 -> optimizer.step() 시 업데이트 안되도록한다.)
#### requires_grad=False
for param in model.parameters():
    param.requires_grad = False
```


```python
for param in model.parameters():
    print(param.requires_grad)
```

    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    False
    


```python
summary(model, (1, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    VGG                                      [1, 1000]                 --
    ├─Sequential: 1-1                        [1, 512, 7, 7]            --
    │    └─Conv2d: 2-1                       [1, 64, 224, 224]         (1,792)
    │    └─ReLU: 2-2                         [1, 64, 224, 224]         --
    │    └─Conv2d: 2-3                       [1, 64, 224, 224]         (36,928)
    │    └─ReLU: 2-4                         [1, 64, 224, 224]         --
    │    └─MaxPool2d: 2-5                    [1, 64, 112, 112]         --
    │    └─Conv2d: 2-6                       [1, 128, 112, 112]        (73,856)
    │    └─ReLU: 2-7                         [1, 128, 112, 112]        --
    │    └─Conv2d: 2-8                       [1, 128, 112, 112]        (147,584)
    │    └─ReLU: 2-9                         [1, 128, 112, 112]        --
    │    └─MaxPool2d: 2-10                   [1, 128, 56, 56]          --
    │    └─Conv2d: 2-11                      [1, 256, 56, 56]          (295,168)
    │    └─ReLU: 2-12                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-13                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-14                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-15                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-16                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-17                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-18                        [1, 256, 56, 56]          --
    │    └─MaxPool2d: 2-19                   [1, 256, 28, 28]          --
    │    └─Conv2d: 2-20                      [1, 512, 28, 28]          (1,180,160)
    │    └─ReLU: 2-21                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-22                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-23                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-24                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-25                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-26                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-27                        [1, 512, 28, 28]          --
    │    └─MaxPool2d: 2-28                   [1, 512, 14, 14]          --
    │    └─Conv2d: 2-29                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-30                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-31                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-32                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-33                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-34                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-35                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-36                        [1, 512, 14, 14]          --
    │    └─MaxPool2d: 2-37                   [1, 512, 7, 7]            --
    ├─AdaptiveAvgPool2d: 1-2                 [1, 512, 7, 7]            --
    ├─Sequential: 1-3                        [1, 1000]                 --
    │    └─Linear: 2-38                      [1, 4096]                 (102,764,544)
    │    └─ReLU: 2-39                        [1, 4096]                 --
    │    └─Dropout: 2-40                     [1, 4096]                 --
    │    └─Linear: 2-41                      [1, 4096]                 (16,781,312)
    │    └─ReLU: 2-42                        [1, 4096]                 --
    │    └─Dropout: 2-43                     [1, 4096]                 --
    │    └─Linear: 2-44                      [1, 1000]                 (4,097,000)
    ==========================================================================================
    Total params: 143,667,240
    Trainable params: 0
    Non-trainable params: 143,667,240
    Total mult-adds (G): 19.65
    ==========================================================================================
    Input size (MB): 0.60
    Forward/backward pass size (MB): 118.89
    Params size (MB): 574.67
    Estimated Total Size (MB): 694.16
    ==========================================================================================




```python
print(model) # instance변수(attribute)들을 출력
```

    VGG(
      (features): Sequential(
        (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (1): ReLU(inplace=True)
        (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (3): ReLU(inplace=True)
        (4): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (6): ReLU(inplace=True)
        (7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (8): ReLU(inplace=True)
        (9): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (11): ReLU(inplace=True)
        (12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (13): ReLU(inplace=True)
        (14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (15): ReLU(inplace=True)
        (16): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (17): ReLU(inplace=True)
        (18): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (19): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (20): ReLU(inplace=True)
        (21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (22): ReLU(inplace=True)
        (23): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (24): ReLU(inplace=True)
        (25): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (26): ReLU(inplace=True)
        (27): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (29): ReLU(inplace=True)
        (30): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (31): ReLU(inplace=True)
        (32): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (33): ReLU(inplace=True)
        (34): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (35): ReLU(inplace=True)
        (36): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      )
      (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
      (classifier): Sequential(
        (0): Linear(in_features=25088, out_features=4096, bias=True)
        (1): ReLU(inplace=True)
        (2): Dropout(p=0.5, inplace=False)
        (3): Linear(in_features=4096, out_features=4096, bias=True)
        (4): ReLU(inplace=True)
        (5): Dropout(p=0.5, inplace=False)
        (6): Linear(in_features=4096, out_features=1000, bias=True)
      )
    )
    


```python
# def __init__(self):
#     self.features = nn.Sequential()
#     self.avgpool = nn.AdaptiveAvgPool2d()
#     self.classifier = nn.Sequential() ===> classifer
```


```python
512 * 7 * 7
```




    25088




```python
# classifer를 정의해서 model에 추가.
# VGG19의 attribute 변수 classifier에 추론기가 정의설정되 있는 상태
## => 새로운 추론기(개/고양이 분류)를 정의한 뒤 classifier attribute 변수에 할당한다.
model.classifier = nn.Sequential(
    nn.Linear(512 * 7 * 7, 4096),
    nn.ReLU(),
    nn.Dropout(p=0.3),
    nn.Linear(4096, 4096),
    nn.ReLU(),
    nn.Dropout(p=0.3),
    nn.Linear(4096, 2)  # 개/고양이 두개  class 를 분류 => out: 2
)
```


```python
summary(model, (1, 3, 224, 224))
```




    ==========================================================================================
    Layer (type:depth-idx)                   Output Shape              Param #
    ==========================================================================================
    VGG                                      [1, 2]                    --
    ├─Sequential: 1-1                        [1, 512, 7, 7]            --
    │    └─Conv2d: 2-1                       [1, 64, 224, 224]         (1,792)
    │    └─ReLU: 2-2                         [1, 64, 224, 224]         --
    │    └─Conv2d: 2-3                       [1, 64, 224, 224]         (36,928)
    │    └─ReLU: 2-4                         [1, 64, 224, 224]         --
    │    └─MaxPool2d: 2-5                    [1, 64, 112, 112]         --
    │    └─Conv2d: 2-6                       [1, 128, 112, 112]        (73,856)
    │    └─ReLU: 2-7                         [1, 128, 112, 112]        --
    │    └─Conv2d: 2-8                       [1, 128, 112, 112]        (147,584)
    │    └─ReLU: 2-9                         [1, 128, 112, 112]        --
    │    └─MaxPool2d: 2-10                   [1, 128, 56, 56]          --
    │    └─Conv2d: 2-11                      [1, 256, 56, 56]          (295,168)
    │    └─ReLU: 2-12                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-13                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-14                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-15                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-16                        [1, 256, 56, 56]          --
    │    └─Conv2d: 2-17                      [1, 256, 56, 56]          (590,080)
    │    └─ReLU: 2-18                        [1, 256, 56, 56]          --
    │    └─MaxPool2d: 2-19                   [1, 256, 28, 28]          --
    │    └─Conv2d: 2-20                      [1, 512, 28, 28]          (1,180,160)
    │    └─ReLU: 2-21                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-22                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-23                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-24                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-25                        [1, 512, 28, 28]          --
    │    └─Conv2d: 2-26                      [1, 512, 28, 28]          (2,359,808)
    │    └─ReLU: 2-27                        [1, 512, 28, 28]          --
    │    └─MaxPool2d: 2-28                   [1, 512, 14, 14]          --
    │    └─Conv2d: 2-29                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-30                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-31                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-32                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-33                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-34                        [1, 512, 14, 14]          --
    │    └─Conv2d: 2-35                      [1, 512, 14, 14]          (2,359,808)
    │    └─ReLU: 2-36                        [1, 512, 14, 14]          --
    │    └─MaxPool2d: 2-37                   [1, 512, 7, 7]            --
    ├─AdaptiveAvgPool2d: 1-2                 [1, 512, 7, 7]            --
    ├─Sequential: 1-3                        [1, 2]                    --
    │    └─Linear: 2-38                      [1, 4096]                 102,764,544
    │    └─ReLU: 2-39                        [1, 4096]                 --
    │    └─Dropout: 2-40                     [1, 4096]                 --
    │    └─Linear: 2-41                      [1, 4096]                 16,781,312
    │    └─ReLU: 2-42                        [1, 4096]                 --
    │    └─Dropout: 2-43                     [1, 4096]                 --
    │    └─Linear: 2-44                      [1, 2]                    8,194
    ==========================================================================================
    Total params: 139,578,434
    Trainable params: 119,554,050
    Non-trainable params: 20,024,384
    Total mult-adds (G): 19.64
    ==========================================================================================
    Input size (MB): 0.60
    Forward/backward pass size (MB): 118.88
    Params size (MB): 558.31
    Estimated Total Size (MB): 677.80
    ==========================================================================================




```python
## 학습
model_save_path = "/content/drive/MyDrive/pytorch/models/cat_dog_model1.pth"

model = model.to(device)
loss_fn = nn.CrossEntropyLoss()
optimizer = optim.Adam(model.parameters(), lr=LR)
result = fit(train_loader, valid_loader, model, loss_fn, optimizer, N_EPOCH,
             save_best_model=True, device=device, mode="multi",
             save_model_path=model_save_path)
```

    Epoch[1/20] - Train loss: 0.74102 Train Accucracy: 0.44700 || Validation Loss: 0.53387 Validation Accuracy: 0.50800
    ====================================================================================================
    저장: 1 - 이전 : inf, 현재: 0.5338698164559901
    Epoch[2/20] - Train loss: 0.31299 Train Accucracy: 0.77450 || Validation Loss: 0.05345 Validation Accuracy: 0.97900
    ====================================================================================================
    저장: 2 - 이전 : 0.5338698164559901, 현재: 0.053450556471943855
    Epoch[3/20] - Train loss: 0.25657 Train Accucracy: 0.80050 || Validation Loss: 0.05484 Validation Accuracy: 0.98100
    ====================================================================================================
    Epoch[4/20] - Train loss: 0.24577 Train Accucracy: 0.79350 || Validation Loss: 0.06212 Validation Accuracy: 0.97900
    ====================================================================================================
    Epoch[5/20] - Train loss: 0.22494 Train Accucracy: 0.81000 || Validation Loss: 0.05652 Validation Accuracy: 0.97900
    ====================================================================================================
    Epoch[6/20] - Train loss: 0.21597 Train Accucracy: 0.81550 || Validation Loss: 0.06841 Validation Accuracy: 0.97800
    ====================================================================================================
    Epoch[7/20] - Train loss: 0.21125 Train Accucracy: 0.81350 || Validation Loss: 0.08351 Validation Accuracy: 0.97500
    ====================================================================================================
    Epoch[8/20] - Train loss: 0.21726 Train Accucracy: 0.81650 || Validation Loss: 0.06116 Validation Accuracy: 0.97900
    ====================================================================================================
    Epoch[9/20] - Train loss: 0.19188 Train Accucracy: 0.83050 || Validation Loss: 0.06576 Validation Accuracy: 0.97600
    ====================================================================================================
    Epoch[10/20] - Train loss: 0.17815 Train Accucracy: 0.82200 || Validation Loss: 0.05585 Validation Accuracy: 0.97900
    ====================================================================================================
    Epoch[11/20] - Train loss: 0.19429 Train Accucracy: 0.82350 || Validation Loss: 0.05663 Validation Accuracy: 0.98200
    ====================================================================================================
    Epoch[12/20] - Train loss: 0.20662 Train Accucracy: 0.81600 || Validation Loss: 0.06237 Validation Accuracy: 0.97900
    ====================================================================================================
    Early stopping: Epoch - 11
    569.0854172706604 초
    


```python
# 테스트셋으로 검증
from module.train import test_multi_classification

best_model = torch.load(model_save_path)
```


```python
loss, acc = test_multi_classification(test_loader,
                                      best_model, loss_fn, device=device)
```


```python
print(loss, acc)
```

    0.05025501688942313 0.983
    


```python
from PIL import Image
def predict(image_path, model, transform, device):
    img = Image.open(image_path)  # 이미지 읽기 -> PIL.Image
    input_data = transform(img)   # transform을 이용해서 전처리
    input_data = input_data.unsqueeze(dim=0) # (c, h, w) => (1, c, h, w)
    input_data = input_data.to(device)

    model.eval()
    with torch.no_grad():
        pred = model(input_data)     # 모델을 이용해서 추론
        pred_prob = nn.Softmax(dim=-1)(pred)  # 추론결과를 확률값으로 계산.
        max_v = pred_prob.max(dim=-1)
        label = max_v.indices.item()    # label 조회
        label_name = "cat" if label==0 else "dog" # label name
        prob = max_v.values.item()  # 확률값 조회
    return label, label_name, prob
```


```python
predict("dog.jpg", best_model, test_transform, device)
predict("cat.jpg", best_model, test_transform, device)
predict("dog2.jfif", best_model, test_transform, device)
```




    (1, 'dog', 0.9989990592002869)



## Backbone(Conv base)의 일부를 fine tuning
- Top (출력)쪽의 Layer들을 tuning.


```python
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader

from torchvision import models, datasets, transforms

!pip install torchinfo
from torchinfo import summary

from module.train import fit
from module.utils import plot_fit_result

import os
from zipfile import ZipFile

!pip install gdown -U
import gdown

device = 'cuda' if torch.cuda.is_available() else "cpu"
device
```

    Requirement already satisfied: torchinfo in /usr/local/lib/python3.10/dist-packages (1.8.0)
    Requirement already satisfied: gdown in /usr/local/lib/python3.10/dist-packages (4.6.6)
    Collecting gdown
      Downloading gdown-4.7.1-py3-none-any.whl (15 kB)
    Requirement already satisfied: filelock in /usr/local/lib/python3.10/dist-packages (from gdown) (3.12.4)
    Requirement already satisfied: requests[socks] in /usr/local/lib/python3.10/dist-packages (from gdown) (2.31.0)
    Requirement already satisfied: six in /usr/local/lib/python3.10/dist-packages (from gdown) (1.16.0)
    Requirement already satisfied: tqdm in /usr/local/lib/python3.10/dist-packages (from gdown) (4.66.1)
    Requirement already satisfied: beautifulsoup4 in /usr/local/lib/python3.10/dist-packages (from gdown) (4.11.2)
    Requirement already satisfied: soupsieve>1.2 in /usr/local/lib/python3.10/dist-packages (from beautifulsoup4->gdown) (2.5)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (3.3.1)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (3.4)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (2.0.7)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (2023.7.22)
    Requirement already satisfied: PySocks!=1.5.7,>=1.5.6 in /usr/local/lib/python3.10/dist-packages (from requests[socks]->gdown) (1.7.1)
    Installing collected packages: gdown
      Attempting uninstall: gdown
        Found existing installation: gdown 4.6.6
        Uninstalling gdown-4.6.6:
          Successfully uninstalled gdown-4.6.6
    Successfully installed gdown-4.7.1
    




    'cuda'




```python
os.makedirs("data", exist_ok=True)
os.makedirs("datasets", exist_ok=True)
```


```python
url = 'https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV'
path = r'data/cats_and_dogs_small.zip'
gdown.download(url, path, quiet=False)
```

    Downloading...
    From (uriginal): https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV
    From (redirected): https://drive.google.com/uc?id=1YIxDL0XJhhAMdScdRUfDgccAqyCw5-ZV&confirm=t&uuid=c64fd62b-f653-4d9b-b2c1-e0186e95fcc7
    To: /content/data/cats_and_dogs_small.zip
    100%|██████████| 90.8M/90.8M [00:00<00:00, 252MB/s]
    




    'data/cats_and_dogs_small.zip'




```python
# 압축 풀기 - zip
image_path = os.path.join("datasets", "cats_and_dogs_small") # 압축 풀 경로
with ZipFile(path) as zfile:  # ZipFile(압축파일경로)
    zfile.extractall(image_path)
    # extractall(압축풀 디렉토리 경로) #경로생략-현재 DIR에 푼다.
```


```python
#### transform 정의
###### Trainset: Image augmentation + Resize + ToTensor
train_transform = transforms.Compose([
    transforms.Resize((224, 224)), # Image Net 학습 모델=>input size: 224, 224
    # 상수 -> 1-상수 ~ 1+상수 범위 에서 변환.
    transforms.ColorJitter(brightness=0.3, contrast=0.3, saturation=0.5, hue=0.15),
    transforms.RandomHorizontalFlip(), #p=0.5(default)
    transforms.RandomVerticalFlip(),
    transforms.RandomRotation(degrees=90), # -90 ~ +90 범위에서 회전.
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
    # 채널별: (평균), (표준편차)  # (각픽셀값-평균)/표준편차
])
```


```python
##### validation/test set ==> Image augmentation은 정의하지 않는다.
##########  Resize, ToTensor, Normalize
test_transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize((0.485, 0.456, 0.406), (0.229, 0.224, 0.225))
])
```


```python
##### 하이퍼파라미터 정의
BATCH_SIZE = 256
N_EPOCH = 1
LR = 0.001
```


```python
train_set = datasets.ImageFolder(os.path.join(image_path, "train"),
                                 transform=train_transform)
valid_set = datasets.ImageFolder(os.path.join(image_path, "validation"),
                                 transform=test_transform)
test_set = datasets.ImageFolder(os.path.join(image_path, "test"),
                                transform=test_transform)
```


```python
###### DataLoader
train_loader = DataLoader(train_set, batch_size=BATCH_SIZE, drop_last=True,
                          shuffle=True,
                          num_workers=os.cpu_count())
                          # Data를 load 할때 병렬처리하겠다. => 한번에 몇개씩 동시에 loading할지.
                          # os.cpu_count() -> cpu 프로세서 개수
valid_loader = DataLoader(valid_set, batch_size=BATCH_SIZE, num_workers=os.cpu_count())
test_loader = DataLoader(test_set, batch_size=BATCH_SIZE, num_workers=os.cpu_count())
```


```python
### 모델 정의
#### VGG19 다운
model2 = models.vgg19(weights=models.VGG19_Weights.DEFAULT)
```


```python
model2
```




    VGG(
      (features): Sequential(
        (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (1): ReLU(inplace=True)
        (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (3): ReLU(inplace=True)
        (4): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (6): ReLU(inplace=True)
        (7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (8): ReLU(inplace=True)
        (9): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (11): ReLU(inplace=True)
        (12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (13): ReLU(inplace=True)
        (14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (15): ReLU(inplace=True)
        (16): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (17): ReLU(inplace=True)
        (18): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (19): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (20): ReLU(inplace=True)
        (21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (22): ReLU(inplace=True)
        (23): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (24): ReLU(inplace=True)
        (25): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (26): ReLU(inplace=True)
        (27): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (29): ReLU(inplace=True)
        (30): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (31): ReLU(inplace=True)
        (32): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (33): ReLU(inplace=True)
        (34): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (35): ReLU(inplace=True)
        (36): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      )
      (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
      (classifier): Sequential(
        (0): Linear(in_features=25088, out_features=4096, bias=True)
        (1): ReLU(inplace=True)
        (2): Dropout(p=0.5, inplace=False)
        (3): Linear(in_features=4096, out_features=4096, bias=True)
        (4): ReLU(inplace=True)
        (5): Dropout(p=0.5, inplace=False)
        (6): Linear(in_features=4096, out_features=1000, bias=True)
      )
    )




```python
# conv base(backbone) layer를 조회 -> 0 ~ 36 중에서 0 ~ 29: flozon, 30~36: trainable
for idx, layer in enumerate(model2.features):
    # print(idx, type(layer))
    if idx < 33: # frozen
        for param in layer.parameters(): # model.parameters(), layer.parameters() ->구성 weight, bias를 반환하는 generator
            # print(param.shape)
            param.requires_grad = False
```


```python
for i, p in enumerate(model2.parameters()):
    print(i, p.requires_grad)
```

    0 False
    1 False
    2 False
    3 False
    4 False
    5 False
    6 False
    7 False
    8 False
    9 False
    10 False
    11 False
    12 False
    13 False
    14 False
    15 False
    16 False
    17 False
    18 False
    19 False
    20 False
    21 False
    22 False
    23 False
    24 False
    25 False
    26 False
    27 False
    28 False
    29 False
    30 True
    31 True
    32 True
    33 True
    34 True
    35 True
    36 True
    37 True
    


```python
### classifier를 변경.
model2.classifier = nn.Sequential(
    nn.Linear(512 * 7 * 7, 4096),
    nn.ReLU(),
    nn.Dropout(p=0.3),
    nn.Linear(4096, 4096),
    nn.ReLU(),
    nn.Dropout(p=0.3),
    nn.Linear(4096, 2)
)
```


```python
## 학습
model_save_path = "/content/drive/MyDrive/pytorch/models/cat_dog_model2.pth"

model2 = model2.to(device)
loss_fn = nn.CrossEntropyLoss()
optimizer = optim.Adam(model2.parameters(), lr=LR)
result = fit(train_loader, valid_loader, model2, loss_fn, optimizer, 20,
             save_best_model=True, device=device, mode="multi",
             save_model_path=model_save_path)
```

    Epoch[1/20] - Train loss: 0.69335 Train Accucracy: 0.44850 || Validation Loss: 0.69287 Validation Accuracy: 0.50000
    ====================================================================================================
    저장: 1 - 이전 : inf, 현재: 0.6928744614124298
    Epoch[2/20] - Train loss: 0.69315 Train Accucracy: 0.44350 || Validation Loss: 0.69316 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[3/20] - Train loss: 0.69317 Train Accucracy: 0.44700 || Validation Loss: 0.69304 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[4/20] - Train loss: 0.69314 Train Accucracy: 0.45500 || Validation Loss: 0.69313 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[5/20] - Train loss: 0.69317 Train Accucracy: 0.44800 || Validation Loss: 0.69335 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[6/20] - Train loss: 0.69342 Train Accucracy: 0.44300 || Validation Loss: 0.69359 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[7/20] - Train loss: 0.69353 Train Accucracy: 0.44250 || Validation Loss: 0.69373 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[8/20] - Train loss: 0.69325 Train Accucracy: 0.44550 || Validation Loss: 0.69343 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[9/20] - Train loss: 0.69315 Train Accucracy: 0.44850 || Validation Loss: 0.69326 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[10/20] - Train loss: 0.69315 Train Accucracy: 0.44450 || Validation Loss: 0.69315 Validation Accuracy: 0.50000
    ====================================================================================================
    Epoch[11/20] - Train loss: 0.69315 Train Accucracy: 0.44800 || Validation Loss: 0.69311 Validation Accuracy: 0.50000
    ====================================================================================================
    Early stopping: Epoch - 10
    528.5934660434723 초
    


```python
## 학습
model_save_path = "/content/drive/MyDrive/pytorch/models/cat_dog_model3.pth"

model2 = model2.to(device)
loss_fn = nn.CrossEntropyLoss()
optimizer = optim.Adam(model2.parameters(), lr=LR)
result = fit(train_loader, valid_loader, model2, loss_fn, optimizer, 20,
             save_best_model=True, device=device, mode="multi",
             save_model_path=model_save_path)
```

    Epoch[1/20] - Train loss: 0.63076 Train Accucracy: 0.70450 || Validation Loss: 0.46847 Validation Accuracy: 0.94800
    ====================================================================================================
    저장: 1 - 이전 : inf, 현재: 0.46847304701805115
    Epoch[2/20] - Train loss: 0.52784 Train Accucracy: 0.67200 || Validation Loss: 0.36505 Validation Accuracy: 0.79900
    ====================================================================================================
    저장: 2 - 이전 : 0.46847304701805115, 현재: 0.3650495260953903
    Epoch[3/20] - Train loss: 0.46833 Train Accucracy: 0.70050 || Validation Loss: 0.20519 Validation Accuracy: 0.94300
    ====================================================================================================
    저장: 3 - 이전 : 0.3650495260953903, 현재: 0.20518812304362655
    Epoch[4/20] - Train loss: 0.42076 Train Accucracy: 0.74350 || Validation Loss: 0.06447 Validation Accuracy: 0.97800
    ====================================================================================================
    저장: 4 - 이전 : 0.20518812304362655, 현재: 0.06446871533989906
    Epoch[5/20] - Train loss: 0.35108 Train Accucracy: 0.75800 || Validation Loss: 0.05881 Validation Accuracy: 0.97700
    ====================================================================================================
    저장: 5 - 이전 : 0.06446871533989906, 현재: 0.058813430834561586
    Epoch[6/20] - Train loss: 0.32142 Train Accucracy: 0.77650 || Validation Loss: 0.05266 Validation Accuracy: 0.97600
    ====================================================================================================
    저장: 6 - 이전 : 0.058813430834561586, 현재: 0.0526590421795845
    Epoch[7/20] - Train loss: 0.30016 Train Accucracy: 0.78350 || Validation Loss: 0.05139 Validation Accuracy: 0.98300
    ====================================================================================================
    저장: 7 - 이전 : 0.0526590421795845, 현재: 0.05138552654534578
    Epoch[8/20] - Train loss: 0.27240 Train Accucracy: 0.79350 || Validation Loss: 0.05308 Validation Accuracy: 0.98200
    ====================================================================================================
    Epoch[9/20] - Train loss: 0.25895 Train Accucracy: 0.79950 || Validation Loss: 0.04213 Validation Accuracy: 0.97800
    ====================================================================================================
    저장: 9 - 이전 : 0.05138552654534578, 현재: 0.0421281480230391
    Epoch[10/20] - Train loss: 0.24931 Train Accucracy: 0.79850 || Validation Loss: 0.03860 Validation Accuracy: 0.98300
    ====================================================================================================
    저장: 10 - 이전 : 0.0421281480230391, 현재: 0.03859595442190766
    Epoch[11/20] - Train loss: 0.23073 Train Accucracy: 0.80650 || Validation Loss: 0.04913 Validation Accuracy: 0.98100
    ====================================================================================================
    Epoch[12/20] - Train loss: 0.22912 Train Accucracy: 0.80950 || Validation Loss: 0.05283 Validation Accuracy: 0.97700
    ====================================================================================================
    Epoch[13/20] - Train loss: 0.20540 Train Accucracy: 0.81400 || Validation Loss: 0.04501 Validation Accuracy: 0.98000
    ====================================================================================================
    Epoch[14/20] - Train loss: 0.21037 Train Accucracy: 0.81600 || Validation Loss: 0.04255 Validation Accuracy: 0.98100
    ====================================================================================================
    Epoch[15/20] - Train loss: 0.20662 Train Accucracy: 0.81250 || Validation Loss: 0.04868 Validation Accuracy: 0.98000
    ====================================================================================================
    Epoch[16/20] - Train loss: 0.19411 Train Accucracy: 0.82250 || Validation Loss: 0.04451 Validation Accuracy: 0.98000
    ====================================================================================================
    Epoch[17/20] - Train loss: 0.19587 Train Accucracy: 0.82400 || Validation Loss: 0.04834 Validation Accuracy: 0.98100
    ====================================================================================================
    Epoch[18/20] - Train loss: 0.17183 Train Accucracy: 0.83100 || Validation Loss: 0.04874 Validation Accuracy: 0.98100
    ====================================================================================================
    Epoch[19/20] - Train loss: 0.18885 Train Accucracy: 0.83100 || Validation Loss: 0.04351 Validation Accuracy: 0.98400
    ====================================================================================================
    Epoch[20/20] - Train loss: 0.18649 Train Accucracy: 0.82750 || Validation Loss: 0.04235 Validation Accuracy: 0.98600
    ====================================================================================================
    Early stopping: Epoch - 19
    971.4693756103516 초
    
