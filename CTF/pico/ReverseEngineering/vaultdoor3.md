# vault-door-3

> Points: 200

> Category: Reverse Engineering

## Description

This vault uses for-loops and byte arrays. The source code for this vault is here: [VaultDoor3.java](https://jupiter.challenges.picoctf.org/static/a4018cec1446761cb2e8cce05db925fa/VaultDoor3.java)

## Solution

This is similar to vault-door-1 in that it is checking a user input to some kind of scrambled ciphertext.

Instead of adding the user input to the buffer array, we are going to add the ciphertext string to it and then output the buffer array.

```java
class vaultdoor3 {
    public static void getPassword() {
        String cipher = "jU5t_a_sna_3lpm12g94c_u_4_m7ra41";
        char[] buffer = new char[32];
        int i;

        for (i = 0; i < 8; i++) {
            buffer[i] = cipher.charAt(i);
        }

        for (; i < 16; i++) {
            buffer[i] = cipher.charAt(23-i);
        }

        for (; i<32; i+=2) {
            buffer[i] = cipher.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = cipher.charAt(i);
        }

        System.out.println(buffer);
    }

    public static void main(String[] args) {
        getPassword();
    }
}
```

### Flag

> picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}
