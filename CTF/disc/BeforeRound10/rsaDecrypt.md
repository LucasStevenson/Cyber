# RSA Decryption

> Points: 150

> Category: Crypto

## Description

Normally, in a CTF challenge, you are given n and have to find p and q. Well, I've already factored n for you - what now?

## Attachments

https://fdownl.ga/BAAFA62B3B

## Solution

### UPDATE

I didn't know the `pow` function in python can take three parameters (which returns a^b mod c).

I also didn't know about the `long_to_bytes` function in the python `Crypto` module. The code below does the same thing as the original solution but in an easier to follow and more concise way.

```py
>>> from Crypto.Util.number import long_to_bytes
>>> n = 123866076296518994316177296478401057712619155986200878799260448000669843742215227147742612238448786236537913842690449015629955372080450951285004618721197161153509895788558696454499259251716789706937392724214421501943502930012644776183903844650127938010978184494249206432961928770464951936577074082800337386673
>>> e = 65537
>>> m = 59608029134280521090530076650233022208585433513083249823903250368964980675769965381391962238262367887225477224953265451074398410499344587738342500806385379151320373742405142456066966027021177125457506467726430630196459340946541486778076530380330532284439882241478556294444494267069124934688943051219928225431
>>> c = pow(m, e, n)
>>> long_to_bytes(c)
```

### Back to the original solution...

We are given all the variables except `c`, so that's probably what we're trying to find and decode.

![formula](<https://render.githubusercontent.com/render/math?math=c=(m^e)\bmod(N)>)

We plug in **m**, **e**, and **N** and use a [modular exponentiation](https://en.wikipedia.org/wiki/Modular_exponentiation) calculator (such as [this](https://planetcalc.com/8977/) one) to find **c**

`c = 3434882488929518722038467895115112756438984408989594973129585453336661922579219214878609731197`

Now we can decode this and get the flag

```py
# Python3
c = hex(3434882488929518722038467895115112756438984408989594973129585453336661922579219214878609731197)[2:]
print(bytes.fromhex(c).decode())
```

### Flag

> ictf{d3cryp+!on!5+h3_b35+\_3ncryp+!on}
