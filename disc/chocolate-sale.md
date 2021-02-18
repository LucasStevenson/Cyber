# chocolate-sale

> Points: 150

> Category: Web

## Description

Zyphen is craving  some dark chocolate, but the vendor's website is buggy again. Order some dark chocolate please.

## Attachments

https://bluelazyparentheses.oleanderson.repl.co/

## Solution

1. Order dark chocolate from the website

2. Just like the cookie-sale challenge, it says we ordered something else. In this case, it says we ordered white chocolate.

3. Check the website cookies

	> "admin=False; chocolate=white; hmac=0a019157c31445c9fdba88452dd59f8827ec58520ae7b49c97ae406dfc172bc0839dcda997e52e5f45cb88a658e1f03148cdcefcd27bfa3c1600a9d12b4df11b"

	- When we change the value of **chocolate** to **dark**, we see this message

		> Are you sure you want White Chocolate labelled as Dark? I doubt the  authenticity of your order
	
	- When we change the value of **admin** to **True**, we see this message

		> @admin#7241 pls implement hmac, set secret key to PotatoGLaDOS

4. Use the online [cyberchef](https://gchq.github.io/CyberChef/) tool and and search for hmac. Input the secret key and make sure to select UTF8.

	- In order to determine which hashing function to use, we go back to the chocolate website and look at the length of the hmac cookie value. It's 128 characters long, meaning that the hash function used is SHA512.

	- For the input, put **dark**, because that is the cookie value that we changed

	- Cyberchef recipe should look like this

		![cf output](https://i.imgur.com/IQ0FS5V.png)
	
5. We get this as the output: **e1611df80e3887fbb8582460f34d2bd8b5bf1f30e48f2a1bae9c96bacdb389c44a9c6a04dcbf147edd1978d4b450f5b8535072c6afd021890b039d1e5a4e66c5**

6. Go back to the chocolate website and change the value of the **hmac** cookie to **e1611df80e3887fbb8582460f34d2bd8b5bf1f30e48f2a1bae9c96bacdb389c44a9c6a04dcbf147edd1978d4b450f5b8535072c6afd021890b039d1e5a4e66c5**

### Flag
> ictf{why_w0uld_u_share_HM4C_key_l1ke_that}
