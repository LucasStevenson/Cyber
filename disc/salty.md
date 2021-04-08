# Salty

> Points: 70

> Category: Reversing

## Description

I like my hashes salty. Like, REALLY salty.

## Attachments

[Python File Download](https://imaginary.ml/r/FB146915)

```py
#!/usr/bin/env python3

import os
import hashlib

def main():
        while True:
                command = input("bash-4.2$ ")
                checkInput(command)
                os.system(command)

def checkInput(inp):
    if hashlib.sha512(("salt" + inp).encode()).hexdigest() == "".join([chr(ord(n) ^ 69) for n in "p\'|$!$\'\'rttvp#\'utvw# q \'$#v#v\'#|qupr&t\'| ursvvu#vr&\'!!|q!p\'!q#uuw}&v v}!q\'s\'trrrv!!q|#& s$p$# r#$\'#&&\'#}p!sqpw |\'t#r} pqwv!p&$u&"]):
        print("You win!")
        print("".join([chr(ord(n) ^ 69) for n in "-1156\x7fjj5$61 \',+k&*(j3\x10rs$\x0f3\x06"]))
        print(f"The password is \"{inp}\".")
                os.system("clear")

if __name__ == "__main__":
        main()
```

## Solution

The `checkInput` function prepends the word "salt" to the input passed into it, then hashes the result. It compares the generated hash to this:

```py
"".join([chr(ord(n) ^ 69) for n in "p\'|$!$\'\'rttvp#\'utvw# q \'$#v#v\'#|qupr&t\'| ursvvu#vr&\'!!|q!p\'!q#uuw}&v v}!q\'s\'trrrv!!q|#& s$p$# r#$\'#&&\'#}p!sqpw |\'t#r} pqwv!p&$u&"])
```

The code above equals to:

`5b9adabb71135fb0132fe4ebaf3f3bf94057c1b9e076330f37cbdd94d5bd4f0028c3e38d4b6b17773dd49fce6a5afe7fabfccbf85d6452e9b1f78e5423d5ca0c`

Putting the hash into [crackstation](https://crackstation.net/), we get

| Hash                                                                                                                                   | Type   | Result    |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| 5b9adabb71135fb0132fe4ebaf3f3bf94057c1b9e076330f37cbdd94d5bd4f00<br />28c3e38d4b6b17773dd49fce6a5afe7fabfccbf85d6452e9b1f78e5423d5ca0c | sha256 | saltwater |

Since the function already prepends "salt" to the input, we know our password is **water**

After entering `water` into the input field, it tells us we are correct and gives us [this pastebin link](https://pastebin.com/vU76aJvC). It's a password protected paste, but putting in `water` again gives us the flag.

### Flag

> ictf{s4lty_w4ter_1nd33d_4f285a3}
