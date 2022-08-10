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

```sh
$ grep -aio "[A-Za-z0-9_\-\{\}]" flag.txt | tr -d "\n"
```

`ictf{un1c0de_m4g1c_nahsdfoasihdfasohdfoiashdfjkadshfljadsfhdsklahflkhjdafs}`

---

# cos2

> Points: 30 | Category: Misc | Author: Eth007

What is the cosine of 42 radians, rounded to 100 decimal places? Wrap your flag in `ictf{}`

---

I used sympy's `evalf()` method

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
