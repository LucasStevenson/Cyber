# Encoding Challenge

Now you've got the hang of the various encodings you'll be encountering, let's have a look at automating it.

Can you pass all 100 levels to get the flag?

The *13377.py* file attached below is the source code for what's running on the server. The *pwntools_example.py* file provides the start of a solution using the incredibly convenient pwntools library. which you can use if you like. pwntools however is incompatible with Windows, so *telnetlib_example.py* is also provided.

For more information about connecting to interactive challenges, see the FAQ. Feel free to skip ahead to the cryptography if you aren't in the mood for a coding challenge!

Connect at `nc socket.cryptohack.org 13377`

[13377.py](https://cryptohack.org/static/challenges/13377_43de0a0efed6ed7bd890d1c79db22fb1.py)

[pwntools_example.py](https://cryptohack.org/static/challenges/pwntools_example_f93ca6ccef2def755aa8f6d9aa6e9c5b.py)

[telnetlib_example.py](https://cryptohack.org/static/challenges/telnetlib_example_5b11a835055df17c7c8f8f2a08782c44.py)

## Solution

```py
#!/usr/bin/env python3
from pwn import *
from Crypto.Util.number import long_to_bytes
import json
import base64
import codecs

r = remote("socket.cryptohack.org", 13377)

def decode(encoding, ct):
    if encoding == "hex":
        return { "decoded": bytes.fromhex(ct).decode() }
    elif encoding == "rot13":
        return { "decoded": codecs.decode(ct, 'rot_13') }
    elif encoding == "bigint":
        return { "decoded": long_to_bytes(int(ct[2:], 16)).decode() }
    elif encoding == "utf-8":
        return { "decoded": "".join([ chr(i) for i in ct ]) }
    elif encoding == "base64":
        return { "decoded": base64.b64decode(ct.encode()).decode() }

while True:
    inp = json.loads(r.recvlineS().rstrip())
    if "flag" in inp:
        print(inp["flag"])
        break
    # print(inp)
    ans = decode(inp["type"], inp["encoded"])
    r.sendline(json.dumps(ans))
```

### Flag

`crypto{3nc0d3_d3c0d3_3nc0d3}`
