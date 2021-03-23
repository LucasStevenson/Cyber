# simple rsa

> Points: 100

> Category: Crypto

## Description

Attacked on... more like alpertron

## Attachments

https://fdownl.ga/E90BE745E0

## Solution

#### Tools used

1. [alpertron](https://www.alpertron.com.ar/ECM.HTM) to factor N

The challenge description references a tool called alpertron. I never used it before, but after taking a look at their website, I put in **N** and alpertron factored it for me.

```
N = 660173 664989 916385 736367 593903 × 961197 989326 823320 457291 281679
```

Now that I have **p** and **q**, I can find **d** and decode the ciphertext.

And on the topic of the cipertext, it's given as a bunch of space separated numbers. Originally, this confused me because alpertron also returned numbers with spaces between them, so I thought there was some kind of significance to it. After asking around, I was told the spaces alpertron includes isn't important and as for the ciphertext, I had to decrypt each number individually.

Formula to find the flag: ![formula](<https://render.githubusercontent.com/render/math?math=m=(c_i^d)\bmod N>)

Below is a python script that decrypts the ciphertext

```py
from Crypto.Util.number import long_to_bytes
N = 634557599394827484518408716527054197491217109177784256003137
e = 17

p = 660173664989916385736367593903
q = 961197989326823320457291281679

totN = (p - 1) * (q - 1)

# In Python 3.8+, you can find d by simply doing:
# d = pow(e, -1, totN)

# but since I'm using Python 3.6, I used the method below
def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b // a) * y, y)

def modinv(a, m):
    g, x, y = egcd(a, m)
    if g != 1:
        raise Exception('modular inverse does not exist')
    else:
        return x % m
# Source: https://stackoverflow.com/questions/4798654/modular-multiplicative-inverse-function-in-python

d = modinv(e, totN)

ct = "22920183178010324016373443603515625 8429431933839268832485642678641699 124676848765984328031674121957933056 14002414191924244276669361796022272 337587917446653715596592958817679803 6599743590836592050933837890625 43276334103547425867991106950436269 5958260438588051333281183456765537 293844199047808331618283286773235712 54116956037952111668959660849 50544702849929377100000000000000000 29606831241262271996845213307591 4181203352191774128676605224609375 68660408884120282915274309282824192 1379209096840925342723840168019929 124676848765984328031674121957933056 19479004955562800041143429584912384 38115448583970168165554454528 50544702849929377100000000000000000 4181203352191774128676605224609375 6599743590836592050933837890625 50544702849929377100000000000000000 14211879482945166685530717421568 4181203352191774128676605224609375 8429431933839268832485642678641699 37553674644104207641884714074112 92764641967130171567625832766767104 4181203352191774128676605224609375 107612640045671820774919891357421875 31588152109649857868144549324788907 54116956037952111668959660849 37000180548008608733053875753320448 94152329294455713577749264203776 107612640045671820774919891357421875 4181203352191774128676605224609375 14002414191924244276669361796022272 124676848765984328031674121957933056 192441327313530246357280390753883639 444089209850062616169452667236328125".split(" ")

[ print(long_to_bytes(pow(int(i), d, N)).decode('utf-8'), end="") for i in ct ]
```

### Flag

> ictf{Amaz1nG_pYth0n_AnD_cHr_sk1lLs_ftw}
