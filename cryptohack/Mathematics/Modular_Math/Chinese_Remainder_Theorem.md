# Chinese Remainder Theorem

The Chinese Remainder Theorem gives a unique solution to a set of linear congruences if their moduli are coprime.

This means, that given a set of arbitrary integers $a_{i}$, and pairwise coprime integers $n_{i}$, such that the following linear congruences hold:

> Note "pairwise coprime integers" means that if we have a set of integers ${n_{1}, n_{2}, \ldots, n_{i}}$, all pairs of integers selected from the set are coprime: $gcd(n_{i}, n_{j}) = 1$.

$$
\begin{gather}
  &x \equiv a_{1} \pmod{n_{1}}\\
  &x \equiv a_{2} \pmod{n_{2}}\\
  &\ldots\\
  &x \equiv a_{n} \pmod{n_{n}}
\end{gather}
$$

There is a unique solution $x \equiv a \pmod{N}$ where $N = n_{1} * n_{2} * \ldots * n_{n}$

In cryptography, we commonly use the Chinese Remainder Theorem to help us reduce a problem of very large integers into a set of several, easier problems.

Given the following set of linear congruences:

$$
\begin{gather}
  &x \equiv 2 \pmod{5}\phantom{0}\\
  &x \equiv 3 \pmod{11}\\
  &x \equiv 5 \pmod{17}
\end{gather}
$$

Find the integer `a` such that $x \equiv a \pmod{935}$

> Starting with the congruence with the largest modulus, use that for $x \equiv a \pmod{p}$ we can write $x = a + k\*p$ for arbitrary integer `k`

## Solution

```py
#!/usr/bin/env python3
import math

a = [2,3,5]
n = [5,11,17]
N = math.prod(n)
ans = 0

for ai, ni in zip(a,n):
    Ni = N//ni
    Mi = pow(Ni,-1,ni)
    ans += (ai*Ni*Mi)
print(ans%N)
```

### Answer

`872`
