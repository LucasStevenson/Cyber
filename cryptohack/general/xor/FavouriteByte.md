# Favourite Byte

For the next few challenges, you'll use what you've just learned to solve some more XOR puzzles.

I've hidden some data using XOR with a single byte, but that byte is a secret. Don't forget to decode from hex first.

`73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d`

## Solution

Since the data was XORed with only a single byte, we can brute force all 255 possibilities.

```py
enc = bytes.fromhex("73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d")
for secret in range(256):
    flag = ""
    for b in enc:
        flag += chr(b ^ secret)
        if flag[0] != 'c':
            break
    else:
        print(flag)
    # we use a for-else, so that we only print the flag if the for loop didn't break
```

### Flag

`crypto{0x10_15_my_f4v0ur173_by7e}`
