1986년에 [[RSA]] Security에서 *Ron Rivest*에 의해 설계된 [[Stream Cipher]]로, 가변 키에 대해 byte단위 연산을 수행함

많은 곳에서 사용되고 있고, 그 중에서도 주로 웹 SSL/TLS, 무선 WEP, WPA에서 사용된다


## Structure
---
![[Pasted image 20231022170606.png | 600]]
<div align="center">RC4 Structure</div>

### Initialization of S
가변 키의 길이는 최소 $1\; byte$에서 최대 $256 \; bytes$이다. 가변키는 $256 \; byte$의 vector $S$를 생성하기 위해 사용된다. 이 $S$의 각 index는 $1\; byte$로 구성된다. 여기서 $S$에 대해서는 오직 permuation만 발생한다.
```pseudo code
for i = 0 to 255 do
	S[i] = i
	T[i] = K[i mod keylen]

j = 0
for i = 0 to 255 do
	j = (j + S[i] + T[i]) mod 256
	swap(S[i], S[j])
```

### Encryption
$S$가 한번 초기화 되면 초기에 입력되는 가변 키는 사용되지 않고, 메시지의 스트림 단위의 값을 섞는데 사용된다. 
```pusedo code
i, j = 0
for each message byte M_i
	i = (i + 1) mod 256
	j = (j + S[i]) mod 256
	swap(S[i], S[j])
	t = (S[i] + S[j]) mod 256
	C_i = M_i XOR S[t] like K_i
```


## Security
---
결과가 매우 비선형적인 특성을 가지고 있고, 알려진 공격에 대해 안전하다는 것이 입증되었다. (몇 개의 [[Cryptanalysis]]) 현실적으로 예측이 불가능한 것으로 밝혀졌다. 이러한 보안성이 유지될려면 사용된 가변 키를 다시 사용해서는 안된다는 점이 존재한다.

