# Just in Time 1

> Points: 125

> Category: Misc

## Description

Yet another guessy challenge... Can you guess all the random numbers in time?

## Attachments

`nc oreos.ctfchallenge.ga 7331`

[Download python file](https://imaginary.ml/r/2FF7-lookatthetime.py)

```py
#!/usr/bin/env python3

import time
import random

def main():
        time.sleep(random.random())
        random.seed(round(time.time(), 2))
        print("If you guess my numbers, I'll give you the flag.")
        print("I'll give you a hint: %d"%random.randint(0, 1000000000))
        for i in range(3):
                try:
                    rnum = random.randint(0, 1000000000)
                    num = int(input("What is number %d? "%(i+1)))
                    if num != rnum:
                        raise Exception()
                except Exception as e:
                    print("Nope! Try again later...")
                    return
        print("Well done!")
        print(open('flag.txt', 'r').read())

if __name__ == '__main__':
        main()
```

## Solution

```py
import time
import random
from pwn import *

startTime = time.time()
r = remote("oreos.ctfchallenge.ga", 7331)
r.recvline()
endtime = time.time()
hint = int(r.recvline().decode().rstrip().split(": ")[1])
#print(hint)

while round(startTime, 2) != round(endtime, 2):
    random.seed(round(startTime,2))
    if random.randint(0, 1000000000) == hint:
        break
    startTime += 0.01

for i in range(3):
    r.recv()
    r.sendline(str(random.randint(0, 1000000000)))
print(r.recv().decode().strip())
```

### Flag

> ictf{t1ck_t0ck_1t5_g3++ing_l@t3}
