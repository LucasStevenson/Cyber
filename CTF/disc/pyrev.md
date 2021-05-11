# pyrev

> Points: 50

> Category: rev

## Description

I think it's time a `dis.dis()` track...

HINT:

The output was generated from the following code (of course somewhat redacted)

```py
def f(n):
    # redacted stuff here

import dis
dis.dis(f)
```

## Attachments

https://imaginary.ml/r/7F0C-out.txt

## Solution

```py
n = ord("i")
for x in (0, 6, -17, 14, -21, 25, -23, 5, 15, 2, -12, 11, -1, 6, -4, -12, -6, 9, 8, 5, -3, -3, 6, -6, 4, -18, -6, 26, -2, -18, 20, -17, -9, -4):
    n -= x
    print(chr(n), end='')
```

### Flag

> ictf{bytecode_could_be_easy_as_py}
