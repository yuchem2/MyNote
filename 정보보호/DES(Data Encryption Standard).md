
2001년에 [[AES(Advanced Encryption Standard)]]가 등장하기 전까지 널리 사용되던 암호화 스키마

1977년에 NBS(현재는 미국 미국 표준 기술 연구소(NIST))에 의해 연방정보처리표준인 PIBS PUB 46으로 발행되었다.

$64 \; bit$ 데이터를 블럭으로 사용하고, $56 \; bit$의 비밀 키를 사용하는 [[Block Cipher]]

## History
+ IBM에서 "Lucifer" cipher를 개발했다. 이는 *Feistel*이 이끄는 팀에 의해 개발되었고, $168 \; bit$의 비밀키를 사용했다
+ 이 암호를 상업 목적으로 NSA 등에서 재 개발을 진행하였다.
+ 1973년에 NBS는 연방표준암호의 제안서 모집을 시작했고, 
+ IBM에서는 "Revised Lucifer"를 제출했고 이 암호가 DES로 채택되었다

DES가 표준으로 채택되었음에도 Lucifer가 $128 \; bit$을 사용하는 반면에 $56 \; bit$을 사용하고 있기 때문에 이에 대한 내용이 계속 논쟁의 요소가 되었다.

하지만 이후의 사건들과 공개적인 분석들은 이러한 설계가 적절했다는 것을 보여주고 있다. 

곧 수년 간 DES는 가장 널리 사용되던 [[Block Cipher Symmetric Encryption]] Algorithm이 되었다. 

## *Encrpytion*
먼저 암호화 함수의 입력은 암호화할 평문과 비밀 키이다. 일반적으로 $64\; bit$의 평문이 사용되고, $56 \; bit$의 비밀 키가 사용된다. 아래 그림은 일반적인 DES의 암호화 과정을 보여준다.

![[Pasted image 20231017093851.png | 550]]
<div align="center">General Depiction of DES Encrpytion Algorithm</div>

### 1. Initial Permutation(IP)
평문을 임의의 permutation을 선택해 transposition을 수행한다
### 2. Round Structure
총 16개의 라운드가 존재하며 각 라운드에서 입력의 입력은 $32 \; bit$씩 반으로 나눠 생각한다([[Feistel Cipher]]와 유사)
+ $L_i = R_{i-1} \qquad R_i = L{i-1} \oplus F(R_{i-1}, K_{i})$
+ $32 \; bit\; R_{i-1}$와 $48 \; bit \; K_i$의 연산은 다음과 같이 수행된다
	+ Perm $E$를 통해 $R_{i-1}$를 $48 \; bit$로 늘린 후 $k_i$와 함께 round function $F$의 입력으로 한다.
	+ 이 연산이 수행된 뒤 8개의 S-box을 통과하게 되고, $32\; bit$의 결과를 얻을 수 있고, 이를 다시 $32 \; bit$ perm $P$를 통해 permutation을 수행한다. 이 결과가 $R_i$이다
	+ 여기서 각 S-box는 $6 \; bits$를 입력으로 받아 $4 \; bits$ 출력을 한다. 각 S-box는 $4 \times 16 \; matrix$(구조는 모두 동일 but 값이 다름)이며 row entity는 $6 \; bits$에서 맨 앞 $bit$와 맨 뒤 $bit$이고, col entity는 나머지 $bit$이다. 

![[Pasted image 20231017095411.png | 550]]
<div align="center">DES Round Structure</div>

### 3. Key Schedule
각 라운드에서 subkey $K_1...K_{16}$이 사용된다. subkey는 다음과 같은 과정을 통해 만들어진다.
+ 먼저 평문과 동일하게 비밀 키 $K$에 대해 inital permuation이 수행되어 $56 \; bit$를 생성하며 이를 $28 \; bit$씩 반으로 나눈다
+ 각 라운드에서 $LK, \; RK$을 key rotation schedule $K$에 의해 각각 하나 또는 두 개로 나눠 회전시킨다. 
+ 그 후 다시 permuation을 수행해 $48 \; bit$의 결과를 생성해 이를 $K_i$로 사용한다
### 4. 32-bit swap
모든 Round가 종료된 뒤 preoutput이라는 과정을 통해 $L_{16}, R_{16}$이 각각 $32\; bit \; swap$된다. 
### 5. Inverse initail permutation
그 후 IP의 역이 수행되며 이 연산의 결과가 최종적인 암호문이 된다. 

## Decrpytion
[[Feistel Cipher]]와 유사한 형태의 연산을 수행함으로 복호화 과정은 암호화 과정을 다시 수행함으로써 가능하며 이때  subkey만을 반대로 적용한다. 
## Example
아래 표는 기술된 평문과 비밀 키를 이용해 DES 암호화를 수행하는 과정에서 각 과정의 결과를 말한다. 
+ Plaintet:      $02468aceeca86420$
+ Key:            $0f1571c947d9e859$
+ Ciphertext: $da02ce3a89ecac3b$

![[Pasted image 20231017101720.png | 600]]
<div align="center">DES Example</div>

![[Avalanche Effect]]
위 예시를 통해 봤을 때 DES는 강력한 [[Avalanche Effect]]를 가진다.

## Strength
### Key size
$56 \; bit$의 비밀 키를 사용하기 때문에 총 $2^{56} = 7.2 \times 10^{16}$의 비밀 키가 존재한다. 이로 인해 [[Brute-force attack]]은 사실상 비실용적이다. 평균적으로 가능한 비밀 키의 절반을 탐색한다고 가정하면 $1 \micro s$에 하나의 DES 암호화를 수행하는 기기는 암호를 푸는데 1000년 이상이 소요된다. 

하지만 최근 기술의 발전으로 가능할 수 있다.
+ 1997년 인터넷을 통해 암호를 푸는데 몇 달 정도 소요
+ 1998년 dedicated HW(DES cracker)는 암호를 푸는데 몇 일 정도만 소요
+ 1999년 암호를 푸는데 약 22시간 소요

DES는 기술의 발전으로 인해 암호를 푸는 데 소요시간이 엄청나게 줄었지만 여전히 [[Brute-force attack]]은 평문을 알아야 암호를 풀 수 있다. 

하지만 암호가 결국 풀린다는 것을 통해 DES의 대체가 필요해졌다. ([[AES(Advanced Encryption Standard)]])

### [[Cryptanalysis]]
DES Algorithm의 특성을 이용해 [[Cryptanalysis]]이 가능할 가능성이 존재한다. 여기서 초점은 각 반복에서 사용되는 8개의 s-box에 있었다. s-box에 대한 설계 기준, 그리고 전체 algorithm에 대한 설계 기준이 공개되지 않았기 때문에 s-box의 약점을 알고 있는 사람들에게 [[Cryptanalysis]]가 가능하도록 구성되었다는 의혹이 존재하기 때문이다. 

이러한 의혹으로 연구가 계속되었고, 수년에 걸쳐 s-box들의 규칙성과 예기치 못한 행동들이 다수 발견되었다. 그럼에도 불구하고 여전히 s-box들에서 추정되는 치명적인 약점들을 발견하는데 성공하지 못했다.
### [[Timing Attacks]]
![[Timing Attacks]]
[HEVI99]는 비밀 키의 Hamming weight(number of bits equal to one)를 산출하는 접근법에 대해 말하고 있다. 이것은 실제 비밀 키 값을 알기에는 거리가 멀지만 첫 접근으로 볼 수 있다. [HEVI99]의 저자들은 DES가 성공적인 [[Timing Attacks]]에 상당히 저항적인 것으로 보이지만 탐색할 여러 방법은 제시한다. 

이러한 가능성에도 [[Timing Attacks]]으로 DES 혹은 [[Triple DES]], [[AES(Advanced Encryption Standard)]]와 같은 강력한 [[Symmetric encryption]] 암호에 대해 성공할 가능성이 낮다. 
### [[Analytic Attacks]]
![[Analytic Attacks]]

DES에 대한 몇 가지 [[Analytic Attacks]]이 존재할 수 있다. 대표적으로 [[Differential Cryptanalysis]]와 [[Linear Cryptanalysis]]가 존재한다.
#### [[Differential Cryptanalysis]]
![[Differential Cryptanalysis]]

유일한 비선형 부분인 s-box에 대한 분석을 진행한다. simple s-box(입력: $x_0x_1x_2$ 출력: $y_0y_1$)를 가정할 때 예시는 아래와 같다.
+ $s-box = \begin{matrix}\qquad x_1x_2 \\\begin{matrix} &&00 & 01 & 10 & 11  \end{matrix} \\ x_0 \;\begin{matrix} 0 \\1  \end{matrix} \begin{pmatrix} 10 & 01 & 11 & 00 \\00 & 10 & 01 & 11 \end{pmatrix} \\ \end{matrix}$
+ 평문을 $X_1=110, \; X_2 = 010$, 비밀 키를 $K=011$라고 가정하자
+ $X_1 \oplus K = 101, \quad X_2 \oplus K = 001, \quad s-box(X_1\oplus K) = 10, \quad s-box(X_2\oplus K) = 01$
+ s-box의 출력이 10이 될 수 있는 입력은 $X_1\oplus K \in \{000, 001\}$이고 01이 될 수 있는 입력은 $X_2\oplus K \in \{001, 110\}$이 된다
+ 평문 $X_1, X_2$를 알고 있음으로 위 식에 양변에 각각 $X_1\oplus$와 $X_2\oplus$를 취하면 다음과 같다.$$\begin{align}X_1\oplus X_1\oplus K &\in \{110, 011\} \\ X_2\oplus X_2\oplus K & \in \{011, 100\}\end{align} $$
+ 위 식은 결국 $K \in \{110, 011\} \quad K\in \{011, 100\}$이므로 $K=011$이다.

예시를 통해 봤을 때 결국 s-box의 입력과 출력을 알면 키 추측이 가능하다

일반적인 DES에 대해 평문 암호문 쌍들에 대해 [[Differential Cryptanalysis]]를 적용하는 것은 아래와 같다.
+ 입력에 있어서 이미 알려진 차이를 가지고 알려진 차이를 찾는다
+ 확률 $p$로 출력 차이를 제공하는 약간의 입력 차이를 가지고, 더 높은 확률의 입출력 차이가 발생하는 사례를 찾는다.
+ 만약 이런 사례가 존재한다면 라운드에 사용된 서브 비밀 키들을 추론할 수 있다.
+ 그 후 여러 라운드에 걸쳐 확률 $p$를 감소시키며 위 방법을 반복한다. 
여기서 이론적으로 암호를 풀기 위해 선택된 평문 $2^{47}$개가 필요하다.

#### [[Linear Cryptanalysis]]
![[Linear Cryptanalysis]]

DES에서 수행된 transformation을 설명하기 위해 선형 근사치를 찾는 것을 기반으로 수행된다. 알려진 평문 $2^{47}$개로 공격할 수 있지만 여전히 실행하기엔 어려움이 존재한다

## Design Criteria
[COPP94]에서 Coppersmith에 의해 보고되었는데, 다음과 같은 내용이 존재한다.
+ S-box에 대한 7가지 기준은 다음과 같다
	+ 비선형적이여야 한다
	+ [[Differential Cryptanalysis]]의 저항성을 가져야 한다
	+ 좋은 *confussion* 을 제공한다
	+ etc...
+ permutation $p$에 대한 3가지 기준은 다음과 같다 
	+ *diffusion*을 향상시킨다.
	+ etc...
