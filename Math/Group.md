> A group $G$, sometimes denoted by $\{G, \cdot \}$, is a set of elements with a binary operation denoted by $\cdot$ that associated to each ordered pair $(a, \; b)$ of elements in $G$ an element $(a \cdot b)$ in $G\times G$, such the following axioms are obeyed:
> 	(A1) Closure: $If \; a \; and \; b \; belong \; to \; G, \; then \; a \cdot b \; is \; also \; in \; G$
> 	(A2) Associative: $a\cdot(b\cdot c) = (a\cdot b) \cdot c \; for \; all \; a, \;b, \; c \; in \; G$
> 	(A3) Identity element: $There \; is \; an \; element \; e\; in \; G\; such \; that \; a \cdot e = e\cdot a = a \; for \; all \; in \;G$
> 	(A4) Inverse element: $For \; each \; a \; in \; G, \; there \; is \; an \; element \; a' \; in \; G \; such \; that \; a \cdot a' = a'\cdot a = e$

임의의 집합의 원소들에게 임의의 2항 연산자을 수행할 때 **닫힘 성질, 결합 법칙, 항등원, 역원**을 만족하는 경우 Group이라고 한다.
## Abelian Group
---
> A group $G$ is said to be *abelian* if it satisfies the following additional condition:
> 	(A5) Commutative: $a\cdot b = b \cdot a \; for \; all \; a, \; b \; in \; G$

Group이 **교환법칙**도 성립하는 경우 Abelian Group이라고 한다.
## Cylic Group
---
> We define exponentiation within a group as a repeated application of the group operator, so that $a^3 = a \cdot a \cdot a$. Furthermore, we define $a^0 = e$ as the identity element, and $a^{-n} = (a')^n$, where $a'$ is the inverse element of $a$ within the group. 
> 
> A group G is *cylic* if every element of $G$ is power $a^k$ ($k$ is an integer) of a fixed element $a \in G$. The element $a$ is said to generate the group $G$ or to be a generate of $G$. A cyclic group is always *abelian* and may be finite or infinte

Abelian Group의 모든 원소가 Abelian Group의 임의의 원소 $a$에 대해 $a^k(k$는 정수)를 만족하는 경우 Cylic Group이라고 하고 이 $a$를 group의 generator라고 한다