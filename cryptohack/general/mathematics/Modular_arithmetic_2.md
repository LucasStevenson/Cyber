# Modular Arithmetic 2

We'll pick up from the last challenge and imagine we've picked a modulus `p`, and we will restrict ourselves to the case when `p` is prime.

The integers modulo `p` define a field, denoted $F_{p}$.

> If the modulus is not prime, the set of integers modulo `n` define a ring.

A finite field $F_{p}$ is the set of integers `{0,1,...,p-1}`, and under both addition and multiplication there is an inverse element `b` for every element `a` in the set, such that `a + b = 0` and `a * b = 1`.

> Note that the identity element for addition and multiplication is different! This is because the identity when acted with the operator should do nothing: `a + 0 = a` and `a * 1 = a`.

Lets say we pick `p = 17`. Calculate $3^{17} \pmod{17}$. Now do the same but with $5^{17} \pmod{17}$

What would you expect to get for $7^{16} \pmod{17}$? Try calculating that.

This interesting fact is known as Fermat's little theorem. We'll be needing this (and its generalisations) when we look at RSA cryptography.

Now take the prime `p = 65537`. Calculate $273246787654^{65536} \pmod{65537}$.

Did you need a calculator?

## Solution

**Fermat's little theorem** states that if `p` is a prime number, then for any integer `a`, the number $a^{p} - a$ is an integer multiple of `p`. This can be expressed as: 

$a^{p} \equiv a \pmod{p}$


If a is not divisible by p, Fermat's little theorem can be expressed as: 

$a^{p-1} \equiv 1 \pmod{p}$

### Answer

`1`
