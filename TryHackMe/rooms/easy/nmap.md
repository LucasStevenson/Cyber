# Nmap

## Task 1: Introduction

### Notes

When a computer runs a network service, it opens a networking construct called a "port" to receive the connection.

> I think of ports as a **logical/virtual endpoint used to identify which application (on a particular host) data should be sent to**

Ports are necessary for making multiple network requests or having multiple services available.

* For example, when you load several webpages at once in a web browser, the program must have some way of determining which tab is loading which web page. This is done by establishing connections to the remote webservers using different ports on your local machine.

![Alt text](https://i.imgur.com/3XAfRpI.png)

Every computer has `65535 total available ports`, however many of these are registered as standard ports (e.g. port 80 for HTTP webservice).

**Nmap** is a port scanner. The basic theory is this: nmap will connect to each port of the target in turn. Depending on how the port responds, it can be determined as being *open*, *closed*, or *filtered* (usually by a firewall). 

### Questions

1. What networking constructs are used to direct traffic to the right application a server?

    `ports`

2. How many of these are available on any network-enabled computer?

    `65535`

3. [Research] How many of these are considered "well-known"? (These are the "standard" numbers mentioned in the task)

    `1024`


## Task 2: Nmap switches

Read the manpages to answer the questions below

### Questions

1. What is the first switch listed in the help menu for a 'Syn Scan' (more on this later!)?

    `-sS`

2. Which switch would you use for a "UDP scan"?

    `-sU`

3. If you wanted to detect which operating system the target is running on, which switch would you use?

    `-O`

4. Nmap provides a switch to detect the version of the services running on the target. What is this switch?

    `-sV`

5. The default output provided by nmap often does not provide enough information for a pentester. How would you increase the verbosity?

    `-v`

6. Verbosity level one is good, but verbosity level two is better! How would you set the verbosity level to two? (Note: it's highly advisable to always use at least this option)

    `-vv`


We should always save the output of our scans -- this means that we only need to run the scan once (reducing network traffic and thus chance of detection), and gives us a reference to use when writing reports for clients.

7. What switch would you use to save the nmap results in three major formats?

    `-oA`

8. What switch would you use to save the nmap results in a "normal" format?

    `-oN`

9. A very useful output format: how would you save results in a "grepable" format?

    `-oG`

10. Sometimes the results we're getting just aren't enough. If we don't care about how loud we are, we can enable "aggressive" mode. This is a shorthand switch that activates service detection, operating system detection, a traceroute and common script scanning. How would you activate this setting?

    `-A`

11. Nmap offers five levels of "timing" template. These are essentially used to increase the speed your scan runs at. Be careful though: higher speeds are noisier, and can incur errors! How would you set the timing template to level 5?

    `-T5`

12. We can also choose which port(s) to scan. How would you tell nmap to only scan port 80?

    `-p 80`

13. How would you tell nmap to scan ports 1000-1500?

    `-p 1000-1500`

14. How would you tell nmap to scan all ports?

    `-p-`

15. How would you activate a script from the nmap scripting library (lots more on this later!)?

    `--script`

16. How would you activate all of the scripts in the "vuln" category?

    `--script=vuln`


## Task 3: Overview

### Notes

When port scanning with Nmap, there are 3 basic scan types. These are:

1. TCP Connect Scans (`-sT`)
2. SYN "Half-open" Scans (`-sS`)
3. UDP Scans (`-sU`)

Additionally, there are several less common port scan types, some of which we will also cover (albeit in less detail). These are:

* TCP Null Scans (`-sN`)
* TCP FIN Scans (`-sF`)
* TCP Xmas Scans (`-sX`)

Most of these (with the exception of UDP scans) are used for very similar purposes, however, the way that they work differs between each scan. This means that, whilst one of the first three scans are likely to be the go-to in most situations, it's worth bearing in mind that other scan types exist.

### Questions

No questions for this section


## Task 4: TCP Connect Scans

### Notes

TCP Connect scans (`-sT`) work by performing the three-way handshake with each target port. In other words, Nmap tries to connect to each specified TCP port, and determines whether the service is open by the response it receives.

For example, if a port if closed, RFC 9293 states that

> "... If the connection does not exist (CLOSED), then a reset is sent in response to any incoming segment except another reset. A SYN segment that does not match an existing connection is rejected by this means."

In other words, if Nmap sends a TCP request with the SYN flag set to a `closed` port, the target server will respond with a TCP packet with the *RST* (Reset) flag set. By this response, Nmap can establish that the port is closed.

If, however, the request is sent to an `open` port, the target will respond with a TCP packet with the SYN/ACK flags set. Nmap then marks this port as being open (and completes the handshake by sending back a TCP packet with ACK set).

The third possibility is when the port is hidden behind a firewall. 

* Many firewalls are configured to simply **drop** incoming packets. Nmap sends a TCP SYN request and receives nothing back. This indicates that the port is being protected by a firewalls and thus the port is considered to be `filtered`.


### Questions

1. Which RFC defines the appropriate behaviour for the TCP protocol?

    `RFC 9293`

2. If a port is closed, which flag should the server send back to indicate this?

    `RST`


## Task 5: SYN Scans

### Notes

SYN scans (`-sS`) sends back a RST TCP packet after receiving a SYN/ACK from the server (this prevents the server from repeatedly trying to make the request). The diagram below shows how SYN scans work

![Alt text](https://i.imgur.com/cPzF0kU.png)

SYN scans have a few advantages

* SYN scans are often **not logged by applications** listening on open ports, as standard practice is to log a connection once it's been fully established. This plays into the idea of SYN scans being stealthy.

* SYN scans are **faster than standard TCP scans** because they dont have to bother about completing a 3-way handshake for every open port


SYN scans also have a few disadvantages

* They require sudo permissions in order to work correctly. This is because SYN scans need the ability to create raw packets, which allows nmap to manually send only the initial SYN packet and analyze the response, without completing the full TCP handshake. 

* Unstable services are sometimes brought down by SYN scans


All in all, the pros outweigh the cons

### Questions

1. There are two other names for a SYN scan, what are they?

    `Half-Open, Stealth`

2. Can Nmap use a SYN scan without Sudo permissions (Y/N)?

    `N`


## Task 6: UDP Scans

### Notes

UDP connections are *stateless*. There in no handshake, and they instead rely on sending packets to a target port and hoping that they make it. The lack of acknowledgement makes UDP significantly more difficult (and much slower) to scan.

When a packet is sent to a UDP port, there should be no response. When this happens, nmap refers to be ports as being `open|filtered`. In other words, it suspects that the port is open, but it could be firewalled.

Very rarely, it might get a UDP response, and when this happens, the port is marked as *open*. More commonly, there is no response, in which case the request is sent a second time as a double-check. If there is still no response then the port is marked *open|filtered* and nmap moves on.

When a packet is sent to a *closed* UDP port, the target should respond with an ICMP (ping) packet containing a message that the port is unreachable. This clearly identifies closed ports, which nmap marks as such and moves on.

> Due to this difficulty in identifying whether a UDP port is actually open, UDP scans tend to be incredibly slow in comparison to the various TCP scans. For this reason it's usually good practice to run an Nmap scan with `--top-ports <number>` enabled. For example, scanning with  `nmap -sU --top-ports 20 <target>`. Will scan the top 20 most commonly used UDP ports, resulting in a much more acceptable scan time.



### Questions

1. If a UDP port doesn't respond to an Nmap scan, what will it be marked as?

    `open|filtered`

2. When a UDP port is closed, by convention the target should send back a "port unreachable" message. Which protocol would it use to do so?

    `ICMP`


## Task 7: NULL, FIN and Xmas

### Notes

NULL, FIN, and Xmas TCP port scans are used primarily as they tend to be even stealthier, relatively speaking, than a SYN "stealth" scan

* NULL scans (`-sN`) are when the TCP request is sent with no flags set at all. The target host should respond with a RST if the port is closed

![Alt text](https://i.imgur.com/ABCxAwf.png)

* FIN scans (`-sF`) work in an almost identical fashion; however, instead of sending a completely empty packet, a request is sent with the FIN flag (usually used to gracefully close an active connection). Once again, nmap expects a RST if the port is closed.

![Alt text](https://i.imgur.com/gIzKbEk.png)

* Xmas scans (`-sX`) send a malformed TCP packet and expects a RST response for closed ports. It sets the flags **(PSH, URG and FIN)**, and these flags give it the appearance of a blinking christmas tree when viewed as a packet capture in Wireshark.

![Alt text](https://i.imgur.com/gKVkGug.png)

The expected response for *open* ports with these scans are similar to that of a UDP scan. If the port is open then there is no response to the malformed packet. Unfortunately, that is also an expected behavior if the port is protected by a firewall, so NULL, FIN and Xmas scans will only ever identify ports as being *open|filtered*, *closed*, or *filtered*.

The overarching goal here is firewall evasion. Many firewalls are configured to drop incoming TCP packets to blocked ports which have the SYN flag set (thus blocking new connection initiation requests). By sending requests which do not contain the SYN flag, we effectively bypass this kind of firewall.

* Whilst this is good in theory, most modern IDS solutions are savvy to these scan types, so don't rely on them to be 100% effective when dealing with modern systems.


### Questions

1. Which of the three shown scan types uses the URG flag?

    `xmas`

2. Why are NULL, FIN and Xmas scans generally used?

    `Firewall evasion`

3. Which common OS may respond to a NULL, FIN or Xmas scan with a RST for every port?

    `Microsoft Windows`


## Task 8: ICMP Network Scanning

### Notes

We can perform a **ping sweep** in nmap if we want to see which IP addresses on a network contain active hosts and which do not. As the name suggests, nmap sends an ICMP packet to each possible IP address for the specified network. When it receives a response, it marks the IP address that responded as being alive.

* This is not always accurate, however it can provide a sort of baseline.

To perform a ping sweep, we use the `-sn` switch in conjunction with IP ranges which can be specified with either a hyphen or CIDR notation. For example, we could scan the `192.168.0.x` network using:

* `nmap -sn 192.168.0.1-254`

or

* `nmap -sn 192.168.0.0/24`

The `-sn` switch tells nmap not to scan any ports -- forcing it to rely primarily on ICMP echo packets to identify targets.

### Questions

1. How would you perform a ping sweep on the 172.16.x.x network (Netmask: 255.255.0.0) using Nmap? (CIDR notation)

    `nmap -sn 172.16.0.0/16`


## Task 9: Nmap Scripting Engine Overview

### Notes

NSE scripts are written in the *Lua* programming language, and can be used to do a variety of things: from scanning vulnerabilities to automating exploits for them.

There are many categories available. Some useful ones include

* `safe`: Won't affect the target
* `intrusive`: Not safe: likely to affect the target
* `vuln`: Scan for vulnerabilities
* `exploit`: attempt to exploit a vulnerability
* `auth`: Attempt to bypass authentication for running services
* `brute`: attempt to bruteforce credentials for running services
* `discovery`: attempt to query running services for further information about the network 


### Questions

1. What language are NSE scripts written in?

    `Lua`

2. Which category of scripts would be a very bad idea to run in a production environment?

    `intrusive`


## Task 10: Working with the NSE

### Notes

If the command `--script=safe` is run, then any applicable *safe* scripts will be run against the target

* Note: only scripts which target an active service will be activated

To run a specific script, we would use `--script=<script-name>`, e.g. `--script=http-fileupload-exploiter`

Some scripts require arguments. These can be given with the `--script-args` Nmap switch. An example of this would be with the `http-put` script (used to upload files using the PUT method). For example:

`nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'`

Note that the arguments are separated by commas, and connected to the corresponding script with periods (i.e. `<script-name>.<argument>`).

Nmap scripts come with built-in help menus, which can be accessed using `nmap --script-help <script-name>`


### Questions

1. What optional argument can the `ftp-anon.nse` script take?

    `maxlist`


## Task 11: Searching for scripts

### Notes

How do we *find* scripts in nmap? We have 2 options for this, which should ideally be used in conjunction with each other

1. The page on the [nmap website](https://nmap.org/nsedoc/) which contains a list of all official scripts.
2. Local storage on your local attacking machine. Nmap stores its scripts on Linux at `/usr/share/nmap/scripts`

We can search through installed scripts by using the `/usr/share/nmap/scripts/script.db` file. Despite the extension, this isn't actually a database so much as a formatted text file containing filenames and categories for each available script

![Alt text](https://i.imgur.com/ijAhZsy.png)

### Questions

Search for "smb" scripts in the `/usr/share/nmap/scripts/` directory using either of the demonstrated methods.

1. What is the filename of the script which determines the underlying OS of the SMB server?

    `smb-os-discovery.nse`

2. Read through this script. What does it depend on?

    `smb-brute`


## Task 12: Firewall Evasion

### Notes

There is another very common firewall configuration which is important to know how to bypass

Your typical Windows host will, with its default firewall, block all ICMP packets. This presents a problem: not only do we often use *ping* to manually establish the activity of a target, nmap does the same thing by default. This means nmap will register a host with this firewall config as dead and not bother scanning it at all.

Fortunately, nmap provides an option for this: `-Pn`, which tells nmap to not bother pinging the host before scanning it. This means that nmap will always treat the target host(s) as being alive, effectively bypassing the ICMP block.

* However, this comes at the price of potentially taking a very long time to complete the scan (if the host is really dead then nmap will still be checking and double checking every specified port)

> It's worth noting that if you're already directly on the local network, nmap can also use ARP requests to determine host activity. If a host is present and online, it will respond to the ARP request with its MAC address. Nmap then notes that host as "up" or "alive"


### Questions

1. Which simple (and frequently relied upon) protocol is often blocked, requiring the use of the `-Pn` switch?

    `ICMP`

2. [Research] Which Nmap switch allows you to append an arbitrary length of random data to the end of packets?

    `--data-length`


## Task 13: Practical

### Questions

1. Does the target ip respond to ICMP echo (ping) requests (Y/N)?

    Perform a simple `ping` command on the target IP: `ping 10.10.6.222`. We are not getting any data back, so it does not seem the target ip responds to ICMP requests

    ANS: `N`

2. Perform an Xmas scan on the first 999 ports of the target -- how many ports are shown to be open or filtered?

    Do the Xmas scan: `nmap -Pn -sX -vv -p1-999 10.10.6.222`

    ANS: `999`

3. There is a reason given for this -- what is it? *Note: The answer will be in your scan results. Think carefully about which switches to use -- and read the hint before asking for help!*

    ANS: `no response`

4. Perform a TCP SYN scan on the first 5000 ports of the target -- how many ports are shown to be open?

    ```sh
    $ sudo nmap -Pn -sS -vv -p1-5000 10.10.6.222
    ...
    Not shown: 4995 filtered tcp ports (no-response)
    PORT     STATE SERVICE       REASON
    21/tcp   open  ftp           syn-ack ttl 125
    53/tcp   open  domain        syn-ack ttl 125
    80/tcp   open  http          syn-ack ttl 125
    135/tcp  open  msrpc         syn-ack ttl 125
    3389/tcp open  ms-wbt-server syn-ack ttl 125
    ...
    ```

    ANS: `5`

5. Open Wireshark and perform a TCP Connect scan against port 80 on the target, monitoring the results. Make sure you understand what's going on. Deploy the ftp-anon script against the box. Can Nmap login successfully to the FTP server on port 21? (Y/N)

    ```sh
    $ sudo nmap -Pn -sT -vv -p21 --script=ftp-anon 10.10.6.222
    ...
    PORT   STATE SERVICE REASON
    21/tcp open  ftp     syn-ack
    | ftp-anon: Anonymous FTP login allowed (FTP code 230)
    |_Can't get directory listing: TIMEOUT
    ...
    ```

    ANS: `Y`
