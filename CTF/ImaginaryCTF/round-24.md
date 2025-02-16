# Sanity Check Round 24

> Points: 15 | Category: Misc | Author: Board

Welcome to round 24!

- `ictf{w3lcome_to_round_24!}`

---

Flag is given to us

`ictf{w3lcome_to_round_24!}`

---

# basic

> Points: 30 | Category: Misc | Author: Board

What is 51 (decimal) in base12? Wrap your answer in `ictf{}`

---

51 in base 10 is equivalent to 43 in base 12

`ictf{43}`

---

# Lost Flag

> Points: 50 | Category: Forensics | Author: a 6,000 foot tall fire squid

Quasar said he lost his flag but it seems to me like it's in this file.

- https://imaginaryctf.org/f/fOweJ#flag.zip

---

A `flag.zip` file is provided. This is what the unzipped content looks like

```sh
lucas@lucas:/tmp/flag$ ls -a
.  ..  .DS_Store  flag.jpg
```

I thought I would just check out the `.DS_STORE` file, although I wasn't expecting to find anything useful in there

```sh
lucas@lucas:/tmp/flag$ cat .DS_Store 
.jpgIloflag.jpgIlocblob�������!ictf{mac_is_better_than_templeos}Ilocblob��������
                                                                               @� @ @ @
       DSDB ` @ @ @
```

To my surprise, there was the flag.

`ictf{mac_is_better_than_templeos}`

---

# Rotating Secret Assembler

> Points: 50 | Category: Crypto | Author: puzzler7

Encrypt the flag as many times as you want! I'm making sure to never use the same public key twice, just to be safe

- `nc puzzler7.imaginaryctf.org 3000`

---

Connecting to the server shows us the [source code](https://pastebin.com/wbkFhUBS). We should focus our attention to the `get_new_primes()` method, because that's how `p` and `q` are getting chosen. Here's an illustration of how it works

```
queue = [a,b,c,d,e,f,g,h,i,j]
get_new_primes() -> returns last two elements of the queue: (i,j)
		 -> updates queue by removing last element and prepending a new prime
queue = [x,a,b,c,d,e,f,g,h,i]
```

What would `get_new_primes()` return if we were to call it again? Let's try it

```
queue = [x,a,b,c,d,e,f,g,h,i]
get_new_primes() -> returns last two elements of the queue: (h,i)
		 -> updates queue by removing last element and prepending a new prime
queue = [y,x,a,b,c,d,e,f,g,h]
```

As we can see above, if we were to encrypt the flag twice, we can compute one of the primes (let's say `p`) by doing GCD(n1,n2). We can get `q` by doing `q=n/p`. Now we have enough information to derive the private key, and use that to decrypt the ciphertext.

```py
#!/usr/bin/env python3
from pwn import *
import re
from math import gcd
from Crypto.Util.number import long_to_bytes

r = remote("puzzler7.imaginaryctf.org", 3000)
r.recv()
r.recv()
r.sendline(b"y")
firstPK = int(re.search(r"\((\d+)\,", r.recvS())[1]) # first n value
firstEnc = int(re.search(r": (\d+)", r.recvS())[1]) # ciphertext
r.sendline(b"y")
secondPK = int(re.search(r"\((\d+)\,", r.recvS())[1]) # second n value
r.close()

p = gcd(firstPK, secondPK)
q = firstPK // p
e = 65537
totN = (p-1)*(q-1)
d = pow(e, -1, totN)
m = pow(firstEnc, d, firstPK)
print(long_to_bytes(m))
```

The script does all the work for us and returns the flag

`ictf{why_would_I_throw_away_perfectly_good_primes?}`

---

# Pickle

> Points: 50 | Category: Misc | Author: Eth007

This pickle seems to be hiding the flag...

- https://imaginaryctf.org/f/1O5dx

---


---

# xorrot

> Points: 50 | Category: Reversing | Author: puzzler7

I'm rather tired, but I whipped up this xor scheme. See if you can crack it?

- https://imaginaryctf.org/f/VTZOE#xorrot.py

---

What the provided `xorrot.py` file is doing is first generating a random byte (255 possible options) and saving that in a variable called `key`. It then reads each character of the flag and xors the flag character with the key and immediately after sets the key equal to the flag char. Clearly, the randomly generated key only affects the first character. While we *can* find original value of the key, we actually don't have to. The important thing to notice is that the `i`th byte of the output is equal to `flag[i] XOR key`, but after the first character, the key is equal to `flag[i-1]` (the previous flag character). So, `output[i] = flag[i] XOR flag[i-1]`. We know the flag starts with the letter 'i', so to get the rest of the flag, we just need to start a for loop at index 1 and append `output[i] XOR flag[i-1]`. Below is a python script which does just that.

```py
#!/usr/bin/env python3
enc = "970a17121d121d2b28181a19083b2f021d0d03030e1526370d091c2f360f392b1c0d3a340e1c263e070003061711013b32021d173a2b1c090f31351f06072b2b1c0d3a390f1b01072b3c0b09132d33030311"

out = bytes.fromhex(enc)
flag = [ord('i')]
for i in range(1, len(out)):
    flag.append(out[i] ^ flag[i-1])
print(bytes(flag).decode())
```

`ictf{it_would_probably_help_if_the_key_affected_more_than_just_the_first_char_lol}`

---

# Almost SSTI

> Points: 50 | Category: Web | Author: puzzler7

I heard that you can prevent SSTI by enforcing really strong restrictions on user input length, so I've  done that! Surely my webserver is now completely impregnable from any bugs.

- http://puzzler7.imaginaryctf.org:3002/

---

The attached link shows us the backend source code. We can see it's a flask application that has a `/ssti` route. The `ssti` route has a request argument called `query`, but the length of this argument cannot exceed length 2. If it passes the length check, the server calls the flask [render_template_string()](https://tedboy.github.io/flask/generated/flask.render_template_string.html) method on it.

At first I tried passing in random punctuation characters, to the `query` field, but after getting nowhere, I thought to myself: *What if I just don't provide the query argument?*

***NOTE: The intended solution was to pass {{ into the query field, because that causes an error. For this challenge, simply not including the argument in the URL, which is what I did, yields the same result***

When I went to `puzzler7.imaginaryctf.org:3002/ssti`, this page popped up

![ALT](https://i.imgur.com/Dc6ONbM.png)

Nice, looks like we're on the right track. This webapge is the build-in flask debugger. We can run arbitrary python code from the browser here. I opened up the interactive python shell and got the flag

```py
>>> import os
>>> os.listdir()
['man_this_sure_is_an_odd_name_for_a_flag_file', 'Dockerfile', 'docker-compose.yml', 'server.py']
>>> open('man_this_sure_is_an_odd_name_for_a_flag_file').read()
'ictf{oops_I_left_my_debugger_on_I_need_to_run_home_before_my_webserver_burns_down}'
```

`ictf{oops_I_left_my_debugger_on_I_need_to_run_home_before_my_webserver_burns_down}`

---

# Relatively Small Arguments

> Points: 75 | Category: Crypto | Author: puzzler7

Last challenge before the big CTF! Bog-standard RSA, but I made some of the numbers smaller, just for slightly faster calculation.

- https://imaginaryctf.org/f/3z4Km#rsa.py

---

In this challenge's implementation of RSA, `d`, the private key, is a randomly generated 32 bit prime number. 32 bits is too much to brute force, but it still felt really small in the grand scope of things. After doing some research, I came across [this crypto stackexchange](https://crypto.stackexchange.com/questions/109/rsa-with-small-exponents) thread that discusses the security issues with using small exponents in RSA. One of the posts said this

> You need to keep d larger than the 4th root of n=pq. Otherwise Wiener's Attack can be used to compute d

In this situation, `n` is the product of two 512 bit primes, whereas `d` is a mere 32 bit prime number. It's pretty clear Wiener's attack is the way to go.

I found [this website](https://cryptohack.gitbook.io/cryptobook/untitled/low-private-component-attacks/wieners-attack) (highly recommended checking it out) that dives into the theory behind Wiener's attack and also provides a sagemath implementation of it in python. That's where the code below came from. I replaced the example `c, e, n` values with the ones we were given and ran it in [SageMathCell](https://sagecell.sagemath.org/)

```py
def wiener(e, n):
    # Convert e/n into a continued fraction
    cf = continued_fraction(e/n)
    convergents = cf.convergents()
    for kd in convergents:
        k = kd.numerator()
        d = kd.denominator()
        # Check if k and d meet the requirements
        if k == 0 or d%2 == 0 or e*d % k != 1:
            continue
        phi = (e*d - 1)/k
        # Create the polynomial
        x = PolynomialRing(RationalField(), 'x').gen()
        f = x^2 - (n-phi+1)*x + n
        roots = f.roots()
        # Check if polynomial has two roots
        if len(roots) != 2:
            continue
        # Check if roots of the polynomial are p and q
        p,q = int(roots[0][0]), int(roots[1][0])
        if p*q == n:
            return d
    return None
# Test to see if our attack works
if __name__ == '__main__':
    n = 134872711253918655399533296784203466697159038260837702891888089821702090938512308686613559851138816682269099219724900870388583883202954112422023894133671598222066489215524613014212242490437041258588247678792591072443719118562580052842727775772283919113007499992167089258075609504428713653013187230671841726369
    e = 50920242742169837294267124730818234703309561711363177522992049271988492365017092545331352650316729342598781520444569769450329777448285534584484096179230968844630486688656705514759778561817709539781927624692111848722199024819005269510690240743887870339853351421726436719236180272680237157536332997570569192069
    c = 133155317855020316110137499609990113815646625767974277474197900721563685454745247616867035013963212538345727281661922602291072931578581035070345294335733120033652413487827994383327148598029065495228796201084369245315585407592741900307825557286213370482646401885352854920924352919398804532780740979273692054391

    d = wiener(e,n)
    assert not d is None, "Wiener's attack failed :("
    print(pow(c,d,n))
```

This gives us the decrypted message as type long integer. Converting this number to bytes uncovers the flag

```py
>>> from Crypto.Util.number import long_to_bytes
>>> long_to_bytes(52412147353052408266368823661871870241518361252898097260353704506631296985625701331972477)
b'ictf{have_fun_at_ICTF_22!!!_559543c1}'
```

`ictf{have_fun_at_ICTF_22!!!_559543c1}`

---

# Self-Reference

> Points: 75 | Category: Misc | Author: puzzler7

I recently learned that python lists can contain themselves. Here's a whole bunch of such lists - do they also contain a flag?

- https://imaginaryctf.org/f/LrX5f#out.pickle

---

Unpickling the file gives us a list (which we'll call `unpickled`) of 128 elements. Each one of these elements is a self-referencing list. The fact that `unpickled` has 128 elements is a major hint, because that means the index of each element most likely corresponds to a character's decimal representation on the ascii table. Since we know the flag starts with "ictf", there has to be some sort of relation between `unpickled[ord('i')]` -> `unpickled[ord('c')]` -> `unpickled[ord('t')]` and so on.

```py
#!/usr/bin/env python3
import pandas as pd

unpickled = pd.read_pickle("out.pickle")
def solve(i, seen=[]):
    if i in seen:
        return []
    ret = [i]
    # must iterate through entire array to compare values, can't use .index() method
    # unpickled.index(unpickled[i][0]) -> RecursionError
    # have to use `is` keyword to compare self-referenced lists
    for idx, el in enumerate(unpickled):
        if el is unpickled[i][0]:
            ret += solve(idx,seen+[i])
	    break
    return ret

print(bytes(solve(ord('i'))))
```

`ictf{sphInx_oF-blaCk=qu@rTz~jUdge+my^v0w}`

---

# Unchained

> Points: 75 | Category: Web | Author: puzzler7

I've written so many Flask apps in the past, I thought I'd diversify a bit and try out Django! I wrote this Django app for you guys out of Django **and nothing else so don't even bother looking!**

- http://puzzler7.imaginaryctf.org:3006/

---

I got the flag by going to this url: `http://puzzler7.imaginaryctf.org:3006/flag?user=admin&user=test`. The reason this works is because of something known as **HTTP parameter pollution**, which is a "web application vulnerability exploited by injecting encoded query string delimiters in already existing parameters". In the case of this challenge, it works because Django reads the last definition of user, and Flask reads the first.

HTTP parameter pollution: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/HTTP%20Parameter%20Pollution

`ictf{only_accessible_by_schroedinger's_admin}`

---

# Geoguessr Sucks

> Points: 50 | Category: Forensics | Author: puzzler7

I went on a crazy vacation to the middle of nowhere, and I took a real picture with a real camera of this very important field. Where is it?

Flag format is `ictf{lat_long}`, both rounded up to 5 decimal places beause this exact spot is very important to me. Example: `ictf{1.23456_7.89101}`

- https://imaginaryctf.org/f/1OD91

---

Naturally, the first thing I wanted to do was take a look at the [exif metadata](https://blog.idrsolutions.com/what-is-exif-metadata/), because that's more than likely where the GPS information would be. I googled "view image metadata online", and one of the first results was [this website](https://jimpl.com/). I uploaded the provided image onto the site, and it pulled up all the image exif data. After skimming through the results, I found the latitude and longitude

![GPS data](https://i.imgur.com/nTDuwAQ.png)

Great. The only step now is to convert those results to decimal degrees, because right now it's in degree/minute/second (DMS) format. [This website](https://andrew.hedges.name/experiments/convert_lat_long/) is an online DMS to decimal converter. Putting in the information from the screenshot above gives us `lat=39.133697222222224, long=-98.52833611111112`. Round those to 5 decimal places and we got the flag.

`ictf{39.13370_-98.52834}`

---
