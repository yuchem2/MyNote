
[[Block Cipher]] 및 [[Symmetric encryption]]를 바탕으로 하는 [[암호화 알고리즘(Cypotographic algorithms)]]

대칭키 블럭 암호는 대부분 [[Feistel Cipher Structure]]를 기초하고 있다

## Motivation for the [[Feistel Cipher Structure]]
---
어떠한 블럭 암호의 연산이 평문 블럭 $n$ bit에 수행되면, 암호 블럭 $n$ bits가 결과로 나오게 된다. 
이 경우 $2^n$ 개의 평문이 존재하고, 이에 따른 암호화가 가역적이기 위해 각각은 고유한 암호 블럭을 생성해야 한다. 

이러한 치환을 가역적인(Reversible, or nonsingular) 치환이라고 한다. 이 경우에 가능한 치환의 수는 총 $2^n!$이다. 또한 하나의 치환을 테이블로 구성하면 $2^n$의 entries를 가지게 되며 테이블의 크기는 $2^n \times 2 \times n \; bits$ 이다

이러한 치환을 하는 블럭 암호를 *ideal block cipher*라고 한다. 
