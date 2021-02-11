# from-the-shadows

> Points: 125

> Category: Cracking

## Description
I managed to get my hands on some shadowy stuff. Can you crack the password? Rockyou might help.

Flag is ictf{PasswordYouGet}

## Attachments
https://fdownl.ga/CDCFDAEF4C

## Solution
We are given a zip file that contains a ``passwd`` and ``shadow`` file.

[Unshadow](https://www.commandlinux.com/man-page/man8/unshadow.8.html) the two files

```sh
unshadow passwd shadow > someFileName
```

Then run the file you just created through [John the Ripper](https://tools.kali.org/password-attacks/john) and use the rockyou wordlist.

```sh
john --wordlist=rockyou.txt someFileName
```

### Flag
> ictf{2complicated}
