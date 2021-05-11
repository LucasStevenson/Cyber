# JaWT Scratchpad

> Points: 400

> Category: Web Exploitation

## Description

Check the admin scratchpad!

`https://jupiter.challenges.picoctf.org/problem/61864/` or http://jupiter.challenges.picoctf.org:61864

## Solution

Since we cannot login as admin, we will login as a random user. We can see a jwt in localStorage and when we put it in [jwt.io](https://jwt.io/), we see the payload. In order to modify the payload data, which in this case is the user that we want to change to **admin**, we need the `secret key` that's used to sign the tokens.

For this, we'll use john the ripper.

1. Put our jwt into a txt file called **jwt.txt**

```sh
$ echo eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoidCJ9.TNaf1KB2o0J4rRc-hEgOAgb-Vn-x81qjDRfAfktzrAQ > jwt.txt
```

2. Run it through john the ripper and use rockyou wordlist

```sh
$ john --wordlist="rockyou.txt" jwt.txt
```

It gives us the string **ilovepico**

3. Go back to [jwt.io](https://jwt.io/) and put **ilovepico** into where it says "your-256-bit-secret". Then change the payload so that the user is **admin**.

4. Copy the new jwt and paste it into localStorage on the jawt website. Reload the page and there's the flag.

### Flag

> picoCTF{jawt_was_just_what_you_thought_1ca14548}
