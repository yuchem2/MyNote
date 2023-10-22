[[ECB(Eletronic Codebook)]]와 동일하게 메시지를 여러 블럭으로 쪼개는 것은 동일하지만, 각 암호화 과정이 서로 영향을 주도록 설계된 [[Block Cipher#Modes of Operation]]

직전에 수행된 암호화의 결과인 암호문 블럭이 현재 평문 블럭에 영향을 주어 암호화를 수행한다. 모든 과정을 동일하게 설계하기 위해 맨 처음 과정에는 평문 블럭에 영향을 주는 블럭으로 Initial Vector(IV)가 사용된다. 이때 주로 IV는 모든 값을 0 초기화하고 사용한다.$$\begin{align}C_i & = E(K, P_i \oplus C_{i-1}) \\ C_0 & = IV\end{align}$$

![[Pasted image 20231022150710.png | 600]]
<div align="center">CBC mode</div>

#### Advantages & Limitations
+ 각 암호문 블럭은 모든 메시지 블럭에 영향을 받는다
+ 그래서 메시지의 전체 구성에 따라 같은 평문 블럭일지라도 다른 암호문 블럭으로 암호화될 수 있다
+ IV를 암호화, 복호화하는 측에서 동일하게 사용하여야 한다는 단점이 존재한다
+ 메시지의 마지막 블럭에 대한 처리가 필요하다
	+ 블럭의 빈 부분을 단순히 non-data value(e.g. null)로 채운다
	+ 블럭의 빈 부분을 채우는 크기에 대한 정보를 넣어 채운다 e.g. $[b1\;b2\;b3\;0\;0\;0\;0\; 5]$ 