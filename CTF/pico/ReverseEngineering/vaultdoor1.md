# vault-door-1

> Points: 100

> Category: Reverse Engineering

## Description

This vault takes some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: [VaultDoor1.java](https://jupiter.challenges.picoctf.org/static/87e103a8db01087de9ccf5a7a022ddf8/VaultDoor1.java)

## Solution

We can modify the code so that instead of checking the values, we can assign it to an array and then just print out the array.

1. Create a char array named `password` with size 32

2. Replace `.charAt(val)` with `[val]`

3. Replace all the double equals (check) with single equals (assign)

```java
class vaultdoor1 {
    public static void main(String[] args) {
        char[] password = new char[32];
        password[0] = 'd';
        password[29] = 'a';
        password[4] = 'r';
        password[2] = '5';
        password[23] = 'r';
        password[3] = 'c';
        password[17] = '4';
        password[1] = '3';
        password[7] = 'b';
        password[10] = '_';
        password[5] = '4';
        password[9] = '3';
        password[11] = 't';
        password[15] = 'c';
        password[8] = 'l';
        password[12] = 'H';
        password[20] = 'c';
        password[14] = '_';
        password[6] = 'm';
        password[24] = '5';
        password[18] = 'r';
        password[13] = '3';
        password[19] = '4';
        password[21] = 'T';
        password[16] = 'H';
        password[27] = '6';
        password[30] = 'f';
        password[25] = '_';
        password[22] = '3';
        password[28] = 'd';
        password[26] = 'f';
        password[31] = '4';
        System.out.println(password);
    }
}
```

### Flag

> picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4}
