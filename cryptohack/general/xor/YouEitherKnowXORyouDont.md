# You either know, XOR you don't

I've encrypted the flag with my secret key, you'll never be able to guess it

> Remember the flag format and how it might help you in this challenge!

`0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104`

## Solution

```py
#!/usr/bin/env python3
enc = bytes.fromhex("0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104")

def getPartOfKey(flag_start):
    # the idea is to pass in the start of the flag and XOR each character with the corresponding byte in the encoded output (enc variable)
    partialKey = []
    for i, c in enumerate(flag_start):
        partialKey.append(ord(c) ^ enc[i])
    return bytes(partialKey)

# we know that the flag starts with 'crypto{', so we'll pass that into the function
print(getPartOfKey("crypto{")) # b'myXORke'

# we can make an educated guess that the full key is 'myXORkey'
key = b'myXORkey'

flag = []
for i in range(len(enc)):
    flag.append(enc[i] ^ key[i%len(key)])
print(bytes(flag))
```

### Flag

`crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}`
