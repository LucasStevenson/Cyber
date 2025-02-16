# Sanity Check Round 11

> Points: 15 | Category: Misc | Author: Board

Welcome to Round 11! DM flags to me to get points, and rise up on the leaderboard! Have fun and enjoy Round 11!

- aaaabaaacaaadaaaeaaafaaagaaaha`ictf{Round_11_Sanity_Check}`aaiaaajaaakaaalaaamaaanaaaoaaa

---

This one is pretty self explanatory.

`ictf{Round_11_Sanity_Check}`

---

# Spider

> Points: 75 | Category: Web | Author: Robin_Jadoul

Got some glue?

_Note:_ There's a rate limiter of 3 requests/second

- https://spider.031337.xyz/

---

Another hint was posted that referenced http headers. We can take a look at the headers by using the `-I` option in curl. When putting the numbers 0-2 into the url, I noticed the `x-flag` header corresponded to the first few characters of the flag.

For example, https://spider.031337.xyz/0 -> `x-flag: i`, https://spider.031337.xyz/1 -> `x-flag: c`, https://spider.031337.xyz/2 -> `x-flag: t`, and so on

Below is a script that appends all the numbers from 0 to length of the flag to the url and prints the value of the x-flag header

```sh
#!/bin/bash
count=0
while true; do
	res="$(curl -sI https://spider.031337.xyz/${count} | grep "x-flag" | cut -d " " -f 2)"
	echo $res
	if [[ $res =~ "}" ]]; then
		break
	fi
	((count=count+1))
	sleep 0.4;
done
```

Running the script gives us the flag one character at a time.

`ictf{f0ll0w_th3_numb3rs}`

---

# Find Me

> Points: 50 | Category: Misc | Author: Robin_Jadoul

Oh no, the flag is hiding. Don't think or it'll get away...

- https://imaginary.ml/r/5D9B-find_me.txt

---

Download the file and grep the start of the flag (ictf)

```sh
$ wget https://imaginary.ml/r/5D9B-find_me.txt
$ grep -ai "ictf{" 5D9B-find_me.txt
```

`ictf{gr3p_0r_4ny_t3xt_3d1t0r...Fl4g_f0rm4t_m4tt3rs}`

---

# filling-blanks

> Points: 50 | Category: Reversing | Author: ainz

Who doesn't like filling blanks? Fix the code and get the flag.

- https://imaginary.ml/r/9178-predict
- https://imaginary.ml/r/6D64-solve.py
- `nc 20.94.210.205 1337`

---

We're given a python file with blanks that we're supposed to fill in.

```py
from pwn import *
from ctypes import CDLL, c_int

exe = ELF('./predict', checksec=False)
libc = CDLL('libc.so.6')

p = remote('20.94.210.205', 1337)

# constants = [____.rand() for i in range(4)]
constants = [libc.rand() for i in range(4)]

p.recvuntil('flag: \n')

# seed = libc.____(0) // 10
seed = libc.time(0) // 10
libc.srand(seed)

for i in range(4):
    random = libc.rand()

    # constants[i] = _____(constants[i] * (random % 1000))
    constants[i] = c_int(constants[i] * (random % 1000))

# constants = [i._____ for i in constants]
constants = [i.value for i in constants]

guess = c_int(sum(constants)).value

p.sendline(str(guess))
print(p.recv())
```

Running the python file will give us the flag.

`ictf{exp3r1menting_w1th_ch@lleng3_f0rmat$_f0r_$c1ence}`

---

# Uncrashable

> Points: 50 | Category: Pwn | Author: puzzler7

My friend is hosting a server he claims is completely untouchable! Can you crash it to prove him wrong?

- https://imaginary.ml/r/FCD8-babypwn2.c
- https://imaginary.ml/r/05A5-babypwn2
- `nc oreos.imaginaryctf.org 30000`

---

This challenge was a simple buffer overflow attack. Spamming the input field with junk will cause a segfault which will give us the flag.

```
$ nc oreos.imaginaryctf.org 30000

This is a very secure and foolproof server, which is very important, because if this server broke, very bad things would happen.
Please enter an address for the information you would like to retrieve. The first 8 bytes entered will be treated as the address. Large addresses will be truncated.

Address: ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
Here is your very important information: 0x66666666.
Goodbye!
Error 11: ictf{b0f?ju5+_@dd_m0r3_buff3r}
```

`ictf{b0f?ju5+_@dd_m0r3_buff3r}`

---

# Add and Add

> Points: 75 | Category: Crypto | Author: Robin_Jadoul

It just keeps on adding?

- https://imaginary.ml/r/CB05-add_and_add.py
- https://imaginary.ml/r/032C-output.txt

---

```py
import string

with open("output.txt") as f:
    ct = f.readline()
flag = b"ictf{"

def getHex(s):
    x = b'\xbd'
    while s:
        y = s[:len(x)]
        s = s[len(x):]
        x += bytes([(a+b)%256 for a,b in zip(x,y)])
    return str(x[1:].hex())

while flag.decode()[-1] != "}":
    for i in string.printable:
        #print(getHex(flag+i.encode()))
        if getHex(flag + i.encode()) in ct:
            flag += i.encode()
            break
print(flag.decode())
```

`ictf{4dd1ng_4nd_casc4d1ng_and_adding_4nd_c4scad1ng}`

---

# Word

> Points: 50 | Category: Forensics | Author: StealthyDev

Word documents. We use them everywhere and they can be just as sneaky as PDF.

- https://imaginary.ml/r/D55F-Word.docx

---

I solved this challenge by using the `strings` command and then grepping the beginning of the flag.

```sh
$ strings D55F-Word.docx | grep ictf
```

Another way of doing this challenge is to unzip the file. This will create a directory with all the unzipped content. Once you open the directory, you'll see a `flag.txt` file with the flag in it.

`ictf{4ll_0ff1c3_F1l3s_4r3_Z1p5}`

---

# Mission Impossible

> Points: 75 | Category: Reversing | Author: puzzler7

Your mission, should you choose to accept it, it to discover the flag hidden in this binary. As always, should you or any of your teammates be caught or killed, the ictf Board will disavow any knowledge of your actions. Good luck.

- https://imaginaryctf.org/r/9062-mission

---

```py
def putchar(n):
    print(chr(n), end="")

putchar(0x69);
putchar(99);
putchar(0x74);
putchar(0x66);
putchar(0x7b);
putchar(0x6e);
putchar(0x33);
putchar(0x76);
putchar(0x33);
putchar(0x72);
putchar(0x5f);
putchar(0x72);
putchar(0x75);
putchar(0x6e);
putchar(0x5f);
putchar(0x40);
putchar(0x5f);
putchar(0x62);
putchar(0x69);
putchar(0x6e);
putchar(0x40);
putchar(0x72);
putchar(0x79);
putchar(0x5f);
putchar(0x79);
putchar(0x30);
putchar(0x75);
putchar(0x5f);
putchar(100);
putchar(0x30);
putchar(0x6e);
putchar(0x74);
putchar(0x5f);
putchar(0x2b);
putchar(0x72);
putchar(0x75);
putchar(0x35);
putchar(0x74);
putchar(0x21);
putchar(0x7d);
putchar(10);
```

`ictf{n3v3r_run_@_bin@ry_y0u_d0nt_+ru5t!}`

---

# A really cool button

> Points: 50 | Category: Web | Author: Max49

That button looks VERY tempting to press.. will you press it?

- https://button.max49.repl.co/

---

```sh
$ curl -sX POST https://button.max49.repl.co/noflag.htm
```

`ictf{f1ag_1n_th3_c0mm3nts!}`

---

# small enough

> Points: 100 | Category: Crypto | Author: Robin_Jadoul

Hmm, there's definitely something to be factored here, but is it enough?

- https://imaginary.ml/r/290A-small_enough.py
- https://imaginary.ml/r/E212-output.txt

---

The program generates 5 random large prime numbers (let's call them [p, q, r, s, t]). It assigns the product of the first three ([p, q, r]) to a variable `N1` and the product of the last three ([r, s, t]) to a variable `N2`.

We can get the value of `r` by doing `gcd(N1, N2)`. We still don't have enough information to get either of the other 2 primes, but based on the name of the challenge, we can assume that the flag is smaller than `r`. This means we can essentially "replace" `N` with `r` when computing `d` and from here, getting the flag is easy.

```py
#!/usr/bin/env python3
from math import gcd
from Crypto.Util.number import long_to_bytes

N1 = 1354616944055425568429155646763721157050005833918771653842088349185561579185569786053596610005971046860156170074868089349434873638776801169039096108786319236080460557543040233527915085522601980147117409461921174418431909137832963507394178031344449370193688811539847060652109108672313366472749313185770315283575848363784164121798791735477731497217780326990634467083004635096892523188203729889490992358140596472369382314177481901877704885967882466797093841051444641
N2 = 734888425692009517806596710542917995288296755432316763296320438485255812606155457371930497770694452945082355675143307685518127631619274254510792085165142938278078046077176268425761437751075508839969620550675163327813101242939350843809839340713674745241270978956115781771026314052358698070281827120204821571910601243357910915844077625211338069004855974936931032302013946238264380954854287650589464612415365966740796461355265942743157008278052488948826109009212163
c1 = 336230498507814696802991321600495287646767287609112573410833806536366969013950060082945350002045594807735478359333001387264016681832536418768609649698649956641307011245165886799368552972534267621469656607547395347304702569293702143168375869379104872396923941268878210568060478491463240388016042386513527427403635885415306030110730565450160758441657878302243544708431360050900153235106176166586862772188881866569485993875717856609728704718642294143901864293792646
e = 65537

r = gcd(N1, N2)
d = pow(e, -1, r-1)

flag = long_to_bytes(pow(c1, d, r))
print(flag)
```

`ictf{sometimes_a_prime_is_all_you_really_need}`

---

# Winnie

> Points: 75 | Category: Reversing | Author: Robin_Jadoul

You had guided dynamic reverse engineering, let's try some less guided static reverse engineering.
There's a lot of stuff, not everything is very relevant.

- https://imaginary.ml/r/D14D-winnie.exe

---

---

# Tax Evasion

> Points: 100 | Category: Misc/pyjail | Author: puzzler7

They caught you for tax evasion and hauled you off to jail. Can you escape the audit? Flag is in flag.txt

https://www.python.org/dev/peps/pep-0578/

- https://imaginary.ml/r/BC94-jail.py
- `nc stephencurry.imaginaryctf.org 5000`

---

---

# kevin

> Points: 75 | Category: Forensics | Author: ainz

Recover the secret from shares using [Shamir's Secret Sharing Scheme](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing)
Shares are hidden in the image in the format `ictf{share}`

And don't forget to wrap the secret within `ictf{}` before submitting it as flag.

Fun fact: The heads of King Ghidorah are named Ni(2), Ichi(1) and San(3) from left to right in this image.

- https://imaginary.ml/r/CFB8-kevin5.png

---

---

# Keycode

> Points: 100 | Category: Reversing | Author: puzzler7

What a wild world that this would be, if this file was both the lock and the key!

- https://imaginary.ml/r/91A2-key

---

---

# eyes-on-stack

> Points: 75 | Category: pwn | Author: ainz

`pwntools` black magic go brrr...

- https://imaginary.ml/r/3C28-pwn_12c
- `nc 20.94.210.205 1338`

---

---

# Textbook RSA 2: Timmy the Lonely Oracle

> Points: 100 | Category: Crypto | Author: puzzler7

Timmy encrypted a flag for his friend, but he got lonely because no one else wanted to use his encryption system. To make new friends, he's offering to encrypt or decrypt anything you want - provided, of course, you don't decrypt the flag. Can you betray the trust of your new friend and steal the flag out from under his nose ||you monster||?

- https://imaginary.ml/r/45C6-oracle.py
- `nc oreos.imaginaryctf.org 6789`

---

Useful article: https://bitsdeep.com/posts/attacking-rsa-for-fun-and-ctf-points-part-1/

The server allows us to encrypt plaintext and decrypt ciphertext. It also gives us the cipertext of the flag, but it (of course) does not allow us to decrypt it. We need to find a way to encrypt some value that will, in turn, allow us to decrypt the flag.

`ictf{y0u_m@d3_+!mmy_cry_y0u_3v!1_h@ck3r}`

---

# Beep Boop Bakery

> Points: 100 | Category: Web, Programming, Crypto | Author: Artemis37

I'm hungry, so I want to go buy some chocolate chip cookie sfrom the bakery, but I'm also broke. Can you help me figure out a way to get cookies for a discount? Apparently, the baker gets 80% off on all cookies, but robots are banned from the bakery.

- https://bakery.031337.xyz/

---

```py
import requests
from bs4 import BeautifulSoup
import hashlib

storeURL = "https://bakery.031337.xyz/"
discountCodesURL = "https://bakery.031337.xyz/topsecret/alldiscountcodes.txt"
secretURL = "https://bakery.031337.xyz/secret_menu.html"

def SHA256(m):
    return hashlib.sha256(m.encode()).hexdigest()

allCodes = requests.get(discountCodesURL).text.split("\n")[2:]
codeHash = BeautifulSoup(requests.get(secretURL).content, 'html.parser').find("p").text.split(": ")[1]

for code in allCodes:
    if SHA256(code) == codeHash:
        p = requests.post(storeURL, data={"discount_code": code}, cookies={"customer_type":"baker"})
        print(p.text)
        break
```

`ictf{y@y_n0w_1_c@N_3a+_mY_d3l1ciOU$_C0okIe5!!!}`

---
