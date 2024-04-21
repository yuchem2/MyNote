## Neural Network
DL은 생물의 신경망 원리를 모방한 심층 신경망 이론을 기반으로 고안된 ML 기법의 일종으로, 그 이론에서 핵심적인 부분은 neuron과 synapse이다. 

각 neuron은 weight을 가진 연결선을 통해 다른 neuron과 연결되어 계산되며, 이 계산이 차례대로 이루어져 출력이 나오게 된다. 이 과정을 반복해 출력하는 결과 값이 점진적으로 원하는 기댓값에 수렴하도록 학습하는 과정을 '신경망 학습'이라고 한다. 이 과정은 다음과 같은 과정의 반복으로 이루어진다. 
1. 전방 계산(Forward Propagation)
2. 오차 계산(Loss calculation)
3. 후방 가중치 계산(Back Propagation)

![Perceptrons](Deep%20Learning%20and%20Neural%20Networks.md#Perceptrons)

![Optimization Algorithms](Deep%20Learning%20and%20Neural%20Networks.md#Optimization%20Algorithms)

![Backpropagation](Deep%20Learning%20and%20Neural%20Networks.md#Backpropagation)
## Basic of Learning
### Dataset
데이터 셋은 training, validation, testing으로 구성
+ Train set: 모델을 학습하는데 사용되는 실질적인 데이터
+ Valid set: train set으로 학습된 모델의 성능을 측정하기 위해 사용되는 데이터
+ Test set: 모든 학습 및 검증이 끝난 모델을 마지막에 딱 한번 예상되는 성능을 측정하기 위해 사용되는 데이터
### Batch size
각 데이터 하나에 대하여 계산된 gradient를 모두 일일이 역전파시켜서 모델을 학습할 수도 있지만, 연산량이 크고 학습이 불안정할 가능성이 존재한다. 이를 해결하기 위해 여러 데이터를 묶어 gradient를 계산해 평균을 내 역전파를 시행하는 방법이 존재한다. 이때 묶는 데이터의 개수를 batch size라고 한다. 

일반적으로 batch size는 CPU와 GPU의 메모리가 2진수 단위기 때문에 2의 제곱수로 선정해 연산의 효율성을 높인다. 추가적으로 Nvidia GPU의 경우 기본 단위가 32bit이기 때문에 32의 약수나 배수가 효율이 좋다.
### Epoch, iteration
+ Epoch: 한번 train set을 모두 학습한 단위
+ Iteration: 하나의 mini-batch 학습 단위
### Hyper-parameter
+ Parameter: 모델 내부에서 결정되는 변수(weight, bias, ...)로 개발자가 조정하지 않고, 학습을 통해 조정되는 변수
+ Hyper-parameter: 모델을 구성할 때 개발자가 직접 조정하는 값(learning rate, epoch, iteration, ...)

## Example
### VGG
```python
import numpy as np
import json
from PIL import Image
import matplotlib.pyplot as plt
%matplotlib inline
  
import torch
import torchvision
from torchvision import models, transforms
from google.colab import drive

print("PyTorch Version: ",torch.__version__)
print("Torchvision Version: ",torchvision.__version__)

drive.mount('/content/drive')

# 학습된 VGG-16 모델을 읽기
# 처음 실행할 때에는 학습된 파라미터를 다운로드하므로 실행에 시간이 걸립니다
# VGG-16 모델의 인스턴스를 생성
use_pretrained = True  # 학습된 파라미터를 사용
net = models.vgg16(pretrained=use_pretrained)
net.eval()  # 추론 모드(평가 모드)로 설정

# 모델의 네트워크 구성을 출력
print(net)

# 입력 화상의 전처리 클래스
class BaseTransform():
    """
    화상의 크기를 리사이즈하고 색상을 표준화한다.
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
        self.base_transform = transforms.Compose([
            transforms.Resize(resize),  # 짧은 변의 길이가 resize의 크기가 된다
            transforms.CenterCrop(resize),  # 이미지 중앙을 resize × resize으로 자르기
            transforms.ToTensor(),  # Torch 텐서로 변환
            transforms.Normalize(mean, std)  # 색상 정보의 표준화
        ])

    def __call__(self, img):
        return self.base_transform(img)

# 이미지 전처리 동작 확인

# 1. 화상 읽기
image_file_path = '/content/drive/MyDrive/소프트웨어응용/1차시실습/data/goldenretriever-3724972_640.jpg'
img = Image.open(image_file_path)  # [높이][너비][색RGB]

# 2. 원본 화상 표시
plt.imshow(img)
plt.show()

# 3. 화상 전처리 및 처리된 이미지의 표시
resize = 224
mean = (0.485, 0.456, 0.406)
std = (0.229, 0.224, 0.225)
transform = BaseTransform(resize, mean, std)
img_transformed = transform(img)  # torch.Size([3, 224, 224])

# (색상, 높이, 너비)을(높이, 너비, 색상)으로 변환하고 0-1로 값을 제한하여 표시
img_transformed = img_transformed.numpy().transpose((1, 2, 0))
img_transformed = np.clip(img_transformed, 0, 1)
plt.imshow(img_transformed)
plt.show()

# ILSVRC 라벨 정보를 읽어 사전형 변수를 생성합니다
ILSVRC_class_index = json.load(open('/content/drive/MyDrive/소프트웨어응용/1차시실습/data/imagenet_class_index.json', 'r'))


# 출력 결과에서 라벨을 예측하는 후처리 클래스
class ILSVRCPredictor():
    """
    ILSVRC 데이터에 대한 모델의 출력에서 라벨을 구한다.
    Attributes
    ----------
    class_index : dictionary
            클래스 index와 라벨명을 대응시킨 사전형 변수.
    """

    def __init__(self, class_index):
        self.class_index = class_index

    def predict_max(self, out):
        """
        최대 확률의 ILSVRC 라벨명을 가져옵니다.
        Parameters
        ----------
        out : torch.Size([1, 1000])
            Net에서의 출력.
        Returns
        -------
        predicted_label_name : str
            가장 예측 확률이 높은 라벨명
        """

        maxid = np.argmax(out.detach().numpy())
        predicted_label_name = self.class_index[str(maxid)][1]

        return predicted_label_name
        
# ILSVRC 라벨 정보를 읽어 사전형 변수를 생성
ILSVRC_class_index = json.load(open('/content/drive/MyDrive/소프트웨어응용/1차시실습/data/imagenet_class_index.json', 'r'))

# ILSVRCPredictor 인스턴스 생성
predictor = ILSVRCPredictor(ILSVRC_class_index)

# 입력 화상 읽기
image_file_path = '/content/drive/MyDrive/소프트웨어응용/1차시실습/data/흰토끼.png'
img = Image.open(image_file_path)  # [높이][너비][색RGB]

# 전처리 후, 배치 크기의 차원을 추가
transform = BaseTransform(resize, mean, std)  # 전처리 클래스 작성
img_transformed = transform(img)  # torch.Size([3, 224, 224])
inputs = img_transformed.unsqueeze_(0)  # torch.Size([1, 3, 224, 224])

# 모델에 입력하고, 모델 출력을 라벨로 변환
out = net(inputs)  # torch.Size([1, 1000])
result = predictor.predict_max(out)

# 예측 결과를 출력
print("입력 화상의 예측 결과: ", result)
```
### Convolutional Neural Network(CNN)
CV에서 가장 성공한 네트워크 구조로, 이미지를 처리해야하는 다양한 문제들에서 활용되고 있다. Convolutional computation을 이용한 구조로, 연산은 다음과 같다.$$f(i) = z(i) \otimes u = \sum_{x=-\frac{h-1}{2}}^{\frac{h-1}{2}} z(i+x)u(x)$$
![|400](Pasted%20image%2020240420172857.png)

Convolutional computation을 사용하는 가장 큰 이유는 이미지의 특징을 쉽게 추출할 수 있기 때문이다. 학습을 진행할수록 각 kernel들이 입력 데이터를 가장 잘 표현할 수 있도록 학습된다. 여기서 조절 가능한 hyper-parameter 값은 다음과 같다.
+ kernel size: 커널의 크기. 일반적으로 $3\times 3, 5 \times 5, 7\times 7$이 사용된다.
+ padding: 원본 맵에 외각에 추가적인 값을 넣는 것.
+ stride: kernel이 움직이는 정도

연산을 통해 나온 출력 크기는 다음과 같은 방법으로 구할 수 있다. $$(OH, OW) = (\frac{H+2P-FH}{S} + 1, \frac{W+2P-FW}{S} + 1)$$
+ (H, W): 입력 크기
+ (FH, FW): 필터 크기
+ S: stride
+ P: padding
+ (OH, OW): 출력 크기
#### Pooling layer
특징 맵의 weight parameter 개수를 줄이기 위해 사용되는 레이어로, stride값을 통해 특징 맵의 크기를 줄일 수 있다. ($\cfrac{1}{s}$) 또한 연속적인 conv 층이 점점 커지는 window를 보도록 만들어 필터의 공간적인 계층 구조를 형성하는 데 도움을 준다.
#### AlexNet
![|400](Pasted%20image%2020240420173955.png)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F


class CNN(nn.Module):
  def __init__(self):
    super(CNN, self).__init__() # nn.Module의 다양한 변수를 상속받기 위함
    self.conv1 = nn.Conv2d(in_channels=1, out_channels=3, kernel_size=5, stride=1)
    self.conv2 = nn.Conv2d(in_channels=3, out_channels=10, kernel_size=5, stride=1)
    self.fc1 = nn.Linear(10 * 12 * 12, 50)
    self.fc2 = nn.Linear(50, 10)

  def forward(self, x):
    print("연산 전", x.size())
    x = F.relu(self.conv1(x))
    print("conv1 연산 후", x.size())
    x = F.relu(self.conv2(x))
    print("conv2 연산 후",x.size())
    x = x.view(-1, 10 * 12 * 12)
    print("차원 감소 후", x.size())
    x = F.relu(self.fc1(x))
    print("fc1 연산 후", x.size())
    x = self.fc2(x)
    print("fc2 연산 후", x.size())
    return x

cnn = CNN()
output = cnn(torch.randn(16, 1, 20, 20))  # Input Size: (10, 1, 20, 20)

class Mini_Alex(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Sequential(
            nn.Conv2d(384, 384, 3, 1, 1), # in_channels: 384, out_channels: 384, kernel_size=3x3, stride=1, padding=1
            nn.ReLU() # 13 유지
            # 384x13x13
        )
        self.conv2 = nn.Sequential(
            nn.Conv2d(384, 256, 3, 1, 1),
            nn.ReLU(),
            nn.MaxPool2d(3, 2) # 13 -> 6
            # 256x6x6
        )
        self.fc1 = nn.Linear(256 * 6 * 6, 4096)
        self.fc2 = nn.Linear(4096, 4096)
        self.fc3 = nn.Linear(4096, 1000)


    def forward(self, x): # input size = 384x13x13
        print('연산 전:', x.size())
        out = self.conv1(x)
        print('conv1 block 후 결과:', out.size())
        out = self.conv2(out)
        print('conv2 block 후 결과:', out.size())
        out = out.view(out.size(0), -1)
        print('차원 감소 후 결과:', out.size())
        out = F.relu(self.fc1(out))
        print('fc1 연산 후 결과:', out.size())
        out = F.relu(self.fc2(out))
        print('fc2 연산 후 결과:', out.size())
        out = self.fc3(out)
        out = F.log_softmax(out, dim=1)
        print('fc3 연산 후 결과:', out.size())
        return out

alex = Mini_Alex()
output = alex(torch.randn(10, 384, 13, 13))  # Input Size: (10, 384, 13, 13)
print('\n최종 출력 결과:', output.size())
```

### 시계열 데이터
시계열 데이터는 시간 축에 따라 신호가 변하는 동적 데이터를 말한다. mult-layer perceptron, conv 등은 정적 데이터에 집중된 형태이므로, 시계열 데이터에 부적합하다. 시계열 데이터를 정적 데이터로 변환하는 경우 정보 손실이 일어나기 때문에 시계열의 특성을 반영하는 순환 신경망 혹은 LSTM을 활용한다. 고전적으로 ARIMA, SARIMA, Prophet 등이 존재했다. 현재는 다음과 같은 모델이 사용된다.
+ 순환 신경망(RNN; Recurrent Neural Network))
+ LSTM(long short-term memory), GRU(Gated Recurrent Unit)

시계열 데이터는 다음과 같은 특성을 가진다. 요소의 순서가 중요하고, 샘플마다 길이가 다르며, 앞 뒤 데이터에 영향을 받으며(문맥 의존성), 계절성(특정 시간에 따라 추세가 다름)을 가진다. 일반적으로 가변 길이의 3D 행렬로 볼 수 있다. $$x=(a^1a^2...a^t)$$
#### RNN
순환 신경망은 유연한 구조라 여러 문제에 적용이 가능한 데, 대표적으로 미래 예측, 언어 번역, 음성 인식, 생성 모델의 응용 등에 적용되고 있다. 가장 기본적인 형태의 RNN은 아래와 같은 구조이다. 모든 순간이 같은 가중치를 공유하는 특징을 가진다.

![|400](Pasted%20image%2020240420175650.png)
$$h^i = \tau_1(Wh^{i-1}+Ua^i)$$

원하는 결과에 따라 다양한 방식으로 사용될 수 있다. 

![|400](Pasted%20image%2020240420180159.png)
 
 기본 RNN은 vanishing gradient 문제가 발생한다. RNN은 순환 구조를 가지고 있고, 각 단계에서 이전 단계의 출력을 현재 단계의 입력과 함께 사용하는 특징을 가지고 있다. 이로 인해 시간 단위가 커지면 커질수록 gradient가 매우 작아지는 문제가 발생한다. 이를 해결하기 위해 LSTM과 GRU과 같은 변형이 등장하기 시작했다. 
#### LSTM(Long Short Term Memory)
기존 RNN에서 vanishing gradient 문제를 해결하고, 장기 의존성을 모델링 하기 위해 고안된 NN으로, cell state와 control gate라는 구조적 요소를 사용해 정보를 저장하고 조절한다.
+ Cell state: LSTM은 장기적인 정보를 저장하기 위한 cell state를 유지한다. 시간을 거치면서 정보를 보존하거나 제거함으로써 장기적인 의존성을 유지하고, vanishing gradient 문제를 해결한다.
+ Gate
	+ Forget: 어떤 정보를 삭제할 지 결정. 
	+ Input: 어떤 정보를 새로운 cell state에 추가할 지를 결정. 
	+ Output: 현재 cell state를 기반으로 다음 hidden state를 계산

![|400](Pasted%20image%2020240420180317.png)

![|400](Pasted%20image%2020240420181236.png)

```python
import torch
import torch.nn as nn
import torch.nn.functional as F  

class LSTM1(nn.Module):
  def __init__(self):
    super().__init__()
    self.lstm = nn.LSTM(input_size=4, hidden_size=64, bidirectional=False, batch_first=True)
    self.sigmoid = torch.nn.Sigmoid()
    self.fc = nn.Linear(64*1, 1) # (hidden_size * bidirectional(True->2), 원하는 ouput크기)


  def forward(self, x):
    h_0 = torch.zeros(1*1, 32, 64) # (bidirectional(True->2)*num_layer, batch, hidden_size)
    c_0 = torch.zeros(1*1, 32, 64)
    print("연산 전", x.size())
    out, (h, c) = self.lstm(x, (h_0, c_0))
    print("최종층 출력",out.shape)
    o = out[:,-1,:]
    print("마지막 hidden unit 출력", o.shape)
    o = self.sigmoid(self.fc(o))
    return o

lstm = LSTM1()
output = lstm(torch.randn(32, 30, 4))  # Input Size: (32, 30, 4)
print(output)


class LSTM2(nn.Module):
  def __init__(self):
    super().__init__()
    self.lstm = nn.LSTM(input_size=4, hidden_size=64, bidirectional=True, batch_first=True)
    self.sigmoid = torch.nn.Sigmoid()
    self.fc = nn.Linear(64*2, 1)
  
  def forward(self, x):
    h_0 = torch.zeros(2*1, 32, 64)
    c_0 = torch.zeros(2*1, 32, 64)
  
    print("연산 전", x.size())
    out, (h, c) = self.lstm(x, (h_0, c_0))
    print("최종층 출력",out.shape)
    o1 = out[:, -1, :64] # 순방향 마지막 출력
    print("합치기 전 o1", o1.shape)
    o2 = out[:, 0, 64:] # 역방향 마지막 출력
    print("합치기 전 o2", o2.shape)
    o = torch.concat((o1, o2), dim=1)
    print("합친 결과", o.shape)
    o = self.sigmoid(self.fc(o))
    return o


lstm = LSTM2()
output = lstm(torch.randn(32, 30, 4))  # Input Size: (32, 30, 4)
print(output)
```

<img src="https://miro.medium.com/v2/resize:fit:1400/format:webp/1*goJVQs-p9kgLODFNyhl9zA.gif"  height="300"/>
#### GRU(Gated Recurrent Unit)
LSTM을 간소화한 모델로, reset gate와 update gate 두 개만을 활용한다. 게이트 수가 줄어들어 계산 비용이 낮아지고 학습이 빠르다는 장점이 존재한다. 

![|400](Pasted%20image%2020240420180625.png)
## Implement DL
1. 전처리, 후처리, 네트워크 모델의 입출력 확인
2. 데이터 셋 작성
3. 데이터 로더 작성
4. 네트워크 모델 작성
5. 순전파 정의
6. 손실 함수 정의
7. 최적화 기법 설정
8. 학습/검증 실시
9. 테스트 데이터로 추론

### 데이터 로드 및 정규화
```python
import torch
import torchvision
import torchvision.transforms as transforms
import matplotlib.pyplot as plt
import numpy as np

# 데이터 정규화 설정
transform = transforms.Compose(
    [transforms.ToTensor(),
     transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])

# 데이터셋 작성
trainset = torchvision.datasets.CIFAR10(root='./data', train=True,
                                        download=True, transform=transform)
testset = torchvision.datasets.CIFAR10(root='./data', train=False,
                                       download=True, transform=transform)

# 데이터 로더 작성
trainloader = torch.utils.data.DataLoader(trainset, batch_size=4,
                                          shuffle=True)
testloader = torch.utils.data.DataLoader(testset, batch_size=4,
                                         shuffle=False)

classes = ('plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck')

# 이미지 시각화 함수
def imshow(img):
    img = img / 2 + 0.5     # unnormalize
    npimg = img.numpy()
    plt.imshow(np.transpose(npimg, (1, 2, 0)))
    plt.show()

# 학습용 이미지를 무작위로 가져오기
dataiter = iter(trainloader)
images, labels = next(dataiter)

# 이미지 보여주기
imshow(torchvision.utils.make_grid(images))

# 정답(label) 출력
print(' '.join('%5s' % classes[labels[j]] for j in range(4)))
```
+ `ToTensor()`: Pytorch에서 주로 텐서 형태의 데이터로 작업을 수행한다. 이때 입력 데이터가 NumPy 배열 또는 PIL 이미지 형식인 경우 `ToTensor()`를 사용하여 텐서 형식으로 변환 가능하다. 0~255 사이의 수를 0~1 사이의 수로 변환해준다.
+ `Normalize(mean, std)`: 텐서를 가져와 평균 및 표준 편차로 정규화를 진행한다. 정확하게 하기 위해선 학습 데이터로부터 각 픽셀별로 평균을 구하거나 채널 별로 평균을 구해서 계산해야 한다.
	+ 정규화를 진행하는 이유는 입력 데이터의 분포가 일정하지 않으면 모델의 성능이 저하될 수 있기 때문이다. 
### 모델 구축
```python
import torch.nn as nn
import torch.nn.functional as F

class Model(nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.conv1 = nn.Conv2d(3, 6, 5) # in_channels, out_channels, kernel_size
        self.pool = nn.MaxPool2d(2, 2)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(16 * 5 * 5, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = x.view(-1, 16 * 5 * 5)
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        
        return x

model = Model()
```
### Loss function & optimizer 설정
```python
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
```
### 학습 진행
```python
for epoch in range(2):   # 전체 데이터셋을 여러번 반복하기
    training_loss = 0.0
    model.train()
    for i, data in enumerate(trainloader):
        # 입력 받기: 데이터는 [inputs, labels] 형태의 리스트
        inputs, labels = data

        # 파라미터 변화도를 0으로 만들기
        optimizer.zero_grad()

        # 순전파 + 역전파 + 최적화
        outputs = model(inputs)
        loss = criterion(outputs, labels) # 손실을 계산
        loss.backward() # 파라미터들의 에러에 대한 변화도를 계산하여 누적함
        optimizer.step() # loss function를 효율적으로 최소화 할 수 있게 파라미터 수정

        # 통계 출력
        training_loss += loss.item()
        if i % 2000 == 1999:    # 미니배치 2000개 마다 출력
            print('[%d, %5d] loss: %.3f' % (epoch + 1, i + 1, training_loss / 2000))
            training_loss = 0.0

print('Finished Training')
```
### 검증 진행
```python
from tqdm import tqdm

val_loss = 0.0
corrects = 0
model.eval()
for i, data in tqdm(enumerate(testloader)):
    # 입력 받기: 데이터는 [inputs, labels] 형태의 리스트
    inputs, labels = data
    # print(inputs.size)

    # 순전파 + 역전파 + 최적화
    outputs = model(inputs)
    loss = criterion(outputs, labels) # 손실을 계산
    _, preds = torch.max(outputs, 1)  # 라벨을 예측

    # 통계 출력
    val_loss += loss.item()
    corrects += torch.sum(preds == labels.detach())

val_loss = val_loss / (i+1)
val_acc = corrects / len(testloader.dataset)
print('val Loss: {:.4f} val Acc: {:.4f}'.format(val_loss, val_acc))
```

### test 데이터로 추론
```python
from PIL import Image

# test 이미지 불러오기
image_file_path = '/content/drive/MyDrive/소프트웨어응용/3차시실습/goldenretriever-3724972_640.jpg'

img_origin = Image.open(image_file_path)  # [높이][너비][색RGB]
print('원본 크기:', np.shape(img_origin))

# 데이터 전처리
img = img_origin.resize((32, 32))
print('resize 크기:', np.shape(img))
input = transform(img)
print('transform 크기:', input.size())
print()

# inference
model.eval()
output = model(input) # 모델에 입력 넣기
print(output)
_, preds = torch.max(output, 1)  # 라벨을 예측
print("classes: ", classes)
print("preds: ", preds)

# 결과 시각화
plt.imshow(img_origin)
plt.show()

print('예측 class:', classes[preds[0]])

softmax = nn.Softmax(dim=1)
soft_out = softmax(output)
print(soft_out)
print(torch.sum(soft_out).item())
```
