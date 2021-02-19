# privacy-enhanced-email

> Points: 125

> Category: Misc (Cracking)

## Description

I need to read the encrypted flag ``flag.enc`` but I am not a very technical person. Help me out please.

## Attachments

https://fdownl.ga/9B59988A1C

## Solution

We need the passphrase for ``private.pem`` file in order to decrypt ``flag.enc``. We will use john the ripper.

1. First we need to convert the pem file into a format john can understand. We use ``ssh2john.py`` for this.

    ```sh
    $ locate ssh2john.py
    /usr/share/john/ssh2john.py

    $ python /usr/share/john/ssh2john.py private.pem > privateHash
    ```

2. Now that it's in a format john can understand, we can try and crack the password.

    ```sh
    $ john --wordlist=rockyou.txt privateHash
    ```

    After the program runs for a bit, we can see that the password is ``sherlock``

3. Now that we have the passphrase, we can decrypt ``flag.enc``

    ```sh
    $ openssl rsautl -decrypt -inkey private.pem -in flag.enc 
    ```

    It will ask you for the passphrase. Type it in and you will see the flag.

### Flag
> ictf{1ts_raining_easy_challenges_yay!}
