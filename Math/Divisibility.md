> A nonzero $b$ *divides* $a$ if $a = mb$ for some $m$, where $a, b, m$ are integers. That is, $b$ divides $a$ if there is no remainder no division. The notation $b|a$ is commonly used to mean $b$ divides $a$. Also if $b|a$, we say that $b$ is a *divisor* of $a$.

## Basic Properties
---
+ $if \; a|1, \; then \; a = \pm1$
+ $if\; a|b \; and \; b|a, \; then \; a = \pm b$
+ $Any \; b \neq 0 \;divides\; 0$
+ $if\; a|b \; and \; b|c, \; then\; a|c$   $e.g. \; 11|66 \; and \; 66|198 \Rightarrow 11|198$ 
+ $if \; b|g\;and \; b|h,\; then \; b|(mg+nh) \; for\; arbitrary\; integers\;m\;and\;n$

+ $if \; b|g,\; then \; g \; is \; of \; the \; form \; g = b \times g_1 \; for \;some\; integer\; g_1$
+ $if \; b|h,\; then \; g \; is \; of \; the \; form \; h = b \times h_1 \; for \;some\; integer\; h_1$	$$So, \; mg+nh=mbg_1 + mbh_1 = b \times (mg_1 + nh_1)$$
## Divison Algorithm 
---
> Given any positive integer $n$ and any nonegative integer $a$, if we divide $a$ by $n$, we get an integer quotient $q$ and an integer remainder $r$ that obey the following realtionship: $$a=qn+r\qquad \qquad 0 \leq r <n; \; q=\lfloor a/{n} \rfloor$$where $\lfloor x \rfloor$ is the largest intger less than or equal to $x$
