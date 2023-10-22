메시지를 $bit$의 stream으로 취급해 수행되는 [[Block Cipher#Modes of Operation]]

[[CBC(Cipher Block Chaining)]]와 유사하지만, 각 과정에서 만들어진 결과인 암호 스트림은 다른 과정에 영향을 주지 않는다. 오직 [[Block Cipher]]의 암호화 과정으로 나온 결과물이 다음 키 값에 영향을 주게 된다. 즉, 평문과 암호문은 암호키와 전혀 관계가 없으며 연쇄과정에서 각 과정의 암호키가 바뀌는데도 영향을 주지 않는다.$$\begin{align}C_i & = P_i \oplus O_i \\ O_i & = E(K, O_{i-1})\\ O_0 & = Nonce\end{align}$$여기서 $Nonce$는 한 번만 사용되는 번호라는 뜻으로, *Replay Attack*을 방지하기 위해 IV대신 사용되는 값으로 주로 [[Random Numbers]]를 한 번만 사용하고 버림으로써 수행된다.

Noise가 있는 채널을 사용하는 경우에 [[Stream Cipher]]를 통해 메시지를 전달할 때 사용된다. 즉, error feedback 문제가 있는 경우에 사용되며 메시지가 존재하기 전에 미리 암호화를 해야 하는 경우에 사용된다

![[Pasted image 20231022153331.png | 600]]
<div align="center">OFB mode</div>

#### Advantages & Limitations
+ $bit$ error가 다른 과정으로 전파되지 않는다
+ [[CFB(Cipher Feedback)]]에 비해 *message stream modification attack*에 취약하다