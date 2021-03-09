# vault-door-4

> Points: 250

> Category: Reverse Engineering

## Description

This vault uses ASCII encoding for the password. The source code for this vault is here: [VaultDoor4.java](https://jupiter.challenges.picoctf.org/static/834acd392e0964a41f05790655a994b9/VaultDoor4.java)

## Solution

1. Convert each element in `myBytes` array to it's base10 (decimal) representation with `Byte.toString()` method.

2. This will return a string, so convert to type `int` with `Integer.parseInt()` method

3. Convert it to it's string representation by putting `(char)` before it.

```java
class vaultdoor4 {
    public static void main(String[] args) {
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 0146, 064 ,
            'a' , '8' , 'c' , 'd' , '8' , 'f' , '7' , 'e' ,
        };

        for (int i = 0; i < myBytes.length; i++) {
            System.out.print((char) Integer.parseInt(Byte.toString(myBytes[i])));
        }
        System.out.println();
    }
}
```

### Flag

> picoCTF{jU5t_4_bUnCh_0f_bYt3s_f4a8cd8f7e}
