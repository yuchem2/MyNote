## Definition
> Every artificial intelligence system is capable of recognizing the environment it is in, and can take actions based on the recognition. Computer vision is a field of artificial intelligence that deals with the visual perception part.

모든 AI 시스템은 자신이 있는 환경을 인식하고, 그에 따라 행동할 수 있다. **컴퓨터 비전은 시각적 지각 부분을 다루는 AI 분야이다.** 

시각적 지각(Visual perception)은 가장 기본적으로 시력이나 시각적 입력을 통해 패턴이나 객체를 관찰하는 행위를 말한다. 
## Vision System
비전 시스템은 이미지를 인식하는 눈이나 센서와 이러한 이미지를 해석하는 뇌나 기계로 구성된다. 결과로 이미지에서 추출된 데이터에 기반으로 예측된 이미지 구성 요소를 출력한다.

![|400](Pasted%20image%2020240418105922.png)

일반적으로 우리(사람)은 1~2개의 이미지만으로도 물체의 특징이 무엇인지 배우고 인식할 수 있다. 그러나 컴퓨터는 보다 더 복잡한 연산이 필요하고, 배우는데 약 백만개 이상의 이미지가 필요하다.

![|400](Pasted%20image%2020240418105946.png)

과학자들은 사람의 비전 시스템을 영감을 받아 이를 컴퓨터에 적용 시키고자 하였고, 높은 성능을 이끌어 내고 있다. 인간의 비전 시스템을 모방하기 위해서는 두 가지 중요한 구성요소가 필요하다.
### Sensing Device
비전 시스템은 특정 일을 완수하기 위해 설계되었다. 그렇기 때문에 특정 환경의 구성 요소를 잘 캡쳐할 수 있는 기기를 선택하는 것이 중요하다. 다음과 같은 기기들이 사용될 수 있다. 
+ Camera
+ Radar: 전파를 이용해 반사파를 이용해 대상을 탐지하고, 방향, 거리, 속도 등을 파악
+ Lidar: 근적외광, 가시광, 자외선을 이용해 물체를 비추고, 반사광을 통해 거리를 측정
+ X-ray
+ CT scan
### Interpreting devices
컴퓨터 비전의 알고리즘은 모두 해당 기기에서 구현되고, 구성된다. 즉, 비전 시스템에서의 뇌의 역할을 수행한다. 과학자들은 interpreter를 사람의 뇌가 일하는 것에서 영감을 받아 이와 유사한 시스템을 통해 컴퓨터를 학습 시키고자 하였다. 이러한 기술을 ANNs(Artificial Neural Networks)라고 한다. 

![|400](Pasted%20image%2020240418111447.png)

ANNs은 여러개의 artificial neuron으로 구성되는데, 이것은 실제 뉴런을 모델링하여 설계되었다. 

![|400](Pasted%20image%2020240418111344.png)

## Applications of Computer Vision
### Image Classification
이미지 분류는 주어진 이미지를 사전에 정의된 카테고리나 클래스로 분류하는 과정을 말한다. 이미지의 내용, 패턴, 특징 등을 분석해 이미지가 어떤 카테고리에 속하는지를 결정한다. 주로 CNNs(Convolutional Neural Networks) 기술을 기반으로 모델이 만들어 지고 있다.

e.g. CT scan이나 X-ray 이미지에서 암 병변이 존재하는 경우 인식, 도로 표지판을 인식, 구별
### Object Detection and Localization
객체 탐지 및 위치 파악은 이미지 내에 존재하는 여러 객체를 파악하고, 그 위치를 찾아낼 수 있는 기술을 말한다. 이러한 시스템에는 YOLO, SSD, Faster R-CNN 등이 존재한다. 
### Generating art (style transfer)
한 이미지의 스타일을 다른 이미지로 전달하는데 사용됩니다.

![|400](Pasted%20image%2020240418112348.png)
### Creating Images
일반적으로 자연어를 입력 받아 입력에 따라 이미지를 생성해 내는 기술로, 대표적으로 GANs(Generative Adversarial Networks)이 존재한다. 이 모델은 물체, 사람, 장소 등 다양한 요소로 부터 정확한 합성 이미지를 생서하는 딥러닝 모델이다. 

![|400](Pasted%20image%2020240418112636.png)
### Face Recognition
얼굴 인식(FR)은 특정 사람을 식별하고, 구별할 수 있는 기술이다. 해당 기술은 요새 웹에 있는 자신의 사진들을 찾거나, 사진 속 여러 인물들 중 아는 사람을 찾는 등에 사용되고 있다. 

![|400](Pasted%20image%2020240418112851.png)
### Image Recommendation System
주어진 이미지와 유사한 이미지를 찾아내는 기술로, 온라인 쇼핑몰에서 사용자에게 제품을 제안하는데 많이 사용되고 있다.
## Computer Vision Pipeline
기본적인 Machine Learning 기법을 사용하면, 다음과 같은 전체적인 틀을 과정으로 한다.
1. Input data(데이터 수집)
2. Pre-processing: 전처리 과정 + 불필요한 부분 제거 등
3. Feature extraction: 특징 추출
4. ML model: ML 기법을 사용한 모델을 통해 필요한 작업을 수행한다. (분류, 인식 등)
### Input data
CV에서는 입력 데이터로 이미지를 받는다. 이러한 이미지는 원형이 아날로그 데이터이기 때문에 디지털 이미지로 변환이 필요하다. 픽셀로 구성된 행렬로 구성되고, 각 픽셀은 0~255의 값을 가진다. 

흑백 이미지의 경우 2차원 이미지로 구성되며 컬러 이미지의 경우 3차원 이미지로 구성된다. 보통 RGB 채널의 이미지를 사용하기 때문에 흑백이미지x3의 크기를 가진다.
### Image Preprocessing
일반적으로 ML, DL 모두 모델을 학습시키기 전에 입력할 이미지를 전처리하거나 클렌징을 진행한다. 이는 모델이 더 입력 데이터를 확실하게 이해하고, 학습시키기 위함이다. 전처리나 클렌징하는 기법은 다양한 기법이 존재한다.
+ Image resizing
+ Geometric and color transformation
+ Converting color to grayscale
+ etc...
#### Converting color to grayscale
컬러 이미지를 흑백 이미지로 변환하는 기술은 데이터의 크기를 줄이기 위함이다. 정보 자체가 손실되는 것이기 때문에 잘 사용하지는 않지만, 객체 인식 기술에서는 종종 흑백 이미지로 바꾸는 경향이 존재한다.
#### Data augmentation
하나의 원본 이미지를 여러 방법으로 재가공해 데이터를 늘리는 방법
+ Dex-texturized
+ De-colorized
+ Edge enhanced
+ Salient edge map
+ Flip/rotate
### Feature Extraction
특징 추출은 CV pipeline에서 가장 핵심적인 부분이다. 유용한 특징을 추출해야 ML 모델에서 학습이 잘 진행될 수 있다. 

전통적으로 ML 모델을 사용하는 경우 특징 추출은 일반적으로 사람에 의해 결정되었기 때문에 좋은 특징을 선택하는 것이 매우 중요했다. 좋은 특징은 ML 모델이 분류를 잘 할 수 있는 특징을 말하며 즉, 객체의 특성을 확실하게 보여주는 특징을 말한다. 좋은 특징은 다음과 같은 특성을 가진다.
+ Identifiable: 그 객체를 식별 가능하게 하는 특징
+ Easily tracked and compared: 쉽게 추적 가능하고 다른 객체와 비교할 수 있는 특징
+ Consistent across different scales, lighting conditions, and viewing angles: 다양한 척도, 조명 조건 및 관찰 각도에서 일관된 특징
+ Still visible in noisy images or when only part of an object is visible: 잡음이 있는 이미 혹은 객체의 일부만 보여도 항상 보이는 특징

DL 모델의 경우 모델에서 직접 특징을 추출하기 때문에 자동으로 좋은 특징이 추출될 수 있다. 

특징을 사용하는 이유는 입력으로 받는 이미지는 분류 등에 필요하지 않는 너무 많은 정보를 가지고 있기 때문이다. 그래서 특징 추출 과정에 분류 등에 필요한 정보들만 추출해 이미지를 단순화하는 작업이다. 이를 통해 불필요한 부분이 삭제되면서 연산량도 감소하고, 더 좋은 결과를 얻어낼 수 있다.
### Model
이렇게 추출된 특성을 통해 이미지를 분류하거나, 객체를 탐지하거나 등의 작업을 모델에 계속해서 학습시켜 오류를 줄이는 과정을 반복해 좋은 결과를 도출하게 끔 유도한다.
