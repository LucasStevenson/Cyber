# Evaluative

> Difficulty: Very easy

## Description

You will be given

1. A list of coefficients $a_{0} - a_{8}$ where $-100 <= a_{i} <= 100$
2. The integer x, where $-100 <= x <= 100$

The polynomial is constructed as follows:

$a_{0} + a_{1}*x + a_{2}*x^{2} + a_{3}*x^{3} + \ldots + a_{n}*x^{n}$

You will need to evaluate the polynomial at x


```
Example Input
---
1 -2 3 -4 5 -6 7 -8 9
5

Output
---
2983941
```

## Solution

This is a really straight forward challenge. Could pretty easily be done in 2 lines, but I'll show the more readable solution first

```py
coeffs_str = input()
x_str = input()

coeffs, x = [int(x) for x in coeffs_str.split()], int(x_str)
ans = 0
for i, a in enumerate(coeffs):
    ans += a*pow(x,i)

print(ans)
```

For (unnecessary) style points, let's do the two-liner

```py
coeffs, x = input(), input()
print(sum([ a*pow(int(x), i) for i, a in enumerate(map(int, coeffs.split())) ]))
```

## Flag

`HTB{eV4LuaT1nG_p0LyN0M1aL5_f0R_7H3_w1N}`
