[[Symmetric encryption]]을 기반으로 사용하는 방식으로, [[Block Cipher]]에서 사용하는 기법 중 하나인 [[CBC(Cipher Block Chaining)]]를 이용한다. 연산의 마지막 블록을 [[MAC(Message Authentication Code)]]으로 사용하는 것이다.
### [[DAA(Data Authentication Algorithm)]]
![[DAA(Data Authentication Algorithm)]]
### [[CMAC]]
[[DAA(Data Authentication Algorithm)]]의 결과의 길이가 매우 짧다는 단점을 해결하기 위해 등장한 방법이다. 이를 해결하기 위해 메시지 길이는 제한을 두게 되었지만 암호성을 획득할 수 있게 되었다. 이 방법에서는 2개의 키와 padding을 사용한다. 

![[Pasted image 20231206163224.png | 600]]
