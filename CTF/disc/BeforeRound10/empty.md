# Empty

> Points: 100

> Category: Web

## Description

This website is raining cats and dogs but no flags :(

## Attachments

http://oreos.ctfchallenge.ga:12345/

## Solution

Looking through the website source code, we see a comment all the way at the end that says

```html
<--qwerty:123--> is so op
```

Putting the username as **qwerty** and the password as **123** didn't work.

For this challenge, I used burpsuite as a proxy. While intercepting the data being passed between me and website, I saw a cookie named `ChangeValue` being set to `True` by the backend. By using the username `qwerty` and password `123`, as well as changing the value of `ChangeValue` to `False`, we get the flag.

### Flag

> ictf{3mptine55_3v3rywh3r3}
