# Textbook RSA 1: Many Mods Run Amok

> Points: 100

> Category: Crypto

## Description

I've heard that RSA can be broken if you reuse the same modulus. Just to be safe, I'm using a gazillion moduli! Now nothing can force open my encryption!

## Attachments

https://fdownl.ga/60D2FE1E1F, https://fdownl.ga/E9B7F950D3

## Solution

### POST-SOLVE NOTE

The `eval()` function evaluates the input and gives it as a list of tuples. This achieves the same thing as my old code but in a much neater and more understandable way.

```py
vals = eval(open("output.txt", "r").read())
ans = ""
for i in vals:
    for asciiVal in range(32, 127):
        if pow(asciiVal, 65537, i[0]) == i[1]:
            ans += chr(asciiVal)
```

### Back to the original solution...

```py
with open("output.txt") as f:
    line = f.readline().replace("[", "").replace("(", "")
    line = line.split("), ")

    lines = [ i.split(", ") for i in line ]
    del lines[-1]

    ans = ""
    for i in lines:
        for asciiVal in range(32, 127):
            if pow(asciiVal, 65537, int(i[0])) == int(i[1]):
                    ans += chr(asciiVal)
    print(ans)
```

### Flag

> ictf{s0_m@ny_m0d5_s01!++13+!m3}
