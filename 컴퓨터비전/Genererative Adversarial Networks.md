## GAN architecture
GAN은 적대적 학습(adversarial)에 기반을 두고 있다. 기본적으로 서로 경쟁하는 두 개의 신경망으로 구성된다.
+ Generator(생성기): 랜덤 노이즈를 받아 원래 데이터 셋에서 샘플링 된 것처럼 보이는 이미지를 생성
+ Discriminator(판별기): 관측값이 원래 데이터 셋에서 온 것인지 생성기의 위조물인지 예측

이러한 경쟁은 모든 데이터 분포를 모방할 수 있도록 도와준다. GAN은 일반적으로 다음과 같은 단계를 거치게 된다.
1. 생성기는 랜덤 숫자를 받아 이미지를 반환
2. 생성된 이미지는 실제 정답 데이터 셋에서 가져온 이미지 스트림과 함께 판별기에 입력된다
3. 판별기는 실제 이미지와 가짜 이미지를 모두 받아서 0과 1사이의 확률을 반환

생성기는 flatten된 벡터로 시작하는 역 CNN으로 구성되고, 이미지는 학습 데이터 셋의 이미지와 유사한 크기로 업 스케일 된다.

![500](Pasted%20image%2020240619002511.png)
### Deep Convolutional GANs(DCGANs)
2014년의 원래 GAN 논문에서는 생성기와 판별기 모델을 구축하기 위해 MLP가 사용되었다. 그러나 이후 CNN이 판별기에게 더 큰 예측력을 제공해 생성기의 정확성과 전체 모델의 정확성을 향상시키는 것이 입증되었다.

이러한 유형의 GAN을 Deep Convolutional GAN(DCGAN)라고 하며, 2016년 Alec Radford 등에게 개발되었다.
### Discriminator
판별자의 목표는 이미지가 가짜인지, 진짜인지 예측하는 것. 이것은 전형적인 supervised classification 문제이므로, 전통적인 분류 모델을 사용할 수 있다. CONV와 sigmoid activation을 사용해 모델을 구성할 수 있다.

![500](Pasted%20image%2020240619003347.png)

```python
def descriminator_model():
	discriminator = Sequential()
	diesriminator.add(Conv2D(32, kernel_size=3, strides=2, 
			input_shape=(28, 28, 1), padding='same'))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Conv2D(64, kernel_size=3, strides=2, padding='same'))
	discriminator.add(ZeroPadding2d(padding=((0,1), (0,1))))

	discriminator.add(BatchNormalization(momentum=0.8))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Conv2D(128, kernel_size=3, strides=2, padding='same'))
	discriminator.add(BatchNormalization(momentum=0.8))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Conv2D(256, kernel_size=3, strides=2, padding='same'))
	discriminator.add(BatchNormalization(momentum=0.8))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Flatten())
	discriminator.add(Dense(1, activation='sigmoid'))

	discriminator.summary()

	img_shape = (28, 28, 1)
	img = Input(shape=img_shape)

	probability = discriminator(img)
	return Model(img, probability)

```
### Generator
임의의 데이터를 받아 훈련 데이터 셋을 모방하여 가짜 이미지를 생성하는 것. 훈련 데이터 셋의 완벽한 복제품을 만들어 판별자를 속이는 것이 목표.

훈련이 진행되면서 생성자는 점차 나아지게 되는데, 판별자도 동시에 훈련되고 있어 생성자는 판별자가 자신의 속임수를 배우면서 계속해서 개선되어야 한다.

![500](Pasted%20image%2020240619003552.png)

Feature map을 스케일링하기 위한 업샘플링이 필요하다. 전통적인 CNN은 입력 이미지를 다운샘플링하기 위해 pooling layer를 사용하지만, 반대로 입력 픽셀의 각 행과 열을 반복하여 이미지 크기를 스케일링하는 업샘플링을 사용한다. `keras.layers.UpSampling2D(size=(2,2))`

![500](Pasted%20image%2020240619004115.png)

```python
def generator_model():
	generator = Sequential()
	generator.add(Dense(128 * 7 * 7, activation = 'relu', input_dim = 100))
	generator.add(Reshape((7, 7, 128)))
	generator.add(UpSampling2D(size=(2,2)))

	generator.add(Conv2D(128, kernel_size=3, padding='same'))
	generator.add(BatchNormalization(momentum=0.8))
	generator.add(Activation('relu'))
	generator.add(UpSampling2D(size=(2,2)))

	generator.add(Conv2D(64, kernel_size=3, padding='same'))
	generator.add(BatchNormalization(momentum=0.8))
	generator.add(Activation('relu'))
	
	generator.add(Conv2D(1, kernel_size=3, padding='same'))
	generator.add(Activation('tanh'))
	generator.summary()

	noise = Input(shape=(100, ))
	fake_image = generator(noise)
	return Model(noise, fake_image)
```
### Training GAN
판별자와 생성자 모델을 결합하여 end-to-end generative adversarial network을 학습시킨다. 
+ 판별자는 훈련 예제와 생성자가 생성한 이미지 모두 올바른 레이블을 할당할 확률을 최대화하여 더 나은 판별자가 되도록 훈련된다.
+ 생성자는 판별자를 속일 확률을 최대화하는 방향으로 훈련하게 된다.

![500](Pasted%20image%2020240619005524.png)

모델의 훈련과정은 두 가지 과정으로 이루어지게 된다.
1. 판별자 훈련. 생성자로부터 생성된 이미지와 훈련 데이터로 레이블된 이미지르 받아 두 이미지를 분류하는 방법을 학습.

```python
discriminator = discriminaotr_model()
discriminator.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

noise = np.random.nornal(0, 1, (batch_size, 100))
gen_imgs = generator.predict(noise)

# Train the discriminator (real classified as ones and generated as zeros)
d_loss_real = discriminator.train_on_batch(imgs, valid)
d_loos_fake = discriminator.train_on_batch(gen_imgs, fake)
```

2. 생성자 훈련. 생성자가 이미지를 잘 위조했는지 여부를 판별자가 알려줘야 함. 따라서 생성자를 훈련시키기 위해 판별자와 생성자 모델로 구성된 결합 네트워크를 만들어 학습을 진행해야 함
	+ 생성자를 훈련할 때 생성자와 판별자가 서로 다른 방향으로 움직이게 되는 손실 함수를 가지고 있기 때문에, 판별자 모델의 가중치를 고정해야 한다.
	+ 판별자 가중치를 고정하지 않으면 생성자가 학습하는 방향으로 판별자가 끌리게 되어 생성된 이미지를 진짜로 예측하게 될 가능성이 높아지게 된다.
	+ 판별자 모델의 가중치를 고정하는 것은 이전에 판별자를 훈련할 때 컴파일한 기존 판별자 모델에 영향을 미치지 않는다.

![500](Pasted%20image%2020240619005906.png)


```python
generator = generator_model()
z = Input(shape=(100, ))
image = generator(z)

discriminator.trainable = False
valid = discriminator(img)
combinded = Model(z, valid)

combined.compile(loss='binary_crossentropy', optimizer=optimizer)
g_loss = self.combined.train_on_batch(noise, valid)
```

각 epoch마다 컴파일된 두 모델(판별자와 결합 모델)이 동시에 훈련된다. 훈련 과정에서 판별자와 생성자는 모두 개선되게 된다. epoch 후에 결과를 출력하여 생성자가 합성 이미지를 생성하는 성능을 관찰 가능

![500](Pasted%20image%2020240619010407.png)
### GAN minimax function
GAN은 최적화 문제라기보다 zero-sum game에 더 가깝다. zero-sum game에서는 총 utility 점수가 플레이어들 사이에 분배되고, 한 플레이어의 점수가 증가하면 다른 플레이어의 점수가 감소하게 된다. 

AI 분야에서는 이러한 문제를 minimax game 이론이라고 한다. Minimax는 턴 기반의 두 플레이어 게임에서 일반적으로 사용되는 의사결정 알고리즘이다. 이 알고리즘의 목표는 최적의 다음 수를 찾는 것. 
+ 한 플레이어는 최대화자(maximizer)로서 가능한 높은 점수를 얻으려고 하고,
+ 다른 플레이어는 최소화자(minimizer)로서 최대화자의 점수를 낮추려고 반대 움직임을 취한다

![500](Pasted%20image%2020240619010757.png)

판별자의 목표는 이미지의 올바른 레이블을 얻는 확률을 최대화하는 것이고, 반면에 생성자의 목표는 적발될 확률을 최소화하는 것이다. 이를 식으로 표현하면 다음과 같다.

![500](Pasted%20image%2020240619011021.png)

위 식에서 왼쪽 부분을 최대화, 오른쪽 부분을 최소화하는 방향으로 학습이 진행된다.

### Evaluating GAN models
초기에는 사람이 진짜 이미지 같다고 생각해서 평가를 진행하였다. (주관적인 방법) 그래서 객관적인 평가 지표가 필요했고, 다음 두 지표를 통해 객관적인 평가가 가능하다.
+ Inception score: 높으면 높을 수록 GAN의 성능이 높다고 판단.
	+ High predictability of the generated image(생성한 이미지가 얼마나 진짜 같은 지)
	+ Diverse genearted samples(생성한 이미지가 얼마나 다양하게 생성되는 지)
+ Frechet inception distance(FID):Frechet distance라는 지표를 이용해  inception score를 좀 개선한 형태. 낮으면 낮을 수록 성능이 좋음
	+ inception score보다 GAN을 평가하는 성능이 좋지만, 사용하는 sample의 사이즈가 50,000개가 넘어야만 사용할 수 있음
## GAN Appliactions
+ Text-to-photo synthesis: text를 입력하면, 이미지를 생성해주는 Stack GAN 이용
+ Image-to-image translation(Pix2Pix GAN): 하나의 이미지를 다른 스타일로 바꿔주는 GAN
+ Image super-resolution GAN(SRGAN: Super GAN): 해상도가 낮은 이미지를 해상도를 높여주는 GAN
## Building GAN

![500](Pasted%20image%2020240619011831.png)

```python
from __future__ import print_function, division
from keras.datasets import fashion_mnist
from keras.layers import Input, Dense, Reshape, Flatten, Dropout
from keras.layers import BatchNormalization, Activation, ZeroPadding2D
from keras.layers.advanced_activations import LeakyReLU
from keras.models import Sequential, Model
from keras.optimizers import Adam
import numpy as np
import matplotlib.pyplot as plt

# download and visualize the data
(training_data, _), (_, _) = fashion_minst.load_data()

x_train = training_data / 127.5 - 1.
x_train = np.expand_dims(x_train, axis = 3)

def visualize_input(img, ax):
	ax.imshow(img, cmap='gray')
	width, height = img.shape
	thresh = img.max() / 2.5
	for x in range(width):
		for y in range(height):
			ax.annotate(str(round(img[x][y], 2)), xy=(y, x), 
						horizontalalignment='center',
						verticalalignment='center',
						color='white' if img[x][y]<thresh else 'black')

fig = plt.figure(figsize = (12, 12))
ax = fig.add_subplot(111)
visualize_input(training_data[3343], ax)

# generator
def build_generator():
	generator = Sequential()
	generator.add(Dense(128 * 7 * 7, activation = 'relu', input_dim = 100))
	generator.add(Reshape((7, 7, 128)))
	generator.add(UpSampling2D())

	generator.add(Conv2D(128, kernel_size=3, padding='same', activation='relu'))
	generator.add(BatchNormalization(momentum=0.8))
	generator.add(UpSampling2D())

	generator.add(Conv2D(64, kernel_size=3, padding='same', activation='relu'))
	generator.add(BatchNormalization(momentum=0.8))
	
	generator.add(Conv2D(1, kernel_size=3, padding='same', activation='relu'))

	noise = Input(shape=(100, ))
	fake_image = generator(noise)
	return Model(noise, fake_image)

# discriminator
def build_discriminator():
	discriminator = Sequential()
	diesriminator.add(Conv2D(32, kernel_size=3, strides=2, 
						input_shape=(28, 28, 1), padding='same'))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Conv2D(64, kernel_size=3, strides=2, padding='same'))
	discriminator.add(ZeroPadding2d(padding=((0,1), (0,1))))

	discriminator.add(BatchNormalization(momentum=0.8))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Conv2D(128, kernel_size=3, strides=2, padding='same'))
	discriminator.add(BatchNormalization(momentum=0.8))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Conv2D(256, kernel_size=3, strides=2, padding='same'))
	discriminator.add(BatchNormalization(momentum=0.8))
	discriminator.add(LeakyReLu(alpha=0.2))
	discriminator.add(Dropout(0.25))

	discriminator.add(Flatten())
	discriminator.add(Dense(1, activation='sigmoid'))

	img = Input(shape=(28, 28, 1))
	probability = discriminator(img)
	return Model(img, probability)

# combined model
optimizer = Adam(learning_rate=0.0002, beta_l=0.5)

discriminator = build_discriminator()
discriminator.compile(loss='binary_crossentropy', optimizer= optimizer, 
						metrics=['accurcay'])
discriminator.trainable = False

generator = build_generator()

z = Input(shape=(100, ))
img = generatoir(z)

valid = discriminator(img)
combined = Model(inputs=z, outputs=valid)
combined.compile(loss='binary_crossentropy', optimizer=optimizer)

# train function
def plot_generated_images(epoch, generator, examples=100, dim=(10, 10), 
						  figsize=(10, 10)): 
	noise = np.random.normal(0, 1, size=[examples, latent_dim])
	generated_images = generator.predict(noise)
	generated_images = generated_image.reshape(examples, 28, 28)

	plt.figure(figsize=figsize)
	for i in range(generated_images.shape[0]):
		plt.subplot(dim[0], dim[1], i+1)
		plt.imshow(generated_images[i], interpolation='nearest', cmap='gray_r')
		plt.axis('off')
	plt.tight_layout()
	plt.savefig('gan_genrated_image_epoch_%d.png' % epoch)


def train(epochs, batch_size=128, save_interval=50):
	valid = np.ones((bath_size, 1))
	fake = np.zeros((bath_size, 1))

	for epoch in range(epochs):
		# train discriminator
		idx = np.random.randint(0, x_train.sahpe[0], batch_size)
		imgs = x_train[idx]

		noise = np.random.normal(0, 1, (batch_size, 100))
		gen_imgs = generator.predict(noise)

		d_loss_real = discriminator.train_on_batch(imgs, valid)
		d_loss_fake = discriminator.train_on_batch(gen_imgs, fake)
		d_loss = 0.5 * np.add(d_loss_real, d_loss_fake)

		# train generator
		g_loss = combined.train_on_batch(noise, valid)
		print("%d [D loss: %f, acc: %.2f%%] [G loss: %f] " 
			% (epoch, d_loss[0], 100*d_loss[1], g_loss))

		if eopch % save_interval == 0:
			plot_generated_images(epoch, geneartor)

train(epochs=1000, batch_size=32, save_interval=50)
```

