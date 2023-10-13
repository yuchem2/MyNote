
Feistel은 *ideal block cipher*에 근사하기 위해 두 개이상의 간단한 암호 조합을 통해 암호학적으로 강한 암호를 만든 [[Product Ciphers]]의 방식을 이용하였다.

이 접근 방식에서 핵심은 $k \; bits$길이의 비밀 키와 $n \; bits$의 블럭을 이용한 [[Block Cipher]]를 개발해 *ideal block cipher*가 목표로 하는 $2^n!$개가 아니라 $2^k$개의 치환을 구성하는 것이다. 

여기서 선택한 방식이 Substitution과 Permutation을 교대로 사용하는 방식이다. 
+ Substituion: 각 평문의 요소는 유일하게 암호문으로 대체된다
+ Permutation: 평문 요소의 순열은 그 순열의 permutation으로 대체된다. (각 요소들의 순서만 변경)

즉, Fiestel은  *confusion*과 *diffustion* 함수를 교대로 사용하는 [[Product Ciphers]]를 개발하자는 제안을 실제로 구상한 것이다 ([[Substitution-Permutation(S-P) networks]])
