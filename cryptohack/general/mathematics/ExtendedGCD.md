# Extended GCD

Let `a` and `b` be positive integers.

The extended Euclidean algorithm is an efficient way to find integers `u, v` such that

`a*u + b*v = gcd(a,b)`

> Later, when we learn to decrypt RSA, we will need this algorithm to calculate the modular inverse of the public exponent

Using the two primes `p = 26513, q = 32321`, find the integers `u,v` such that

`p*u + q*v = gcd(p,q)`

Enter whichever of `u` and `v` is the lower number as the flag.

> Knowing that `p,q` are prime, what would you expected `gcd(p,q)` to be? For more details on the extended Euclidean algorithm, check out [this page](http://www-math.ucdenver.edu/~wcherowi/courses/m5410/exeucalg.html)


## Solution

Below is an implementation of the extended euclidean algorithm.

```py
def extended_gcd(p,q):
    if p == 0:
        return q, 0, 1
    gcd, x, y = extended_gcd(q%p, p)
    return gcd, y-(q//p)*x,x

_, u, v = extended_gcd(26513, 32321)
# this challenge wants us to return the lower of the two values
print(min(u,v))
```

### Answer

`-8404`
