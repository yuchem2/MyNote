## Definition
이미 하나의 작업을 수행하기 위해 많은 양의 데이터로 학습된 모델을 데이터가 부족한 새로운 작업을 수행할 수 있도록 모델을 학습시키는 방법.

보통 사전 학습된 모델이 높은 성능을 보인 작업과 유사한 형태의 작업에 도입해 사용하게 된다. 

이러한 방법의 학습을 사용하는 이유는 크게 1. 데이터 문제 2. 계산량 문제가 있다.
+ 데이터 문제: 모델을 학습시키는 것은 많은 양의 데이터를 요구하는데, 이러한 데이터를 모으는 것이 매우 어려운 문제이기 때문에 대부분의 경우 데이터가 부족하다.
+ 계산 문제: 많은 양의 데이터를 학습시키기 위해서는 DL을 사용하는 경우 많은 양의 계산량이 필요하고, 이로 인해 많은 양의 계산량을 수행할 수 있는 컴퓨팅 리소스가 필요

위에 기술한 이러한 문제를 쉽게 해결할 수 있는 방법이 전이학습
## How transfer learning works
모델은 학습 과정에서 feature maps에 대한 학습을 진행하게 된다. 학습을 진행하면서 특정 task에서 높은 성능을 보이는 방향으로 적절한 feature map을 추출하게 된다. 

즉, 이미 학습된 모델 중 높은 성능을 보이는 모델들은, 해당 task 분야에서 사용하기 위해 입력으로 들어온 데이터의 feature map을 매우 잘 추출하는 방향으로 학습이 왼료된 모델이라고 볼 수 있다. 또한, 일반적으로 레이어가 깊어질 수록 높은 수준의 feature map을 추출하게 되는데, 이러한 특징을 이용해 source domain과 target domain의 차이에 따라 적절하게 전이학습에 사용될 모델의 layer를 선정할 수 있다. 
+ Source domain: 사전 학습된 모델의 훈련에 사용된 원본 데이터 셋
+ Target domain: 새롭게 수행할 작업을 학습하는 데 사용될 데이터 셋
## Transfer learning approaches
### Use a pretrained network as a classifier
추가적인 학습이나 수정 없이 미리 학습된 모델을 직접 사용해 이미지를 분류하는 것

```python
from keras.preprocessing.iamge import load_img
from keras.preprocessing.image import img_to_array
from keras.applications.vgg16 import preprocess_input
from keras.appliactions.vgg16 import decode_predictions
from kreas.appliactions.vgg16 import VGG16

model = VGG16(weights = 'imagenet', include_top = True, input_shape = (224, 224, 3))

image = load_img('path/to/image.jpg', target_size = (244, 244))
image = img_to_array(image)
image = image.reshpae((1, image.shape[0], image.shape[1], image.shape[2]))
image = preprocess_input(image)

yhat = model.predict(image)
label = decode_prediction(image)
label = label[0][0]
print('%s (%.2f%%)' % (label[1], label[2]*100))
```
### Using a pretrained network as a feature extractor
미리 학습된 CNN을 사용해 특징 추출 레이어 부분의 parameter를 고정하고, 특정 작업을 위해 새로운 layer를 추가하는 것이다. FC 이전 레이어를 고정시킨 다음 FC부분을 새롭게 구성하거나 이전에 존재하는 FC의 parameter만 학습시키는 등의 방법을 말한다.
### Fine Tuning
전이 학습은 target domain이 source domain과 많이 다르더라도 효과적일 수 있다. parameter를 랜덤한 값에서 출발하는 것보다 이미 어느정도 feature를 잘 추출하는 parameter에서 시작하는 것과의 차이가 존재하기 때문이다. 이러한 문제에서 핵심은 source domain에서 적절한 feature map을 추출하고 이를 target domain에 맞게 fine tuning하는 것이다.


Fine-Tuning은 모델의 일부 레이어를 고정하고 고정되지 않은 레이어와 새롭게 추가된 레이어를 함께 학습시키는 방법을 말한다.

![500](Pasted%20image%2020240618202116.png)

Fine-Tuining에서 고정될 레이어를 결정하는 것은 주로 source domain과 target domain의 유사성에 따라 달라지게 된다. 유사한 도메인인 경우 마지막 feature map까지 고정할 수 있고, 매우 다른 도메인인 경우 더 적은 수의 layer를 고정해 광범위하게 모델을 학습시킨다.

 이러한 것을 결정할 때 일반적인 가이드 라인은 다음과 같다.
 + 사용 가능한 데이터의 양
 + Target domain과 source domain간의 유사성

이를 통해 적절한 수준의 전이 학습은 다음과 같은 경우로 결정될 수 있다.
+ 사용 가능한 데이터 셋의 크기가 작고 source domain과 target domain이 유사한 경우
	+ 사전학습된 모델이 새로운 데이터 셋에도 유효할 확률이 높기 때문에 특징 추출 부분만 고정하고, 분류기 부분만 재 학습하는 것이 좋다.
	+ 새로운 데이터 셋이 작을 때 특징 추출 부분을 재 학습시킨다면 overfitting 될 가능성 존재
+ 사용 가능한 데이터 셋의 크기가 크고 source domain과 target domain이 유사한 경우
	+ 두 도메인이 유사하므로 특징 추출 부분을 고정하고, 분류기만을 재학습할 수 있다.
	+ 하지만 새로운 데이터 셋에 데이터의 수가 많기 때문에 사전 학습된 모델 전부, 혹은 일부를 재 학습시키더라도 성능이 증가할 수 있다. 일반적으로 60-80%를 고정하고 나머지를 재학습하는 것이 좋다.
+ 사용 가능한 데이터 셋의 크기가 작고 source domain과 target domain이 매우 다른 경우
	+ 두 도메인이 다르므로, 고수준의 레이어를 고정하는 것은 좋지 않다. 모델의 초반 부분부터 재학습하거나 전체 모델을 재 학습하는 것이 좋다.
	+ 그러나 데이터 셋이 작기 때문에  전체 네트워크를 미세 조정하면 overfitting 가능성 존재
	+ 사전 학습된 모델의 첫 번째 1/3 혹은 절반을 고정하는 것이 추천됨
+ 사용 가능한 데이터 셋의 크기가 크고 source domain과 target domain이 매우 다른 경우
	+ 새로운 데이터 셋이 매우 크므로, 처음부터 학습시켜도 괜찮다.
	+ 그러나 실제로 사전 학습된 모델의 가중치부터 학습을 시작하는 것이 도움이 된다. 이렇게 하는 경우 모델의 학습률이 올라가는 속도가 랜덤한 값으로 가중치가 초기화된 모델부터 시작하는 것에 비해 빠르다.
	+ 전체 모델의 가중치를 모두 고정하지 않고 학습하는 것이 좋다.

![500](Pasted%20image%2020240618204304.png)

## A pretrained network as a feature extractor
```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator  
from keras.preprocessing import image  
from keras.applications import imagenet_utils  
from keras.applications import vgg16  
from keras.applications import mobilenet  
from tensorflow.keras.optimizers import Adam, SGD  
from keras.metrics import categorical_crossentropy  
from keras.layers import Dense, Flatten, Dropout, BatchNormalization  
from keras.models import Model
  
from sklearn.metrics import confusion_matrix  
import itertools  
import matplotlib.pyplot as plt  
%matplotlib inline

from sklearn.datasets import load_files  
from tensorflow.keras import utils as np_utils  
import numpy as np
from keras.preprocessing import image    
from keras.applications.vgg16 import preprocess_input  
from tqdm import tqdm

train_path  = 'data/train'  
valid_path  = 'data/valid'  
test_path  = 'data/test'

train_batches = ImageDataGenerator(preprocessing_function=vgg16.preprocess_input).flow_from_directory(  
    train_path, target_size=(224,224), batch_size=30)  
valid_batches = ImageDataGenerator(preprocessing_function=vgg16.preprocess_input).flow_from_directory(  
    valid_path, target_size=(224,224), batch_size=30)  
test_batches = ImageDataGenerator(preprocessing_function=vgg16.preprocess_input).flow_from_directory(  
    test_path, target_size=(224,224), batch_size=30)

base_model = vgg16.VGG16(weights = "imagenet", include_top=False, input_shape = (224,224, 3))  

for layer in base_model.layers:  
    layer.trainable = False

last_layer = base_model.get_layer('block5_pool')  
last_output = last_layer.output  
 
x = Flatten()(last_output)  
    
x = BatchNormalization()(x)  
x = Dropout(0.5)(x)  
x = Dense(2, activation='softmax', name='softmax')(x)  
  
new_model = Model(inputs=base_model.input, outputs=x)  
new_model.summary()

new_model.compile(Adam(learning_rate=0.0001), loss='categorical_crossentropy', metrics=['accuracy'])
new_model.fit(train_batches, steps_per_epoch=4,
			  validation_data=valid_batches, validation_steps=2, epochs=20, verbose=2)

def load_dataset(path):  
    data = load_files(path)  
    paths = np.array(data['filenames'])  
    targets = np_utils.to_categorical(np.array(data['target']))  
    return paths, targets

def path_to_tensor(img_path):  
    img = image.load_img(img_path, target_size=(224, 224))  
    x = image.img_to_array(img)  
    return np.expand_dims(x, axis=0)  
  
def paths_to_tensor(img_paths):  
    list_of_tensors = [path_to_tensor(img_path) for img_path in tqdm(img_paths)]  
    return np.vstack(list_of_tensors)  
  
test_tensors = preprocess_input(paths_to_tensor(test_files))
print('\nTesting loss: {:.4f}\nTesting accuracy: {:.4f}'.format(*new_model.evaluate(test_tensors, test_targets)))
score = new_model.evaluate(test_tensors, test_targets)  
print('\n', 'Test accuracy:', score[1])
```