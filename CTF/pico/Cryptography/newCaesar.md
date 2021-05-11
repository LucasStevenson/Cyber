# New Caesar

> Points: 60

> Category: Cryptography

## Description

We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{})

`ihjghbjgjhfbhbfcfjflfjiifdfgffihfeigidfligigffihfjfhfhfhigfjfffjfeihihfdieieih`
[new_caesar.py](https://mercury.picoctf.net/static/2fc43dd1a3718df7debf367b0e092831/new_caesar.py)

## Solution

```py
import string

ALPHA = string.ascii_lowercase[:16]

def b16_encode(plain):
    enc = ""
    for c in plain:
        binary = "{0:08b}".format(ord(c))
        enc += ALPHA[int(binary[:4], 2)]
        enc += ALPHA[int(binary[4:], 2)]
    return enc

obj = {}
for i in range(33, 128):
    obj[b16_encode(chr(i))] = chr(i)

def decode(ct):
    n = 2
    arr = [ct[i:i+n] for i in range(0, len(ct), n)]
    ans = ""
    for i in arr:
        if obj.get(i) == None:
            return None
        ans += obj.get(i)
    return ans

ct = "ihjghbjgjhfbhbfcfjflfjiifdfgffihfeigidfligigffihfjfhfhfhigfjfffjfeihihfdieieih"


def shift(c, k):
    t1 = ord(c) - 97
    t2 = ord(k) - 97
    return ALPHA[(t1 + t2) % len(ALPHA)]

#print(decode(ct))
for k in ALPHA:
    tmp = ""
    for i in ct:
        tmp += shift(i, k)
    print(decode(tmp))
```

### Flag

> picoCTF{et_tu?\_0797f143e2da9dd3e7555d7372ee1bbe}
