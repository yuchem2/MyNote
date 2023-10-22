메시지를 $bit$의 stream으로 취급해 수행되는 [[Block Cipher#Modes of Operation]]

[[OFB(Output FeedBack)]]와 유사하지만, Nonce를 한번 사용하는 것이 아니라 counter라는 값을 만들어 이를 사용하다. $n$개의 블럭에 대해 $n$개의 counter를 생성해 수행되어 feedback이 존재하지 않는다. 이때 각 block에 사용되는 counter의 값은 모두 달라야 하며 처음 사용되는 counter는 무조건 Nonce여야 한다.  $$\begin{align}C_i & = P_i \oplus O_i \\ O_i & = E(K, Counter_i)\\ Counter_1 & = Nonce\end{align}$$
빠른 속도의 암호화가 필요한 네트워크에서 주로 사용된다. 

![[Pasted image 20231022154550.png | 600]]
<div align="center">CTR mode</div>

#### Advantages & Limitations
+ 동시에 암호화를 수행할 수 있다
+ 한번에 많은 데이터가 모이는 높은 속도의 link에 적절하다
+ 암호문 블럭에 대해 랜덤적인 접근을 해 평문을 유추하기 어렵다
+ 하지만 counter와 비밀 키를 한 번 이상 사용하지 않음이 보장되어야 한다. 만약 아닌 경우 평문이 유추될 수 있다.