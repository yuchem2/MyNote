## [[Hash Function]]
![[Hash Function]]

[[암호화 알고리즘(Cypotographic algorithms)]]에서는 이러한 [[Hash Function]]을 이용해 메시지의 변화를 감지하거나 [[Digital Signature]]을 생성하는데 사용된다. 이외에도 다양하게 사용될 수 있다. 

[[Hash Function]]을 사용하는 경우에 가장 큰 특징은 다른 [[암호화 알고리즘(Cypotographic algorithms)]]과 다르게 키를 사용하지 않는다는 점이다. 

![[Pasted image 20231129162112.png | 600]]

<div align="center">Cryptographic Hash Function; h = H(M)</div>
## Uses
### [[Message Authentication]]

![[Pasted image 20231129162315.png| 600]]

<div align="center">Simplified Examples of Use of a Hash Function for Message Authentication</div>

위 예시의 그림은 4가지 방법으로 [[Hash Function]]을 이용해 Message Authentication을 하는 예시를 보여주고 있다. 
1. 메시지를 hash code로 변환한 뒤 메시지와 hash code를 함께 하나의 데이터로 하여 암호화해 전송한다. 전송을 받은 측에서는 복호화하고, hash code 부분과 메시지를 나눈 뒤 메시지를 hash code로 변환해 두 값이 같은지 비교한다. 
2. 변환된 hash code만을 암호화 한 후 원본 메시지와 하나의 데이터로 하여 전송한다. 전송 받은 측에서는 암호화된 부분을 복호화하고, 원본 메시지를 hash code로 변환해 두 값이 같은 지 비교한다. 
3. 암호화 없이 수행하는 방법으로, 비밀 값 $S$를 선택해 이를 원본 메시지와 함께 하나로 데이터로 하여 hash code로 변환한 뒤 그 hash code와 원본 데이터를 하나의 데이터로 하여 전송한다. 전송 받은 측에서는 이를 hash code부분과 원본 데이터로 나눈 뒤 비밀 값 $S$와 원본 데이터 부분을 hash code로 변환해 두 값이 같은지 비교한다. 
4. 3번 과정에서 전달 부분에 암호화만 추가된 형태이다.

위 경우에서 암호화는 모두 [[Symmetric encryption]] 알고리즘을 사용한다. 
### [[Digital Signature]]

![[Pasted image 20231129162530.png | 600]]

<div align="center">Simplified Examples of Digiter Signature</div>

위 예시의 그림은 4가지 방법으로 [[Hash Function]]을 이용해 [[Digital Signature]]을 하는 예시를 보여주고 있다. 
1. 원본 메시지를 hash code로 변환한 뒤에 이를 자신의 private key를 통해 암호화 한다. 함호화된 결과를 원본 메시지와 함께 전송한다. 전달받은 측에서는 원본 메시지와 암호화된 hash code부분을 나누고, 원본 메시지를 hash code로 변환하고, 암호화된 부분을 전송 측의 public key로 복호화한다. 그 두 결과의 값을 비교한다.
2. 1번 과정에서 전달할 때 추가적으로 [[Symmetric encryption]]부분이 추가된 형태이다
### Other Uses
+ one-way password file을 생성하는데 사용
+ intrusion detection과 virus detection을 하는 데 사용
+ pseudorandom function이나 [[PRNG(Pseudo-Random Number Generator)]]를 만드는데 사용
## Security Requirements
1. 메시지의 크기와 관계 없이 적용이 가능해야 한다
2. 고정된 길이의 $h$를 얻을 수 있어야 한다
3. 메시지에 관계 없이 $H(M)=h$가 계산하기 쉬워야 한다
4. One-way property: $h$를 알고 있을 때 [[Hash Function]]의 입력을 찾는 것은 불가능해야 한다
5. 'Weak' collision resistance: $x$를 알고 있을 때 $H(x)=H(y)$을 만족하는 $y$를 찾는 것은 불가능해야 한다.  
6. 'Strong' colision resistance: $H(x) = H(y)$를 만족하는 $x, y$를 찾는 것은 불가능해야 한다.
### Brute-Force Attacks
공격에 걸리는 평균적인 시간은 [[Hash Function]]을 구성하고 있는 알고리즘에 따라 결정되지 않고, 결과 $h$의 길이에 따라 결정된다. 
#### Preimage and Second Preimage Attacks
이 공격은 주어진 $h$와 같은 결과를 나오게 하는 $y$를 찾는 공격이다. $y$를 랜덤으로 선택해 충돌이 발생할 때까지 시도한다. $m$-bit hash code를 찾는데 걸리는 노력은 $2^m$이 필요하다. 평균적으로 공격자는 $2^{m-1}$ 개의 $y$를 생성함으로써 $H(y) = h$인 $y$를 찾을 수 있다. 
#### Birthday Attacks(Collision Resitant Attack)
$H(x)=H(y)$를 만족하는 $x, y$를 찾는 공격. 위 방법보다 평균적으로 적은 노력을 시도해 공격이 성공할 수 있다. 이러한 이유는 *birthday paradox*에 의해 설명되고 있다. 

$[0:N-1]$ 범위에서 [[Uniform distribution]]을 만족한다고 할 때, 랜덤한 수를 뽑는다고 가정하자. 이러한 경우에서 반복적으로 같은 수를 뽑을 수를 확률은 $\sqrt N$개를 뽑은 후에 50%를 초과하게 된다. 그러므로, $m$-bit hash code에 대해서 랜덤한 data block을 $\sqrt {2^m}=2^{m/2}$만큼 뽑으면 같은 hash code를 가지는 두 data block을 찾을 수 있다. 

이러한 이론적 배경을 가지고 공격은 다음과 같이 수행된다. 이 과정에서 [[Cryptographic Hash Function#Digital Signature]]의 1번 과정과 같은 전달이 수행되고 있다고 가정한다. 
1. 공격자는 공격하고자 하는 메시지 $x$와 같은 의미를 가지는 $2^{m/2}$개의 메시지 $x'$를 생성하고, 메시지와 그 hash code를 저장한다. 
2. 공격자는 전송자의 서명이 필요한 사기 메시지 $y$를 준비한다. 
3. 공격자는 사기 메시지 $y$와 동일한 의미를 가지는 다른 형태의 메시지 $y'$를 생성한다. 각각의 메시지 $y'$에 대해 hash code를 계산하고 앞서 $H(x')$와 같은 결과를 같은 $y'$를 찾을 때까지 반복해 $y'$을 생성한다. 

이러한 방식으로 공격을 진행할 때 $64\;bit$ hash code를 사용하는 경우 $2^{32}$의 시도만으로 공격이 성공할 수 있다. 이로 인해 크기가 큰 hash code와 [[MAC(Message Authentication Code)]]의 필요성이 대두됬다.
### [[Cryptanalysis]]
알고리즘의 일부 속성을 이용해 완전한 검색 이외에 일부에 대한 공격을 수행하고자 한다. 이러한 공격의 저항성을 측정하는 방법은 [[Brute-force attack]]에 필요한 노력과 비교하는 것이다. 즉, 이상적인 [[Cryptographic Hash Function]]은 [[Brute-force attack]]보다 크거나 같은 [[Cryptanalysis]] 시도를 필요로 한다. 

[[Hash Function]]을 사용하는 [[암호화 알고리즘(Cypotographic algorithms)]]의 경우 대부분 반복적인 구조를 사용하고 있다. 이는 메시지를 블록으로 다루기 때문이다. 아래와 같은 형태로 구성되어 수행되는데, 여기서 함수 $f$가 반복적으로 수행된다. 그러므로 공격자들은 함수 $f$의 충돌에 집중해 공격을 시도한다. 

![[Pasted image 20231130115452.png | 600]]

<div align="center">General Structure of Secure Hash Code</div>

## Algorithm
### Insecure Simple [[Hash Function]]
#### Bit-by-Bit Exclusive-OR(XOR)
모든 블록에 대해 bit-by-bit XOR 연산을 수행한다. 다음과 같은 식으로 표현할 수 있다.  $$C_i = b_{i1} \oplus b_{i2} \oplus ... \oplus b_{im}$$ where $$\begin{align}C_i&=ith \; bit \; of \; the \; hash\; code \; 1\leq i \leq n \\ m&=number \; of \; n-bit\; blocks \; in \; the \; input\\b_{ij} &= ith \; bit \; in \; jth\; block\end{align}$$
이러한 형식의 연산은 각 비트 자리마다의 간단한 parity bit를 만드는 것과 동일하다. 데이터 무결성 검사로서 랜덤 데이터에 대해 효과적인 방법이다. 하지만 예측 가능한 형식적인 데이터에 대해서는 효과가 낮아진다. 
#### One-bit Circular Shift
위 방법을 향상시킨 방법으로 다음과 같은 방식으로 수행한다.
1. $n$-bit hash code를 0으로 초기화
2. 데이터의 각 연속하는 $n$-bit 블록에 다음과 같은 연산을 수행한다.
	+ 현재 hash code를 1 bit씩 왼쪽으로 shift
	+ hash code 블록에 XOR을 수행한다.

데이터 무결성검사로서는 효과적이지만, 보안적으로는 쓸모없는 방식이다.

### Secure [[Hash Function]]
