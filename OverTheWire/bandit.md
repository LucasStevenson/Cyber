# OverTheWire "Bandit" Game

This is my writeup for the OverTheWrite "Bandit" challenges found [here](https://overthewire.org/wargames/bandit/)

The first half-ish of these challenges are mostly geared towards beginners. I personally found that the difficulty started to ramp up a bit starting from Level 12 onwards

## Level 0

First, we need to `ssh` into the server

```sh
$ ssh bandit0@16.16.163.126 -p 2220
```

When it asks for the password, we will put in `bandit0` (this is given to us in the challenge description)

When we successfully `ssh` into the server, there is a `readme` file that if we `cat` out will show us a password

```sh
$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

We will use the password `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If` to advance onto level 1

**Password**: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

## Level 1

We disconnect from `bandit0` user by pressing `Ctrl-D`. Now we need to `ssh` into the **bandit1** user with the password we got from the previous challenge

```sh
$ ssh bandit1@bandit.labs.overthewire.org -p 2220
```

When it asks for the password, we put in `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If` (previous challenge password)

Now we are inside the `bandit1` user. The goal here is to cat out the contents of a file literally named a hyphen: `-`

This is what happens when we `ls` the home directory

```sh
$ ls -la
-rw-r-----  1 bandit2 bandit1   33 Sep 19 07:08 -
drwxr-xr-x  2 root    root    4096 Sep 19 07:08 .
drwxr-xr-x 70 root    root    4096 Sep 19 07:09 ..
-rw-r--r--  1 root    root     220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root    root    3771 Mar 31  2024 .bashrc
-rw-r--r--  1 root    root     807 Mar 31  2024 .profile
```

The way to view the contents inside of the file is to run the following `cat` command. It will then show us the password

```sh
$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

**Password**: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

## Level 2

All we need to do for this challenge is to `cat` out the contents of a file that has spaces. We could use backslash characters to escape the filename, or we could simply type in the first few characters of the file and then press `Tab` and it will autocomplete it for us

```sh
$ cat spaces\ in\ this\ filename
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

**Password**: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`


## Level 3

For this challenge, there is a directory called `inhere`, and using a simple `ls -laR` command to recursively list everything in the directory, we can see that there is a hidden file inside the directory

```sh
$ ls -laR
total 24
drwxr-xr-x  3 root root 4096 Sep 19 07:08 .
drwxr-xr-x 70 root root 4096 Sep 19 07:09 ..
-rw-r--r--  1 root root  220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root root 3771 Mar 31  2024 .bashrc
drwxr-xr-x  2 root root 4096 Sep 19 07:08 inhere
-rw-r--r--  1 root root  807 Mar 31  2024 .profile

./inhere:
total 12
drwxr-xr-x 2 root    root    4096 Sep 19 07:08 .
drwxr-xr-x 3 root    root    4096 Sep 19 07:08 ..
-rw-r----- 1 bandit4 bandit3   33 Sep 19 07:08 ...Hiding-From-You
```

We will simply cat out the contents of the hidden file that will show us the password

```sh
$ cd inhere
$ cat ...Hiding-From-You
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

**Password**: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

## Level 4

In this challenge, there are many files inside of `inhere`, but only one of them has human-readable text that contains the password. In order to find which file is the one we want to look inside, I ran the following `file` command to see the filetypes of each file

This is what we see at first when we `ls` the `inhere` directory

```sh
$ ls -la
drwxr-xr-x 2 root    root    4096 Sep 19 07:08 .
drwxr-xr-x 3 root    root    4096 Sep 19 07:08 ..
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file00
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file01
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file02
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file03
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file04
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file05
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file06
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file07
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file08
-rw-r----- 1 bandit5 bandit4   33 Sep 19 07:08 -file09
```

Then I ran the `file` command to see the filetypes of all the files in the directory

```sh
$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

As we can see, `-file07` is the one that contains our password

```sh
$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

**Password**: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`


## Level 5

The hint for this challenge is this

```
human-readable
1033 bytes in size
not executable
```

With this information, we can use the `find` command in with the `size` parameter to recurisvely search for a file of size `1033` bytes in size

Running the command below will show us the file within the deeply nested `inhere` directory with the password

```sh
$ find . -type f -size 1033c
./inhere/maybehere07/.file2
$ cat ./inhere/maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

**Password**: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`


## Level 6

This is the challenge description

```
owned by user bandit7
owned by group bandit6
33 bytes in size
```

Just like the previous challenge, we can use the `find` command with the data in the challenge description

This time, we're going to make use of the `-group` and `-user` tags
* The `-group` tag helps us find files accessible by users of a certain group
* The `-user` tag helps us find files owned by a particular user

```sh
$ find . -type f -size 33c -group bandit6 -user bandit7 2>/dev/null
./var/lib/dpkg/info/bandit7.password
```

(I ran the command above in the root directory because there was nothing in the home directory)

Now we just need to cat out that file that was returned to us from the `find` command

```sh
$ cat ./var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

**Password**: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`


## Level 7

Challenge Description: `The password for the next level is stored in the file data.txt next to the word millionth`


In this challenge, we are simply given a `data.txt` file in the home directory. We can `cat` out the  contents of the file and use `grep` to find the password next to the word `millionth`

```sh
$ cat data.txt | grep millionth
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

**Password**: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`



## Level 8

Challenge Description: `The password for the next level is stored in the file data.txt and is the only line of text that occurs only once`

We need to find a way to unqiuely filter out the entries. We are going to use the `uniq` command along with `sort`

```sh
$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

The reason that we need to sort the data first before passing it through `uniq` is explained in uniq's manpages itself: "Repeated lines in the input will not be detected if they are not adjacent, so it may be necessary to sort the files first".

**Password**: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`


## Level 9

Challenge Description: `The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.`

Since we know that the file contained some un-humanreadable text, to filter out the readable stuff from the unreadable, we will use `strings`

```sh
$ strings data.txt
```

The `strings` command outputs quite a bit of gibberish, but going off of the challenge description and looking through the output, we can see that the password is most likely `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

**Password**: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`


## Level 10

Challenge Description: `The password for the next level is stored in the file data.txt, which contains base64 encoded data`

All we need to do in this challenge is base64 decode the contents in `data.txt`. We can do this in one simple command

```sh
$ cat data.txt | base64 --decode
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

**Password**: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`


## Level 11

Challenge Description: `The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions`

According to a quick google search, we can rotate text by piping the text to this command: `tr 'a-z' 'n-za-m' | tr 'A-Z' 'N-ZA-M'`

To solve this challenge, we just run the command below

```sh
$ cat data.txt | tr 'a-z' 'n-za-m' | tr 'A-Z' 'N-ZA-M'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

**Password**: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`


## Level 12

> Sidenote: The way I did this challenge feels rather cumbersome, but at least it works

Challenge Description: `The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)`

We're given a txt file that is the hexdump of a heavily compressed file that we're trying to convert back to it's original state. 

The first thing we do is convert the hexdump into a binary file. I did this using `xxd` with the `-r` option. I then went through the iterative process of checking the filetype of the newly created file and then performing an action depending on the filetype.

For instance, the filetype after converting the hexdump to a binary file was a `gzip` file. This meant that we needed to `gunzip` the file. This peels back one layer of compression. After gunzip-ing, we then had a `bzip2` compressed file. We decompress it with the `-d` option, and keep this general process going until we get the original text with the password.

The commands below show some of what I did to solve this level.

```sh
lucas@lucas:/tmp/testing$ xxd -r data.txt > out.bin
lucas@lucas:/tmp/testing$ ls
data.txt  out.bin
lucas@lucas:/tmp/testing$ file out.bin 
out.bin: gzip compressed data, was "data2.bin", last modified: Thu Sep 19 07:08:15 2024, max compression, from Unix, original size modulo 2^32 574
lucas@lucas:/tmp/testing$ mv out.bin out.gz
lucas@lucas:/tmp/testing$ gunzip out.gz 
lucas@lucas:/tmp/testing$ ls
data.txt  out
lucas@lucas:/tmp/testing$ file out 
out: bzip2 compressed data, block size = 900k
lucas@lucas:/tmp/testing$ bzip2 -d out
...[continues the process]
lucas@lucas:/tmp/testing$ file data8
data8: ASCII text
lucas@lucas:/tmp/testing$ cat data8
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

**Password**: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`


## Level 13

Challenge Description: `The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on`

`Cat`-ing out the `sshkey.private` file in the home directory shows us the private key

```sh
$ cat sshkey.private
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----
```

So this RSA private key is technically our "password", but in order to use it to access the next level, we need to do the following

1. Close ssh connection to the server by pressing `Ctrl-D`
2. Navigate to the `/tmp` directory and create a new file that will hold the private key (I called this file `privatekey`)
3. Save the private key into the new file (`privatekey` in my case) 
4. Change the permissions of `privatekey` to 600 (**read** and **write** operations only for the current user) with the command `chmod 600 privatekey`
5. Access level 14 with the following command: `ssh -i privatekey bandit14@bandit.labs.overthewire.org -p 2220`


## Level 14

Challenge Description: `The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.`

> Sidenote: This challenge took me WAY longer than it should have. I completely forgot that in the previous challenge, it literally told us where the password was located (`/etc/bandit_pass/bandit14`). I was so confused when the challenge description so casually mentioned "the password". Whoopsies :P

We can `cat` out the password and then pipe it to netcat `nc`, which will transfer the data to port 30000 on localhost

```sh
$ cat /etc/bandit_pass/bandit14 | nc localhost 30000
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

**Password**: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`


## Level 15

Challenge Description: `The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.`

All we need to do for this challenge is send the password in `/etc/bandit_pass/bandit15` to port 30001 over a SSL/TLS connection. We can do this using `openssl`

```sh
$ cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001
...[buncha stuff about setting up the connection]
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

**Password**: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`


## Level 16

Challenge Description: `The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.`

My first thought was to use `nmap` for this challenge. The idea is to scan for all the open ports within the specified range and get info such as which services are running on each port. Below is the nmap command I ended up going with 

```sh
$ nmap -p31000-32000 localhost
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00016s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

* The `-p` argument specifies the port range that we want to scan

As we can see, port `31518` and `31790` are running SSL, which is what we want. I tested port `31790` first by running the command below to send the current level's password over a SSL/TLS connection. It turns out that `31790` was the correct port, and it gave us an RSA private key

```sh
$ cat bandit16 | openssl s_client -ign_eof localhost:31790
...[buncha stuff about setting up the connection]
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

Just like before, we have to update the permissions for the file that will hold this private key

```sh
$ chmod 600 privatekey2
```

Now we can continue on to level 17 with this command: `ssh -i privatekey2 bandit17@bandit.labs.overthewire.org -p 2220`

## Level 17

Challenge Description: `There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new`

To view the difference between two files, we can use the `diff` command

```sh
$ diff passwords.old passwords.new
42c42
< ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

The `diff` command shows change between two versions of text
* The line starting with `< ktfg...` is from the original text, which is `passwords.old` in this case. 
* The line starting with `> x2gL...` is from the new version, which is `passwords.new` in this case

**Password**: `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`


## Level 18

Challenge Description: `The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.`

We can make use of ssh's `-t` flag, which according to the man pages `Force[s] pseudo-terminal allocation. This can be used to execute arbitrary screen-based programs on a remote machine`

```sh
$ ssh -t bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

After entering the password, the output from the `cat readme` command is containing the password is immediately outputted

**Password**: `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`


## Level 19

Challenge Description: `To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.`

There is an executable file in our home directory. Running the binary file tells us that we can run terminal commands as another user

```sh
$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
```

Since we can run commands as another user, let's `cat` out the contents of `/etc/bandiit_pass/bandit20` to get the password

```sh
$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

**Password**: `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`


## Level 20

Challenge Description: `There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).`

For this challenge, we need to setup a local server on a port that will accept incoming connections and responds to the client with the password from the previous challenge. To setup the local server, I used netcat (`nc`) with the `-l` option. 

> According to the man pages, `-l` is "used to specify that nc should listen for an incoming connection rather than initiate a connection to a remote host"

I ran the command below to start up our server

```sh
$ while true; do echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -lv 8080; done
Listening on 0.0.0.0 8080
```

In another terminal window, I executed the binary in the home directory and passed in the port number of our local server

```sh
$ ./suconnect 8080
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
```

Back in the terminal running our local server, we now see the password

```sh
Connection received on localhost 34530
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
Listening on 0.0.0.0 8080
```

**Password**: `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`


## Level 21

Challenge Description: `A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.`

I first looked inside the `/etc/cron.d/` directory for anything interesting. After listing out the contents of the directory, `cronjob_bandit22` looked the most promising, so I `cat`-ed out the file and saw that it was running a shell script 

```sh
$ cd /etc/cron.d
$ ls
cronjob_bandit22  cronjob_bandit24  otw-tmp-dir
cronjob_bandit23  e2scrub_all       sysstat
$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Looking into the `/usr/bin/cronjob_bandit22.sh` file, we see the following 

```sh
$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

The password from `/etc/bandit_pass/bandit22` is being redirected to `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`. All we need to do is `cat` out the contents of that file.

```sh
$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

**Password**: `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`


## Level 22

Challenge Description: `A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.`

Just like in the previous challenge, I ran the same commands to look into the most interesting/promising cronjobs in `/etc/cron.d`

```sh
$ cd /etc/cron.d
$ ls
cronjob_bandit22  cronjob_bandit24  otw-tmp-dir
cronjob_bandit23  e2scrub_all       sysstat
$ cat cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

Let's take a look inside `/usr/bin/cronjob_bandit23.sh`

```sh
$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

> I initially thought that I could simply run the program since it literally prints out the path that it's copying the password to, but then I realized that the `myname` variable is not set to what we want. We want it to equal `bandit23`, but `whoami` will return `bandit22`.

What I ended up doing is running the following command

```sh
$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

This gives us the name of the file in `/tmp` that we need to cat out to get the password

```sh
$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

**Password**: `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`


## Level 23

Challenge Description: `A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.`

```sh
$ cd /etc/cron.d/
$ cat cronjob_bandit24 
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

Contents inside of `/usr/bin/cronjob_bandit24.sh`

```sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

The shell script above iterates over all the files in `/var/spool/bandit24/foo`, checking if the file owner is `bandit23`. If it finds a match, it will run the file and terminate the process only if it takes longer than 60 seconds.

To solve this challenge, we just need to write a simple shell script with file owner `bandit23` that redirects the password in `/etc/bandit_pass/bandit24` to a file that we can look inside. All the steps are shown below.

```sh
$ cd /tmp
$ vim script.sh
#!/bin/sh
cat /etc/bandit_pass/bandit24 > /tmp/chall23_password.txt

$ chown bandit23 script.sh
$ chmod 777 script.sh
$ cp script.sh /var/spool/bandit24/foo
```

Now we wait for a minute (because that's how often the cronjob runs) and cat out the file

```sh
$ cat chall23_password.txt
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

**Password**: `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`


## Level 24

Challenge Description: `A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time`

I decided to use the `pwntools` python library for this challenge. `Pwntools` allows us to easily send and receive data from a server, which makes it perfect for brute-forcing the 4-digit pin.

```py
from pwn import *
r = remote("localhost", 30002)
print(r.recv())

for i in range(0, 10001):
    r.sendline(f'gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 {i}'.encode())
    response = r.recv()
    if b"Wrong" not in response:
        print(response)
        break
```

`The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4`

**Password**: `iCi86ttT4KSNe1armKiwbQNmB3YJP3q4`


## Level 25

Challenge Description: `Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.`

We can immediately see `bandit26.sshkey` in our home directory. This is the privatekey necessary to log into `bandit26`.

<details>

<summary>bandit26.sshkey</summary>

```sh
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEApis2AuoooEqeYWamtwX2k5z9uU1Afl2F8VyXQqbv/LTrIwdW
pTfaeRHXzr0Y0a5Oe3GB/+W2+PReif+bPZlzTY1XFwpk+DiHk1kmL0moEW8HJuT9
/5XbnpjSzn0eEAfFax2OcopjrzVqdBJQerkj0puv3UXY07AskgkyD5XepwGAlJOG
xZsMq1oZqQ0W29aBtfykuGie2bxroRjuAPrYM4o3MMmtlNE5fC4G9Ihq0eq73MDi
1ze6d2jIGce873qxn308BA2qhRPJNEbnPev5gI+5tU+UxebW8KLbk0EhoXB953Ix
3lgOIrT9Y6skRjsMSFmC6WN/O7ovu8QzGqxdywIDAQABAoIBAAaXoETtVT9GtpHW
qLaKHgYtLEO1tOFOhInWyolyZgL4inuRRva3CIvVEWK6TcnDyIlNL4MfcerehwGi
il4fQFvLR7E6UFcopvhJiSJHIcvPQ9FfNFR3dYcNOQ/IFvE73bEqMwSISPwiel6w
e1DjF3C7jHaS1s9PJfWFN982aublL/yLbJP+ou3ifdljS7QzjWZA8NRiMwmBGPIh
Yq8weR3jIVQl3ndEYxO7Cr/wXXebZwlP6CPZb67rBy0jg+366mxQbDZIwZYEaUME
zY5izFclr/kKj4s7NTRkC76Yx+rTNP5+BX+JT+rgz5aoQq8ghMw43NYwxjXym/MX
c8X8g0ECgYEA1crBUAR1gSkM+5mGjjoFLJKrFP+IhUHFh25qGI4Dcxxh1f3M53le
wF1rkp5SJnHRFm9IW3gM1JoF0PQxI5aXHRGHphwPeKnsQ/xQBRWCeYpqTme9amJV
tD3aDHkpIhYxkNxqol5gDCAt6tdFSxqPaNfdfsfaAOXiKGrQESUjIBcCgYEAxvmI
2ROJsBXaiM4Iyg9hUpjZIn8TW2UlH76pojFG6/KBd1NcnW3fu0ZUU790wAu7QbbU
i7pieeqCqSYcZsmkhnOvbdx54A6NNCR2btc+si6pDOe1jdsGdXISDRHFb9QxjZCj
6xzWMNvb5n1yUb9w9nfN1PZzATfUsOV+Fy8CbG0CgYEAifkTLwfhqZyLk2huTSWm
pzB0ltWfDpj22MNqVzR3h3d+sHLeJVjPzIe9396rF8KGdNsWsGlWpnJMZKDjgZsz
JQBmMc6UMYRARVP1dIKANN4eY0FSHfEebHcqXLho0mXOUTXe37DWfZza5V9Oify3
JquBd8uUptW1Ue41H4t/ErsCgYEArc5FYtF1QXIlfcDz3oUGz16itUZpgzlb71nd
1cbTm8EupCwWR5I1j+IEQU+JTUQyI1nwWcnKwZI+5kBbKNJUu/mLsRyY/UXYxEZh
ibrNklm94373kV1US/0DlZUDcQba7jz9Yp/C3dT/RlwoIw5mP3UxQCizFspNKOSe
euPeaxUCgYEAntklXwBbokgdDup/u/3ms5Lb/bm22zDOCg2HrlWQCqKEkWkAO6R5
/Wwyqhp/wTl8VXjxWo+W+DmewGdPHGQQ5fFdqgpuQpGUq24YZS8m66v5ANBwd76t
IZdtF5HXs2S5CADTwniUS5mX1HO9l5gUkk+h0cH5JnPtsMCnAUM+BRY=
-----END RSA PRIVATE KEY-----
```

</details>

We still need to figure out what shell `bandit26` is using. To do this, we can look inside `/etc/passwd`.

```sh
$ grep -ai "bandit26" /etc/passwd
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

User `bandit26` uses `/usr/bin/showtext` as it's shell. Looking inside this file, we see this

```sh
$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

So when we log into `bandit26`, it runs this file and then immediately closes. There must be something about `more` that we can exploit...

