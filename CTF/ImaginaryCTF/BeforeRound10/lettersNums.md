# Letters = Numbers

> Points: 75

> Category: Programming

## Description

In `values.txt`, it links each letter to a numerical value. In `wordlist.txt`, there is a list of words. Each word's letters have values which when added up, make the value of the word. Your goal is to find out which 7 letter word in `wordlist.txt` has the largest value. Flag Format is `ictf{word_with_largest_value_here}`.

## Attachments

https://fdownl.ga/C6EC972D42

## Solution

1. Save the letters and their values to a dictionary

2. Check 7 character long words by iterating through each character and adding it's corresponding value (which we get from the dictionary) to a counter.

3. Output the word with the largest value.

```py
# Python3
with open("values.txt") as f:
    vals = {}
    line = f.readline()
    while line:
        letter, value = line.split("=")[0].rstrip(), int(line.split("=")[1].rstrip())
        vals[letter] = value
        line = f.readline()

with open("wordlist.txt") as f:
    lines = f.readlines()
    lines = [ line.rstrip() for line in lines if line.strip() ]
    answer = ["", 0] # word, val
    for word in lines:
        if len(word) != 7:
            continue
        count = 0
        for char in word:
            count += vals[char]
        if count > answer[1]:
            answer = [word, count]
print(answer[0])
```

### Flag

> ictf{zyzzyva}
