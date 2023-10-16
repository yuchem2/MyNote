
Feistel은 *ideal block cipher*에 근사하기 위해 두 개이상의 간단한 암호 조합을 통해 암호학적으로 강한 암호를 만든 [[Product Ciphers]]의 방식을 이용하였다.

이 접근 방식에서 핵심은 $k \; bits$길이의 비밀 키와 $n \; bits$의 블럭을 이용한 [[Block Cipher]]를 개발해 *ideal block cipher*가 목표로 하는 $2^n!$개가 아니라 $2^k$개의 치환을 구성하는 것이다. 

여기서 선택한 방식이 Substitution과 Permutation을 교대로 사용하는 방식이다. 
+ Substituion: 각 평문의 요소는 유일하게 암호문으로 대체된다
+ Permutation: 평문 요소의 순열은 그 순열의 permutation으로 대체된다. (각 요소들의 순서만 변경)

즉, Fiestel은  *confusion*과 *diffustion* 함수를 교대로 사용하는 [[Product Ciphers]]를 개발하자는 제안을 실제로 구상한 것이다 ([[Substitution-Permutation(S-P) networks]])

## Structure
---
먼저 *invertible* [[Product Ciphers]]에 기초를 둔 형태이다. 즉, 암호화 과정을 반대로 수행하면 복호화가 수행되는 형태의 [[Product Ciphers]] 구조 이다. 

전체 과정은 $n$개의 라운드로 구성되고, 모든 라운드는 같은 동일한 기능을 수행한다. 즉, $n$번의 기능이 반복적으로 수행된다. 임의의 라운드 $i$에서 수행되는 연산은 다음과 같다. 
+ 입력된 데이터의 길이가 $2w \; bits$이고, 비밀 키 $K$로부터 서브 비밀 키를 생성해 $K_i$라고 정의한다.
+ 일반적으로 서브 비밀키와 비밀 키는 상이하다. 
+ 입력된 데이터를 두 부분 $LE_{i-1}, \; RE_{i-1}$으로 나눈다. 이 때 각 부분의 길이는 $w \; bits$이다. (최초의 입력 평문은 $LE_0, RE_0$로 구성된다)
+ 입력된 데이터에 대해 Substition은 $LE_{i-1}$에 수행된다. Round function $F$의 출력과 XOR 연산을 수행함으로써 Substition이 수행된다. XOR 연산으로 인해 *invertible*한 암호가 될 수 있었다.
+ Round function $F$는 입력으로 서브 비밀 키 $K_{i-1}$와 $RE_{i-1}$를 받는다. 
+ Permutation은 $LE_{i-1}, \; RE_{i-1}$를 서로 바꾸면서 수행된다. 
+ 위 연산들을 정리해서 수식으로 표현하면 다음과 같다. $$\begin{align}LE_{i}& = RE_{i-1} \\ RE_i & = LE_{i-1} \oplus F(RE_{i-1}, K_i)\end{align} $$
암호화 과정과 복호화 과정은 서로 반대 순서로 작동하기 때문에 복호화 라운드 $i$에 대한 입력을 $LD, \; RD$로 나눈다고 정의하면 다음과 같이 표현할 수 있다.
$$\begin{align} LD_i & = RE_{n-i} \\ RD_i & = LE_{n-i} \\\\ LD_{i} & = RD_{i-1} \\ RD_i & = LD_{i-1} \oplus F(RD_{i-1}, K_{n-i+1}) \\ \end{align}$$
위 수식들을 활용해 새로운 정의를 유도해 낼 수 있다.
$$\begin{align}LD_{16} & = RD_{15} & \Rightarrow LE_0 & = LE_1\\ RD_{16} & = LD_{15} \oplus F(RD_{15}, K_1) & \Rightarrow LE_0 &= RE_1 \oplus F(LE_1, K_1)
\end{align}$$

![[Pasted image 20231016135525.png | 500]]
<div align="center">Feistel Encryption and Decryption</div>
