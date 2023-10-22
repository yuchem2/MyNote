[[DES(Data Encryption Standard)]]의 대체로 등장한 [[Block Cipher Symmetric Encryption]]에 기초한 [[Block Cipher]]

이 암호의 가장 큰 특징은 [[DES(Data Encryption Standard)]]를 연속적으로 적용한 형태라는 점이고, 이러한 형태는 여러가지가 존재하지만 이 중에서도 [[DES(Data Encryption Standard)]]를 3번 적용한 이 암호가 여러 장점으로 인해 [[DES(Data Encryption Standard)]]의 대체로 가장 많이 사용되었다. 

그러나 여전히 한계가 존재했고, 이후 [[AES(Advanced Encryption Standard)]]가 이 암호의 새로운 대체로 사용되었다. 

## Double DES
---
단순히 [[DES(Data Encryption Standard)]]를 두 개의 비밀 키를 이용해 적용한 형태로 수식으로 나타내면 다음과 같다. $$\begin{align} C &=E(K_2, E(K_1, P)) \\ D&=D(K_1,D(K_2,C))\end{align}$$이 구조에서 필요한 key의 길이는 $56\times 2 = 112 \; bits$가 되어 암호학적인 관점에서 보안성이 매우 증가한 것처럼 보이지만 여러 문제점이 존재했다. 
![[Pasted image 20231022141329.png | 400]]
<div align="center">Double enryption</div>

### Meet in the Middle Attack
이 알고리즘의 핵심은 위 식의 암호화 부분을 다음과 같이 정의함으로부터 시작되었다. $$X=E(K_1, P) = D(K_2, C)$$알려진 평문, 암호문 쌍을 $(P, C)$라고 하고 공격의 과정을 설명하면 다음과 같다.
1. $P$를 가능한 모든 키($2^{56}$)에 대해 암호화하고, 이 결과를 표로 정리해 저장한다. 
2. 그 후 $C$를 가능한 모든 키($2^{56}$)에 대해 복호화하고, 이 결과가 표에 존재하는 지 확인한다. 
3. 같은 결과가 존재하는 경우 그 두 개의 키를 새로운 평문, 암호문 쌍에 대해 테스트 해본다.
4. 만약 올바른 결과를 도출하면 그 키들을 올바른 키라고 판단한다

이러한 공격이 성공할 확률은 $1-2^{-16}$이며 오직 $O(2^{56})$가 소요된다. 그러므로 암호 키의 길이가 2배로 증가했지만, [[DES(Data Encryption Standard)]]의 키를 유추하는 데 소요되는 시간($O(2^{55})$)에 비하면 보안성이 크게 증가하지 않은 것으로 보인다.

## Triple DES with Two Keys
---
이러한 이유로 인해 새롭게 [[DES(Data Encryption Standard)]]를 연속적으로 사용하는 방법이 필요했고, 두 개의 키를 사용하며 3번의 [[DES(Data Encryption Standard)]]를 적용하는 방법이 제시되었다. 이 과정을 수식으로 표현하면 다음과 같다. $$\begin{align} C & = E(K_1, D(K_2, E(K_1, P))) \\ D & = D(K_1, E(K_2, D(K_1, C)))\end{align}$$이 수식에서 암호학적인 중요성은 보이지 않지만 하나의 가장 큰 강점은 하나의 [[DES(Data Encryption Standard)]]도 암호화, 복호화를 할 수 있다는 것이다. $$\begin{align} C & = E(K_1, D(K_1, E(K_1, P))) = E(K_1, P) \\ D & = D(K_1, E(K_1, D(K_1, C)))=D(K_1, C)\end{align}$$
이 형태의 경우 공격이 성공하는데 걸리는 시간이 $O(2^{56} \times \frac{2^{64}}{n} = 2^{120-log_2n})$이다. 

## Triple DES with Three Keys
---
3개의 키를 사용해 3번의 [[DES(Data Encryption Standard)]]를 적용하는 경우 암호화, 복호화 과정을 수식으로 쓰면 다음과 같다. $$\begin{align} C & = E(K_3, D(K_2, E(K_1, P))) \\ D & = D(K_1, E(K_2, D(K_3, C)))\end{align}$$
인터넷을 기초로 한 응용 프로그램에서 이 형태의 [[Triple DES]]를 적용해 사용하고 있다. (PGP, [[S//MIME]])