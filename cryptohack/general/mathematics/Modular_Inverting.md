# Modular Inverting

As we've seen, we can work within a finite field $F_{p}$, adding and multiplying elements, and always obtain another element of the field.

For all elements `g` in the field, there exists a unique integer `d` such that `g * d ≡ 1 mod p`.

This is the multiplicative inverse of `g`.

Example: `7 * 8 = 56 ≡ 1 mod 11`

What is the inverse element: `3 * d ≡ 1 mod 13`?

> Think about the little theorem we just worked with. How does this help you find the inverse of an element?

## Solution

According to **Fermat's Little Theorem**, if `a` is not divisible by `p`, $a^{p-1} \equiv 1 \pmod{p}$

From this rule we know that $3^{12} \equiv 1 \pmod{p}$, and since we're trying to find `d`, we can rewrite it as:

$3 * 3^{11} \equiv 1 \pmod{p}$

Using a little bit of python, we can do `pow(3,11,13)` to find `d`

### Answer

`9`
