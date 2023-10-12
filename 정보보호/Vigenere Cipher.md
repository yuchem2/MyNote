
가장 잘 알려져 있고, 가장 간단한 형태의 [[Poly-alphabetic Cipher]]

관련된 [[Mono-alphabetic Cipher]] 규칙들의 집합을 0-25까지의 변환을 가지는 26개의 [[Caesar Cipher]]들로 구성하며 각 [[Caesar Cipher]]가 비밀키가 된다. 이 비밀키를 통해 문자를 치환하는 형태를 가진다


평문의 순열 $P=p_0,p_1,p_2, ..., p_{n-1}$, 비밀 키 $K = k_0, k_1, k_2, ..., k_{m-1}$이며 여기서 일반적으로$m<n$이다. 그리고 암호 문 $C = C_0, C_1, C_2, ..., C_{n-1}$라고 정의한다면 암호화는 다음과 같은 과정으로 수행된다
$$\begin{align} C & = C_0, C_1, C_2, ..., C_{n-1}=E(K,P) = E[(k_0, k_1,k_2,...,k_{m-1}), (p_0, p_1, p_2, ..., p_{n-1})
\\ & = (p_0 + k_0) \bmod 26, (p_1+k_1) \bmod 26, ..., (p_{m-1} + k_{m-1}) \bmod 26, \\ & \quad \; (p_m +k_0) \bmod 26, (p_{m+1} + k_1) \bmod 26, ..., (p_{2m-1} + k_{m-1}) \bmod 26, ...\end{align}$$
즉, 암호화는 $C_i = (p_i + k_{i \bmod m}) \bmod 26$ 이고, 복호화는 $p_i = (C_i-k_{i \bmod m}) \bmod 26$ 이다

e.g. keyword: deceptive
	  plaintext: wearediscoverdsaveyourself
	  ciphertext: ZICVTWQNGRZGVTWAVZHCQYGLMGJ

평문에 대해서 비밀키가 반복적으로 사용되어 암호화가 진행된다. 이로 인해 문자의 빈도수에 대한 측정이 가능하고, 결국에는 비밀키를 유추해낼 수 있을 가능성이 존재한다. ([[Cryptanalysis]])

이러한 문제점을 해결하기 위해 등장한 암호가 [[Autokey Cipher]]이다
