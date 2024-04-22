## Applications
소리처리를 통해 할 수 있는 작업들은 다음과 같다.
+ Audio classification: 음성 인식
+ Speech recognition: 컴퓨터가 사람이 발성한 음성을 인식하여 내용을 텍스트로 변환하는 기술
	+ 사람의 음성 신호를 수학적 모델링과 딥러닝 알고리즘을 사용하여 분석하고 처리하여 음성 신호를 텍스트로 변환한다.
	+ 이 기술은 음성 인식 소프트웨어, 음성 인식 시스템, 음성 검색, 자동 번역, 음성 인식 음악 및 영화 검색 등 다양한 응용 분야에서 사용된다.
	+ Feature extraction: 정규화, 스펙트로그램(또는 MFCC 등)의 기법을 사용한 오디오 신호 전처리
	+ Acoustic Model: 각 시간 단계 t마다 어휘 문자 c에 대한 확률 분포 P_t(c)를 예측하는 네트워크
	+ 디코딩
		+ Greedy (argmax): 디코더를 위한 가장 간단한 전략. 가장 높은 확률을 가진 문자는 전달되고, 내용에 대한 의미론적 이해에 관계없이 각 시간 단계에서 선택된다.
		+ Language model을 사용하여 context를 추가할 수 있으므로 acoustic model의 오류를 수정할 수 있다.
+ Sound event detection: 특정 음향 사건 또는 소리를 감지하는 기술
+ Sound event localization: 소리가 발생한 위치를 감지하는 기술. 음향 사건 또는 소리를 감지하는 것뿐만 아니라 그 소리가 어디서 발생했는지 파악하는 것도 포함
+ Audio Generation & denoising: 음향을 생성해 내거나 노이즈를 제거하는 기술
+ etc...

## Preprocessing
딥러닝 모델의 입력으로 사용하기 위해서는 전처리(특징 추출) 과정이 필요하다. 소리(파형)는 시간 영역보다 주파수 영역에서 더 많은 정보를 가지고 있음

※ 푸리에 변환(Fourier transform): 시간 영역에서 주기적인 신호를 주파수 영역으로 변환하는 수학적인 기법

![|400](Pasted%20image%2020240422221303.png)


일반적으로 푸리에 변환을 해 소리를 주파수 영역 데이터로 변환한 후 특징을 추출하게 된다. 
+ MFCC(Mel-Frequency Cepstral Coefficients): Mel 스펙트럼을 계산한 후 로그를 취하고 DCT(discrete cosine transform)을 적용하여 얻은 계수
+ FFT(Fast FT): 기본 푸리에변환(DFT)보다 빠르다는 특징을 가지고 있는데, 현재는 기술에 발전에 따라 DFT와 FFT의 속도차이가 없음


푸리에 변환을 이용하는 경우 소리는 시계열 데이터임에도 불구하고 시간에 대한 정보를 나타낼 수가 없게 된다. 이를 해결하기 위해 STFT가 연구되었다. 시계열 정보를 넣기 위해 원본 데이터를 일정 slot으로 나눠 각각 FFT를 한 뒤 모아서 사용한다. 

![|400](Pasted%20image%2020240422222056.png)

이렇게 변환을 진행하고, 난 결과를 spectogram 형태로 사용한다. spectorgram은 주파수 영역에서 시간에 따른 신호의 변화를 시각화하는 방법을 말한다. 인간의 청각 특성을 반영한 주파수 척도를 Mel scale이라고 한다. mel scale은 주파수 간 간격의 청각적인 인식 간격 비율에 대응하는 단위로 측정하게 된다. 즉, 낮은 주파수에 높은 감도를 가지고 있고, 높은 주파수에 낮은 감도를 가지고 있다.

Spectorgram에 mel scale을 적용한 결과를 Mel-Spectrogram이라고 한다. 인간의 청각 특성을 고려하여 주파수 영역을 변환하여 특정 추출에 이용한다.

```python
import numpy as np
import librosa 
import librosa.display
import matplotlib.pyplot as plt

nfft = 2048 # 나눌 time slot 단위
win_len = nfft # 그 slot 내에서 win_length만큼 fft를 수행 nfft >= win_length
hop_len = win_len // 2
nb_mel_bands = 40
sr = 16000 # sample rate: 아날로그 신호를 디지털 신호로 변환할 때 단위시간당 샘플링한 횟수를 의미

audio_file = './pig_data/sample/farm01_R0001_0001_16000.wav'

y, sr = librosa.load(audio_file, sr=sr)
D = nb.abs(librosa.stft(y, n_fft=nfft, win_length=wien_len, hop_length=hop_len))
mel_spec = librosa.feature.melspectorgram(S=D, sr=sr, n_mels=nb_mel_bands, hop_length=hop_len, win_length=win_len).T

librosa.display.specshow(librosa.amplitude_to_db(mel_spec.T, ref=0.0002), sr=sr, hop_length=hop_len, y_axis='mel', x_axis='time')
plt.colorbar(format='%2.0f dB')
plt.show()
```

![|400](Pasted%20image%2020240422223010.png)
## Audio Classification
소리를 이미지 데이터로 변환한 후에는 일반적인 image classification과 동일하다.
### VGGish
Image에서 사용되는 VGG 모델을 소리에 적용한 연구로, 주로 소리 데이터의 특징을 추출하는데 사용된다. 

![|400](Pasted%20image%2020240422223230.png)
## Sound Event Detection
특정 음향 사건 또는 소리를 감지하는 기술로, 어느 시간에 어떤 소리가 발생했는지 감지하는 기술이다. 일반적인 입력과 결과는 아래와 같다. 

![|400](Pasted%20image%2020240422223353.png)
### SED-CRNN
입력 데이터를 특정 단위로 자른 뒤 STFT를 수행해 spectorgram feature를 뽑아 낸 후 DL 학습을 시켜 감지를 하는 모델

![|400 ](Pasted%20image%2020240422223557.png)

이 모델의 경우 결과로 일정 threshold 값을 기준으로 cough의 발생 여부를 측정한다. cough detection model의 output score가 threshold 이상이면 1아니면, 0이라고 판단하게 된다.

이 모델의 성능 평가 기준으로는 Segment based 정확도와 Event based 정확도를 사용한다.

![|400](Pasted%20image%2020240422223756.png)

![|400](Pasted%20image%2020240422223805.png)
