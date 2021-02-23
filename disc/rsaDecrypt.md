# RSA Decryption

> Points: 150

> Category: Crypto

## Description

Normally, in a CTF challenge, you are given n and have to find p and q. Well, I've already factored n for you - what now?

## Attachments

https://fdownl.ga/BAAFA62B3B

## Solution

We are given all the variables except ``c``, so that's probably what we're trying to find and decode.

![c equation](https://latex.codecogs.com/svg.latex?\Large&space;c=(m^e)\bmod(N))

We plug in **m**, **e**, and **N** and use a [modular exponentiation](https://en.wikipedia.org/wiki/Modular_exponentiation) calculator (such as [this](https://planetcalc.com/8977/) one) to find **c**

``c = 3434882488929518722038467895115112756438984408989594973129585453336661922579219214878609731197``

Now we can decode this and get the flag

```py
# Python3
c = hex(3434882488929518722038467895115112756438984408989594973129585453336661922579219214878609731197)[2:]
print(bytes.fromhex(c).decode())
```
### Flag
> ictf{d3cryp+!on!5+h3_b35+_3ncryp+!on}

