## [[Hash Function]]
![[Hash Function]]

[[암호화 알고리즘(Cypotographic algorithms)]]에서는 이러한 [[Hash Function]]을 이용해 메시지의 변화를 감지하거나 [[Digital Signature]]을 생성하는데 사용된다. 이외에도 다양하게 사용될 수 있다. 

[[Hash Function]]을 사용하는 경우에 가장 큰 특징은 다른 [[암호화 알고리즘(Cypotographic algorithms)]]과 다르게 키를 사용하지 않는다는 점이다. 

![[Pasted image 20231129162112.png | 600]]

<div align="center">Cryptographic Hash Function; h = H(M)</div>

## Uses
### [[Message Authentication]]

![[Pasted image 20231129162315.png| 600]]

<div align="center">Simplified Examples of Use of a Hash Function for Message Authentication</div>

위 예시의 그림은 4가지 방법으로 [[Hash Function]]을 이용해 Message Authentication을 하는 예시를 보여주고 있다. 
1. 메시지를 hash code로 변환한 뒤 메시지와 hash code를 함께 하나의 데이터로 하여 암호화해 전송한다. 전송을 받은 측에서는 복호화하고, hash code 부분과 메시지를 나눈 뒤 메시지를 hash code로 변환해 두 값이 같은지 비교한다. 
2. 변환된 hash code만을 암호화 한 후 원본 메시지와 하나의 데이터로 하여 전송한다. 전송 받은 측에서는 암호화된 부분을 복호화하고, 원본 메시지를 hash code로 변환해 두 값이 같은 지 비교한다. 
### [[Digital Signature]]

![[Pasted image 20231129162530.png | 600]]

<div align="center">Simplified Examples of Digiter Signature</div>
