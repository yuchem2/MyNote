물체 탐지 알고리즘은 이미지 상에 존재하는 여러 개의 물체의 위치를 확인하고, 그 물체가 무엇인지를 찾는 CV 분야 작업이다. 이러한 작업을 수행하기 위해 두 가지 작업이 필수적이다.
+ 이미지에서 한 개 혹은 그 이상의 물체의 위치를 탐지
+ 이미지에 존재하는 각 물체를 식별

물체 탐지 기술은 현재 다양한 분야에서 넓게 사용되고 있다.
+ 자율 주행 기술
+ 로봇
+ 환경 모니터링
+ 물류 시스템
+ 농장 시스템
+ 배송 시스템
## General Object Detection Framework
### Region proposals
이미지를 분석하고 추가 분석이 필요한 Rol(Region of Interest)를 제안하는 단계. RoI는 시스템이 객체가 있을 가능성 이 높다고 판단하는 영역을 말한다.

이 방법은 여러 접근 방식이 존재한다. 모델은 이미지 내 RoI를 분석하고 각 영역에 객체가 있는지(foreground), 없는지(background) 여부를 해당 영역의 객체 존재 확률(objectness score)를 기반으로 분류. Objectness score가 특정 임계값을 초과하는 경우 해당 영역은 객체가 있는 것으로 판단하게 된다.
### Feature extraction and network predictions
입력 이미지에서 특징을 추출되기 위해 사용되는 사전 학습된 CNN 모델이 포함된다. 이 모델은 주어진 작업에 대한 대표적인 특징을 추출하고, 객체를 분류하는 데 사용된다. 일반적으로 사전 학습된 image classification 모델을 사용하여 시각적인 특징을 추출하게 된다. 

모델은 객체가 있을 가능성이 높다고 판단한 모든 영역을 분석하고, 각 영역에 대해 대해 다음 두 예측을 수행한다.
+ Bounding-box prediction: 객체를 둘러싼 박스의 좌표
+ Class prediction: 각 객체에 대한 클래스 확률을 예측하는 softmax function

수 천 개의 영역이 제안되기 때문에 각 객체 주변에는 올바른 클래스와 함께 여러 개의 바운딩 박스가 존재할 가능성이 있다. 대부분의 문제에서는 하나의 객체에 하나의 박스를 예측하는 것이 목표이다. 이를 해결하기 위한 방법이 NMS.
### Non-maximum suppression (NMS)
동일한 객체에 대해 여러 개의 박스를 그린 결과에서, 객체 하나에 하나의 박스만 존재하도록 보장하는 기법. 객체 주변의 모든 박스를 살펴보고 예측 확률이 가장 높은 박스를 찾아 다른 박스를 제거

이 알고리즘은 다음과 같은 단계로 수행된다.
1. 신뢰 임계값(confidence threshold)라는 값보다 예측이 낮은 모든 바운딩 박스를 제거
2. 남은 모든 박스를 살펴보고 가장 높은 확률을 가진 바운딩 박스를 선택
3. 동일한 클래스 예측을 가진 남은 박스들 간의 겹침을 계산. 이 값을 IoU(Intersection over Union)이라 한다.
4. IoU 값이 NMS threshold보다 작은 박스를 제거

이 알고리즘은 다양한 모델에서 표준적으로 선택되는 형태이다.
### Evaluation metrics
객체 감지 모델을 평가할 때 주로 사용되는 평가 지표는 다음과 같다.
1. 초당 프레임 수(FPS): 감지 속도를 측정하는 가장 일반적인 지표
2. mean average precision(mAP): 객체 감지 작업에서 가장 일반적으로 사용되는 평균 지표. 0에서 100까지의 백분율이며, 높은 값이 보통 더 좋다.

+ IoU: 두 개의 바운딩 박스 사이의 겹침을 평가하는 지표 (정답 바운딩 박스와 예측 바운딩 박스)
	+ 0(겹치지 않음)에서 1(완전히 겹침)까지 범위를 가짐. 두 바운딩 박스의 겹침이 클수록 좋음
	+ 겹치는 영역의 면적을 전체 영역의 합으로 나누어 계산
	+ 올바른 예측을 정의하는데 사용되며, 이는 IoU 임계값 보다 높은 예측(TP)를 의미
	+ 임계값은 작업에 따라 조정할 수 있으며, 일반적으로 0.5가 표준값이다.
	+ MS COCO와 같은 과제는 mAP@0.5 혹은 mAP@0.75를 사용

![500](Pasted%20image%2020240618223201.png)

+ Precision-recall Curve (PR curve)
	+ TP와 FP를 정의한 후, 주어진 클래스에 대해 테스트 데이터 셋에서 예측의 precision과 recall을 계산할 수 있다. 
	+ PR curve는 객체 탐지 모델의 성능을 평가하는 지표로, 신뢰도(confidence)를 변경하여 각 객체 클래스에 대한 곡선을 그린다.
	+ 모델은 precision이 높은 상태로 recall이 증가할 때 좋은 것으로 간주된다. 즉, confidence threshold를 변경해도 precision, recall이 모두 높은 것이 좋은 모델.

![500](Pasted%20image%2020240618223952.png)

mAP를 계산하는 방법은 다음과 같다.
1. 각 바운딩 박스의 objectness score를 얻는다.
2. precision과 recall을 계산
3. score threshold 값을 변경시켜 각 클래스에 대해 PR curve를 계산
4. AP를 계산. PR 곡선의 아래 영역의 면적을 AP라고 한다. 각 클래스에 대해 AP를 계산한다.
5. 모든 클래스에 대한 평균 AP가 mAP

 ※ 참고: confusion matrix
 
![500](Pasted%20image%2020240618224530.png)

## R-CNNs
### R-CNN
R-CNN은 일반적으로 region-based CNN으로 알려져 있으며, 이는 2014년에 Ross Girshick 등에 의해 개발되었다. 이후 2015년 Fast R-CNN, 2016년 Faster R-CNN으로 확장되었다.

R-CNN은 R-CNN 파생 모델들 중 가장 기초적인 모델이지만, 다양한 객체 탐지 알고리즘들이 동작하는 방식을 이해하는 기초가 된다. 객체 탐지 및 위치 결정 문제에 대한 초기 CNN 중 성공적인 응용 중 하나였다.

![500](Pasted%20image%2020240618225715.png)

다음 네 가지 요소로 구성된다.
+ Extract regions of interest (RoI)
+ Feature extraction module
+ Classification module
+ Localization moduel

R-CNN은 RoI를 추출하기 위해 Selective search algorithm을 사용한다. 이 알고리즘은 객체가 있을 가능성이 있는 영역을 제공하기 위해 사용되는 greedy search algorithm.
+ 유사한 픽셀과 질감을 결합하여 직사각형 상자로 객체가 있을 수 있는 영역을 찾음
+ 이미지에서 모든 가능한 위치를 조사하는 exhaustive search algorithm과 유사한 영역을 계층적으로 구분하는 bottom-up segmentation algorithm의 장점을 결합하여 모든 가능한 객체의 위치를 포착
+ Selective search algorithm은 이미지에서 blobs를 찾기 위해 segmentation algorithm을 적용하고, 그 blob이 어떤 객체가 될 수 있을지를 파악
+ Bottom-up segmentation algorithm은 이러한 영역 그룹을 재귀적으로 결합하여 조사할 약 2000개의 영역을 생성한다.
	1. 모든 인접 영역 간의 유사성을 계산
	2. 가장 유사한 두 영역을 함께 그룹화하고, 새로운 유사성을 해당 영역과 그 이웃들 간에 계산
	3. 이 과정을 하나의 영역으로 전체 객체가 포함될 때까지 반복

![500](Pasted%20image%2020240618225518.png)

![500](Pasted%20image%2020240618225634.png)

R-CNN의 학습은 위에 언급한 대로 총 4개의 모듈로 구성된다. 각 selective search algorithm을 제외한 R-CNN 모듈은 모두 독립적으로 학습이 수행된다. 즉, R-CNN을 훈련하기 위해서는 feature extractor CNN, SVM classifier, bounding-box regressor를 독립적으로 학습시켜야 한다. 이로 인해 R-CNN 학습은 매우 느리고, 계산 비용이 많이 든다.

R-CNN은 다음과 같은 단점을 가지고 있다.
1. 객체 탐지 속도가 매우 느림
2. 훈련이 여러 단계의 파이프라인으로 구성
3. 공간 및 시간 측면에서 훈련 비용이 매우 비쌈
### Fast R-CNN
2015년 Ross Girshick에 의해 개발된 R-CNN의 후속 모델로, 많은 부분에서 R-CNN과 유사. 감지 속도를 향상시키고 동시에 감지 정확도를 높이기 위해 다음 두 가지 변경사항을 추가한 모델
+ 전체 입력 이미지에 CNN feature extrator를 적용한 후에 RoI를 추출하는 방식
+ 전통적인 ML 알고리즘인 SVM 대신 softmax를 이용해 ConvNet의 역할을 확장하여 분류 부분을 수행

다음 네 가지 요소로 구성된다.
+ Feature extraction module
+ RoI extractor
+ RoI pooling layer
+ Two-head output layer

![500](Pasted%20image%2020240618230121.png)

객체의 클래스뿐만 아니라 관련된 bounding box의 위치와 크기를 학습하기 위한 end-to-end 학습 구조이기 때문에 손실 함수가 multi-task loss function이다. 객체 분류 정확도와 객체 위치 정확도를 증가시키는 것이 목표이기 때문에 다음과 같이 구성된다. 

![500](Pasted%20image%2020240618230542.png)

Fast R-CNN은 R-CNN에 비해 테스트 시간이 매우 향상되었다. 이전에 R-CNN은 각 이미지당 2000개의 RoI를 각각 CNN으로 연산을 수행해야 됬기 때문. 또한, 훈련 시간도 빨라졌는데, 모든 구성 요소가 하나의 CNN 네트워크로 포함되어 있기 때문이다. 

그러나 여전히 큰 병목현상(bottleneck)이 존재한다. RoI를 추출하기 위해 사용되는 selective search alogrithm이 매우 느리며, 이것은 별도의 다른 모델에서 생성되기 때문
### Faster R-CNN
2016년 Shapoqing Ren 등에 의해 개발된 Fast R-CNN의 향상된 모델. Fast R-CNN과 유사하게, 입력 이미지가 CNN에 제공되어 CNN feature map을 생성한다. 개선된 사항은 다음과 같다.
+ CNN feature map에 selective search를 사용하는 대신, RPN(region proposal network)가 사용된다. 
+ 예측된 RoI는 RoI pooling layer를 사용해 재구성되고, 제안된 영역 내에서 이미지를 분류하고, bounding box offset를 예측하는 데 사용된다. 

이러한 개선은 RoI의 수를 줄이고, 모델의 test 시간을 거의 실시간으로 가속해 성능을 향상시켰다.

다음 두 가지 주요 네트워크로 구성된다.
+ Region proposal network(RPN)
	+ selective search 대신 feature extrator의 마지막 feature map에서 RoI를 제안하는 ConvNet
	+ 객체 존재 여부를 나타내는 objectness score와 박스의 위치를 리턴
+ Fast R-CNN
	+ feature extractor의 기본 네트크. 
	+ RoI pooling layer를 사용해 고정 크기의 RoI를 추출
	+ 두 개의 FC 레이어를 통해 출력

![500](Pasted%20image%2020240618231505.png)

+ Base network to extract features
	+ 입력 이미지에서 특징을 추출하는 데 사용되는 모델로, 해결하려는 문제에 기반하여 인기 있는 CNN 모델을 사용할 수 있음
+ Region proposal network(RPN)
	+ 사전 학습된 CNN의 마지막 feature map을 기반으로 관심 객체를 포함할 가능성이 있는 영역을 식별
	+ 이미지의 흥미로운 영역에 모델의 주의를 집중시키는 것으로도 알려져 있음
	+ selective search 대신 RoI를 사용해 region proposal을 직접 R-CNN 구조에 통합
	+ 다음과 같은 두개의 레이어로 구성되게 된다.

![500](Pasted%20image%2020240618233245.png)

RPN은 anchor box를 분류하여 객체의 존재 여부를 출력하고 객체의 네 좌표(x, y, w, h)를 근사화하기 위해 훈련된다. 
+ Anchor box: RPN은 sliding window approach를 통해 feature map의 각 위치에 대해 k개의 영역을 생성한다. 이렇게 생성된 영역을 anchor box라고 한다.
+ Human annotators가 bouinding box를 레이블링을 한 뒤 학습이 진행된다. (ground truth)
+ 각 anchor box에 대해 orverlap probability value (p)를 계산한다. 이 값은 anchor가 ground-truth bounding box와 얼마나 겹치는 지를 나타내는 값

![500](Pasted%20image%2020240618232815.png)

![500](Pasted%20image%2020240618232802.png)

결과를 추출해내는 FC layer는 base ConvNet에서 오는 feature map과 RPN에서 오는 RoI를 입력으로 받는다. 그런 다음 선택된 영역을 분류하고, 예측 클래스와 함께 bounding box parameter를 출력한다. Faster R-CNN은 object classification layer에 softmax activation을 사용하고, location regression layer는 bounding box의 위치를 정의하는 좌표에 대한 linear regression을 사용한다. 

모든 네트워크는 multi-task loss를 사용해 함께 학습된다. $L = L_{cls} + L_{loc}$
### Summary

![500](Pasted%20image%2020240618233335.png)

위와 같은 순서로 발전해온 R-CNN은 여전히 한계점을 가지고 있다.
+ 데이터 훈련이 다루기 힘들고 오래 걸린다.
+ 훈련이 여러 단계로 이루어진다. ()
+ 추론 시간이 매우 느려 실시간 탐지에는 여전히 부족하다.
## Single-Shot Detector(SSD)
SDD는 고정 크기의 bounding box와 해당 box 내 object class instance의 존재 점수(score for presence)를 생성하는 feed-forward CNN에 기반하며, 최종 감지를 위해 NMS 단계를 거친다. 총 다음과 같은 구조를 가진다.
+ Base network to extract feature maps
+ Multi-scale feature layers
+ NMS

![500](Pasted%20image%2020240618233847.png)

SSD는 VGG16 구조를 기반으로 하며, FC를 제거한 후에 구축된다. 이러한 기본 모델은 다음과 같은 형식으로 예측을 수행한다.
+ 이미지의 보트가 여러 개 있는 경우, 모델의 작업은 이미지 내 모든 보트 주위에 bounding 박스를 그리도록 작동한다.

![500](Pasted%20image%2020240618234133.png)

Multi-scale feature layer는 truncated 기본 네트워크 끝에 추가된 CONV로, 여러 sacle에서 객체를 예측할 수 있도록 크기를 점진적으로 줄인다. 
+ 서로 다른 CONV feature map을 추출해 여러 스케일의 bounding box를 만들어 낼 수 있도록 하는 기술
+ Multi-scale dections을 하는 이유는 기본 네트워크는 배경의 말 특징을 감지할 수 있지만, 카메라에 가장 가까운 말은 감지하지 못할 수 있기 때문. 이러한 문제를 multi-sacle로 여러 sacle의 feature map을 추출한다면 해결할 수 있음
+ 특히 아래 표를 통해 multi-scale 사용 여부에 따라 mAP 점수가 달라지는 것을 확인할 수 있음

![500](Pasted%20image%2020240618235029.png)

![500](Pasted%20image%2020240618235121.png)

![500](Pasted%20image%2020240618235223.png)

위 모델들을 거친 SSD는 다음과 같은 결과를 추출하게 된다.

![500](Pasted%20image%2020240618233913.png)

SSD에서 추론 시간에 각 클래스 별로 생성되는 많은 수의 bounding box로 인해, NMS 기법을 적용하여 대부분의 bounding box를 정리하는 것이 중요. 
+ SSD는 예측된 box를 신뢰도 점수(confidence scores)에 따라 정렬
+ 가장 높은 신뢰도 예측부터 시작해 SSD는 이전에 예측된 동일 클래스의 bounding box들의 간의 IoU 임계값 이상으로 겹치는 지 평가
+ IoU 임계값을 초과화여 겹침이 있는 경우, 이 박스들은 무시. 높은 신뢰도 점수를 가진 다른 박스들과 너무 많이 겹쳐 있기 때문에 같은 객체를 감지할 가능성이 높기 때문

![500](Pasted%20image%2020240618235444.png)

## You only Look Once(YOLO)
YOLO는 Joseph Redmon 등이 개발한 객체 탐지 모델로, 다음과 같은 버전들을 통해 점진적으로 개선되어 왔다.
+ YOLOv1, 2016
+ YOLOv2 (or YOLO9000), 2016
+ YOLOv3, 2018
+ ...
+ YOLOv9, 2024

YOLO는 빠른 객체 탐지를 위해 설계된 end-to-end DL 시리즈로, 실시간 객체 탐지를 위한 최초의 시도였다. 현재 존재하는 객체 탐지 모델 중에서 속도가 빠른 편에 속한다. 모델의 정확도는 R-CNN과 유사하거나 그만큼 우수하진 않지만, 실시간 비디오나 카메라 입력에서 빠른 탐지 속도로 인해 널리 사용되고 있다.

YOLO는 이전 객체 탐지 모델과 다른 접근 방식을 취하고 있다. (region proposal이 없음)

![500](Pasted%20image%2020240619000143.png)

YOLO는 입력 이미지를 S x S 셀의 그리드로 나눈다. 만약 ground-truth box의 중심이 셀 내에 위치한다면, 그 셀은 해당 객체의 존재를 탐지해야 하는 책임이 있다. 그 셀을 기준으로 Prediction feature map을 얻게 되고 다음과 같은 정보를 가지고 있다.
+ B 개의 bounding box의 좌표
+ 객체 존재 여부 점수 (P0)
+ 클래스 예측

![500](Pasted%20image%2020240619000358.png)

이러한 feature map을 통해 다양한 스케일에서 예측이 가능하다. 해당 feature map에서는 세 개의 box가 존재하는데, SSD의 anchor 개념과 유사하게 YOLOv3는 각 셀당 세가지 다른 스케일에서 예측할 수 있는 아홉개의 anchor를 사용한다.

![500](Pasted%20image%2020240619000918.png)


![500](Pasted%20image%2020240619000943.png)

![500](Pasted%20image%2020240619000957.png)