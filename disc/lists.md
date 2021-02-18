# Lists

> Points: 100

> Category: Reverse Engineering

## Description

Joe was learning about lists, but I don't understand what the hell he did! Could you help me?

## Attachments

https://fdownl.ga/649A09306A

## Solution

```py
arr = [{'character': 'pf{', 'index': 1}, {'character': 'tic', 'index': 0}, {'character': 'lk1', 'index': 4}, {'character': '}lz', 'index': 5}, {'character': 'hyt', 'index': 2}, {'character': 's0n', 'index': 3}]
ans = [None for _ in (arr)]

for i in arr:
    string = ""
    for k, v in i.items():
        if k == "character":
            string += v[1]+v[2]+v[0]
        else:
            ans[v] = string
print(''.join(ans))
```

### Flag
> ictf{pyth0nsk1llz}
