# Networking Primer - Layers 5-7

The upper layer protocols handle our applications

## HTTP

HTTP is a stateless application layer level protocol that enables the transfer of data in clear text between a client and server over TCP

* HTTP normally utilizes ports 80 or 8000 over TCP. In exceptional circumstances, it can be modified to use alternate ports, or even UDP

### (Some) HTTP Methods

Method      |   Description
----        |   ----
`HEAD`      |   **required**: is a safe method that requests a response from the server similar to a GET request except that the message body is not included. It is a great way to acquire more information about the server and its operational status
`GET`       |   **required**: Get is the most common method used. It requests information and content from the server
`POST`      |   **optional**: Post is way to submit information to a server based on the fields in the request
`PUT`       |   **optional**: PUT will create or update an object at the URI supplied, whereas POST will create child entities at the provided URI
`DELETE`    |   **optional**: Delete will remove the object at the given URI
`TRACE`     |   **optional**: Allows for remote server diagnosis. The remote server will echo the same request send in its response


## HTTPS

HTTPS is a modification of HTTP designed to utilize **Transport Layer Security** (`TLS`) or **Secure Sockets Layer** (`SSL`) for data security.

TLS can wrap regular HTTP traffic within TLS, which means that we can **encrypt our entire conversation**, not just the data sent or requested.

* "Entire conversation" means all parts of the exchange are protected: headers, cookies, URL query parameters, form submissions, response body, etc.


### TLS Handshake via HTTPS

![HTTPS wireshark capture](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/81/https.png)

1. We can see that the client establishes a session to the server using port 443 `boxed in blue`. This signals to the server that it wishes to use HTTPS as the application communication protocol

2. After session is initiated via TCP, a TLS ClientHello is sent next to begin the TLS handshake. During the TLS handshake, several parameters are agreed upon:

    * Session identifier
    * Peer x509 certificate (CA stuff: how each side proves its identity)
    * Compression algorithm to be used
    * Encryption algorithm
    * 48 byte master secret (derived from the pre-master secrets from both the client and server) shared between the client and server to validate the session

3. Once the session is established, all data and methods will be sent through the TLS connection and appear as **TLS Application Data** `as seen in the red box`

    * TLS is still using TCP as its transport protocol, so we will still see acknowledgment packets from the stream coming over port 443


## FTP

File Transfer Protocol (FTP) is an Application Layer protocol that enables quick data transfer between devices. FTP itself is known as an insecure protocol, and most users have moved to use tools suchas SFTP to transfer files through secure channels.

FTP uses ports **20** and **21** over TCP.

* Port 20 is used for data transfer
* Port 21 is used for issuing commands controlling the FTP session


FTP is cpable of running two different modes: `active` or `passive`

* `Active` (Default): the client sends a `PORT` command over the control connection (port 21) telling the server which client IP and port to use, and the server then initiates the data connection from its data port (port 20) to that client port
* `Passive`: the client would send the `PASV` command and wait for a response from the server informing the client what IP and port to utilize for the data transfer channel connection

Basically,

* `Active` = client says "here's my data port, you (server) connect to me"
* `Passive` = server says "here's my data port, you (client) connect to me"


### FTP Command & Response Examples

![FTP wireshark data](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/81/ftp-example.png)

The image above shows several examples of requests issued over the FTP **command channel** (usually port 21) in `green arrows`, and the responses sent back from the FTP server in `blue arrow`

As we can see, there are some common commands passed over port 21, which we'll go more into detail below

### (Some) FTP Commands

Command     |   Description
---         |   ---
`USER`      |   specifies the user to log in as
`PASS`      |   sends the password for the user attempting to log in
`PORT`      |   when in active mode, this will change the data port used
`PASV`      |   switches the connection to the server from active mode to passive
`LIST`      |   displays a list of the files in the current directory
`CWD`       |   will change the cwd to the one specified
`PWD`       |   prints out the current dir
`SIZE`      |   will return the size of the file specified
`RETR`      |   retrieves a file from the FTP server
`QUIT`      |   ends the session


## SMB

Server Message Block (`SMB`) is a protocol that enables sharing resources between hosts over common networking architectures. 

* SMB is a connection-oriented protocol that requires user authentication from the host to the resource to ensure the user has correct permissions to use that resource or perform actions.
* Since modern changes, SMB now supports direct TCP transport over port 445, and NetBIOS over TCP Port 139

Like any other application that uses TCP as its transport mechanism, actions such as the 3-way handshake and acknowledging received packets will still take place.


### SMB on the Wire

![SMB Wireshark capture](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/81/smb-actions.png)

Noteworthy things about the the captured network data above:

* We can see SMB performs the TCP handshake each time it establishes a session in `orange boxes`
* Looking at the source and destination ports in the `blue box`, port **445** is being used, signaling SMB traffic over TCP
* The `green boxes` tell us a bit about what is happening in the SMB communication
    - In this example, there are many failed login attempts, which is an example of something to look deeper into

## Questions

1. What is the default operational mode method used by FTP?

    `Active`

2. FTP utilizes what two ports for command and data transfer? (separate the two numbers with a space)

    `20 21`

3. Does SMB utilize TCP or UDP as its transport layer protocol?

    `TCP`

4. SMB has moved using what TCP port?

    `445`

5. Hypertext Transfer Protocol uses what well known TCP port number?

    `80`

6. What HTTP method is used to request information and content from the webserver?

    `GET`

7. What web based protocol uses TLS as a security measure?

    `HTTPS`

8. True or False: when utilizing HTTPS, all data sent across the session will appear as TLS Application data?

    `True`
