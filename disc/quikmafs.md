# Quikmafs

> Points: 125

> Category: Reversing

## Description

I bet you can't solve all of these math problems before time runs out, and even if you could, I bet you wouldn't see the flag there anyway!

(flag format is subtly different, and special thanks to `puzzler` for this challenge)

## Attachments

https://fdownl.ga/B06E35A33C

## Solution

I wrote a python script that uses pwntools to solve all the problems (there's 272 btw).

```py
from pwn import *

p = process("./quikmafs")

while True:
    try:
        q = p.recv()
        #print(q)
        ans = eval(q[-8:-3])
        print(ans)
        p.sendline(str(ans).encode())
    except Exception as e:
        # i'm only expecting an EOFError once we reach the end of the file, but just in case it's something else, I'll print out the error
        print(e)
        break

print(p.recv())
```

Completing all the problems doesn't give us the flag. It just gives us this message: **"Well done! You solved them all! Did you see the flag along the way?"**

After running it a few times, I noticed that

1. the signs of the answers seemed to stay the same.
2. none of the answers were 0

This means that each answer is either one of **two options**: positive or negative. Binary is the first thing that comes to mind when I think of something with only two options.

This means that each negative answer can be represented by a 0 and every positive answer can be represented with a 1. This will give us a binary string that we can turn into ascii text.

The final script looks like this

```py
from pwn import *
from Crypto.Util.number import long_to_bytes

p = process("./quikmafs")
s = ""
while True:
    try:
        q = p.recv()
        # print(q)
        ans = eval(q[-8:-3])

        if ans < 0:
            s += '0'
        else:
            s += '1'
        # print(ans)
        p.sendline(str(ans).encode())
    except Exception as e:
        print(e)
        break

# print(p.recv())
print(long_to_bytes(int(s, 2)))
```

### Flag

> CTF{y0u_ar3_t3h_qu1km@fs_w1z@rd!}
