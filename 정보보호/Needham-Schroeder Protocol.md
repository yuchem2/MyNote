[[인증(Authentication)]] 기능을 포함하는 KDC를 사용하는 private [[Key Distribution]] protocol
## Overview
KDC에 의해 인증에 참여하는 두 당사자 $A, B$의 session이 생성된다. 
1. A $\rightarrow$ KDC: $ID_A || ID_B || N_1$
2. KDC $\rightarrow$ A: $E(K_a, [K_s || ID_B || N_1 || E(K_b, [K_s || ID_A])])$
3. A $\rightarrow$ B: $E(K_b, [K_s || ID_A])$
4. B $\rightarrow$ A: $E(K_s, N_2)$
5. A $\rightarrow$ B: $E(K_s, f(N_2))$
## Features
통신의 두 당사자가 안전하게 새로운 session key를 교환하기 위해 사용된다. 하지만 이 protocol의 경우 replay attack에 취약하다. 3번 과정을 보면, 오래된 session key를 보내더라도 문제가 없이 작동할 수 있기 때문이다. 

이 문제를 해결하기 위해 timestamps를 넣은 버전과 nonce를 사용하는 버전이 논의되었다. 
