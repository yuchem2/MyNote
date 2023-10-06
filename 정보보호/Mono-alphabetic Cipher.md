
[[Caesar Cipher]]의 비밀 키의 경우의 수가 매우 작기 때문에 이를 증가시키기 위해 나온 암호이다. 

이 암호에서는 substitution이 자유롭게 일어나는데 이러한 형태를 permutation이라고 한다

Permutation은 임의의 유한 집합 $S$에 대해 다음과 같이 정의될 수 있다. $$\begin{align} Let \; S = \{a, b, c\}, && \\ & there \; are \; six \; permutations \; of \; S, \quad abc, \; acb, \; bac, \; bca, \; cab, \; cba\end{align}$$
일반적으로는 유한 집합 $S$의 크기 $|S| = n$이면 $S$의 permutation의 수는 $n!$이다

이를 알파벳에 적용하면 permuation의 수는 $26!$이고, 가능한 비밀키의 개수는 $26!$이거나 $4\times 10^{26}$보다 많다

이러한 접근으로 암호를 만든 것이 [[Mono-alphabetic Cipher]]이다

키의 수가 많아졌지만 이 암호의 경우 [[Cryptanalysis]]의 위험이 존재한다



