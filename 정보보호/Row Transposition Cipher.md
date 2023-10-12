
[[Rail Fence Cipher]]보다 좀 더 복잡한 형태의 [[Transposition Ciphers]]

메시지를 직사각형 안에 한 행씩 쓴 후 열에 따라 재배치를 진행한다. 그 후 비밀 키와 관련된 열의 순서로 메시지를 읽어 낸다. 읽어낸 결과가 암호문이 된다

e.g. key:         4    3    1    2     5    6   7
	 plaintext: $\begin{matrix} a & t & t & a & c & k & p \\ o & s & t & p & o & n & e \\ d & u & n & t &i & l & t \\ w &o & a & m &x & y & z \end{matrix}$
	 ciphertext: TTNAAPTMTSUOAODWCOIXKNLYPETZ
