# Sanity Check Round 13

> Points: 15 | Category: Misc | Author: Board

Thanks to everyone who participated in ImaginaryCTF 2021 last weekend! I hope y'all are ready for some daily challenges!

- `ictf{unpuzzled_1_was_a_bad_challenge_sorry}`

---

This one's pretty self explanatory

`ictf{unpuzzled_1_was_a_bad_challenge_sorry}`

---

# hippopotomonstrosesquippedaliophobia

> Points: 30 | Category: Misc | Author: Eth007

ptsd

- https://imaginaryctf.org/r/54E9-hippopotomonstrosesquippedaliophobia.zip

---

When we unzip the file, we get a ton of directories and files. The flag is in one of the files, so we can use the `-r` option (which means recursive) in grep to find it.

`$ grep -r ictf{`

After typing that command in the terminal, we get the flag.

`ictf{grep_-r_to_the_rescue}`

---

# Compressed

> Points: 50 | Category: Misc | Author: Max49

My friend compressed my flag! Can you reinflate it so it returns back to its normal size please?

- `785ecb4c2e49abaeca314c8a372c8e4f3630c8892f324ccf28b1af050079eb092d`

---

I just put the attached string into cyberchef and used the `magic` function

`ictf{zl1b_1s_c00l_r1ght?}`

---

# Rules of Engagement

> Points: 50 | Category: Programming | Author: StealthyDev

The rules are simple. If the number is divisible by two, shift it right by one. Otherwise, if it is a multiple of five, divide by five. If it is neither but it is a multiple of three, then add eight. Repeat this process until it yields the flag.

- `328125, 309375, 3712, 3264, 384375, 221875, 1536, 1536, 3200, 296875, 2752, 303125, 3648, 153125, 303125, 3136, 3456, 159375, 296875, 2496, 303125, 340625, 159375, 359375, 296875, 2624, 296875, 2688, 3328, 159375, 328125, 3648, 296875, 1536, 371875, 3520, 296875, 2624, 159375, 371875, 303125, 3648, 3200, 390625`

---
```py
#!/usr/bin/env python3
arr = [328125, 309375, 3712, 3264, 384375, 221875, 1536, 1536, 3200, 296875, 2752, 303125, 3648, 153125, 303125, 3136, 3456, 159375, 296875, 2496, 303125, 340625, 159375, 359375, 296875, 2624, 296875, 2688, 3328, 159375, 328125, 3648, 296875, 1536, 371875, 3520, 296875, 2624, 159375, 371875, 303125, 3648, 3200, 390625]

while arr[0] != 105:
    for index, num in enumerate(arr):
        if num % 2 == 0: 
            arr[index] >>= 1
        elif num % 5 == 0:
            arr[index] //= 5
        elif num % 3 == 0:
            arr[index] += 8

print("".join(list(map(chr, arr))))
```

`ictf{G00d_Var1abl3_Nam3s_R_Th3ir_0wn_R3ward}`

---

# Another Login Page

> Points: 50 | Category: Web | Author: Max49

I have another login page for you to get past! I'm so confident it's secure that I'll even give you the source code!

- https://login-page.max49.repl.co/

---

The backend website source code tells us what us what the admin username and password is (username: theadminaccount, password: sfN7?v,2T3ZVSk+2RX~). We cannot simply type this into the login fields because both the username and password input fields have a `max-length` attribute set to 10.

There are a few ways of doing this challenge. One way is to write a python script that makes the post request, which bypasses the max-length problem we have on the login form.

```py
import re
import requests

url = "https://login-page.max49.repl.co/login"
r = requests.post(url, data={'username': 'theadminaccount', 'pass': 'sfN7?v,2T3ZVSk+2RX~'})

flag = re.search(r"ictf\{.+\}", r.text)
print(flag.group(0))
```

Another way of doing this challenge is to simply delete the `max-length` attributes on both the input fields on the login page and submit the username and password there. 

Lastly, we can use curl to send the information, but it's important that we change our user-agent to something else because Max49 blocked curl requests.

```sh
$ curl -A "qwerty" -X POST -d "username=theadminaccount" --data-urlencode "pass=sfN7?v,2T3ZVSk+2RX~" https://login-page.max49.repl.co/login
```

All 3 methods work and will get us the flag.

`ictf{cr34t3_sup3r_l0ng_chann3l_nam3s_by_ed1t1ng_maxlength!_roocash_}`

---

# I can't hear you!

> Points: 75 | Category: Crypto | Author: puzzler7

My friend's trying to send me a message, but her ISP is really sketchy, so her messages just come out all garbled and a *bit flipped*. I think she said something about making sure I get the message with a **repetition code**? I'm not sure - can you translate for me?

- https://imaginaryctf.org/r/5EA1-flag.txt

---

```py
from Crypto.Util.number import *

ct = "100011110010101100010101001011011100100010111110100011101101000110010010100101110010001110011100100110011101011100101110010110010100011001010110100101010011011011011111001110011100001000011110100101110100001010100111100101011010101011011100001101001011101101110011100011110001011010010100010101110010000110000011010110110001100000100111100011101110100100101100001110010011101110110101010110011001010101001100001011101010100101100111001111101101010001011101001101101011010010010001100110011010110001010101010101011011100111100010100101111100010111001101010011010110011011011111100110101110010101010010001101101100110000001000010011110100010111001101010110010101011101111011000111011001110111111001010110110010011011011110001011101100101010001110001101011111100010101110010111110100001011001011100011110011101011001110"

blocks = [ct[i:i+3] for i in range(0, len(ct), 3)]

binFlag = ""
for b in blocks:
    counter = 0
    for c in b:
        counter += int(c)
    binFlag += str(round(counter/3))
print(long_to_bytes(int(binFlag, 2)))
```

`ictf{I_can_hear_despite_the_noise}`

---

# Sense and Sensibility

> Points: 50 | Category: Misc/Reversing | Author: StealthyDev

Apply a little Marianne and some common Elinor to overcome the fact this was clearly not written by Jane.

- https://imaginaryctf.org/r/6F8F-SenseAndSensibility.py

---

---

# Trace the Line

> Points: 75 | Category: Web | Author: Sajiv/puzzler7

I have a request - can you take your pencil and trace a perfect line???

Note: There is a rate limit of 5 requests/second.

- http://puzzler7.imaginaryctf.org:7000/

---

```py
import re
import time
import requests

url = "http://puzzler7.imaginaryctf.org:7000/"
nextStr = ""
counter = 0
while True:
    r = requests.get(url + nextStr)
    if "ictf{" in r.text and "fake" not in r.text:
        print(re.search(r"ictf\{.*\}", r.text).group(0))
        break
    nextStr = re.search(r"action=\"/(.+)\"", r.text).group(1)
    counter += 1
    print("FAILED ATTEMPT {}".format(counter))
    time.sleep(0.2)
```

`ictf{7h3r3-r3@lly-1s-@-l1n3!!!}`

---
