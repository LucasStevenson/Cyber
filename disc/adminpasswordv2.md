# adminpasswordv2

> Points: 100

> Category: Web Exploitation

## Description

Could you help Toby break into **this** website. Apparently, only oreobot is allowed in...

## Attachments

http://oreos.ctfchallenge.ga:1337/

## Solution

When I read the description, I automatically thought of the [picobrowser](https://github.com/LucasStevenson/CyberCTFs/blob/main/pico/Web/picobrowser.md) challenge.


1. We are going to change our user-agent to **oreobot** with the curl **-A** option.

2. Just like the original adminpassword challenge, the username and password is probably **admin** and **password**, respectively.

	- In order to put in these values to the form, we will add `-X POST -d username=admin -d password=password` to the curl command

```sh
$ curl -A "oreobot" -X POST -d username=admin -d password=password http://oreos.ctfchallenge.ga:1337/adminLogin
```

### Flag

> ictf{C00l_l33t_fl@gs}
