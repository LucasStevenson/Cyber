# Sanity Check Round 25

> Points: 15 | Category: Misc | Author: Board

Welcome to round 25!

- `ictf{w3lcome_to_round_25!}`

---

Flag is given to us

`ictf{w3lcome_to_round_25!}`

---

# cos1

> Points: 30 | Category: Misc | Author: Eth007

What is the cosine of 42 radians, rounded to 6 decimal places? Submit your answer wrapped in `ictf{}`.

---

1. import `cos` method from math module
2. use built in `round()` method to round result to 6 decimal places

```py
>>> from math import cos
>>> round(cos(42), 6)
-0.399985
```

`ictf{-0.399985}`

---

# mixup

> Points: 30 | Category: Forensics | Author: Eth007

Something went wrong here...

- https://imaginaryctf.org/f/CJVKB#flag.txt

---

After initially cat-ing out the file, it looked like the characters got scrambled. I decided to take a closer look by opening the file up in vim

![flag.txt in vim](https://i.imgur.com/SaBsFKK.png)

There's special unicode characters that are scrambling the text and overall making it behave weirdly. In order to get the flag, we need to ignore these. I used the `-o` option in grep to only print out the matching parts (rather than the whole line) of the specified pattern, which in this case is a set of alphanumeric characters. Each match will be printed on a new line, so in order to get it all on one, I piped the result to the `tr` command and removed the newline characters. The full command is shown below.

```sh
$ grep -aio "[A-Za-z0-9_\-\{\}]" flag.txt | tr -d "\n"
```

We get the flag

`ictf{un1c0de_m4g1c_nahsdfoasihdfasohdfoiashdfjkadshfljadsfhdsklahflkhjdafs}`

---

# cos2

> Points: 30 | Category: Misc | Author: Eth007

What is the cosine of 42 radians, rounded to 100 decimal places? Wrap your flag in `ictf{}`

---

We can't follow the same process as the `cos1` challenge, because 100 decimal places is way too much to be accurately rounded via `round()`. I ended up using sympy's `evalf()` method

```py
>>> from sympy import cos
>>> cos(42).evalf(100)
-0.3999853149883512939547073371772020283804228791424190606167446601513424425835587794388549191368621883
```

`ictf{-0.3999853149883512939547073371772020283804228791424190606167446601513424425835587794388549191368621883}`

---

# Enormous

> Points: 50 | Category: Crypto | Author: puzzler7

I hear that more bits of security is better, so I'm using MASSIVE primes, and a TON of them! I bet you can't recover my flag now...

- https://imaginaryctf.org/f/3osbc#enc.py
- https://imaginaryctf.org/f/kjJkR#out.txt

---

Looking at the RSA implementation in the provided `enc.py` file, we can see that `n` is a huge number (10124 bits to be exact), and `e` is only 31. At first, I thought `m` would be small enough so that $m^{e} < n$. If this were the case, we could get the plaintext by taking the `e`th root of the ciphertext. Unfortunately, after giving this idea a shot, it did not return the flag, so we know that $m^{e} > n$. Because it didn't seem like the flag was padded, $m^{e}$ was most likely *barely* bigger than `n`.

> Moving forward, we're going to use the fact that a modular congruence in the form $a \equiv b \pmod{n}$ can be rewritten as $a - b = kn$ for some integer k.

```py
#!/usr/bin/env python3
from Crypto.Util.number import long_to_bytes
from sympy import integer_nthroot

with open("out.txt") as f:
    n = int(f.readline().rstrip().split("= ")[1])
    c = int(f.readline().rstrip().split("= ")[1])
    e = 31

for k in range(-1000, 0):
    m, isroot = integer_nthroot(c - n*k, e)
    if isroot:
        print(f"{k=}")
        print(long_to_bytes(m))
        break
```

`ictf{d0nt_f0rget_t0_pad_y0ur_pl@intexts!}`

---

# Listen Carefully

> Points: 75 | Category: Misc | Author: puzzler7

Keep your ears peeled.

- `nc puzzler7.imaginaryctf.org 4001`

---

---

# Personalized

> Points: 75 | Category: Crypto | Author: puzzler7

I found this place that just hands out flags! The only problem is that they're all encrypted, tailored to you, personally. Can you decrypt it?

- https://imaginaryctf.org/f/9FgS0#enc.py
- `nc puzzler7.imaginaryctf.org 4002`

---

```py
#!/usr/bin/env python3
import multiprocessing
from random import seed, getrandbits

def testSeeds(r):
    for i in r:
        seed(i)
        res = getrandbits(32)
        if res < 100:
            print(i, res)

processes = []
ranges = [range(i, i+20000000) for i in range(0, 100000000, 20000000)]
for r in ranges:
    p = multiprocessing.Process(target=testSeeds, args=(r,))
    p.start()
    processes.append(p)

for p in processes:
    p.join()
```

seed      | getrandbits(32) | e
-------   | --------------- | ---
8115501   | 36              | 73
32606471  | 91              | 183
82660432  | 4               | 9


```py
#!/usr/bin/env python3
import math
from pwn import *
import re
from sympy import integer_nthroot
from Crypto.Util.number import long_to_bytes
from random import seed, getrandbits

seedNum = 82660432
seed(seedNum)
e = 2*getrandbits(32)+1

moduli = []
ciphertexts = []

for _ in range(e//3):
    r = remote("puzzler7.imaginaryctf.org", 4002)
    r.recv()
    r.sendline(long_to_bytes(seedNum))
    r.recvline()
    res = r.recvS()
    n = int(re.search(r"n = (\d+)", res)[1])
    c = int(re.search(r"c = (\d+)", res)[1])
    moduli.append(n)
    ciphertexts.append(c)
    r.close()

def CRT(rems, mods):
    N = math.prod(mods)
    return sum([ ci*(N//ni)*pow(N//ni, -1, ni) for ci, ni in zip(rems, mods) ]) % N

m = integer_nthroot(CRT(ciphertexts, moduli), e)[0]
print(long_to_bytes(m))
```

`ictf{just_f0r_y0uuuuuuuu}`

---

# show-me-what-you-got

> Points: 75 | Category: Pwn | Author: Eth007

Error messages? What error messages?

- https://imaginaryctf.org/f/xhFHQ#vuln
- `nc got.ictf.kctf.cloud 1337`

---

---

# aes

> Points: 50 | Category: Crypto | Author: Eth007

```py
Python 3.8.10 (default, Mar 15 2022, 12:22:08)
[GCC 9.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import random
>>> from Crypto.Cipher import AES
>>> key = random.choice(open("rockyou.txt", "rb").readlines()[:10000]).strip()
>>> key = key.zfill(16)
>>> cipher = AES.new(key, AES.MODE_ECB)
>>> cipher.encrypt(open("flag.txt", "rb").read().zfill(48))
b"\xd6\x19O\xbeA\xb0\x15\x87\x0e\xc7\xc4\xc1\xe9h\xd8\xe6\xc6\x95\x82\xaa#\x91\xdb2l\xfa\xf7\xe1C\xb8\x11\x04\x82p\xe5\x9e\xb1\x0c*\xcc[('\x0f\xcc\xa7W\xff"
```

---

```py
#!/usr/bin/env python3
from Crypto.Cipher import AES

enc = b"\xd6\x19O\xbeA\xb0\x15\x87\x0e\xc7\xc4\xc1\xe9h\xd8\xe6\xc6\x95\x82\xaa#\x91\xdb2l\xfa\xf7\xe1C\xb8\x11\x04\x82p\xe5\x9e\xb1\x0c*\xcc[('\x0f\xcc\xa7W\xff"
for key in [ k.rstrip().zfill(16) for k in open("rockyou.txt", "rb").readlines()[:10000] ]:
    cipher = AES.new(key, AES.MODE_ECB)
    decrypted = cipher.decrypt(enc)
    if b"ictf{" in decrypted:
        print(decrypted)
        break
```

`ictf{d0nt_us3_w3ak_k3ys!!!!}`

---

# same

> Points: 75 | Category: Crypto | Author: Eth007

Something here is the same...

- https://imaginaryctf.org/f/iC3Mt#same.py
- https://imaginaryctf.org/f/EF3m1#output.txt

---

$c_{1} \equiv m^{e_{1}} \pmod{n}$  
$c_{2} \equiv m^{e_{2}} \pmod{n}$  

*Bézout's identity*: if $gcd(e_{1}, e_{2}) = 1$ then
$\exists(a,b) \in \mathbb{Z} : a\*e_{1} + b\*e_{2} = 1$

We can find `a` and `b` by using the extended euclidean algorithm. Once we get those two values, we can get `m`

$m \equiv (c_{1}^a * c_{2}^b) \pmod{n}$

```py
#!/usr/bin/env python3
from Crypto.Util.number import long_to_bytes

with open("output.txt") as f:
    n, c1, c2 = [ int(num.rstrip()) for num in f.readlines() ]
    e1, e2 = [1337, 31337]

def extended_gcd(a, b):
    if a == 0:
        return b, 0, 1
    gcd, x, y = extended_gcd(b%a, a)
    return gcd, y-(b//a)*x, x

_, a, b = extended_gcd(e1, e2)
m = pow(c1,a,n) * pow(c2,b,n) % n
print(long_to_bytes(m))
```

`ictf{n3ver_r3use_m0dul1}`

---
