> A subset of the larger class of [[Group]]s. A ring $R$, sometimes denoted by $\{R, +, \times\}$, is a set of elements with two binary operations, called addition and multiplication, such that for all $a, b, c$ in $R$ the following axioms are obeyed: 
> 	(A1-A5) $R$ is an *abelian group* with respect to *addition*; that is $R$ satisfies axioms A1 through A5. For the case of an additive group, we denote the identity element as $0$ and the inverse of $a$ as $-a$
> 	(M1) Closure under multiplication: $If\; a \; and \; b \; belong \; to \; R,\; then \; ab \; is \; also \; in \; R$
> 	(M2) Associativity of multiplication: $a(bc) = (ab)c \; for \; all \; a, b, c \; in \; R$
> 	(M3) Distributive laws: $a(b+c) = ab+ac \; for \; all \; a, b, c \; in R \qquad (a+b)c = ab+bc\; for \; all \; a, b, c \; in \; R$
> 	

임의의 집합의 원소들에게 이항 연산자 $+$에서 *Abelian Group*의 조건을 만족하고, 이항 연산자 $\times$에 한에서는 **닫힘 성질, 결합 법칙, 분배 법칙**을 만족하는 경우 Ring이라고 한다. 이항 연산자 $+$에 대해 **역원**이 만족되므로 이항 연산자 $-$에 대해서도 *Abelain Group*의 조건을 만족하게 된다.
## Commutative Ring
---
> A ring $R$ is said to be *commutative* if it satisfies the following additional condition:
> 	(M4) Commutativity of multiplication: $ab = ba \; for\; all \; a, b \; in \; R$

Ring이 이항 연산자 $\times$에 대해 **교환법칙**도 성립하는 경우commutative ring이라고 한다. 

## Integral Domain
---
> A *commutative ring* $R$ is *integral domain* when it obeys the following axioms:
> 	(M5) Multiplicative identity: $There \; is \; an \; element \; 1 \; in\; R \; such \; that \; a1 = 1a = a \; for \; all \; a \; in \; R$
> 	(M6) No zero divisors: $If \; a, b \; in \; R \; and \; ab=0, \; then \; either \; a = 0 \; or\; b = 0$

Commuative ring이 이항 연산자 $\times$에 대해 **항등원**을 만족하고 **결과가 0이면 연산에 참여한 원소 중 최소 하나가 0인 경우**에 Integral Domain이라고 한다. 