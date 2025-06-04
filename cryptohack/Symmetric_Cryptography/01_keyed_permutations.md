# Keyed Permutations

 AES, like all good block ciphers, performs a "keyed permutation". This means that it maps every possible input block to a unique output block, with a key determining which permutation to perform.

 > A "block" just refers to a fixed number of bits or bytes, which may represent any kind of data. AES processes a block and outputs another block. We'll be specifically talking the variant of AES which works on 128 bit (16 byte) blocks and a 128 bit key, known as AES-128.

 Using the same key, the permutation can be performed in reverse, mapping the output block back to the original input block. It is important that there is a one-to-one correspondence between input and output blocks, otherwise we wouldn't be able to rely on the ciphertext to decrypt back to the same plaintext we started with.

 What is the mathematical term for a one-to-one correspondence?


 ## Solution

The mathematical term for a one-to-one correspondence is a **bijection**

### Answer

`crypto{bijection}`
