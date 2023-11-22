> Define function $\phi (n)$ as the number of positive integers less than $n$ and relatively prime to $n$. By convention, $\phi (1) = 1$
> 	If $n$ is prime, $\phi(n) = n -1$
> 	There are two [[Prime Number]]s $p, q$ with $p\neq q$. For $n=pq$, $$\phi(n) = \phi(pq) = \phi(p) \times \phi(q) = (p-1) \times (q-1)$$

즉, $\phi(n)$은 *reduced set of residues*이며 $Z_n$의 subset이다.

## Euler's Theorem
> For every $a$ and $n$ that are relatively prime:$$a^{\phi(n)} \equiv 1 \pmod n$$

위 식의 다른 형태는 다음과 같다. $$a^{\phi(n)+1} \equiv a \pmod n$$