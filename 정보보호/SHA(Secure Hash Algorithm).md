1993년에 NIST와 NSA에서 처음으로 설계한 Secure [[Cryptographic Hash Function]]. 1995년에 SHA-1이 등장하면서 공개되었다. [[DSA]]에 사용되는 US 표준이다. 

SHA는 [[Hash Function]] MD4를 기초로 하고 있기 때문에 MD4와 설계상으로 유사하다. 다양한 출력을 제공하지만 초기에는 160 bit hash code를 출력하였다. 

![[Pasted image 20231201154039.png | 600]]
<div align="center">Comparision of SHA Parameters</div>

## SHA-512
### SHA-512 Logic

![[Pasted image 20231201154311.png | 600]]
<div align="center">Message Digest Generation Using SHA-512</div>

입력으로 주어질 수 있는 메시지의 최대 크기는 $2^{128} \; bit$이다. 출력되는 hash code의 길이는 $512\; bit$이다. 위 그림과 같이 입력 값은 $1024 \; bit$ block으로 나눠져 연산이 수행된다.
1. Append padding bits: 메시지의 길이는 항상 896 modulo 1024가 되도록 padding된다. ($message\; length \equiv 896 \pmod  {1024}$). Padding은 입력으로 주어진 메시지의 길이가 이 값을 만족하더라도 항상 추가된다. 그래서 padding bit의 수는 1-1024의 범위를 가진다. 패딩은 위 그림과 같이 단일 1 bit와 필요한 bit수만큼의 0 bit로 채워진다.
2. Append Length: Padding이 추가된 후 128 bit 블록이 추가된다. 이 블록은 부호없는 정수 블럭으로 여겨지며, 원본 메시지의 길이 정보를 포함한다. 1, 2번 과정을 통해 확장된 메시지의 길이는 $N\times 1024 \; bit$가 되며 $N$개의 1024 bit 블록으로 나눠지게 된다.
3. Initialize hash buffer: 512 bit 버퍼는 계산 과정 및 결과 hash code를 저장하기 위해 사용된다. 8개의 64 bit 레지스터로 구성되어 있다. 특정 정해진 값으로 초기화 된다. $$\begin{align}a&=6A09E667F3BCC908 & e&=510E527FADE682D1\\b&=BB67AE8584CAA73B & f&=9B05688C2B3E6C1F\\c&=3C6EF372FE94F82B & g&=1F83D9ABFB41BD6B\\d&=A54FF53A5F1D36F1 & h&=5BE0CD19137E2179\end{align}$$
4. Process message in 1024 bit blocks: 알고리즘의 핵심 부분으로 총 80개의 round로 구성되어 있다. 위 그림에서 F로 표기된 부분이다. 연산 과정 그림은 다음과 같다. ![[Pasted image 20231201160332.png | 500]]각 라운드는 입력으로 512 bit 버퍼를 입력으로 받는다. 또한, 1024 bit 메시지 블록으로 비롯한 64 bit의 값으로 나눈 값 $W_t$을 사용하며, 추가적으로 상수 $K_t$를 사용한다. $K_t$는 80개의 prime number의 3제곱근의 첫 64 bit 값이다. 
5. Output: $N$개의 블록에 대한 연산이 종료되면 $N$번째 stage에서 512 bit의 hash code가 결과로 나오게 된다. 
### SHA-512 Round Function
위 그림으로 제시된 round function을 자세히 보면 아래 그림과 같다. 

| Single round | Single Block |
| ------------ | ------------ |
| ![[Pasted image 20231201161738.png]]             | ![[Pasted image 20231201161743.png]]             |

위에서 $W_t$가 입력 메시지 블록 1024 bit로부터 비롯했다고 서술했다. 그러나 메시지 블록은 1024 bit로, 이를 64 bit로 나누면 총 16개의 $W_t$만 존재한다. 이후 W_t는 연산을 통해 만들어 진다. 
## SHA-3
SHA-1는 아직 공격으로부터 안전하다. 하지만 SHA-1과 유사한 MD5나 SHA-0가 공격으로 안전하지 않게 되었다. 이로 인해 기존과 다른 방식을 사용하는 [[Hash Function]]이 필요해졌다. 

SHA-3의 요구사항은 다음과 같았다. 
+ SHA-2(SHA-512)를 완전히 대체할 수 있어야 함. (같은 hash code를 사용하면서)
+ SHA-2처럼 온라인에서 사용 가능해야 함. (512~1024 bit의 작은 블록 사이즈 유지)
+ 평가 요소
	+ 유연성, 간단성
	+ 실행 시간 및 공간 필요성
	+ 보안성이 이론적으로 충분해야 한다