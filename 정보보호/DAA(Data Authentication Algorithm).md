[[DES(Data Encryption Standard)]]를 [[CBC(Cipher Block Chaining)]] 모드로 사용해 이 결과를 [[MAC(Message Authentication Code)]]로 사용하는 알고리즘 기법
## Algorithm
초기 값 $IV=0$으로 하고, 마지막 블록에 zero padding을 한다. 메시지는 [[DES(Data Encryption Standard)]]를 [[CBC(Cipher Block Chaining)]] 모드로 사용한 방법의 초기 값으로 입력되어 마지막 블록을 MAC으로 사용한다. 이때 마지막 블록 전체($64\; bit$)를 사용할 수 도 있고 좌측 상위 몇 개의 비트만을 MAC으로 사용할 수도 있다. 

![[Pasted image 20231206162843.png | 600]]

하지만 이렇게 MAC을 사용하게 되면 길이가 매우 짧아진다는 단점이 존재한다. 그러나 이러한 단점에도 국가나 회사에서 널리 사용되고 있다. 