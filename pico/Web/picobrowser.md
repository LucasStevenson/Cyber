# picobrowser

> Points: 200

> Category: Web

## Description

This website can be rendered only by **picobrowser**, go and catch the flag!

``https://jupiter.challenges.picoctf.org/problem/28921/``

## Solution

When I click on the flag button, it says that I'm not picobrowser and shows my [user-agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) data.

I can set my user-agent with the curl ``-A`` option. Curl returns all the website's HTML, so we can pipe the output into grep and just get the flag.

```
curl -A "picobrowser" https://jupiter.challenges.picoctf.org/problem/28921/flag | grep picoCTF
```

### Flag
> picoCTF{p1c0_s3cr3t_ag3nt_84f9c865} 
