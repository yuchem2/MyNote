
1949년에 **Claude Shannon**이 제안한 아이디어 (<a href="https://onlinelibrary.wiley.com/doi/abs/10.1002/j.1538-7305.1949.tb00928.x">Communication Theory of Secrecy System</a>)

이 아이디어는 현대 [[Block Cipher]]에 기초가 된 아이디어로, substition-transpotion [[Product Ciphers]]에 대한 내용이다.

여기서 암호 시스템을 위한 기본적인 두 개의 블럭을 *diffusion*과 *confusion*로 말한다. **Claude Shannon**는 통계를 기초한 [[Cryptanalysis]]를 막기 위해 이 아이디어를 고안해냈다.

여기서 **Claude Shannon**이 말하는 *strongly ideal block cipher*는 암호문의 모든 통계는 사용되는 비밀 키와 독립적인 암호를 말하며 이는 앞서 [[Block Cipher]]에서 언급한 *ideal block cipher*도 이를 만족하지만, 비현실적이다.

## *Diffussion*
*평문의 통계적 구조는 암호문의 long-range statistics로 소멸된다*

공격자가 암호문의 각 문자를 통계적 추론으로 평문을 유추할 수 없게 한다. 

이는 평문이 많은 암호문 값에 영향을 미치도록 함으로써 이루어질 수 있다. 
e.g. 평균 연산으로 문자 $M=m_1, m_2, m_3, ...$ 를 암호화
$$y_n = \left( \sum^k_{i=1} m_{n+i}\right) \bmod 26$$
$k$개의 연속적인 문자를 추가하여 암호문 $y_n$을 얻는다. 

위 예시는 평문의 통계적 구조가 소멸되었다는 것을 보인다. 따라서 암호문의 각 문자 빈도수는 평문의 빈도수보다 더 [[Uniform distribution]]한 형태일 것이다. 

Binary [[Block Cipher]]에서 *diffusion*은 데이터에 대해 어떤 permutation을 반복적으로 수행한 다음 그 결과에 함수를 적용함으로써 달성될 수 있다. 
## *Confussion*
*암호문의 통계와 비밀 키의 값 사이의 관계를 가능한 복잡하게 만든다*

공격자가 암호문의 통계를 어느정도 파악하더라도 비밀 키를 추론해내지 못하게 한다. 

이는 복잡한 substitution 알고리즘에 의해 이루어질 수 있다. 


## S-P Networks
*diffusion*을  permutation(P-box)로, *confussio*을 substitution(S-box)로 구성한 [[Block Cipher]] 네트워크로, 매우 성공적인 성능을 보여 현대 [[Block Cipher]]의 초석이 되었다. 