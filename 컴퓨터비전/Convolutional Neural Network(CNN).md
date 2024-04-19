## Image classification using MLP
초기에 2D 데이터 이미지를 분류하기 위해 MLP(ANN)을 이용해 학습을 진행하였다. 하지만 이 경우 2D 이미지 데이터를 1D vector로 변환하는 과정이 필요하다. 이러한 과정을 image flattening이라고 한다. 이 과정을 거쳐서 학습을 진행하는 경우 다음과 같은 문제들이 존재한다. 
1. 공간적인 특징이 없어진다. 2D 데이터를 1D로 변경했기 때문에, 같은 모양이더라도, 모양이 회전되거나 뒤집히거나 등의 문제를 인식하지 못할 수 있다. 
2. 계산 부하가 심하다. MLP의 경우 모든 레이어가 서로 연결되어 있어 레이어가 많아지거나 한 레이어에 perceptron이 많아질수록 계산량이 급격하게 증가하게 된다.

### Code
```python
from keras.models import Sequential
from keras.layers import Flatten, Dense

model = Sequential()
model.add(Flatten(input_shpae = (28, 28)))
model.add(Dense(512, actiavtion = 'relu'))
model.add(Dense(512, activation = 'relu'))
model.add(Dense(10, activation = 'softmax'))

model.summary()
```
## CNN Architecture
MLP의 이러한 문제들을 해결하기 위해 등장한 기법이 convolution 연산이다. 이 기법을 사용한 레이어를 convolutional layer라고 하고, 이를 이용한 네트워크를 Convolutional Neural Network(CNN)이라고 한다. CNN은 DL 기법으로 ML과 다르게 특징 추출을 직접 수행하게 된다.

CNN을 사용하는 경우 위에서 언급한 문제들이 해소되는데, 먼저 입력 자체를 2D 이미지로 받기 때문에 공간적 특징이 무시되지 않는다. 또, CNN 레이어의 경우 sliding windows를 통해 각 perceptron이 이전 레이어의 전체 값을 받는 것이 아니라 일부만 받기 때문에 비교적 계산 부하가 낮아지게 된다.

![|400](Pasted%20image%2020240419154014.png)

일반적인 CNN 구조는 특징을 추출하는 부분과 분류, 감지 등의 모델의 목적에 해당하는 연산을 하는 부분과 예측을 하는 부분으로 나눠지게 된다. 특징 추출을 convolutional layer를 통해 수행하고, 여전히 FC layer들이 분류 등의 문제에서 효율적이기 때문에 FC layer를 사용한다. 그 후 결과를 목적에 따라 출력하는 것이다.

![|400](Pasted%20image%2020240419155744.png)

CNN에서 중요한 레이어는 convolutional layer(CONV), pooling layer(POOL), fully connected layer(FC)이다. 
### Convolutional layers
Kernel(filter)라는 window를 이미지의 픽셀 단위로 움직이면서 의미있는 특징을 추출하는 작업을 수행한다. Kernel이 지나가면서 실제 이미지에서 추출하는 값을 가진 field를 receptive field라고 한다. 또, 이 결과로 나온 convoluted image를 feature map 혹은 activation map이라고 한다.

![|400](Pasted%20image%2020240419154702.png)


각 convolutional layer에서 조정할 수 있는 값은 kernel size, stride size, padding size이다. 
+ 일반적으로 kernel size는 $3 \times 3, 5\times 5, 7\times 7$ 정도가 많이 사용된다. 
+ stride는 kernel이 움직이는 정도를 조절하는 값이다.
+ padding은 원본 이미지에 추가적인 외각 픽셀을 더해 추출되는 feature map의 크기를 조정할 수 있다.

![|400](Pasted%20image%2020240419160215.png)
### Pooling Layer
Pooling layer는 subsampling이라고도 하는데, 다음 레이어로 전달되는 parameter의 수를 줄임으로써 네트워크의 크기를 줄이는 데 사용된다. 풀링 방법에는 max, min, average 방법이 존재하며, 입력된 데이터에서 중요한 특징 정보를 유지하면서 크기를 줄이는 데 사용한다.

![|400](Pasted%20image%2020240419160431.png)
![|400](Pasted%20image%2020240419160447.png)
### Fully connected layers
말 그대로 레이어에 존재하는 모든 perceptron이 다음 레이어와 모두 연결되어 있는 레이어를 말하며 이는 전형적인 MLP 형태의 구조이다. 이 방법은 주로 분류 문제에서 사용되는데, 여전히 분류 문제에서 MLP가 좋은 성능을 보이기 때문이다.

### 3D Images Convolution
![|400](Pasted%20image%2020240419162947.png)

## Image Classification Using CNNs
### Load MNIST dataset
```python
# Load MNIST database
from keras.datasets import mnist

(x_train, y_train), (x_test, y_test) = mnist.load_data()

print("train set %d" % len(x_train))
print("test set %d" % len(x_test))

# visualize training images
import matplotlib.pyplot as plt
%matplotlib inline
import matplotlib.cm as cm
import numpy as np

fig = plt.figure(figsize=(20, 20))
for i in range(6):
	ax = fig.add_subplot(1, 6, i+1, xticks=[], yticks=[])
	ax.imshow(x_train[i], cmap='gray')
	ax.set_title(str(y_train[i]))

# visualize image more detail
def visualize_input(img, ax):
    ax.imshow(img, cmap='gray')
    width, height = img.shape
    thresh = img.max()/2.5
    for x in range(width):
        for y in range(height):
            ax.annotate(str(round(img[x][y],2)), xy=(y,x),
                        horizontalalignment='center',
                        verticalalignment='center',
                        color='white' if img[x][y]<thresh else 'black')

fig = plt.figure(figsize = (12,12))
ax = fig.add_subplot(111)
visualize_input(X_train[0], ax)
```
### Preprocessing
```python
from keras.utils import to_categorical

# rescale and one-hot encoding
num_classes = 10
x_train, x_test = x_train.astype('float32')/255, x_test.astype('float32')/255
y_train, y_test = to_categorical(y_train, num_classes), to_categorical(y_test, num_classes)

# reshape to fit our model
img_rows, img_cols = 28, 28
x_train = x_train.reshape(x_train.shape[0], img_rows, img_cols, 1)
x_test = x_test.reshape(x_test.shape[0], img_rows, img_cols, 1)
input_shape = (img_rows, img_cols, 1)
```
### Building model architecture
```python
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout

model = Sequential()
# CONV_1: add CONV layer with RELU activation and depth = 32 kernels
model.add(Conv2D(32, kernel_size=(3, 3), strides=1, padding='same',activation='relu',input_shape=(28,28,1)))
# POOL_1: downsample the image to choose the best features
model.add(MaxPooling2D(pool_size=(2, 2)))


# CONV_2: here we increase the depth to 64
model.add(Conv2D(64, (3, 3), strides=1, padding='same', activation='relu'))
# POOL_2: more downsampling
model.add(MaxPooling2D(pool_size=(2, 2)))


# flatten since too many dimensions, we only want a classification output
model.add(Flatten())
model.add(Dropout(rate=0.3))
# FC_1: fully connected to get all relevant data
model.add(Dense(64, activation='relu'))
model.add(Dropout(rate=0.5))
# FC_2: output a softmax to squash the matrix into output probabilities for the 10 classes
model.add(Dense(10, activation='softmax'))

model.summary() # print the model architecture summary
```

#### Number of parameters
위 형태로 모델을 구성한 후 `model.summary()`를 실행하면 아래 그림과 같은 결과가 나온다. 마지막을 보면 해당 모델이 사용하는 총 parameter의 수를 확인할 수 있는데, 이결과는 다음과 같은 연산 식으로 계산된다. $$\text{number of params} = \text{fileters} \times \text{kernel size} \times \text{depth of the previous layer} + \text{number of filters(for biases)}$$e.g. CONV_2의 경우: $\text{params} = 64 \times 3 \times 3 \times 32 + 64 = 18496$

![|400](Pasted%20image%2020240419161900.png)

#### Dropout
Dropout은 overfitting을 막기 위한 기법 중 가장 일반적인 방법으로, 몇 개의 perceptron을 꺼버리는 역할을 수행한다. 이를 통해 특정 특징에 대한 학습만 과도하게 되는 것을 방지해 모든 특징에 대한 학습이 균등하게 수행되도록 유도한다. 일반적으로 Dropout은 FC에서 많이 적용된다. 

![|400](Pasted%20image%2020240419162745.png)

### Compile and trining
```python
from keras.callbacks import ModelCheckpoint

# complie the model
model.compile(loss='categorical_crossentropy', optimizer='rmsprop', metrics=['accuracy'])

# train the model
chekpointer = ModelCheckpoint(filepath='model.weights.best.keras', verbose=1, save_best_only=True)
hist = model.fit(x_train, y_train, batch_size=32, epochs=12, validation_data(x_test, y_test), callbacks=[checkpointer], verbose=2, shuffle=True)
```

### Load and Calculate accuracy
```python
# load 
model.load_weights('model.weights.best.keras')

# calculate accuracy
score = model.evaluate(x_test, y_test, verbose=0)
accurcy = 100 * score[1]
print(accurcy)
```
## Image Classification for Color Images
```python
import keras
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
from keras.datasets import cifar10
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout
from keras.callbacks import ModelCheckpoint

# load the pre-shuffled train and test data
(x_train, y_train), (x_test, y_test) = cifar10.load_data()

# visualize the training set
fig = plt.figure(figsize=(20,5))
for i in range(36):
    ax = fig.add_subplot(3, 12, i + 1, xticks=[], yticks=[])
    ax.imshow(np.squeeze(x_train[i]))

# rescale
x_train, x_test = x_train.astype('float32')/255, x_test.astype('float32')/255

# one-hot encode the labels
num_classes = len(np.unique(y_train))
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

# break train dataset into train/validation
(x_train, x_valid) = x_train[5000:], x_train[:5000]
(y_train, y_valid) = y_train[5000:], y_train[:5000]

# define model architecture
model = Sequential()

model.add(Conv2D(filters=16, kernel_size=2, padding='same', activation='relu', input_shape=(32, 32, 3)))
model.add(MaxPooling2D(pool_size=2))

model.add(Conv2D(filters=32, kernel_size=2, padding='same', activation='relu'))
model.add(MaxPooling2D(pool_size=2))

model.add(Conv2D(filters=64, kernel_size=2, padding='same', activation='relu'))
model.add(MaxPooling2D(pool_size=2))

model.add(Dropout(0.3))
model.add(Flatten())
model.add(Dense(500, activation='relu'))
model.add(Dropout(0.4))
model.add(Dense(10, activation='softmax'))

model.summary()

# compile
model.compile(loss='categorical_crossentropy', optimizer='rmsprop', metrics=['accuracy'])

# train
checkpointer = ModelCheckpoint(filepath='model.weights.best.keras', verbose=1, save_best_only=True)
hist = model.fit(x_train, y_train, batch_size=32, epochs=100,
          validation_data=(x_valid, y_valid), callbacks=[checkpointer],
          verbose=2, shuffle=True)

# load
model.load_weights('model.weights.best.keras')

# evaluate
score = model.evaluate(x_test, y_test, verbose=0)
print('\n', 'Test accuracy:', score[1])

# visualize some predictions
y_hat = model.predict(x_test)
cifar10_labels = ['airplane', 'automobile', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck']
fig = plt.figure(figsize=(20, 8))

for i, idx in enumerate(np.random.choice(x_test.shape[0], size=32, replace=False)):
    ax = fig.add_subplot(4, 8, i + 1, xticks=[], yticks=[])
    ax.imshow(np.squeeze(x_test[idx]))
    pred_idx = np.argmax(y_hat[idx])
    true_idx = np.argmax(y_test[idx])
    ax.set_title("{} ({})".format(cifar10_labels[pred_idx], cifar10_labels[true_idx]),
                 color=("green" if pred_idx == true_idx else "red"))    
```
