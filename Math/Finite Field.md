> A subset of [[Field]], consiting of those fields with a finite number of elements

암호학에서 점점 더 중요하게 생각하는 수학적 정의로, [[AES(Advanced Encryption Standard)]]와 같은 수많은 [[암호화 알고리즘(Cypotographic algorithms)]]과 elliptic curve cyptography이 Finite Field의 특성에 크게 의존한다. 또한 [[MAC(Message Authentication Code)]]인 [[CMAC]]과 [[인증(Authentication)]]된 암호화 방식 GCM도 이러한 특성을 기초하고 있다

## Galois Field
---
수학자 *Galois*는 [[Finite Field]]를 처음 연구한 사람으로, 이 사람의 이름을 따 [[Finite Field]]는 다음과 같은 방법으로 표기한다
+ $GF(p^n)$
+ $GF(p)$
+ $GF(2^n)$
## $GF(p)$
---
> For a given prime $p$, we define the finite field of order $p$, $GF(p)$, as the set $Z_p$ of integers $\{0, 1, ..., p-1\}$ togetehr with the arithmetic operations modulo $p$. 


> We futher observed that any integer in $Z_n$ has a multiplicative inverse iff that integer is relatively prime to $n$. If $n$ is prime, then allof the nonzero integers in $Z_n$ are relatively prime to $n$,and therefore there exists a multiplicative inverse for all of the nonzero integers in $Z_n$. Thus, for $Z_p$ we can add the following properties to those listed in:
> 	Multiplicative inverse($\omega^{-1}$)
>
