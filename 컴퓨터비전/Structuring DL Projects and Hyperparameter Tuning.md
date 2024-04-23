## Defining Performance Metrics
$$\text{accuracy} = \cfrac{\text{correct predictions}}{\text{total number of examples}}$$
정확도만을 모델의 평가요소로 사용하는 경우 특정 경우에서 모델을 정확하게 평가하지 못하는 경우가 존재할 수 있음.
e.g. 발병 확률이 매우 낮은 희귀병을 검출하는 모델 -> 100 퍼센트로 건강하다고 해도 정확도가 높게 나옴

### Confusion matrix
이러한 경우를 해결하기 위해 새로운 평가 요소가 필요하다. 그 중에서 confusion matrix를 적용해 다음과 같은 요소들을 사용할 수 있다.

![|400](Pasted%20image%2020240423223449.png)
+ True Positive(TP): 실제로 true인 것을 true로 예측
+ True Negative(TN): 실제로 false인 것을 false로 예측
+ False Positive(TP): 실제로 true인 것을 false로 예측
+ False Negative(FN): 실제로 false인 것을 true로 예측

위 값을 통해 아래와 같은 요소들을 계산해 모델의 성능 평가 요소로 사용할 수 있다.
+ $\text{Recall} = \cfrac{TP}{TP+FN}$: sensitivity(민감도)라고 한다. 실제로 positive인 경우에서 모델이 positive로 예측한 확률을 확인할 수 있다.
+ $\text{Precision} = \cfrac{TP}{TP+FP}$: specificity라고 한다. Positive로 예측한 경우에서 맞게 예측한 확률을 확인할 수 있음.
+ $\text{F-score} = 2 \times \cfrac{Recall \times Precision}{Recall + Precision}$: Recall과 precision 모두를 반영하기 위한 둘의 조화평균
## Desining a Baseline Model
학습을 시작하기에 앞서 원하는 일을 수행하는 모델의 baseline model을 정의하는 것이 중요하다. 해결하는 문제에 따라 네트워크 유형과 구조에 맞게 기준을 잘 정해야 합니다. 다음과 같은 고려사항이 존재할 수 있습니다.
+ MLP or CNN or RNN, ... etc 중 어떤 네트워크
+ YOLO or SSD와 같은 객체 인식 모델 사용 여부
+ 네트워크의 깊이를 얼마나 깊게 할 것인가
+ 활성화 함수
+ 옵티마이저
+ Dropout이나 batch normalization 등의 regularization layer를 추가할 것인가

일반적으로 baseline model은 이미 개발된 우수한 모델을 baseline으로 선택한 뒤 여러가지 조정을 해보면서 실험을 하게 된다.
## Getting your data ready for training
### Data Loader
Baseline model을 선택하였으면, 이제 학습을 위한 데이터 셋을 각 단계에 맞게 나눌 필요가 있다. 
+ Train set: 모델을 학습할 때 사용하는 데이터 셋.
+ Validation set: 학습과정에서 한 epoch이 종료될 때 모델의 정확도와 오류를 확인하고 싶을 때 사용된다. 중간과정에서 모델의 성능을 확인할 때 사용하는 데이터 셋.
+ Test set: 학습할 때 사용하지 않고, 오직 모든 과정이 종료된 뒤 모델의 성능을 평가할 때 사용하는 데이터 셋.
### Data Processing
데이터 전처리를 하는 방법은 매우 다양한 방법들이 존재한다.
+ Image grayscaling
+ Image resizing
+ Data normalization: scale이 높은 feature가 잇는 경우 학습이 편향되므로, 데이터의 분포를 일반화하기 위한 작업

![|400](Pasted%20image%2020240423225421.png)

## Evaluating the model and Interpreting its performance
Baseline model을 확립하고, 데이터를 전처리한 후 모델에 대한 학습을 진행한다. 훈련이 완료된 후 병목 현상이 있는지 판단하고, 어떤 구성 요소가 성능이 나쁜지 확인하고, 어떤 구성 요소가 성능이 나쁜지 진단하고, 성능 저하가 overfitting, underfitting 혹은 데이터 셋의 문제가 있는지 판단할 필요가 있다.
### Diagnosing overfitting and underfitting
+ 학습 오차 <<< 검증(valid) 오차: overfitting으로 판단할 수 있음. Hyper-parameter를 조정해 학습을 다시 하는 것이 권장
+ 학습 오차 $\simeq$ 검증 오차: underfitting으로 판단할 수 있음. 모델을 조정하거나 epoch을 증가시켜 학습을 더하는 것이 권장

위와 같은 추론은 오차를 그래프를 통해 확인하면 쉽게 판단할 수 있음

![|400](Pasted%20image%2020240423230059.png)

### Collecting more data vs. tuning hyper-parameters
모델의 성능을 확인한 뒤 hyper-parameter를 수정할 것인지, 데이터의 크기를 늘리지 선택하는 것은 다양한 이유가 존재할 수 있다. 일반적으로 다음과 같은 단계를 통해 결정해볼 수 있다.
1. 훈련 데이터 셋에서의 성능이 현재로서는 적절한지 판단
2. 훈련 정확도와 검증 정확도를 시각화하고 확인
3. Train dataset에서 성능이 낮다면, underfitting의 신호이기 때문에 더 많은 데이터를 수집할 필요가 없습니다. Hyper-parameter를 조정하거나 데이터를 cleansing하는 것이 권장된다.
4. Tran dataset에서 성능이 좋지만, test dataset에서 성능이 낮다면 overfitting이라고 판단할 수 잇습니다. 이 경우 많은 데이터를 더 수집하는 것이 효과적일 수 있다.
### Parameter vs. Hyper-parameter
Parameter는 네트워크에서 자동으로 학습을 통해 업데이트되는 변수를 말합니다. 이에 반해 Hyper-parameter는 개발자가 직접 조정하는 값들로 아래와 같은 요소들이 존재할 수 있습니다.
+ Network architecture
	+ number of hidden layers
	+ number of neurons in each layer
	+ activation type: ReLU or leaky ReLU가 성능이 높음
+ Learning and optimization
	+ learning rate and decay schedule
		+  learning rate decay는 학습 과정에서 learning rate를 조절하는 방법이다. 초기 learning rate는 크게 설정하고, 학습이 진행되면서 점차 작아지게 만들면서 학습을 하는 기법
	+ mini-batch size
	+ optimization algorithms
	+ number of training iterations or epochs
		+ early stopping: 검증 오차를 모니터하다가 값이 증가하기 시작하면 학습을 멈추게 만드는 기법 `EarlyStopping(monitors='val_loss', min_delta=0, patience=20)`
			+ `min_delta`: 최소 변화율
			+ `patience`: 오차가 개선되지 않을 때 훈련을 멈추기까지 몇 번의 epoch을 기다려야 하는 지 결정
+ Regularization techniques to avoid overfitting
	+ L2 regularization
	+ Dropout layers
	+ Data augmentation
#### Optimization Algorithms
##### Gradient Descent with momentum
앞서 배운 BGD, SGD, MB-GD가 사용되었다. SGD의 단점을 줄이기 위해 momentum이라는 값을 추가한 방식이 존재한다. SGD에서 발생하는 수직 진동을 momentum으로 줄이기 위함이었다. 

![|400](Pasted%20image%2020240423231413.png)

Mementum 연산은 아래와 같이 간단하다. Velocity term은 이전 gradient의 가중 평균이다. $$w_{\text{new}}=w_{\text{old}} - \alpha \cfrac{dE}{dw_i} + \text{velocity term}$$
##### Adam (Adaptive moment estimation)
최근 가장 많이 사용되는 optimizer로, 학습에서 진동은 줄이면서 학습 속도를 증가시키는 optimizer이다. `keras.optimizers.Adam(lr=0.001, beta_1=0.9, bata_2=0.999, epsilon=None, decay=0.0)`
#### L2 Regularizatoin
기본 아이디어는 정규화 항을 추가해 오차 함수에 패널티를 준다. Hidden unit의 가중치 값을 줄이고, 이를 통해 모델을 단순화하기 위해 가중치를 거의 0에 가깝게 만든다. 과도하게 에러가 수정하는 경우를 막기 위함.$$\text{error function}_{new} = \text{error function}_{old} + \text{regularization term} = \text{error function}_{old} + \cfrac{\lambda}{2m}\times \sum\|w\|^2$$`model.add(Dense(units=16, kernel_regularizer=regularizers.l2(λ), activation='relu'))`
#### Batch Normalization
학습 과정에서 각 배치 단위 별로 데이터가 다양한 분포를가지더라도 가 ㄱ배치별로 평균과 분산을 이용해 정규화하는 것을 말함
## Example Code
```python
import keras
from keras.models import Sequential
from keras.utils import to_categorical
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from keras.layers import Dense, Activation, Flatten, Dropout, BatchNormalization
from keras.layers import Conv2D, MaxPooling2D
from keras.datasets import cifar10
from keras import regularizers
from tensorflow.keras import optimizers
import numpy as np
from matplotlib import pyplot

# download and split the data
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
x_train = x_train.astype('float32')
x_test = x_test.astype('float32')

print("training data = ", x_train.shape)
print("testing data = ", x_test.shape)

# Normalize the data to speed up training
mean = np.mean(x_train)
std = np.std(x_train)
x_train = (x_train-mean)/(std+1e-7)
x_test = (x_test-mean)/(std+1e-7)

# let's look at the normalized values of a sample image
x_train[0]


# one-hot encode the labels in train and test datasets
# we use “to_categorical” function in keras
num_classes = 10
y_train = to_categorical(y_train,num_classes)
y_test = to_categorical(y_test,num_classes)

# break training set into training and validation sets
(x_train, x_valid) = x_train[5000:], x_train[:5000]
(y_train, y_valid) = y_train[5000:], y_train[:5000]

# let's display one of the one-hot encoded labels
y_train[0]


# build the model
  
# number of hidden units variable
# we are declaring this variable here and use it in our CONV layers to make it easier to update from one place
base_hidden_units = 32

# l2 regularization hyperparameter
weight_decay = 1e-4

# instantiate an empty sequential model
model = Sequential()

# CONV1
# notice that we defined the input_shape here because this is the first CONV layer.
# we don’t need to do that for the remaining layers

model.add(Conv2D(base_hidden_units, (3,3), padding='same', kernel_regularizer=regularizers.l2(weight_decay), input_shape=x_train.shape[1:]))
model.add(Activation('relu'))
model.add(BatchNormalization())

# CONV2
model.add(Conv2D(base_hidden_units, (3,3), padding='same', kernel_regularizer=regularizers.l2(weight_decay)))
model.add(Activation('relu'))
model.add(BatchNormalization())
model.add(MaxPooling2D(pool_size=(2,2)))
model.add(Dropout(0.2))

# CONV3
model.add(Conv2D(2*base_hidden_units, (3,3), padding='same', kernel_regularizer=regularizers.l2(weight_decay)))
model.add(Activation('relu'))
model.add(BatchNormalization())

# CONV4
model.add(Conv2D(2*base_hidden_units, (3,3), padding='same', kernel_regularizer=regularizers.l2(weight_decay)))
model.add(Activation('relu'))
model.add(BatchNormalization())
model.add(MaxPooling2D(pool_size=(2,2)))
model.add(Dropout(0.3))

# CONV5
model.add(Conv2D(4*base_hidden_units, (3,3), padding='same', kernel_regularizer=regularizers.l2(weight_decay)))
model.add(Activation('relu'))
model.add(BatchNormalization())

# CONV6
model.add(Conv2D(4*base_hidden_units, (3,3), padding='same', kernel_regularizer=regularizers.l2(weight_decay)))
model.add(Activation('relu'))
model.add(BatchNormalization())
model.add(MaxPooling2D(pool_size=(2,2)))
model.add(Dropout(0.4))

# FC7
model.add(Flatten())
model.add(Dense(num_classes, activation='softmax'))

# print model summary
model.summary()

# data augmentation
datagen = ImageDataGenerator(
    featurewise_center=False,
    samplewise_center=False,
    featurewise_std_normalization=False,
    samplewise_std_normalization=False,
    zca_whitening=False,
    rotation_range=15,
    width_shift_range=0.1,
    height_shift_range=0.1,
    horizontal_flip=True,
    vertical_flip=False)
    
# compute the data augmentation on the training set
datagen.fit(x_train)

# training
batch_size = 64
epochs=125

from keras.callbacks import ModelCheckpoint  

checkpointer = ModelCheckpoint(filepath='model.125epochs.keras', verbose=1, save_best_only=True)

# you can try any of these optimizers by uncommenting the line
# optimizer = keras.optimizers.rmsprop(learning_rate=0.001,decay=1e-6)
# optimizer = keras.optimizers.adam(learning_rate=0.0005,decay=1e-6)

optimizer = keras.optimizers.RMSprop(learning_rate=0.0003,decay=1e-6)
model.compile(loss='categorical_crossentropy', optimizer=optimizer, metrics=['accuracy'])
#history = model.fit_generator(datagen.flow(x_train, y_train, batch_size=batch_size), callbacks=[checkpointer],
#                steps_per_epoch=x_train.shape[0] // batch_size, epochs=epochs,verbose=2,
#                validation_data=(x_test,y_test))

history = model.fit(x_train, y_train, batch_size=batch_size, epochs=epochs,
          validation_data=(x_valid, y_valid), callbacks=[checkpointer],
          verbose=2, shuffle=True)

# evaluating the model
scores = model.evaluate(x_test, y_test, batch_size=128, verbose=1)
print('\nTest result: %.3f loss: %.3f' % (scores[1]*100,scores[0]))

# plot learning curves of model accuracy
pyplot.plot(history.history['accuracy'], label='train')
pyplot.plot(history.history['val_accuracy'], label='test')
pyplot.legend()
pyplot.show()
```