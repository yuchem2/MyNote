
문자를 $5 \times 5$ 행렬에 저장한 것을 비밀키로 사용하는 암호로, [[Block Cipher]]의 아이디어를 제공한 암호이다. 또한 [[Mono-alphabetic Cipher]]에 비해 보안성이 향상되었고 [[Substitution Ciphers]]를 기본 아이디어로 한다

알파벳은 26자리에 불과하지만 $5 \times 5$ 행렬로 구성함으로써 총 $26 \times 26 = 676$개의 행렬을 구성할 수 있어 식별이 더욱 어려워졌다

![[Pasted image 20231006162737.png | 400]]
<div align="center">
	Example of Playfair key matrix
</div>

이러한 비밀키를 바탕으로 아래와 같은 규칙을 통해 평문을 암호화한다
1. 같은 쌍에 있는 반복되는 평문 문자는 x와 같은 필터 문자로 분리된다. 
    e.g. ballon $\rightarrow$ ba lx lo on
2. 행렬의 같은 행에 속하는 두 개의 평문 문자는 오른쪽의 문자로 각각 대체되며 행의 첫 번째 요소는 순환적으로 마지막에 이어진다 
    e.g. ar $\rightarrow$ RM
3. 행렬의 같은 열에 속하는 두 개의 평문 문자는 아래의 문자로 각각 대체되며 열의 맨 마지막에 있는 원소는 위로 순환한다
    e.g. mu -> CM
4. 2, 3의 경우에 포함되지 않은 두 개의 평문 문자는 자신의 행에 있는 문자와 다른 평문 문자가 차지하는 열로 대체된다
    e.g hs $\rightarrow$ BP, ea $\rightarrow$ IM (or JM)

위 규칙에서 가장 중요한 점은 문자를 한 글자씩 암호화를 진행하는 것이 아니라 두 개의 문자를 쌍으로 취급해 암호화를 진행하는 것이다. 

두 개의 문자를 쌍으로 취급해 많이 등장하는 문자와 적게 등장하는 문자들간의 빈도 수 차이를 줄일 수 있었고, 이를 통해 앞서 [[Mono-alphabetic Cipher]]에 [[Cryptanalysis]]를 적용해 푼 방식을 적용할 수 없도록 하였다. 

아래는 위에서 말한 상대 빈도수에 대한 일반적인 분포에 대한 표이다

![[Pasted image 20231006164233.png | 600]]
<div align="center">
	Table of frequency ranked letters 
</div>

