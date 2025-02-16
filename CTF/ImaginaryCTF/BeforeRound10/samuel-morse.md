# Samuel Morse

> Points: 50

> Category: Crypto

## Description

https://en.wikipedia.org/wiki/Morse_code

## Attachments

`qq xqxq x qqxq { xx 0 qxq qqq 3 _ xqxq 0 xqq 3 _ qq qqq _ xqxq 0 0 qxqq }`

Note: flag is in uppercase (format: ICTF{.\*})

## Solution

Looking at the encoded flag, I noticed it's mostly made up of _x_ and _q_'s. Since the flag starts with _ictf_, it's safe to say that **qq = i, xqxq = c** and so on.

![table](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b5/International_Morse_Code.svg/315px-International_Morse_Code.svg.png)

**.. = i and -.-. = c** in morse code, so that means **q = .** and **x = -**

Below is a python script that replaces every q with a dot and every x with a dash and then decodes it.

```py
s = "qq xqxq x qqxq { xx 0 qxq qqq 3 _ xqxq 0 xqq 3 _ qq qqq _ xqxq 0 0 qxqq }"
s = s.replace("q", ".").replace("x", "-")


character = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z','0','1','2','3','4','5','6','7','8','9']
code = ['.-', '-...', '-.-.', '-..', '.', '..-.', '--.', '....', '..', '.---', '-.-', '.-..', '--', '-.', '---', '.--.', '--.-', '.-.', '...', '-', '..-', '...-', '.--', '-..-', '-.--', '--..', '-----', '.----', '..---', '...--', '....-', '.....', '-....', '--...', '---..', '----.']

def decode(s):
  s = s.split(" ")
  ans = ''.join([ character[code.index(i)] if i in code else i for i in s ])
  return ans
print(decode(s))
```

### Flag

> ICTF{M0RS3_C0D3_IS_C00L}
