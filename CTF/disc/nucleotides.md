# Nucleotides

> Points: 150

> Category: Programming/Math

## Description

**Nucleotides** are the subunits which make up the DNA. The 4 kinds of nucleotides in the DNA are distinguished by their nitrogen heterocycle substituents: **adenine(A)**, **cytosine( C )**, **guanine(G)**, and **thymine(T)**.
In a parallel universe, the DNA is composed of 8 kinds of nucleotides instead of 4: **A, C, G, K, M, R, T, and U**. Find out how many unique strands of length 8 (may contain repeated nucleotides) are possible in this universe. Also, find the 250th strand (counting starts from 1) in this set, after this set is sorted alphabetically.

## Attachments

[Github Link](https://github.com/ainzs-evil-twin/ictf-Feb-2021/blob/main/nucleotides/README.md)

## Solution

```py
# CARTESIAN PRODUCT
from itertools import product

# this could also be an array of chars. Either way works
nucleotides = "ACGKMRTU"

ans = [ p for p in product(nucleotides, repeat=8) ]
strand = "".join(ans[249])
print(f"ictf{{{str(len(ans))}_{str(strand)}}}")
```

### Flag

> ictf{16777216_AAAAAKUC}
