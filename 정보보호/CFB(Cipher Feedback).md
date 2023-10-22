메시지를 $bit$의 stream으로 취급해 수행되는 [[Block Cipher#Modes of Operation]]로, [[Block Cipher#Modes of Operation#Stream Mode]]에서 가장 일반적인 형태이다.

암호화를 수행할 때 평문을 사용할 암호로 암호화를 진행하는 것이 아니라 IV를 만들어 이 값을 비밀 키와 함께 암호화를 진행한 후에 그 결과를 평문을 자른 $bit$의 단위와 같게 만든 후 자른 평문의 $bit$들과 XOR연산을 수행한 결과를 암호문으로 채택하는 형태$$\begin{align}C_i & = P_i \oplus E(K, C_{i-1}) \\ C_0 & = IV\end{align}$$
이를 통해 [[Block Cipher]]를 이용해 [[Stream Cipher]]로 사용할 수 있고, [[인증(Authentication)]]에서도 사용된다

![[Pasted image 20231022152318.png | 600]] 
<div align="center">CFB mode</div>

#### Advantages & Limitations
+ 데이터가 $bit$, $byte$로 전달되는 경우 적합하다
+ 중요한 점은 [[Block Cipher]]가 암호화, 복호화 과정에서 모두 암호화 모드로 사용된다는 점이다
+ 만약 error가 발생한다면 이 오류가 다른 블럭으로 전파된다는 문제가 존재한다