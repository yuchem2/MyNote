## Outline
예술분야, 특히 회화에서 인간은 이미지의 내용과 스타일 간의 복잡한 상호작용을 구성하여 독특한 시각적 경험을 창출하는 기술에 숙달되어 있다. 이러한 과정의 알고리즘적 기반은 알려지지 않았으며, 유사한 능력을 가진 AI도 존재하지 않는다.

요즘 시대에 이렇게 DL을 이용해 새로운 예술 창작물을 만드는 시도가 계속되고 있다. 이러한 시도를 하기 위해서는 CNN이 어떻게 feature map을 추출해내는 지에 대한 이해가 필요하다.

하지만, 여전히 CNN이 특정 패턴이나 객체를 인식하는 방식이 왜 그렇게 잘 작동하는 지에 대한 이해가 부족하다(블랙 박스). 이를 위해 CNN에서 내부 feature map을 시각화하여 모델이 세상을 어떻게 보고 객체 간의 구별을 위해 어떤 특징을 중요하게 여기는 지 파악할 수 있다.

DL의 주요 과제 중 하나는 각 레이어에서 정확히 무슨 일이 일어나는 지를 이해하는 것. 
+ 학습 후 각 레이어는 점차적으로 더 높은 수준의 이미지 특징을 추출하며, 최종 레이어에서는 이미지가 무엇을 포함하는 지에 대한 결정을 내림
+ 모델이 학습을 통해 무엇을 학습했는지 이해하기 위해 블랙박스를 열어 feature map을 시각화 하는 것이 필요
+ 추출된 특징을 시각화하는 한 가지 방법은 모델을 뒤집은 후 입력 이미지가 특정 해석을 이끌어내도록 변경하는 것

![500](Pasted%20image%2020240619015014.png)

CNN feature를 시각화하기 쉬운 방버은 각 필터가 반응하도록 설계된 시각적 패턴을 표시하는 것이다. 이는 입력 공간에서 gradient ascent로 수행할 수 있다.
+ CNN의 입력 이미지 값에 gradient ascent를 적용해 특정 필터의 반응을 최대화할 수 있다.
+ 결과 입력 이미지는 선택된 필터가 최대 반응을 보이는 이미지
+ Gradient ascent는 gradient descent를 반대로 사용해 특정 layer의 loss를 최대화하는 기법. local maximum에 접근하기 위해 gradient를 양의 방향으로 비례하는 단계로 진행

이러한 기법을 사전학습된 VGG16에 적용해 각 CONV의 feature map을 출력해보면 아래와 같은 결과가 나온다. 각 순서대로 block1_conv1, block3_conv2, block5_conv3이다.  해당 결과를 살펴보면 깊은 쪽에 있는 레이어일 수록 고수준의 패턴을 만들어 내는 것을 확인할 수 있다.

![500](Pasted%20image%2020240619015628.png)

![500](Pasted%20image%2020240619015639.png)

![500](Pasted%20image%2020240619015705.png)

```python
from keras.appliactions.vgg15 import VGG16
from keras import backend as K
from keras.preprocessing.image import save_img
import numpy as np

model = VGG16(weigths='imagenet', include_top=False)

for layer in model_layers:
	if 'conv' not in layer.name:
		continue
	filters, biases = layer.get_weights()
	print(layer.name, layer.output.shape)

layer_name = 'block1_conv1'
filter_index = 0 

layer_dict = dict([(layer.name, layer) for layer in model.layers[1:]])
leyer_output = layer_dict[layer_name].output
loss = K.mean(layer_output[:, :, :, filter_index])

grads = K.gradients(loss, input_img)[0]
gads /= (K.sqrt(K.mean(K.square(grads))) + 1e-5)
iterate = K.function([input_img], [loss, grads])

input_img_data = np.random.random((1, 3, img_width, img_height)) * 20 + 128
for i in range(20):
	loss_value, grads_value = iterate([input_img_data])
	input_img_data += grads_value * step

def deprocess_image(x):
	x -= x.mean()
	x /= (x.std() + 1e-5)
	x *= 0.1
	x += 0.5
	x = np.clip(x, 0, 1)

	x *= 255
	x = x.transpose((1, 2, 0))
	x = np.clip(x, 0, 255).astype('uint8')
	return x

img = input_img_data[0]
img = deprocess_image(img)
imsave('%s_filter_%d.png' % (layer_name, filter_index), img)
```

## DeepDream
2015년 구글에서 발표한 특정 레이어에서 시각화하는 특징을 입력 이미지에 출력하여 꿈같은 환각 이미지를 만드는 모델.

![500](Pasted%20image%2020240619020810.png)

![500](Pasted%20image%2020240619020800.png)

DeepDream은 개 품종과 새 품종이 과다하게 대표되는 사전훈련된 ImageNet을 이용해 훈련되었다. 만약 다른 모델을 사용하고, 그 모델의 대부분의 객채들이 차로 구성된 데이터 셋에서 사전 훈련되었다면 출력 이미지에서 자동차 특징을 살펴볼 수 있을 것이다.

이 프로젝트는 CNN을 역으로 실행하고, feature map을 시각화하는 실험으로 시작되었다. 앞서 기술한 특정 필터의 활성화를 최대화하기 위해 입력에 대해 gradient ascent을 실행하는 기술을 사용한 것.

DeepDream은 위 아이디어를 몇 가지 변경하여 사용한다.
+ 입력 이미지: 시각화된 특징을 이미지에 출력하는 것이 목표이므로 모델에 입력 이미지를 사용 (기존에는 노이즈가 있는 빈 이미지)
+ 필터 최대화 vs 레이어 최대화: 필터 시각화에서는 레이어 내 특정 필터의 활성화만 최대화한다. 하지만 DeepDream에서는 전체 레이어의 활성화를 최대화하여 한 번에 많은 특징을 혼합하려고 한다.
+ Octaves: 입력 이미지를 octave라는 다른 스케일로 처리해 시각화된 특징의 품질을 향상

DeepDream은 Inception v3 모델과 같은 사전 훈련된 모델을 통해 입력 이미지를 통과시키는 것이 아이디어. 특정 레이어에서 값을 최대화하기 위해 입력 이미지를 어떻게 변경해야 하는지 알려준느 gradient를 계산. 10, 20, or 40번 반복해 결국 입력 이미지에 패턴이 나타날 때까지 이 방법을 반복

![500](Pasted%20image%2020240619021529.png)

이러한 방법은 잘 작동하지만, 사전 훈련된 모델이 ImageNet과 같이 비교적 작은 이미지 크기에서 훈련된 경우, 입력 이미지가 크면 이미지에 많은 작은 패턴을 인쇄해 노이즈처럼 보이게 된다. 이를 해결하기 위해 입력 이미지를 octave라는 다른 스케일로 처리
+ Octave는 단순히 간격을 나타내는 단어. DeepDream 알고리즘에 간격을 두고 적용 하는 것. 이미지를 여러 번 축소하여 다른 스케일로 만든다. 이때 스케일의 수는 설정할 수 없다.

![500](Pasted%20image%2020240619021737.png)

## Neural Style Transfer
스타일 이미지와 내용 이미지를 입력으로 받아 내용 이미지의 레이아웃과 스타일 이미지의 질감, 색상 및 패턴을 결합한 새로운 이미지를 만드는 모델.

![500](Pasted%20image%2020240619021906.png)

스타일 전이의 메인 아이디어는 모든 DL 알고리즘과 동일. 먼저 달성하고자 하는 것의 손실 함수를 정의한 뒤에 이 함수를 최적화 하는 작업을 수행한다. 스타일 전이 문제에서 주 목표는 원본 이미지의 레이아웃을 유지하면서 참조 이미지의 스타일을 채택하는 것이다. 손실 함수를 정의할 때 중요한 것은 한 이미지의 콘텐츠와 다른 이미지의 스타일을 보존하고자 하는 것을 기억하는 것

스타일 전이에서 총 loss는 다음과 같다.
+ Content loss: 콘텐츠 이미지와 결합된 이미지 간의 손실 계산. 결합된 이미지에 원본 이미지의 콘텐츠가 더 많이 포함되어야 최소화.
+ Style loss: 스타일 이미지와 결합된 이미지 간의 스타일 손실 계산. 결합된 이미지가 스타일 이미지와 유사한 스타일을 가져야 최소화.
+ Noise loss: 총 변동 손실(total variation loss)라고도 한다. 결합된 이미지의 노이즈를 측정. 

![500](Pasted%20image%2020240619022322.png)

Content loss는 주제와 콘텐츠의 전체 배치 측면에서 두 이미지가 얼마나 다른 지를 측정. 유사한 장명을 포함하는 두 이미지는 완전히 다른 장면을 포함하는 두 이미지보다 손실 값이 작아야 한다. 콘텐츠 이미지와 결합된 이미지의 출력 간의 평균 제곱 오차를 측정

![500](Pasted%20image%2020240619022454.png)

Style loss는 content loss를 정의하는 것보다 어려움. content loss에서는 높은 수준의 특징만 신경 섰기 때문에 VGG19에서 하나의 레이어만 선택하여 그 특징을 보존하면 되었음. 그러나 style loss에서는 여러 레이어를 선택하고자 함. 이는 이미지 스타일의 다중 스케일 표현을 얻고자 하기 위함. 더 낮은 수준의 레이어, 중간 수준의 레이어, 및 더 높은 수준의 레이어에서 이미지 스타일을 포착해야 함.  

이러한 데이터를 Gram matrix를 통해 공동 활성화된 특징 맵을 측정할 수 있음. Gram matrix는 두 feature map이 얼마나 공동으로 활성화되는 지를 수치적으로 측정하는 방법. 우리는 CNN의 여러 레이어에서 스타일과 질감을 포착하는 손실 함수를 만드는 것이므로, CNN의 활성화 레이어 간의 상관 관계를 계산해야 함. 이 상관 관계는 gram matrix, 즉 활성화 간의 특징별 외적을 계산하여 포착할 수 있음

Tortal variance loss는 결합된 이미지의 노이즈를 측정. 모델의 목표는 이 값을 최소화하여 출력 이미지의 노이즈를 최소화하는 것임. total_variation_loss 함수를 생성해 이미지가 얼마나 노이즈가 있는지를 계산
+ 이미지를 한 픽셀 오른쪽 이동시키고, 전이된 이미지와 원본 이미지 간의 제곱 오차를 계산
+ 1단계를 반복하며 이번에는 이미지를 한 픽셀 아래로 이동시킴