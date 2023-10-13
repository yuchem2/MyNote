
[[Block Cipher]] 및 [[Symmetric encryption]]를 바탕으로 하는 [[암호화 알고리즘(Cypotographic algorithms)]]

대칭키 블럭 암호는 대부분 [[Feistel Cipher Structure]]를 기초하고 있다

## Motivation for the [[Feistel Cipher Structure]]
---
어떠한 블럭 암호의 연산이 평문 블럭 $n$ bit에 수행되면, 암호 블럭 $n$ bits가 결과로 나오게 된다. 
이 경우 $2^n$ 개의 평문이 존재하고, 이에 따른 암호화가 가역적이기 위해 각각은 고유한 암호 블럭을 생성해야 한다. 

이러한 치환을 가역적인(Reversible, or nonsingular) 치환이라고 한다. 이 경우에 가능한 치환의 수는 총 $2^n!$이다. 이러한 치환을 하는 블럭 암호를 *ideal block cipher*라고 한다. 

만약 블럭 사이즈 $n$이 작은 수라면, [[Substitution Ciphers]]와 동치가 되는 문제가 존재한다. 이러한 시스템의 경우 평문 통계 분석에 취약하게 된다. 하지만 $n$이 충분히 크고, 평문과 암호문 사이에서 치환이 가역적이라면 평문 통계 분석으로는 이 암호를 유추해 낼 수 없게 된다.

하지만 *ideal block cipher*는 구현 및 성능 관점에서 실용적이지 않는 문제가 존재한다. $n=4$인 가역적인 치환을 하는 암호가 있다고 가정하자. 앞서 서술한 대로 하나의 평문에 대해 하나의 암호가 매핑된다고 가정하면 총 $2^4$개의 비밀 키가 필요하며 이 비밀키의 총 길이는 $4 \; bits \times 2^4 = 64\; bits$이다. 이를 블럭의 사이즈 $n$에 대하여 일반화 하면 다음과 같다.
$$total \; key \; length\; = n\; bits \times \; 2^n = n\times 2^n \; bits $$
자주 사용되는 $64 \; bits$의 경우에는 $64 \times 2^64 = 2^70 \approx 10^{21} \; bits$가 필요하다.


