
[[Poly-alphabetic Cipher]]의 형태로 [[Vigenere Cipher]]의 비밀키 반복의 문제점을 해결하기 위해 제안된 암호

이 암호에서는 [[Vigenere Cipher]]과 다르게 비밀키가 반복되는 것이 아니라 한번 비밀키를 사용한 뒤 암호화된 평문을 비밀키로 사용한다

평문의 순열 $P=p_0,p_1,p_2, ..., p_{n-1}$, 비밀 키 $K = k_0, k_1, k_2, ..., k_{m-1}$이며 여기서 일반적으로$m<n$이다. 그리고 암호 문 $C = C_0, C_1, C_2, ..., C_{n-1}$라고 정의한다면 암호화는 다음과 같은 과정으로 수행된다
$$\begin{align} C & = C_0, C_1, C_2, ..., C_{n-1}=E(K,P) = E[(k_0, k_1,k_2,...,k_{m-1}), (p_0, p_1, p_2, ..., p_{n-1})
\\ & = (p_0 + k_0) \bmod 26, (p_1+k_1) \bmod 26, ..., (p_{m-1} + k_{m-1}) \bmod 26, \\ & \quad \; (p_m +p_0) \bmod 26, (p_{m+1} + p_1) \bmod 26, ..., (p_{2m-1} + p_{m-1}) \bmod 26, ...\end{align}$$


e.g. keyword: deceptivewearediscoveredsav
	  plaintext: wearediscoverdsaveyourself
	  ciphertext: ZICVTWQNGKZEIIGASXSTSLVVWLA

하지만 이 방법에서도 여전히 빈도 특성을 이용한 공격([[Cryptanalysis]])이 가능하다