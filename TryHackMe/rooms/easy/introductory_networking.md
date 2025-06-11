# Introductory Networking

## Task 1: The OSI Model: An Overview

### Notes 

The OSI (Open Systems Interconnection) Model is a standardized model which we use to demonstrate the theory behind computer networking

OSI Model consists of 7 layers

![Alt text](https://muirlandoracle.co.uk/wp-content/uploads/2020/02/OSI-Table.png)

**Layer 7: Application**: Provides networking options to programs running on a computer. It works almost exclusively with applications, providing an interface `(standardized protocols and methods)` for them to use in order to transmit data (e.g. HTTP, SMTP, FTP, etc)

> The application layer acts as a sort of bridge between the software applications running on the device and the lower network layers that handle the actual transmission of data over the network. It abstracts the complexities of the lower layers


**Layer 6: Presentation**: `Translates` the data from L7 into a `standardized format`, as well as handling any `encryption`, `compression`, or other `transformations` to the data.

> The data that this layer receives from Layer 7 tends to be in a format that the application understands, but it's not necessarily in a standardized format that could be understood by the application layer of the *receiving* computer


**Layer 5: Session**: Looks to see if a connection can be setup with the other computer across the network. If a connection can be setup, it's the job of the session layer to maintain it and cooperate with the session layer of the remote computer in order to synchronize communications

> The session that is created is unique to the communication in question. This is what allows you to make multiple requests to different endpoints simultaneously without the data getting mixed up


**Layer 4: Transport**: Does 2 main things

1. Choose the protocol over which the data is to be transmitted. Two most common are TCP and UDP

2. With a protocol selected, the transport layer then divides the tramission up into bite-sized pieces (*segments* for TCP and *datagrams* for UDP), which makes it easier to transmit the message successfully


**Layer 3: Network**: Responsible for locating the destination of your request. For example, the Internet is a huge network; when you want to request information from a webpage, it's the network layer that takes the IP address for the page and figures out the best route to take.

> At this stage, we are working with what is referred to as *Logical addressing* (i.e. IP addresses) which are still software controlled. Logical addresses are used to provide order to networks, categorizing them and allowing us to properly sort them. Currently, the most common form of logical addressing is the IPv4 format.


**Layer 2: Data Link**: Focuses on the *physical* addressing of the transmission. It receives a packet from the network layer (that includes the IP address for the remote computer) and adds in the physical (MAC) address of the receiving endpoint.

> Inside every network enabled computer is a Network Interface Card (NIC) which comes with a unique MAC (Media Access Control) address to identify it

> When information is sent across a network, it's actually the **physical address** that is used to identify where exactly to send the information.

It's also the job of the data link layer to present the data in a format suitable for transmission as well as check the received information to make sure it hasn't been corrupted during transmission 


**Layer 1: Physical**: Hardware of the computer. This is where electrical pulses that make up the data transfer over a network are sent and received. It's the job of the physical layer to convert the binary data of the transmission into signals and transmit them across the network, as well as receiving incoming signals and converting them back into binary data


### Questions

1. Which layer would choose to send data over TCP or UDP? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `4`

2. Which layer checks received information to make sure that it hasn't been corrupted? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `2`

3. In which layer would data be formatted in preparation for transmission? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `2`

4. Which layer transmits and receives data? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `1`

5. Which layer encrypts, compresses, or otherwise transforms the initial data to give it a standardised format? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `6`

6. Which layer tracks communications between the host and receiving computers? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `5`

7. Which layer accepts communication requests from applications? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `7`

8. Which layer handles logical addressing? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `3`

9. When sending data over TCP, what would you call the "bite-sized" pieces of data? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `segments`

10. [Research] Which layer would the FTP protocol communicate with? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `7`

11. Which transport layer protocol would be best suited to transmit a live video? Answer with the number of the layer: e.g. if the answer would be "the application layer", then you would enter "7".

    `UDP`


## Task 2: Encapsulation

### Notes 

The whole process highlighted below is referred to as *encapsulation*, the process by which data can be sent from one computer to another

![Alt text](https://muirlandoracle.co.uk/wp-content/uploads/2020/02/image.jpeg)

When the message is received (in the form of bits) by the second computer, it reverses the process - starting at the physical layer and working up until it reaches the application layer, stripping off the added information as it goes. This is referred to as *de-encapsulation*.


### Questions

1. How would you refer to data at layer 2 of the encapsulation process (with the OSI model)?

    `frames`

2. How would you refer to data at layer 4 of the encapsulation process (with the OSI model), if the UDP protocol has been selected?

    `datagrams`

3. What process would a computer perform on a received message?

    `de-encapsulation`

4. Which is the only layer of the OSI model to add a *trailer* during encapsulation?

    `data link`

5. Does encapsulation provide an extra layer of security (Aye/Nay)?

    `aye`


## Task 3: TCP/IP

### Notes

TCP/IP model servces as the basis for real-world networking. It consist of four layers: Application, Transport, Internet, and Network Interface.

![Alt text](https://muirlandoracle.co.uk/wp-content/uploads/2020/02/image-4.png)

The TCP/IP and OSI model match up something like this

![Alt text](https://muirlandoracle.co.uk/wp-content/uploads/2020/02/image-3.png)

TCP/IP takes its name from the two most important of protocols used: the Transmission Control Protocol that controls the flow of data between two endpoints, and the Internet Protocol, which controls how packets are addressed and sent. There are many more protocols that make up the TCP/IP suite, but for now let's talk about TCP

Before you send any data via TCP, you must first form a stable connection between the two computers. The process of forming this connection is called the *three-way handshake*

When you attempt you make a connection, your computer first sends a special request to the server indicating that it wants to initialize a connection. This request contains something called a SYN bit, which essentially makes the first contact in starting the connection process. The server will then respond with a packet containing the SYN bit, as well as another "acknowledgement" bit called ACK. Finally, your computer will send a packet that contains the ACK bit by itself, confirming that the connection has been setup successfully.


### Questions

1. Which model was introduced first, OSI or TCP/IP?

    `TCP/IP`

2. Which layer of the TCP/IP model covers the functionality of the Transport layer of the OSI model (Full Name)?

    `Transport`

3. Which layer of the TCP/IP model covers the functionality of the Session layer of the OSI model (Full Name)?

    `Application`

4. The Network Interface layer of the TCP/IP model covers the functionality of two layers in the OSI model. These layers are Data Link, and?.. (Full Name)?

    `Physical`

5. Which layer of the TCP/IP model handles the functionality of the OSI network layer?

    `Internet`

6. What kind of protocol is TCP?

    `Connection-based`

7. What is SYN short for?

    `Synchronise`

8. What is the second step of the three way handshake?

    `SYN/ACK`

9. What is the short name for the "Acknowledgement" segment in the three-way handshake?

    `ACK`


## Task 4: Ping

### Questions

1. What command would you use to ping the bbc.co.uk website?
    
    `ping bbc.co.uk`

2. Ping muirlandoracle.co.uk. What is the IPv4 address?

    `217.160.0.152`

3. What switch lets you change the interval of sent ping requests?

    `-i`

4. What switch would allow you to restrict requests to IPv4?

    `-4`

5. What switch would give you a more verbose output?

    `-v`


## Task 5: Traceroute

### Notes

* Traceroute can be used to map the path your request takes as it heads to the target machine

* On Unix systems, traceroute operates over UDP by default. On Windows, it operates on ICMP by default.


### Questions

1. What switch would you use to specify an interface when using Traceroute?

    `-i`

2. what switch would you use if you wanted to use TCP SYN requests when tracing the route?

    `-t`

3. [Lateral Thinking] Which layer of the *TCP/IP* model will traceroute run on by default (Windows)?

    `internet`


## Task 6: WHOIS

### Notes

`Whois` essentially allows you to query who a domain name is registered to.

### Questions

1. What is the registrant postal code for facebook.com

    `94025`

2. When was the facebook.com domain first registered (Format: DD/MM/YYY)

    `29/03/1997`

3. Which city is the registrant based in for microsoft.com

    `Redmond`

4. [OSINT] What is the name of the golf course that is near the registrant address for microsoft.com?

    `Bellevue Golf Course`

5. What is the registered Tech Email for microsoft.com?

    `msnhst@microsoft.com`


## Task 7: Dig

### Notes

Before getting into the Dig command, we first need to understand how `DNS` works and what goes on under the hood

> At the most basic level, DNS allows us to ask a special server to give us the IP address of the website we are trying to access. For example, if we made a request to www.google.com, our computer would first send a request to a special DNS server (which your computer already knows how to find). The server would then go looking for the IP address for Google and send it back to us. Our computer could then send the request to the IP of the Google server.

**DNS pipeline**

1. You make request to website

2. Computer checks its local **"Hosts file"** to see if an explicit `IP -> Domain` mapping has been created

    * *This is an older system than DNS and much less commonly used in modern environments, but it still takes precedence in search order of most operating systems*

3. If no mapping has been created, the computer checks its `local DNS cache` to see if it already has an IP address stored for the website.

    * *If the IP address for the website is already stored there, great. If not, we go onto the next step of the process*

4. The computer will then send a request to the `recursive DNS server`, which is automatically known to the router on your network. This recursive DNS server maintains a cache of results for popular domains, but if the IP is still not found, we continue on

5. The recursive server then passes the request onto a `root name server`. The root name server essentially keeps track of the DNS servers in the next level down, choosing an appropriate one to redirect your request to

6. These lower level servers are called `Top-Level Domain servers`. TLD servers are split up into extensions, so one TLD server handles `.com` domains, another handles `.co.uk` domains, and so on. When a TLD server receives your request for information, it passes it down to an appropriate *Authoritative Name server*

7. `Authoritative name servers` are used to store DNS records for domains directly. When your request reaches the authoritative name server for the domain you're querying, it will send the relevant information back to the **recursive DNS server**, which then caches the IP address and returns it back to you.


**Dig command**

`dig` allows us to manually query recursive DNS servers of our choice for information about domains

`dig <domain> @<dns-server-ip>`

![Alt text](https://muirlandoracle.co.uk/wp-content/uploads/2020/03/dig-demo.png)

The `157` number in the picture above (in the Answer section) tells us the `TTL` (Time to live) of the queried DNS record in seconds. As mentioned previously, when your computer queries a domain name, it stores the results in its local cache. The TTL of the record tells your computer when to stop considering the record as being valid -- i.e. when it should request the data again, rather than relying on the cached copy.

### Questions

1. What is DNS short for?

    `Domain Name System`

2. What is the first type of DNS server your computer would query when you search for a domain

    `recursive`

3. What type of DNS server contains records specific to domain extensions (i.e .com, .co.uk, etc)? Use the long version of the name

    `Top-Level Domain`

4. Where is the very first place your computer would look to find the IP address of a domain?

    `Hosts file`

5. [Research] Google runs two public DNS servers. One of them can be queried with the IP 8.8.8.8, what is the IP address of the other one?

    `8.8.4.4`

6. If a DNS query has a TTL of 24 hours, what number would the dig query show?

    `86400`


