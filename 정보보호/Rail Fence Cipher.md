
가장 단순한 형태의 [[Transposition Ciphers]]

평문을 n개의 행으로 나눠 대각선으로 나눠가며 적은 후 맨 위 행부터 읽은 결과를 암호문으로 한다

여기서 행의 수 n이 비밀 키가 된다. n은 rail fence의 깊이라고 한다

e.g. key: 2
	  plaintext: meet me after the toga party
$$\begin{align} m & \quad e \quad m \quad a \quad t \quad r \quad h \quad t \quad g \quad p \quad r \quad y \\ & e\quad \; t \quad \; e \quad  f \quad e \quad t \quad e \quad o \quad a \quad a \quad t\end{align} $$
	  ciphertext: MEMATRHTGPRYETEFETEOAAT
