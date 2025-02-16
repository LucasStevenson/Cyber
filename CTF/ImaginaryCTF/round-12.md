# Sanity Check Round 12

> Points: 15 | Category: Misc | Author: Board

Welcome to Round 12! DM flags to me to get points, and riseup on the leaderboard! Have fun and enjoy Round 12!

- `ictf{Round_12_Sanity_Check}`

---

This one is pretty self explanatory

`ictf{Round_12_Sanity_Check}`

---

# smth here

> Points: 30 | Category: smth here | Author: enter name here

smth here

- `:4E7L6oD*049p==b?8b=D07EHN`

---

Rot47 cipher. Decipher with cyberchef

`ictf{e@sY_chAll3ng3ls_ftw}`

---

# Slow Internet

> Points: 50 | Category: Web, OSINT | Author: Max49

My friend got me a cool business router for my birthday, but I don't know the login information so I can't set it up. Could you help me out and log into my router as admin?

Note: this is a "default business router"

- https://router.max49.repl.co/

---

I googled "default business router login", and it said the default username for a business router is `cusadmin` and the default password is either `highspeed` or `CantTouchThis`

```sh
$ curl -X POST -d username="cusadmin" -d pass="highspeed" https://router.max49.repl.co/
```

I used `highspeed` as the password, but either one would have worked.

`ictf{w3b_and_0s1nt???_gr3at_c0mb0!}`

---

# Quintessentially Quick Quiche

> Points: 50 | Category: Web | Author: Tirefire

Try out a new HTTP protocol version!

- https://quiche.tirefire.org/

---

I opened the website, reloaded the page, and got the flag.

This reason this works is because modern browsers are http3 enabled, but they need the `alt-svc` header. The first time I visit the website, my browser connects with http1, but the site uses the `alt-svc` header to tell my browser it uses http3. The next time my browser connects (by either reloading or revisiting the page), it uses http3.

*Credit to puzzler7 for this explanation*

`ictf{who_knew_http_over_udp_could_be_so_quic(k)}`

---

# Caesar Tart

> Points: 75 | Category: Crypto | Author: A-Z

My friend sent me a message with a flag inside. More importantly, it also speaks about apple tarts. To test your dedication about apple tarts, I made a fancy caesar cipher to encrypt it. Retrieve the flag and hail the tarts!

Note: flag format is ictf{ALLCAPSNOUNDERSCORE}

- https://imaginaryctf.org/r/40CD-caesartart.py

---

This "fancy caesar cipher" is actually just a viginere cipher. I used [dcode](https://www.dcode.fr/vigenere-cipher) to get the plaintext.



`ictf{MORELIKEVIGENERETART}`

---

# Web Flagchecker

> Points: 75 | Category: Web | Author: puzzler7

I make too many reversing challenges, because flagcheckers are easy. I thought I should expand my horizons, so I made a web challenge

- http://puzzler7.imaginaryctf.org:4000/

---

This is a SSTI attack. The goal is to inject python into the input parameter so that we can access the flag variable, which is already in the flask template context.

`ictf{@llth3br@ck3t5}`

---

# how r u

> Points: 75 | Category: Misc | Author: Artemis37

how are you today? here is a challenge.

- https://imaginaryctf.org/r/518A-how_r_u.py
- `nc stephencurry.imaginaryctf.org 5005`

---

The file provided is a very simple python program that takes in user input, does nothing with it, and prints a useless message if an error occurs. The important thing to notice in this challenge is that the python version is 2.7, and it uses the `input()` function instead of `raw_input()`. The thing about `input()` in python2 is that it *evaluates* the input, meaning that we can put python code in there and the program will run it.

`ictf{n3v3r_3v3r_3v3r_3v3r_u53_1npu7_1n_pyth0n2}`

---

# Too old

> Points: 75 | Category: Crypto/Programming | Author: fbibad/puzzler7

I've used one of the oldest & weakest PRNG to cipher this message. Could you decipher it?

- https://imaginaryctf.org/r/8641-old.py

---

The key is in the range `[10**7, 10**8]`. This range is small enough for us to get the key via brute force. Once we have the key, we can get the plaintext by xoring each byte of the ciphertext with `key&0xff`.

```py
#!/usr/bin/env python3
import random

ct = bytes.fromhex("e649fd7458fb36acb341346324635da87427d8d25f5c8b7665b921052727bf730f1c0273d00c23217873")
flag = b"ictf{"

def test_key(key):
    out = b""
    for c in flag:
        out += bytes([c ^ (key&0xff)])
        if ct.startswith(out) == False:
            return 0
        key = int("{:016d}".format(key**2)[4:12])
    return key

def get_key():
    for i in range(10**7, 10**8+1):
        if test_key(i) != 0:
            return test_key(i)

key = get_key()
for i in ct[len(flag):]:
    flag += chr(i^(key&0xff)).encode()
    key = int("{:016d}".format(key**2)[4:12])
print(flag.decode())
```

`ictf{f0r_l00p5_r1ght_th3r3_4nd_3v3ryWh3r3}`

---
