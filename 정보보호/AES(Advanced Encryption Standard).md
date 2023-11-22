2001년에 NIST로부터 공개된 [[Block Cipher Symmetric Encryption]]을 사용하는 [[Block Cipher]]로, [[DES(Data Encryption Standard)]]를 대체하기 위해 등장하였다

## History

[[DES(Data Encryption Standard)]]는 이론상의 공격과 철저한 비밀 키 탐색 공격을 통해 추론될 수 있다는 것이 밝혀졌고, 새로운 표준 암호의 필요성이 등장했다. 

AES 개발 이전에 [[Triple DES]]도 존재해 이것을 사용할 수도 있었지만 [[Triple DES]]의 경우 느렸고, 블럭의 크기도 작은 문제가 있어 US NIST에서 1997년에 새로운 표준 암호를 위한 제안서를 받기로 결정하였다.

1998년 6월 15개의 지원이 있었고, 1999년 9월에 5개가 최종 후보로 선정되었다. 그 후 최종적으로 *Rijndael*이 2000년 8월에 AES로 채택되었다. 그 후 2001년에 [FIPS PUB 197]이라는 이름으로 표준이 공개되었다

### Requirements
1. 비밀 키 [[Block Cipher Symmetric Encryption]] 암호
2. 블럭 크기가 $128 \; bit$여야 하고, 암호 키는 $128, 192, 256\; bit$여야 한다
3. [[Triple DES]]보다 강력하고 빨라야 한다
4. 활용할 수 있는 기간이 20-30년
5. 모든 상세 정보와 디자인 컨셉을 제공해야 한다
6. [[C]] 혹은 [[Java]]로 구현

### Evaluation Criteria
+ Initial criteria
	+ security: effor to practically [[Cryptanalysis]]
	+ cost: computational
	+ algorithm & implementation characteristics
+ Final criteria
	+ general security
	+ software & hardware implementation ease
	+ implementation attacks
	+ flexibility(in en/decrypt, keying, other factors)

### AES Shortlist
+ after testing and evaluation, shortlist in Aug, 99
	+ MARS(미국, IBM): 복잡하지만 빠르고, 높은 보안성
	+ RC6(미국, RSA): 간단하고 빠름, 낮은 보안성
	+ Twoflish(미국, Counterpane Systems): 복잡하지만 빠르고, 높은 보안성
	+ Rijndael(벨기에, 대학): 명쾌하고, 빠르고 적당한 보안성
	+ Serpent(영국/덴마크/이스라엘, 대학): 느리지만 명쾌하고, 높은 보안성
+ then, subject to further analysis & comment
+ saw contrast between algorithms with
	+ 복잡한 적당한 round와 간단하고 많은 round
	+ 기존 암호를 정제하는 것과 새로운 암호

## AES Cipher - Rijndael
벨기에의 *Rijmen-Daemen*에 의해 설계된 암호로, $128, 192, 256 \; bit$길이의 비밀 키를 제공하고, 블럭 크기가 $128\; bit$이다.  키 길이에 따라 AES-128, AES-192, AES-256으로 불린다

[[Feistel Cipher]]와 유사하게 작동하며 데이터를 $4 \; bytes$의 4개의 그룹으로 나눠 다루며 각 라운드에서 전체 블록에 대해 연산이 수행된다. 

설계의 목표는 알려진 공격으로부터 저항성을 가지고, 대부분의 CPU에서 빠르고, 긴밀하게 작동하는 것과 간단하게 구현하는 것이었다. 

### Simple Sturcture

| AES Encrpytion Process                       | AES Encrpytion and Decrpytion               |
| -------------------------------------------- | ------------------------------------------- |
| ![[Pasted image 20231021192144.png]] | ![[Pasted image 20231021193750.png]]|

1. 입력 평문($16 \; bytes)$은 각 $4 \;bytes$의 그룹으로 나눈다. 즉, 총 4개의 그룹으로 나눠진다. (column을 기준으로 정렬)
2. Inital transformation: XOR key material & incomplete last round
3. 암호화 과정은 동일한 작동을 하는 9, 11, 13개의 라운드와 다른 마지막 라운드로 구성한다. 
	+ byte subsitution: 1개의 s-box가 각 byte에 대해서 수행
	+ shift rows: groups/columns 단위로 permutation이 수행
	+ mix columns: $GF(2^8)$의 계산을 통해 substitution
	+ add round key: bitwise XOR state with the expanded key
4. 마지막 라운드에서는 mix column과정이 제외된 상태로 수행된다. 이는 암호화, 복호화 과정이 reversible하게 작동하기 위함이다. 
5. 모든 연산은 XOR 혹은 테이블을 통해 수행된다. 그러므로 매우 빠르고 효과적으로 작동한다

#### Parameters
| Names                                  | 128bits  | 192 bits | 256 bits |
| -------------------------------------- |:--------:|:--------:|:--------:|
| Key Size(words/bytes/bits)             | 4/16/128 | 6/24/192 | 8/32/256 |
| Plaintext Block Size(words/bytes/bits) | 4/16/128 | 4/16/128 | 4/16/128 |
| Number of Rounds                       |    10    |    12    |    14    |
| Round Key Size(words/bytes/bits)       | 4/16/128 | 4/16/128 | 4/16/128 |
| Expanded Key Size(words/bytes)         |  44/176  |  52/208  |  60/240  |

#### Arithmetic
AES에서 수행되는 모든 연산은 $GF(2^8)$에서 수행되고, 사용되는 prime polynomial은 $m(x) = x^8 + x^4 + x^3 + x + 1$이다. 이 다항식을 2진수와 16진수로 표현하면 다음과 같다. $100011011_{(2)} = 11b$

e.g. $2\cdot 87 \bmod 11b = 100001110 \bmod 11b = 100001110 \oplus 100011011 = 00010101 = 21$
#### Round Steps

![[Pasted image 20231022120836.png | 600]]
<div align="center">AES Encryption Round</div>

##### Substitute Bytes Transformation 
SubBytes라고 불리는 이 단계는 단순히 s-box를 이용해index로 접근하여 이루어진다. 
+ s-box: $16 \times 16$ matrix인이 테이블은 $1\; byte$를 좌우로 $4\;bits$씩 나누어 각각 $x, y$로 하여 column value로, row value로 사용된다. 즉 $1\; byte$가 $yx$로 취급된다. 
+ s-box는 암호화에 사용되는 s-box와 복호화에 사용되는 inverse s-box로 구성된다. 
+ s-box는 "defined" transformation in $GF(2^8)$으로 설계되었다. 이를 통해 "알려진 [[Cryptanalysis]] 공격"에 저항성을 갖게 되었다.  

![[Pasted image 20231022111853.png | 600]]
<div align="center">Substitute byte transformation</div>

##### ShiftRows Transforamtion
입력으로 들어온 $4\times 4$ matrix에 대해 row를 기준으로 연산이 다음과 같이 수행된다.
+ 첫 번째 row에는 어떠한 작업도 수행하지 않는다
+ 두 번째 row에는 $1\;byte$씩 circular shift를 left로 수행한다
+ 세 번째 row에는 $2\;byte$씩 circular shift를 left로 수행한다
+ 네 번째 row에는 $3\;byte$씩 circular shift를 left로 수행한다
복호화 할 경우 위와 같은 연산을 right로 수행한다. (Inverse ShiftRows)

![[Pasted image 20231022114213.png | 600]]
<div align="center">Shift row transformation</div>

##### MixColumns Transformation
입력으로 들어온 $4\times 4$ matrix에 대해 column을 기준으로 다음과 같이 연산이 수행된다. $$\begin{bmatrix}02 & 03 & 01 & 01 \\ 01 & 02 & 03 & 01 \\ 01 & 01 & 02 & 03 \\ 03 & 01 & 01 & 02\end{bmatrix} \begin{bmatrix} s_{0,0} & s_{0,1} & s_{0,2} & s_{0,3} \\ s_{1,0} & s_{1,1} & s_{1,2} & s_{1,3} \\ s_{2,0} & s_{2,1} & s_{2,2} & s_{2,3} \\ s_{3,0} & s_{3,1} & s_{3,2} & s_{3,3}\end{bmatrix} = \begin{bmatrix} s'_{0,0} & s'_{0,1} & s'_{0,2} & s'_{0,3} \\ s'_{1,0} & s'_{1,1} & s'_{1,2} & s'_{1,3} \\ s'_{2,0} & s'_{2,1} & s'_{2,2} & s'_{2,3} \\ s'_{3,0} & s'_{3,1} & s'_{3,2} & s'_{3,3}\end{bmatrix}$$ 각 row에 대해 다음과 같이 쓸 수 있다.$$\begin{align} s'_{0, j} &= (2\cdot s_{0, j})\oplus(3\cdot s_{1, j}) \oplus s_{2, j} \oplus s_{3, j}  \\ s'_{1, j} &= s_{0, j}\oplus(2\cdot s_{1, j}) \oplus (3 \cdot s_{2, j}) \oplus s_{3, j} \\ s'_{2, j} &= s_{0, j}\oplus s_{1, j} \oplus (2 \cdot s_{2, j}) \oplus (3\cdot s_{3, j}) \\ s'_{3, j} &= (3\cdot s_{0, j})\oplus s_{1, j}\oplus s_{2, j} \oplus (2 \cdot s_{3, j}) \end{align}$$
복호화하는 경우에는 다음과 같은 연산을 수행한다. (Inverse MixColumns) $$ \begin{bmatrix}0E & 0B & 0D & 09 \\ 09 & 0E & 0B & 0D \\ 0D & 09 & 0E & 0B \\ 0B & 0D & 09 & 0E\end{bmatrix} \begin{bmatrix} s_{0,0} & s_{0,1} & s_{0,2} & s_{0,3} \\ s_{1,0} & s_{1,1} & s_{1,2} & s_{1,3} \\ s_{2,0} & s_{2,1} & s_{2,2} & s_{2,3} \\ s_{3,0} & s_{3,1} & s_{3,2} & s_{3,3}\end{bmatrix} = \begin{bmatrix} s'_{0,0} & s'_{0,1} & s'_{0,2} & s'_{0,3} \\ s'_{1,0} & s'_{1,1} & s'_{1,2} & s'_{1,3} \\ s'_{2,0} & s'_{2,1} & s'_{2,2} & s'_{2,3} \\ s'_{3,0} & s'_{3,1} & s'_{3,2} & s'_{3,3}\end{bmatrix} $$두 식에서 왼쪽에 있는 행렬을 곱하면 결과가 다음과 같이 나오는 것을 통해 암호화 과정에서 입력으로 들어온 행렬이 복호화 과정에서 출력으로 도출되는 것을 알 수 있다. $$\begin{bmatrix}02 & 03 & 01 & 01 \\ 01 & 02 & 03 & 01 \\ 01 & 01 & 02 & 03 \\ 03 & 01 & 01 & 02\end{bmatrix}\begin{bmatrix}0E & 0B & 0D & 09 \\ 09 & 0E & 0B & 0D \\ 0D & 09 & 0E & 0B \\ 0B & 0D & 09 & 0E\end{bmatrix}= \begin{bmatrix}1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1\end{bmatrix}$$
![[Pasted image 20231022114415.png | 600]]
<div align="center">Mix column transformation</div>

##### Add Round Key Transformation
$128\; bit$의 round key와함께 XOR 연산을 수행한다. column을 기준으로 XOR 연산을 수행하게 된다. XOR연산을 수행하기 때문에 복호화할 때도 동일하게 XOR연산을 수행한다. 간단하게 설계하기 위한 목표를 달성하기 위해 설계되었다. 

![[Pasted image 20231022112051.png | 600]]
<div align="center">Add round key transformation</div>

#### Key Expansion
AES key expasion alogrithm은 $4\; word(16\; bits)$의 key를 통해 $44/52/60 word(176/208/240 \;bits)$의 선형 배열을 만들어 낸다. 이 알고리즘을 통해 addRoundKey에서 사용되는 round key를 생성하게 된다. 다음과 같은 형식으로 수행된다. 
```pseudo code
KeyExpansion (byte key[16], word w[44]) {
	word temp
	for (i=0; i<4; i++) 
		w[i] = (key[4*i], key[4*i+1], key[4*i+2], key[4*i+3])
	
	for (i=4; i<44; i++) {
		temp = w[i-1];
		if (i mod 4 = 0) 
			temp = SubWord(RotWord(temp)) ⊕ Rcon[i/4]
		w[i] = w[i-4] ⊕ temp
	}
}
```
+ 첫 4 word는 key를 그대로 복사해 사용한다
+ 그 후 연산은 다음과 같이 수행된다. 
	+ if $i \bmod 4 =0$, $w_i = w_{i-4} \oplus g(w_{i-1})$
	+ else, $w_i=w_{i-4} \oplus w_{i-1}$

![[Pasted image 20231022132139.png | 600]]
<div align="center">AES Key Expansion</div>

### Example
Plaintext: 012345678abcdeffedcba9876543210
Key: 0f1571c947d9e8590cb7addaf7f698
Ciphertext: ff0b844a0853bf7c6934ab4364148fb9

#### Key Expansion & Encryption
| Key Expansion | Encryption |
| ------------- | ---------- |
| ![[Pasted image 20231022133321.png]]              | ![[Pasted image 20231022133408.png]]           |

#### [[Avalanche Effect]]
| Before change key | After change key |
| ----------------- | ---------------- |
|  ![[Pasted image 20231022133647.png ]]                 |  ![[Pasted image 20231022133828.png]]                |

### Decryption
[[DES(Data Encryption Standard)]]와 다르게 AddRound Key 단계를 제외하고 모든 단계가 역으로 수행되는 과정이 존재해 복호화 과정은 다르게 수행된다. 하지만 암호화 과정과 동일한 순서로 그에 해당하는 역 단계를 수행하며, AddRoundKey 단계의 수행은 [[DES(Data Encryption Standard)]]와 동일하게 암호화 과정과 동일하게 수행하며 암호 키만 반대로 적용한다. 

이러한 특징으로 인해 AES를 *Equivalent Inverse Cipher*라고 할 수 있다.

### Implementation Aspects
$8\; bit$ processor에 효율적이게 구현이 가능하다. 
+ byte substitution이 256개의 entries를 가진 table을 사용하기 때문(s-box의기)
+ shift rows는 간단한 byte shift이기 때문에 연산이 간단
+ add round key는 모든 연산은 byte단위 XOR 연산이기 때문에 이 또한 연산이 간단
+ mix columns은 $GF(2^8)$에 대한 matrix 곱 연산을 수행해야 하지만, 02, 03에 대한 곱셈만 수행하기 때문에 이를 표로 만들어 간단하게 구현할 수 있다.

$32 \; bit$ process에도 효율적이게 구현이 가능하다
+ 각 과정의 연산을 $32\; bit$ word로 재정의해 수행할 수 있다.

| SubBytes    | $b_{i, j} = S[a_{o,j}]$                                                                                                                                                                                                                                           |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ShiftRows   | $\begin{bmatrix}c_{0, j} \\ c_{1, j} \\ c_{2, j} \\ c_{3, j}\end{bmatrix} = \begin{bmatrix}b_{0, j} \\ b_{1, j-1} \\ b_{2, j-2} \\ b_{3, j-3}\end{bmatrix}$                                                                                                       |
| MixColumns  | $\begin{bmatrix}d_{0, j} \\ d_{1, j} \\ d_{2, j} \\ d_{3, j}\end{bmatrix} = \begin{bmatrix}02 & 03 & 01 & 01 \\ 01 & 02 & 03 & 01 \\ 01 & 01 & 02 & 03 \\ 03 & 01 & 01 & 02\end{bmatrix}\begin{bmatrix}c_{0, j} \\ c_{1, j} \\ c_{2, j} \\ bc{3, j}\end{bmatrix}$ |
| AddRoundKey | $\begin{bmatrix}e_{0, j} \\ e_{1, j} \\ e_{2, j} \\ e_{3, j}\end{bmatrix} = \begin{bmatrix}d_{0, j} \\ d_{1, j} \\ d_{2, j} \\ d_{3, j}\end{bmatrix} \oplus \begin{bmatrix}k_{0, j} \\ k_{1, j} \\ k_{2, j} \\ k_{3, j}\end{bmatrix}$                                                                                                                                                                                                                                                                  |
