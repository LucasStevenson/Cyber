# PINsmith

> Difficulty: Easy

## Description

After analyzing CygnusCorp's leaked credentials, you successfully infiltrate a secondary layer of their internal network.  
During your investigation, you discover developer credentials that were inadvertently committed to an internal code repository.
Among the files, you uncover a partial PIN used to access a critical system, though it is incomplete.

CygnusCorp enforces a strict security policy for PINs: no two adjacent digits can be the same.

While the full PIN is obscured, the fragments you've uncovered, along with a leaked access report, provide enough information to craft potential combinations.
However, due to system rate-limiting and the risk of detection, brute-forcing this PIN directly on the terminal would be inefficient and risky.

Instead, you need to construct a list of valid PINs based on the known pattern.
By educated brute-forcing, you can pre-generate the possibilities and test each one in rapid succession, increasing your chances of success while minimizing the time spent interacting with the system.

Your task:
Enumerate every possible valid PIN code that fits the revealed pattern, adhering to the policy of no adjacent repeats.
This list will allow you to systematically and efficiently break into the target system with minimal noise.

Rules:
* Known digits are fixed and must appear in the indicated positions.  
* Unknown positions are represented by "*".  
* Digits may repeat in the PIN, but not in adjacent positions.  
* Output the valid PINs in ascending lexicographical order.

Input Format:
* A single string representing the PIN template.  
* The length of the string determines the length of the PIN.
* Known digits are shown ("0"-"9"), unknown positions are shown as "*".

Output Format:
* Print each valid PIN on a separate line.
* The output must be sorted in ascending lexicographical order.

2 <= number of digits in PIN <= 10

1 <= number of revealed digits <= 6

```
Example Input:
*23

Expected output:
023
123
323
423
523
623
723
823
923
```

## Solution

I'm going to go with a brute force approach first and see what happens.

Here's the overall idea
1. generate every possible combination for the wildcards
2. insert each combination of wildcard values into the PIN template
3. check if modified PIN template is valid
4. Only if it's valid, print it out

> We're basically generating 10^{num of wildcards} combinations and testing each one

```py
# This is a very brute-forcey approach. I'm sure there's a better way, but this is what I'm gonna keep for now
from itertools import product

pin_template: str = input()
def is_valid(pin: str):
    for i in range(1, len(pin)):
        if pin[i] == pin[i-1]:
            return False
    return True

wildcard_idxs = [i for i, c in enumerate(pin_template) if c == '*']
splitted_pin = list(pin_template)

for digits in product(range(0, 10), repeat=len(wildcard_idxs)):
    for i, d in enumerate(digits):
        splitted_pin[wildcard_idxs[i]] = str(d)
    guess = "".join(splitted_pin)
    if is_valid(guess):
        print(guess)
```

## Flag

`HTB{3ff1c13nt_P1n_Cr4cK1nG}`
