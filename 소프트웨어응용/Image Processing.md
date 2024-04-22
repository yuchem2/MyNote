## Applications
이미지 분류, 객체 인식, 객체 segmentation, OCR, Super Resolution, 이미지 생성, 등 다양한 분야에서 활용될 수 있다. 
## Image Classification

![|400](Pasted%20image%2020240421171333.png)
### VGGNet
VGG 모델의 핵심은 네트워크의 깊이를 깊게 만드는 것이 성능에 어떤 영향을 미치는 지를 확인하고자 한 것이다. 깊이의 영향만을 최대한 확인하고자 conv kernel의 사이즈는 가장 작은 $3\times3$으로 고정했다.

![|400](Pasted%20image%2020240421172012.png)

VGG는 ILSVRC 2014에서 우승한 모델로, 딥러닝 분야에서의 큰 기여를 한 모델이다. 핵심 기여로는 깊이에 따른 성능 비교를 해 깊은 네트워크가 이미지 분류 성능 향상에 큰 영향을 미침을 확인했고, small conv kernel을 사용함으로써 더 작은 파라미터를 사용하면서도 더 나은 성능을 보일 수 잇다는 것을 보였다. 

또한, 대규모 데이터셋에서 사전학습된 가중치를 사용해 다른 작업에 대한 전이 학습을 수행할 수 있는 모델이 되어, 새로운 작업에 맞게 fine-tuning을 수행함으로써 적은 데이터 셋에서도 높은 성능을 보이는 모델을 구축할 수 있는 발판을 마련했다.
### ResNet
VGG가 보다 깊은 네트워크의 좋은 성능을 보여주었다. 과연 NN이 깊어지는 것만으로 성능이 향상되는 지를 목적으로 가지고 실험을 진행한 모델. 실제로 Layer를 무작정 늘렸을 때 성능이 오히려 떨어지는 문제가 발생하였다. Train 및 Test에서도 성능이 좋지 않았기 때문에 overfitting에 의한 오류가 아님을 확인할 수 있었다. 

문제를 분석한 결과 역전파의 레이어가 깊어질 수록 gradient vanishing이 발생하여 학습이 제대로 이루어지지 않는 것을 확인할 수 있었다. 이를 해결 하기 위해 ResNet은 skip connection을 이용한 residual learning을 통해 layer가 깊어짐에 따른 gradient vanishing 문제를 해결하고자 하였다. 

![|400](Pasted%20image%2020240421182203.png)

ResNet의 주요 기여는 residual connection을 도입해 gradient vanishing 문제를 해결하였다는 것이고, 이를 통해 깊은 네트워크에서의 성능 향상을 이끌어 낸 것이다. 또한, VGG와 동일하게 전이 학습을 수행할 수 있는 모델로 사용되었다.

#### Code
```python
from torch import nn
import torch

# plain model
class Conv(nn.Module):
  def __init__(self, ):
    super().__init__()
    self.convblock = nn.Sequential(
        nn.Conv2d(3, 32, 3, 1, 1),
        nn.BatchNorm2d(32),
        nn.ReLU(),

        nn.Conv2d(32, 32, 3, 1, 1),
        nn.BatchNorm2d(32),
        nn.ReLU(),
  
        nn.Conv2d(32, 32, 3, 1, 1),
        nn.BatchNorm2d(32),
        nn.ReLU()
    )
    self.fclayer = nn.Linear(32*32*32, 10)

  def forward(self, x):
    x = self.convblock(x)
    x = nn.view(x.size(0), -1)
    out = self.fclayer(x)
    return out

# Res Block
class ResBlock(nn.Module):
  def __init__(self, convblock):
    super().__init__()
    self.convblock = convblock

  def forward(self, x):
    return self.convblock(x) + x  # f(x) + x

# Simple ResNet
class ResConv(nn.Module):
  def __init__(self, ):
    super().__init__()
    self.convblock = nn.Sequential(
        nn.Conv2d(3, 32, 3, 1, 1),
        nn.BatchNorm2d(32),
        nn.ReLU(),

        ResBlock(
            nn.Sequential(
                nn.Conv2d(32, 32, 3, 1, 1),
                nn.BatchNorm2d(32),
                nn.ReLU(),
  
                nn.Conv2d(32, 32, 3, 1, 1),
                nn.BatchNorm2d(32),
                nn.ReLU()
            )
        )
    )
    self.fclayer = nn.Linear(32*32*32, 10)

  def forward(self, x):
    print("input: ", x.shape)
    x = self.convblock(x)
    print("after ResConv: ", x.shape)
    x = x.view(x.size(0), -1)
    print("after view: ", x.shape)
    out = self.fclayer(x)
    print("output: ", out.shape)
    return out

miniResNet = ResConv()
input_x = torch.randn(16, 3, 32, 32) # 32x32 크기의 RGB 이미지를 인풋으로 가정, 16 => batch size
output = miniResNet(input_x)
```

## Batch Normalization
Batch 단위로 학습을 하는 경우 다음과 같은 문제들이 발생할 수 있다. 
+ Batch 단위 간의 데이터 분포 차이
+ 연산 전/후 데이터 간 분포 차이

Batch normalization은 학습 과정에서 각 배치 단위 별로 데이터가 다양한 분포를 가지더라도 각 배치 별로 평균과 분산을 이용해 정규화하는 것을 뜻한다. 
## Transfer Learning
특정 분야에서 학습된 신경망의 일부 능력을 유사하거나 전혀 새로운 분야에서 사용되는 신경망의 학습에 이용하는 것을 의미한다. 이렇게 학습을 진행하는 경우 전체 모델에서 사전 학습된 모델 부분을 backbone이라고 한다. 이 backbone은 전체 모델의 성능에 큰 영향을 미친다.

![|400](Pasted%20image%2020240421190728.png)

Fine-tuning(미세조정) 기법을 사용해 weight와 bias가 조절될 범위를 조정하여 전이 학습에서 학습될 파라미터의 범위를 정하는 기법이다. 

![|400](Pasted%20image%2020240421193206.png)
### Code
```python
import glob
import os.path as osp
import random
import numpy as np
import json
from PIL import Image
from tqdm import tqdm
import matplotlib.pyplot as plt
%matplotlib inline

import torch
import torch.nn as nn
import torch.optim as optim
import torch.utils.data as data
import torchvision
from torchvision import models, transforms
import zipfile
  
torch.manual_seed(1234)
np.random.seed(1234)
random.seed(1234)

# 입력 이미지의 전처리 클래스
# 훈련시와 추론시 처리가 다름
class ImageTransform():
    """
    이미지 전처리 클래스. 훈련시, 검증시의 동작이 다르다.
    이미지 크기를 리사이즈하고, 색상을 표준화한다.
    훈련시에는 RandomResizedCrop과 RandomHorizontalFlip으로 데이터를 증대한다.

    Attributes
    ----------
    resize : int
        리사이즈 대상 이미지의 크기.
    mean : (R, G, B)
        각 색상 채널의 평균 값.
    std : (R, G, B)
        각 색상 채널의 표준 편차.
    """

    def __init__(self, resize, mean, std):
        self.data_transform = {
            'train': transforms.Compose([
                # transforms.Resize(224, 224),
                transforms.RandomResizedCrop(resize, scale=(0.5, 1.0)),  # 데이터 증대
                transforms.RandomHorizontalFlip(),  # 데이터 증대
                transforms.ToTensor(),  # 텐서로 변환
                transforms.Normalize(mean, std)  # 표준화
            ]),

            'val': transforms.Compose([
                transforms.Resize(resize),  # 리사이즈
                transforms.CenterCrop(resize),  # 이미지 중앙을 resize × resize로 자른다
                transforms.ToTensor(),  # 텐서로 변환
                transforms.Normalize(mean, std)  # 표준화
            ])
        }

    def __call__(self, img, phase='train'):
        """
        Parameters
        ----------
        phase : 'train' or 'val'
            전처리 모드를 지정.
        """
        return self.data_transform[phase](img)

# download data
!wget --no-check-certificate https://download.pytorch.org/tutorial/hymenoptera_data.zip
zip_file = 'hymenoptera_data.zip'
zip_ref = zipfile.ZipFile(zip_file, 'r')
zip_ref.extractall('./data')
zip_ref.close()  
!rm hymenoptera_data.zip

# 개미와 벌의 이미지 파일의 경로 리스트를 작성
def make_datapath_list(phase="train"):
    """
    데이터의 경로를 저장한 리스트를 작성한다.
    Parameters
    ----------
    phase : 'train' or 'val'
        훈련 데이터 또는 검증 데이터를 지정
    Returns
    -------
    path_list : list
        데이터 경로를 저장한 리스트
    """
    rootpath = "./data/hymenoptera_data/"
    target_path = osp.join(rootpath+phase+'/**/*.jpg')
    print(target_path)
    path_list = []
    # glob을 이용하여 하위 디렉토리의 파일 경로를 가져온다
    for path in glob.glob(target_path):
        path_list.append(path)
    return path_list

# 실행
train_list = make_datapath_list(phase="train")
val_list = make_datapath_list(phase="val")


size = 224
mean = (0.485, 0.456, 0.406)
std = (0.229, 0.224, 0.225)

# 개미와 벌의 이미지에 대한 Dataset을 작성한다
class HymenopteraDataset(data.Dataset):
    """
    개미와 벌 이미지의 Dataset 클래스. PyTorch의 Dataset 클래스를 상속한다.
    Attributes
    ----------
    file_list : 리스트
        이미지 경로를 저장한 리스트
    transform : object
        전처리 클래스의 인스턴스
    phase : 'train' or 'test'
        학습인지 훈련인지를 설정한다.
    """

    def __init__(self, file_list, transform=None, phase='train'):
        self.file_list = file_list  # 파일 경로 리스트
        self.transform = transform  # 전처리 클래스의 인스턴스
        self.phase = phase  # train or val 지정

    def __len__(self):
        '''이미지 개수를 반환'''
        return len(self.file_list)

    def __getitem__(self, index):
        '''
        전처리한 이미지의 Tensor 형식의 데이터와 라벨을 취득
        '''
        
        # index번째의 이미지를 로드
        img_path = self.file_list[index]
        img = Image.open(img_path)  # [높이][너비][색RGB]

        # 이미지의 전처리를 실시
        img_transformed = self.transform(img, self.phase)  # torch.Size([3, 224, 224])

        # 이미지 라벨을 파일 이름에서 추출
        if self.phase == "train":
            label = img_path[30:34]
        elif self.phase == "val":
            label = img_path[28:32]

        # 라벨을 숫자로 변경
        if label == "ants":
            label = 0
        elif label == "bees":
            label = 1

        return img_transformed, label

# 실행
train_dataset = HymenopteraDataset(file_list=train_list, transform=ImageTransform(size, mean, std), phase='train')
val_dataset = HymenopteraDataset(file_list=val_list, transform=ImageTransform(size, mean, std), phase='val')

# 동작 확인
index = 0
print(train_dataset.__getitem__(index)[0].size())
print(train_dataset.__getitem__(index)[1])

# 배치 크기 지정
batch_size = 32  
# DataLoader 작성
train_dataloader = torch.utils.data.DataLoader(train_dataset, batch_size=batch_size, shuffle=True)
val_dataloader = torch.utils.data.DataLoader(val_dataset, batch_size=batch_size, shuffle=False)

# 사전형 변수에 정리
dataloaders_dict = {"train": train_dataloader, "val": val_dataloader}

# 동작 확인
batch_iterator = iter(dataloaders_dict["train"])  # 반복자(iterator)로 변환
inputs, labels = next(batch_iterator)  # 첫번째 요소를 추출
print(inputs.size())
print(labels)

def imshow(inp, title=None):
    """Imshow for Tensor."""
    inp = inp.numpy().transpose((1, 2, 0))
    mean = np.array([0.485, 0.456, 0.406])
    std = np.array([0.229, 0.224, 0.225])
    inp = std * inp + mean
    inp = np.clip(inp, 0, 1)
    plt.imshow(inp)

    if title is not None:
        plt.title(title)
    plt.pause(0.001)  # pause a bit so that plots are updated

# Get a batch of training data
inputs, classes = next(iter(train_dataloader))
# Make a grid from batch
out = torchvision.utils.make_grid(inputs)
class_names = ('ants', 'bees')
imshow(out, title=[class_names[x] for x in classes])


# 학습된 VGG-16 모델을 로드
# VGG-16 모델의 인스턴스 생성
use_pretrained = True  # 학습된 파라미터를 사용
net = models.vgg16(pretrained=use_pretrained)

# VGG16의 마지막 출력층의 출력 유닛을 개미와 벌의 2개로 바꾼다
net.classifier[6] = nn.Linear(in_features=4096, out_features=2)
# 훈련 모드로 설정
net.train()
print('네트워크 설정 완료: 학습된 가중치를 읽어들여 훈련 모드로 설정했습니다.')
print(net)

# 전이학습에서 학습시킬 파라미터를 params_to_update 변수에 저장한다
params_to_update = []
# 학습시킬 파라미터 이름
update_param_names = ["classifier.6.weight", "classifier.6.bias"]
# 학습시킬 파라미터 이외에는 경사를 계산하지 않고, 변하지 않게 설정
for name, param in net.named_parameters():
    if name in update_param_names:
        param.requires_grad = True
        params_to_update.append(param)
        print(name)
    else:
        param.requires_grad = False

# params_to_update의 내용을 확인
print("-----------")
print(params_to_update)

# 손실 함수 설정
criterion = nn.CrossEntropyLoss()
# 최적화 기법 설정
optimizer = optim.SGD(params=params_to_update, lr=0.001, momentum=0.9)

# 모델을 학습시키는 함수를 작성
def train_model(net, dataloaders_dict, criterion, optimizer, num_epochs):
  
    # epoch 루프
    for epoch in range(num_epochs):
        print('Epoch {}/{}'.format(epoch+1, num_epochs))
        print('-------------')

        # epoch별 학습 및 검증 루프
        for phase in ['train', 'val']:
            if phase == 'train':
                net.train()  # 모델을 훈련 모드로
            else:
                net.eval()   # 모델을 검증 모드로

            epoch_loss = 0.0  # epoch 손실의 합
            epoch_corrects = 0  # epoch 정답의 수

            # 미 학습시 검증 성능을 확인하기 위해 epoch=0의 훈련 생략
            if (epoch == 0) and (phase == 'train'):
                continue

            # 데이터 로더로 미니 배치를 꺼내는 루프
            for inputs, labels in tqdm(dataloaders_dict[phase]):
                # optimizer를 초기화
                optimizer.zero_grad()
                # 순전파(forward) 계산
                with torch.set_grad_enabled(phase == 'train'):
                    outputs = net(inputs)
                    loss = criterion(outputs, labels)  # 손실을 계산
                    _, preds = torch.max(outputs, 1)  # 라벨을 예측
                    # 훈련시에는 오차 역전파
                    if phase == 'train':
                        loss.backward()
                        optimizer.step()
                    # 반복 결과 계산
                    # loss 합계 갱신
                    epoch_loss += loss.item() * inputs.size(0)
                    # 정답 수의 합계 갱신
                    epoch_corrects += torch.sum(preds == labels.data)

            # epoch 당 loss와 정답률 표시
            epoch_loss = epoch_loss / len(dataloaders_dict[phase].dataset)
            epoch_acc = epoch_corrects.double() / len(dataloaders_dict[phase].dataset)
            print('{} Loss: {:.4f} Acc: {:.4f}'.format(phase, epoch_loss, epoch_acc))

# 학습 및 검증 실시
num_epochs=3
train_model(net, dataloaders_dict, criterion, optimizer, num_epochs=num_epochs)
```
## Data Augmentation
데이터 셋을 여러 가지 방법으로 변형해 실질적인 학습 데이터 셋의 규모를 키우는 방법이다. 하지만 원본 데이터에서 큰 변경을 하지 않기 때문에 한계가 존재한다. 색 변경, 돌리기, 자르기, 등 다양한 기법이 존재한다.
+ Classification에서 효율적인 방법으로, 두 원본 데이터의 일부를 crop한 후 합쳐서 그 비율만큼의 라벨링한 데이터를 생성하는 방법
+ Object Detection에서 효율적인 방법으로 여러 개의 이미지를 특정 비율로 자른 다음에 하나로 합쳐 한번에 다양한 물체가 보일 수 있도록 하는 방법(Mosaic) 기법

## Confusion Metric
Accuracy도 훌륭한 성능 판단의 기준이 될 수 있지만, 특정 상황에서 비효율적일 수 있다. ex) 발병확률이 0.001%인 환자의 발병 여부, 제품 불량 판단 등

즉, 특히 이진 분류에서 한쪽 비율이 너무 높아 희박한 가능성으로 발생할 상황을 제대로 분류하지 못하는 경우는 accuracy로는 모델의 성능을 보장하기 힘들다. 이를 위해 이런 경우에는 confusion metric을 이용해 성능을 판단한다.

![|400](Pasted%20image%2020240421194147.png)
+ $\text{Accuray} = \cfrac{TP+TN}{TP+TN+FP+FN}$
+ $\text{Precision}=\cfrac{TP}{TP+FP}$: 정밀도, positive로 예측한 경우 실제로 positive인 비율. 예측 값이 얼마나 정확한가. negative를 positive로 판단하지 않는 것이 중요할 때 사용
+ $\text{Recall}=\cfrac{TP}{TP+FN}$: 재현율, positive로 예측한 경우 중 올바르게 postivie를 맞춘 것의 비율. 실제 정답을 얼마나 맞췄는가. positive를 negative로 판단하지 않는 것이 중요할 때 사용
+ $\text{F1-score}=2 \times \cfrac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$

Precision과 Recall은 Trade-off 관계를 갖고 있는데, 대부분의 경우 두 요소 모두 중요하다. F1-score는 precision과 recall의 조화평균으로 두 값을 모두 반영할 수 있는 수치이다.
## Object Detection
이미지나 비디오에서 특정 객체를 식별하고 위치를 파악하는 기술. 주로 Faster R-CNN, YOLO, SSD 등의 모델을 사용한다. 이 모델들은 이미지를 입력받아 객체가 있는 위치를 추출하고, 각 객체의 클래스를 분류한다.

객체 인식의 결과는 객체의 classification 결과와 box 좌표의 regression으로 이루어진다. Regression은 입력 데이터와 연속적인 출력 값 사이의 함수 관계를 학습하는 기술로, 입력 데이터를 예측할 때, 연속적인 값을 출력해 주로 수치 예측 문제에 사용된다. 입력 데이터와 출력 값이 있는 데이터 셋을 학습하여 새로운 입력 데이터가 들어왔을 때 이에 대한 연속적인 값을 예측할 수 있도록 한다.

![|400](Pasted%20image%2020240421195234.png)

객체 인식 분야는 2-stage분야와 1-stage 분야, 두 분야로 발전을 해왔다. 2-stage의 경우 Region Proposal과 classification을 두 단계로 나눠서 학습하는 것이고, 1-stage는 두 단계를 하나로 만들어 한번에 학습을 진행하는 것이다. 2-stage 기법이 1-stage보다 느리지만 정확하였지만 1-stage가 성능이 높아지면서 상대적으로 느린 2-stage 분야가 사장되었다.
### 2-stage detection
#### R-CNN(Regions with CNN features)
R-CNN은 대표적인 2-stage detection 분야의 모델로, 다음과 같은 단계로 동작한다.
1. Selective search를 통해 RoI(Region Of Interest)를 약 2000개 가량 추출한다. RoI는 객체가 있을 것이라고 유추하는 구역을 말한다.
2. RoI를 동일한 사이즈로 조정한 후 CNN을 통해 학습해 feature를 추출한다.
3. Feature를 사전 학습된 SVM에 넣어 classification을 진행한다. (RoI먀ㅏ다 객체를 유추)
4. Feature를 regression을 통해 bounding box를 예측한다.

![|400](Pasted%20image%2020240421220039.png)
##### Region proposal 
+ Sliding window: 임의 크기의 window를 이동하면서 모든 영역에 대한 탐색을 수행. Window 크기를 개발자가 정하고, 이동 시키며 인식하는 데 많은 시간이 소요되어 매우 비효율적인 기법. 
+ Selective search: 객체 인식이나 검출을 위한 가능한 후보 영역을 알아낼 수 있는 방법을 제공하는 것을 목표로 하는 기법. 객체와 주변 간의 색감, 질감 차이 등을 파악해서 경향성이 같은 것들끼리 Grouping하는 기법. 반복을 통해 group의 개수를 원하는 만큼 조정한다.

#### R-CNN 이후
R-CNN 이후 fast R-CNN, faster R-CNN 등이 연구되었다. 주로 R-CNN의 느린 속도를 개선하기 위해 연구되었다. 하지만 2-stage 계열의 모델은 객체를 검출하는데 많은 시간을 걸려 현실적으로 실시간으로 결과를 도출해야 하는 작업에서 활용되기 어려운 문제가 존재한다. 

![|400](Pasted%20image%2020240421220455.png)
##### Fast R-CNN
Feature를 추출해도 위치가 변하지 않는 특성을 이용해 CNN을 한번만 적용할 수 있도록 수정한 형태이며 이전에는 CNN을 사용하지 않았으나 CNN을 사용해 feature를 추출한다. 
##### Faster R-CNN
RoI 또한 CNN 특징 맵에서 추출하고, selective search가 아닌 다른 DL을 통해 RoI를 추출한다.
### 1-staged detection
#### YOLO
대표적인 1-staged detection 모델이다. End-to-End 방식으로 객체 감지를 수행할 수 있다. 이미지를 그리드로 나눈 후 각 그리드에서 물체가 있는지 여부와 해당 물체가 어떤 클래스인지를 동시에 예측하는 방식을 사용해속도와 정확도를 동시에 향상시켰다.

![|400](Pasted%20image%2020240421233252.png)

YOLO-1의 경우 grid를 $7\times 7$크기로 나눈 뒤 각 cell마다 2개의 box를 유추하고, cell마다 classification을 수행한다. box를 유추한 결과는 box regression vector를 만들고, 각 cell은 classification vector를 만들어 $7\times 7 \times 30$ 크기의 결과 텐서를 만든다. 
+ Bbox : (x, y, w, h, c(cofidence), x, y, w, h, c)
+ classification: (c1, c2, c3, ...., c18, c19, c20)
+ $\text{cofidence} =Pr(Object) \times IOU^{truth}_{pred}=Pr(Object) \times \cfrac{\text{Area of Overlap}}{\text{Area of Union}}$ 

![|300](Pasted%20image%2020240421234011.png)
![|400](Pasted%20image%2020240421233912.png)

+ inference: Bbox 별로 confidence(객체일 확률)와 각 class 별 확률을 곱해준다. 이렇게 되면 실제 탐지되어야 할 객체들의 정보와 그 객체들의 정보가 종합된다.

![|400](Pasted%20image%2020240421234231.png)
+ NMS(Non-maximum suppression): object detection에서 중복된 예측 박스를 제거하는 기술. 
	+ 이를 위해 먼저 박스들을 confidence score에 따라 내림차순으로 정렬한다. 
	+ 그 다음 가장 높은 cofidence score를 가진 박스와 다른 박스들 간의 IOU(Intersection over Union)를 계산하여, 일정 임계값 이상인 박스들을 제거한다.
	+ 이러한 과정을 반복하여 최종적으로 겹치지 않는 예측 박스들만 남겨진다.
	+ 결국 중복된 예측 박스를 제거하고 더욱 정확한 object detection 결과를 얻을 수 있다.

![|400](Pasted%20image%2020240421234842.png)
+ 문제점
	+ Localization 오류: YOLO v1은 하나의 그리드 셀에 여러 개의 물체가 있을 때 이를 정확히 구분하지 못하는 문제가 있다. 이로 인해 object의 취치 정보를 부정확하게 예측할 수 있다.
	+ 작은 물체 검출 미흡: YOLO v1은 작은 물체를 검출하는데 어려움을 겪는다. 작은 물체는 이미지에서 차지하는 비율이 작기 때문에 그리드 셀 안에 들어가는 경우가 많다. 이러한 작은 물체는 적은 수의 픽셀로 표현되기 때문에 세부 정보가 부족하여 검출이 어려워진다.
	+ 새로운 바운딩박스 예측 미흡: 바운딩박스 형태가 data를 통해 학습되므로 새로운 형태의 바운딩박스의 경우 정확히 예측하지 못한다.
### 성능 평가
Object detection에서는 정량적 평가를 mAP(mean Average Precision)를 통해 실시한다. 크게 confusion matrix와 IOU를 이용해 성능 평가를 한다. 만약 IOU의 threshold(임계값)을 0.5라고 하면 아래와 같이 평가된다.

![|400](Pasted%20image%2020240421235726.png)
#### 예시 결과
![|400](Pasted%20image%2020240422194942.png)

![|400](Pasted%20image%2020240422194953.png)

![|400](Pasted%20image%2020240422195036.png)

![|400](Pasted%20image%2020240421235923.png)
