## CNN design patterns
1. Feature extraction and classification

![500](Pasted%20image%2020240618180428.png)

2. Image depth increases, and dimensions decress

![500](Pasted%20image%2020240618180448.png)

3. Fully connected layers

![500](Pasted%20image%2020240618180505.png)
## LeNet-5
1998년 Lecun et al이 공개한 CNN로, 5개의 레이어 구성을 가지고 있다. 3개의 CONV, 2개의 FC 레이어로 구성되어 있다.

![500](Pasted%20image%2020240618180628.png)


```python
from keras.models import Sequential
from keras.layers import Conv2D, AveragePooling2D, Flatten, Dense

model = Sequential()
# conv1
model.add(Conv2D(filters = 6, kernel_size = 5, strides = 1, activation = 'tanh', 
				input_shape = (28, 28, 1), padding = 'same'))
model.add(AveragePooling2D(pool_size = 2, strides = 2, padding = 'valid'))
# conv2
model.add(Conv2D(filters = 16, kernel_size = 5, strides = 1, activation = 'tanh', 
				padding = 'same'))
model.add(AveragePooling2D(pool_size = 2, strides = 2, padding = 'valid'))
# conv3
model.add(Conv2D(filters = 120, kernel_size = 5, strides = 1, activation = 'tanh', 
				padding = 'same'))
model.add(Flatten())
model.add(Dense(units = 84, activation = 'tanh'))
model.add(Dense(units = 10, activation = 'softmax'))
model.summary()
```

LeNet-5은 scheduled decay learning 기법을 사용하고 있다. 

```python
def lr_schedule(epoch):
	if epoch <= 2:
		lr = 0.0005
	elif epoch > 2 and epoch <= 5:
		lr = 0.0002
	elif epoch > 5 and epoch <= 9:
		lr = 0.00005
	else:
		lr = 0.00001
	return lr
```

Hyperparameter를 다음과 같이 구성한 뒤 MNIST dataset을 이용해 성능을 테스트한다.

```python
from keras.callbacks import ModelCheckpoint, LearningRateScheduler

lr_scheduler = LearningRateScheduler(lr_schedule)
checkpoint = ModelCheckpoint(filepath='path_to_save_file/file.hdf5',
							monitor='val_acc',
							verbos=1,
							save_best_only=True)

callbacks = [checkpoint, lr_reducer]
model.compile(loss='categorical_crossentropy', optimizer='sgd',
			 matrics=['accuracy'])

hist = model.fit(X_train, y_train, batch_size=32, epochs=20,
				validation_data=(X_test, y_test), callbacks=callbakcs,
				verbose=2, shuffle=True)

score = model.evaluate(X_test, y_test, verbose=0)
accuracy = 100*score[1]
print('test accuracy: %.4%%' % accuracy)
```
## AlexNet
2012년 ILSVRC 이미지 분류 대회에서 우승한 모델로, 이 모델이 만들어진 가장 큰 동기는 더욱 복잡한 기능을 배울 수 있는 깊은 모델을 만들고자 함이었다.

Krizhevsky et al이 만든 이 모델은 1000개의 다른 class를 가진 1.2M개의 이미지 데이터인 ImageNet 데이터셋을 이용해 학습을 진행하였다. 

AlexNet은 처음으로 공개된 실제로 깊은 네트워크를 가진 모델로써 의미가 있다. LeNet과 매우 유사하지만, 훨씬 더 깊고, 큰 형태를 가지고 있다.

![500](Pasted%20image%2020240618182230.png)

AlexNet 모델이 학술분야에 기여한 사항은 다음과 같다
+ ReLU 활성화 함수 사용
+ Dropout layer 적용
+ Data augmentation 적용
+ Local response normalization 적용
+ L2 regularization 적용
+ 여러 개의 GPUs를 이용한 학습 시행 (병렬 컴퓨팅 = parallel computing의 아이디어)

```python
from keras.models import Sequential
from keras.regularizers import l2
from keras.layers import Conv2D, AveragePooling2D, Flatten, Dense, Activation, MaxPool2D, BatchNormalization, Droupout

model = Sequential()

# 1st layer
model.add(Conv2D(filters = 96, kernel_size = (11, 11), strides = (4, 4), 
				padding = 'valid', input_shape = (227, 227, 3)))
model.add(Activation('relu'))
model.add(MaxPool2D(pool_size = (3, 3), strides = (2, 2)))
model.add(BatchNormalization())

# 2nd layer
model.add(Conv2D(filters = 256, kernel_size = (5, 5), strides = (1, 1), 
				padding = 'same', kernel_regularizer=l2(0.0005)))
model.add(Activation('relu'))
model.add(MaxPool2D(pool_size = (3, 3), strides = (2, 2), padding = 'valid'))
model.add(BatchNormalization())

# 3rd layer
model.add(Conv2D(filters = 384, kernel_size = (3, 3), strides = (1, 1), 
				padding = 'same', kernel_regularizer=l2(0.0005)))
model.add(Activation('relu'))
model.add(BatchNormalization())

# 4th layer
model.add(Conv2D(filters = 384, kernel_size = (3, 3), strides = (1, 1), 
				padding = 'same', kernel_regularizer=l2(0.0005)))
model.add(Activation('relu'))
model.add(BatchNormalization())

# 5th layer
model.add(Conv2D(filters = 256, kernel_size = (3, 3), strides = (1, 1), 
				padding = 'same', kernel_regularizer=l2(0.0005)))
model.add(Activation('relu'))
model.add(BatchNormalization())
model.add(MaxPool2D(pool_size = (3, 3), strides = (2, 2), padding = 'valid'))

model.add(Flatten())

# 6th layer
model.add(Dense(units = 4096, activation = 'relu'))
model.add(Dropout(0.5))

# 7th layer
model.add(Dense(units = 4096, activation = 'relu'))
model.add(Dropout(0.5))

# 8th layer
model.add(Dense(units = 1000, activation = 'softmax'))

model.summary()
```

위 구조로 구현된 AlexNet은 2012 ILSVRC 대회에서 우승하였고, 이전에 비해 높은 성능을 보였다.
## VGGNet
2014년에 Visual Geometry Group에서 개발된 모델이다. AlexNet과 LeNet과 유사한 구조를 가지고 있지만 더욱 많은 CONV를 사용하고 있다. 

여러 layer 수의 모델이 존재하지만, 가장 유명한 모델은 VGG16으로 13개의 CONV, 3개의 FC로 구성되어 있다. 

VGG는 CNN의 hyperparameter인 kernel size, padding, strides와 같은 값을 어떻게 설정하는 것이 좋은지에 대한 논문이며, 이를 위해 아래와 같은 형태의 모델 구성들을 테스트 하였다.

![500](Pasted%20image%2020240618192809.png)

![700](Pasted%20image%2020240618192833.png)

Learning hyperparameter
+ mini-batch gradient descent with momentum of 0.9
+ learning rate: intially set 0.01 and then decreased by a factor of 10 when the validation set accuracy stop improving
## Inception and GoogLeNet
Inception network는 Google에서 2014년에 공개한 "Going Deeper with Convolutions"에서 처음 등장하였다. 해당 구조의 주요한 특징은 DL 내부의 컴퓨팅 리소스의 사용율을 향상시키는 것이다. 해당 논문에서 공개한 GooLeNet은 2014 ILSVRC에서 우승하였다. 이때부터 인간에 필적하는 성능을 보이기 시작했다.

22개의 layer를 사용했지만 일반적인 같은 layer 수의 CNN에 비해 약 12배 만큼의 parameter수를 감소시키면서도 높은 정확도를 보였다.

 Inception 구조: 필터 크기를 선택하고 pooling layer를 배치할 위치를 결정하는 대신 모든 필터와 pooling layer를 하나의 블록에 적용한 형태. 즉 고전적인 구조에서 레이어를 서로 위로 쌓는 대신에 서로 다른 커널 크기를 가진 여러 개의 CONV로 구성된 모듈.

![500](Pasted%20image%2020240618193859.png)

![500](Pasted%20image%2020240618193911.png)

위와 같은 구조로 단순하게 연산을 수행한다면 병렬 처리를 수행하므로 연산 수의 크기가 매우 커지게 된다. 입력 데이터의 크기가 32 x 32 x 200이고, 이러한 데이터가 5 x 5 CONV of 32 filter, 즉 5 x 5 x 32틀 통과한다고 하면, 계산해야 하는 곱의 수는 (32 x 32 x 200) x (5 x 5 x 32) = 163M 이다.

이를 해결하기 위해 연산을 수행하는 CONV 전에 1x1 CONV 레이어를 적용시키면, 이전처럼 수행하는 것보다 10배 정도의 연산 수를 줄일 수 있다. 이러한 방법을 dimensionality reduction layer이라고 한다. 해당 논문을 쓴 연구팀에서 이러한 Bootleneck layer(1x1 CONV)를 사용하더라도 성능을 감소시키지 않는 것도 확인을 하였다. 이러한 사항이 해당 논문의 가장 큰 기여이다.

![500](Pasted%20image%2020240618194713.png)

최종적으로 아래와 같은 Inception module을 완성할 수 있다.

![500](Pasted%20image%2020240618194850.png)

```python
def inception_module(x, filters_1x1, filters_3x3_reduce, filters_3x3, filters_5x5_reduce, filters_5x5, filters_pool_proj, name=None):  
    conv_1x1 = Conv2D(filters_1x1, kernel_size=(1, 1), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init)(x)  
  
    # 3x3 route  
    pre_conv_3x3 = Conv2D(filters_3x3_reduce, kernel_size=(1, 1), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (x)  
    conv_3x3 = Conv2D(filters_3x3, kernel_size=(3, 3), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (pre_conv_3x3)  
  
    # 5x5 route  
    pre_conv_5x5 = Conv2D(filters_5x5_reduce, kernel_size=(1, 1), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (x)  
    conv_5x5 = Conv2D(filters_5x5, kernel_size=(5, 5), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (pre_conv_5x5)  
  
    # pool route  
    pool_proj = MaxPool2D((3, 3), strides=(1, 1), padding='same') (x)  
    pool_proj = Conv2D(filters_pool_proj, kernel_size=(1, 1), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (pool_proj)  
  
    output = keras.layers.concatenate([conv_1x1, conv_3x3, conv_5x5, pool_proj], axis=3, name=name)  
  
    return output


input_layer = Input(shape=(224, 224, 3))  
x = Conv2D(64, (7, 7), strides=2, input_shape=(224, 224, 3), padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (input_layer)  
x = MaxPool2D((3, 3), strides=2, padding='same') (x)  
  
x = Conv2D(192, (3, 3), strides=1, padding='same', activation='relu', kernel_initializer=kernel_init, bias_initializer=bias_init) (x)  
x = MaxPool2D((3, 3), strides=2, padding='same') (x)  
  
# Inception layers  
x = inception_module(x, 64, 96, 128, 16, 32, 32, 'inception3a')  
x = inception_module(x, 128, 128, 192, 32, 96, 64, 'inception3b')  
x = MaxPool2D((3, 3), strides=2, padding='same') (x)  
  
x = inception_module(x, 192, 96, 208, 16, 48, 64, 'inception4a')  
x = inception_module(x, 160, 112, 224, 24, 64, 64, 'inception4b')  
x = inception_module(x, 128, 128, 256, 24, 64, 64, 'inception4c')  
x = inception_module(x, 112, 144, 288, 32, 64, 64, 'inception4d')  
x = inception_module(x, 256, 160, 320, 32, 128, 128, 'inception4e')  
x = MaxPool2D((3, 3), strides=2, padding='same') (x)  
  
x = inception_module(x, 256, 160, 320, 32, 128, 128, 'inception5a')  
x = inception_module(x, 384, 192, 384, 48, 128, 128, 'inception5b')  
  
# FC layers  
x = GlobalAveragePooling2D() (x)  
x = Dropout(0.4) (x)  
x = Dense(1000, activation='softmax') (x)

model = keras.Model(input_layer, x, name='inception_v1')  
model.summary(line_length=140)
```
## ResNet
Microsoft에서 2015년에 공개한 모델로, ressidual module이라는 새로운 구조와 hidden layer에 무거운 batch normalization을 적용하는 형태를 제안하였다.

이러한 기술을 통해 이전보다 매우 깊은 형태의 DL 모델을 구성할 수 있도록 하였다.(50, 101, 152개) 이렇게 깊은 레이어 수임에도, 적은 계산량을 요구하였다.

2015 ILSVRC에서 top-5 error rate가 3.57%가 나오면서, 인간보다 높은 성능을 보이는 모델이 등장하였다.

기존에 DL을 사용할 때 레이어가 깊어지면 깊어질 수록 vanishing gradient가 심해지는 문제가 있었는데, residual module은 이러한 문제를 해결할 수 있는 구조였다.

Residual module의 가장 핵심은 skip connection이다. Vanishing gradient 문제를 해결하기 위해 CONV 레이어를 통과하지 않는 새로운 shortcut path를 만들어 gradient가 소실되지 않도록 돕는다. 

![500](Pasted%20image%2020240618195532.png)

![500](Pasted%20image%2020240618195710.png)

![500](Pasted%20image%2020240618195719.png)

```python
## skip connection
x_shortcut = x
x = conv2D(filters = F1, kernel_size = (3, 3), strides = (1, 1)) (x)
x = Activation('relu') (x)
x = Conv2D(filters = F1, kernel_size = (3, 3), strides = (1, 1)) (x)
x = Add() ([x, x_shortcut])
x = activation('relu') (x)

## bottleneck_residual_block
def bottleneck_residual_block(x, kernel_size, filters, reduce=False, s=2):
	F1, F2, F3 = filters
	x_shortcut = x
	if reduce:
		x_shortcut = Conv2d(filters = F3, kernel_size = (1, 1), strides = (s, s)) (x)
		x = Conv2D(filters = F1, kernel_size = (1, 1), strides = (s, s), padding = 'valid') (x)
		x = BatchNormalization(axis = 3) (x)
		x = Activation('relu') (x)
	else:
		x = Conv2D(filters = F1, kernel_size = (1, 1), strides = (1, 1), padding = 'valid') (x)
		x = BatchNormalization(axis = 3) (x)
		x = Activation('relu') (x)
	x = Conv2D(filters = F2, kernel_size = (1, 1), strides = (1, 1), padding = 'same') (x)
	x = BatchNormalization(axis = 3) (x)
	x = Activation('relu') (x)
	
	x = Conv2D(filters = F3, kernel_size = (1, 1), strides = (1, 1), padding = 'valid') (x)
	x = BatchNormalization(axis = 3) (x)
	x = Activation('relu') (x)

	x = Add() ([x, x_shortcut])
	x = Activation('relu') (x)
	
	return x
```