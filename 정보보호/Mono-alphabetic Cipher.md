
[[Caesar Cipher]]의 비밀 키의 경우의 수가 매우 작기 때문에 이를 증가시키기 위해 나온 암호이다. 

이 암호에서는 substitution이 자유롭게 일어나는데 이러한 형태를 permutation이라고 한다

Permutation은 임의의 유한 집합 $S$에 대해 다음과 같이 정의될 수 있다. $$\begin{align} Let \; S = \{a, b, c\}, && \\ & there \; are \; six \; permutations \; of \; S, \quad abc, \; acb, \; bac, \; bca, \; cab, \; cba\end{align}$$
일반적으로는 유한 집합 $S$의 크기 $|S| = n$이면 $S$의 permutation의 수는 $n!$이다

이를 알파벳에 적용하면 permuation의 수는 $26!$이고, 가능한 비밀키의 개수는 $26!$이거나 $4\times 10^{26}$보다 많다

여기서 비밀키의 값을 통해 permutation을 선택하는 형식으로 암호화를 하는 방식이 [[Mono-alphabetic Cipher]]이다

## **[[Cryptanalysis]]의 위험**

	사람이 사용하는 언어는 redundant해 이 방식을 통해 암호화 하더라도 단어의 알파벳 수를 
	통해 유추가 가능하다. e.g., "th lrd s m shphrd shll nt wnt"
	또한 일반적인 문장에서 각 알파벳은 많이 사용되는 단어와 적게 사용되는 단어가 존재한다
	e.g., English e is by fra the most common letter, "z, j, k, q, x" are fairly rare

![[Pasted image 20231006155252.png | 600]]
<div align="center">
	Table of "English Letter Relative Frequencies"
</div>

![[Pasted image 20231011204140.png | 600]]
<div align="center">
	Figure of "English Letter Relative Frequencies"
</div>

이 암호를 [[Cryptanalysis]]를 풀 경우 가장 중심이 되는 아이디어는 암호문에도 이 상대 빈도가 유지된다는 점이다. 

특정 암호문에 대해 단어 수를 세서 상대 빈도 수로 처리하게 된다면 위와 유사한 표가 작성될 것이고, 이에 따라 그 암호 단어가 어떤 단어를 표현하는지 유추할 수 있다

### Example

암호문이 아래와 같다고 하자

	UZQSOVUOHXMOPVGPOZPEVSGZWSZOPFPESXUDBMETSXAIZ
	VUEPHZHMDZSHZOWSFPAPPDTSVPQUZWYMXUZUHSX
	EPYEPOPDZSZUFPOMBZWPFUPZHMDJUDTMOHMQ

여기서 각 문자의 수를 세면 아래와 같다.

	A: 2, B: 2, C: 0, D: 6, E: 6, F: 3, G: 2, H: 7, I: 1, J: 1, K: 0, L: 0, M: 8, N: 0
	O: 3, P: 16, Q: 2, R: 0, S: 10, T: 3, U: 12, V: 5, W: 4, X: 5, Y: 2, Z: 14

여기서 P와 Z가 가장 많이 등장함으로, e와 t로 가정할 수 있다

그러고 난 뒤 특정 슬롯으로 검사를 하게 되면 ZW가 많이 붙어 있는 상태로 등장하며 이를 th로 가정할 수 있고, 그렇게 보면 ZWP를 the로 예측할 수 있다.

이러한 가정을 반복해 수행한다면 다음과 같은 평문으로 복호화할 수 있다

	it was disclosed yesterday that several informal but 
	direct contacts have been made with political
	representatives of the viet cong in moscow

