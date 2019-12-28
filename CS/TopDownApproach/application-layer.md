# 2.1 Principles on Network Applications

Suppose you have an idea for a new network application. Let's examine how you transform this idea into a real world application. At the core of network application development is writing programs that run on different end systems. In the Web application there are two programs: the browser and the Web server. In a P2P application the programs in the various hosts may be identical. While you must write programs that run on multiple end systems, you do not need to write software that runs on network-core devices as they only function at lower layers.

---

## 2.1.1 Network Application Architectures

A network application architecture is distinctly different from the network architecture. The latter, from the developer's perspective, is fixed; while he can design the application architecture and decide its structure over the end system. A developer will likely choose between the two predominant paradigms: the **client-server architecture** and the **peer-to-peer architecture**.

In a **_client-server architecture_** a server, which is an always-on host, serves the request of many other hosts, called clients. In this architecture, two clients never communicate with each other.
The server has a fixed IP address, so that a client can always contact it by sending a packet to that IP address. Big applications need to use a data center because a single server wouldn't be able to keep up with all the requests by clients. Google, the greatest example, has 30 to 50 data centers around the world.
In a **_P2P architecture_** there is little to no reliance on servers. The application use direct communication between pairs of **intermittently** connected hosts, called **peers**. One of its most compelling features is the **self-scalability:** each new peers generate both workload and service capability to the application. P2P application are also cost-effective, as they don't require server infrastructures ( or a very limited one). However, future P2P applications will have to face three challenges:

1. **ISP Friendly.** ISP have been designed with asymmetrical bandwidth capability ( + downstream, - upstream). P2P apps put a lot of stress on residential ISPs upstreams, so they need to be carefully designed with this in mind.
2. **Security.** Being highly distributed and open, P2P apps can be a challenge to secure.
3. **Incentives.** P2P apps need to convince users to volunteer bandwidth, storage and computations resources.

---

## 2.1.2 Processes Communicating

In the jargon of operating system, programs are actually called processes. When running in the same system, they can communicate using rules set by the O.S. But in a network different hosts can have different OSs
Two processes communicate by exchanging messages across the network. A sending process creates and sends a message; a receiving process receives and possibly responds to the message.

A network application consists of pairs of processes that sends messages to each other. Regardless of its architecture, in a give time there is always a server side and a client side. Within the Web, the browser it's always the client and the Web server always the server; but in a P2P file sharing application, the two can be more dynamic, as a peer can both download (client) and upload (server) a file.

### Sockets

A process sends and receives messages from the network through a **socket**. As an analogy, if a process is a house, the socket is its door. When a process wants to send a message to another host's process. it shoves the message out its socket, assuming the presence of a transportation-layer protocol that will deliver it to the receiving host's socket. The developer has control of everything on the application-side of the socket, but has little to none on the transportation-side. He can only:

- Choose the type of transportation-layer protocol
- Fix some parameters of the transportation packet

### Addressing Processes

As for postal mail, a process needs to know the address of the receiving host in order to send him out a message. More specifically, he needs to know **(1)** the address of the destination host and **(2)** an identifier of the receiving process within the host. IN the internet, an host is identified by an IP address, which is a unique 32-bit number. A destination post number identifies the destination process, which is necessary since an host can run multiple network apps. Popular network apps have a standard port number, such as HTTP (80) and SMTP (25).

---

## 2.1.3 Transport Services Available to Applications

Many networks, including the Internet, provide more than one transport-layer protocols. How a developer choose which one to use? By picking the one with the services that best match the app. The services a transport-layer protocol can offer are:

- **Reliable data transfer**
- **Throughput**
- **Timing**
- **Security**

### Reliable Data Transfer

Packets can get lost in a computer network (e.g. if it overflow the router's buffer). For some apps, like emails of financial apps, such loss can be devastating. Thus, it's necessary that the transport-layer protocol guarantees a reliable data transfer. Using this type of protocol, the sending process can have complete confidence that data will arrive at the receiving process. **Loss-tolerant** applications, like conversational audio/video, can accept to use a protocol without reliable data transfer.

### Throughput

The throughput is the rate at which the sending process can deliver bits to the receiving process. It can fluctuate with time, if other sessions are sharing the network. A transport-layer protocol can guarantee a minimum throughput, which appeals to **bandwidth-sensitive** applications. On the other hand, **elastic applications** use as much throughput as it is available ( the more the better).

### Timing

A transport-layer protocol can also provide timing guarantee, needed especially by interactive real-time applications, such as multiplayer games. In general, lower delay is always preferable.

### Security

A transport-layer protocol can provide security services, like for example **encrypt** all data transmitted, or **end-point authentication**.

---

## 2.1.4 Transport Services Provided by the Internet

The Internet makes two transport-layer protocols available to applications: **TCP** and **UDP.**

### TCP Services

The TCP service model include:

- **connection-oriented service:** TCP has the client and the server exchange information, during the so-called handshaking procedure, before the application-level messages begin to flow. The TCP connection is a full-duplex connection between the two processes.
- **reliable data transfer:** the communicating processes can rely on TCP to deliver all data and in proper order.
- **congestion-control mechanism:** TCP throttles a sending process when the network is congested. TCP congestion control also try to limit each TCP connection to its fair share of bandwidth.

### UDP Services

UDP is a lightweight transport-layer protocol, with minimum services. It doesn't have a handshaking, nor reliable data transfer and congestion control.

---

## 2.1.5 Application-layer protocols

An application-layer protocol defines how an application's processes, on different end systems, pass messages to each others. It defines:

- The types of messages (e.g. request messages and response messages)
- The syntax of the various message types
- The semantics of the fields
- Rules for determining when and how a process sends and responds to messages

Some application-layer protocols are specified in **RFC** and therefore available in the public domain. Two examples are the **HTTP** (RFC 2616) and the **SMTP** (RFC 5321). The first is the app-layer protocol for the Web; the second for internet email applications.

---

# 2.2 The Web and HTTP

Until the 1990s, the Internet was only used in academics fields, and basically unknown otherwise. Then, in the 90s thanks to the work of Berners-Lee, the World Wide Web came to existence, changing the whole world. Maybe its most appealing feature is that the Web is _on-demand_: the users receive what they want when they want. Also, it's easy for everyone to publish things on the Web, both practically and economically.

---

## 2.2.1 Overview of HTTP

The **HyperText Transfer Protocol** (HTTP) is the Web's application-layer protocol. It's defined in **RFC 1945** and **RFC 2616**. HTTP implemented in two programs: the client program and the server program, that run on different end systems and exchange HTTP messages, whose structure is defined by the protocol.

A **Web page** consists of objects (html files, JPEG images, video clips, etc). Most Web pages consist of a single HTML file and several referenced objects. Each **URL** has two components: the **hostname** of the server \***\*and the object's **pathname.\*\* For example

> http://www.school.edu/dep/pic.gif

Has _[www.school.edu](http://www.school.edu)_ as the hostname and _/dep/pic.gif_ for the pathname.

HTTP uses TCP as its transport protocol. The server doesn't store any information about the client. So, for example, it has no way to know if a client has already requested an object. Because of this, HTTP is said to be a **stateless protocol**.

---

## 2.2.2 Non-Persistent and Persistent Connections

Usually the client and the server communicate for an extended period of time, with requests and responses either back-to-back, periodically at fixed intervals or intermittently. When the interaction is over TCP the developer needs to choose whether each request/response pair is sent over a separate TCP connection (**non-persistent**) or if all the pairs are sent over the same TCP connection (**persistent**). HTTP uses persistent connections by default, but both clients and servers can be configured.

This are the steps to transfer a Web page from server to client over a non-persistent connection. Let's suppose the page has a base HTML file and 10 JPEG images. The URL is

> http://school.edu/dep/pic.gif

1. The HTTP client intitiates a TCP connection to the server [\*www.school.edu](http://www.school.edu)\* on port 80
2. The HTTP client sends an HTTP request message including the pathname _/dep/pic.gif_
3. The HTTP server receives the request, retrieves the object from its storage, encapsulates it in a response message and send it to the client.
4. The server tells TCP to close the connection (but TCP only terminates it when it knows that the client has received the message)
5. The client receives the message. TCP connection terminates. The client extracts the file from the response and finds reference to 10 images.
6. The first 4 steps are repeated for each of the images.

As the browser receives the Web page, it displays the page to the user. The HTTP has nothing to do with how a page is interpreted by a client, thus different browsers can have different interpretations of the same page.

Non-persistent connections put the server under a lot of stress, since brand-new connections with their TCP buffer and TCP variables allocated need to be created for each requested object.

With persistent connections, the server leaves the TCP connection open after sending a response, usually closing it after it isn't used for a certain amount of time. An entire Web page, and event multiple Web pages - if they are on the same server - can be sent over a simple TCP connection.

---

## 2.2.3 HTTP Message Format

HTTP specifications include the definition of the two types of HTTP messages: request message and response message.

### HTTP request message

> GET /dir/page.html HTTP/1.1
> Host: www.school.edu
> Connection: close
> User-Agent: Mozilla/5.0
> Accept-Language: France

The message is written in ASCII text. The first line of the message is called the **request line**, the subsequent ones are called the **header lines**. The request line has three fields:

1. the **method**, which can take on several values: GET, POST, HEAD, PUT, DELETE.
2. the **URL**
3. the **HTTP version**

The header line *Host: \*\* specifies the host on which the object resides, required for the Web proxy cache. With the *Connection: close* the browser is telling the server to use a non-persistent connection. The *User-Agent\* specifies the type of browser that is making the request. Finally, the Accept-Language indicates the user's language preference; this is one of the many content negotiation headers available in HTTP. Let's look to a more general format of the request message, which follows the example, except for the entity body. The entity body is empty with the GET method, but is used with the POST method to pass the information the user wrote into, for example, a form.

### HTTP Response Message

Let's see an example of an HTTP response message, which could be the response of the previous request message.

> HTTP/1.1 200 OK
> Connection: close
> Date: Tue, 09 Aug ...
> Server: Apache/2.2.3 (CentOs)
> Last-Modified: Tue, Aug 09 ...
> Content-Length: 6821
> Content-Type: text/html

    [data]

It has three sections: the **status line**, six **header lines** and then the **entity body**. The entity body is the meat of the message, as it contains the requested object. The status line has three fields: the **protocol version**, a **statu code** and a **status message**. In this example, the statuse line indicates the server is using HTTP/1.1 and that everything is OK (file found and sent).

The status code and the associated staus message indicate the result of the request. Here some common ones:

- **200 OK**: request succed and information is returned in the response
- **300 Moved Permanently**: requested objected has been moved. An additional **Location:** header is specified, which the client will automatically use.
- **400 Bad Request**: a generic error code
- **404 Not Found**: the requested object does not exist
- **505 HTTP Version Not Supported**: the requested HTTP version is not supported by the server

---

## 2.2.4 User-Server Interaction: Cookies

An HTTP server is **stateless**. This vastly simplifies the server design and permitted engineers to develop big performance Web servers. However, web sites often wants to identify users, to serve content as a function of user's identity. HTTP uses **cookies** to allow sites to keep track of users. Cookie technology has 4 components

- a cookie header line in the **response** message
- a cookie header line in the **request** message
- a cookie file kept on the user's end system and managed by the user's browser
- a back-end database at the Web site

Suppose Susan, who always accesses the Web using Mozilla Firefox contacts _amazon.com_ for the first time. When the request comes into the Amazon's Web server, the server creates an identification number and an entry in its back-end database with that id. In the response message, it then attach:

> Set-cookie: 1678 _(example of a possible id number)_

When Susan receives the message, her browser sees the header line and appens to the special cookie file the hostname of the server and the id number. Every time she accesses the Amazon server, the browser will automatically include in the request message:

> Cookie: 1678

In this way, Amazon can offer users some services based on the user's sessions,like a shopping cart. Although cookies simplify and enhance the internet experience, they are quite controversial as they can be considered an invasion of privacy.

## 2.2.5 Web Caching

A Web Cache - also called a proxy server - is a network entity that satisfy requests on behalf of a Web server. The cache has its own disk storage where it keeps copies of recently requested objects. An user's browser can first direct his requestes to the Web cache. As an example, suppose the request for

> http://school.edu/picture.gif

1. The browser establishes a TCP connection to the Web cache and sends the request
2. The cach checks to see it it as a copy of the object. If it does, it returns the object in an HTTP response
3. If it doesn't, the Web cache opens a TCP connection with the origin server and sends the former request.
4. When the cache receives the object, it stores a copy in its storage and sends a copy to the browser

Note that a cache is both a server and a client. Web caching has seen a development in the Internet for two reasons:

1. It reduces the response time for a client request
2. It reduces traffic on an institution's access link to the internet, allowing the institution to not upgrade the bandwith as quickly as it should

Through the use of **Content Distribution Networks** (CDNs), Web caches are increasingly playing an important role. CDNs installed many distributed caches through the internet, thus localizing the traffic.

---

## 2.2.6 The Condition GET

Although caching can reduce user-perceived response time, in introduces a new problem: the copy of an object stored in the cache may be stale; that is, the object may have been modified in the Web server. Conditional GET is the HTTP machanism that allows the cache to verify that his objects are up to date. A request is a conditional GET if:

- the request message uses the GET method
- the request message includes the GET an **If-Modified-Since** header line

If the object has not been modified, the Web server sends a response with `304 Not Modified` as its status code and message, and an empty entity body.

---

# 2.3 File Transfer: FTP

In a typical FTP server, the user is sitting in front of one host (the local one) and wants to transfer file from or to a remote host. He has to provide authorization information (his identity and password), and then he can transfer files. HTTP and FTP are both files transfer protocols and have many things in common: for example, they both run on top of TCP.
The most striking difference is that FTP uses two parallel TCP connection to transfer a file: a **control connection** and a **data connection**. THe control connection is used for sending control information such as user identification, password, commands to change remote directory, and commands to "put" or "get" files. The data connection is used to actually send a file.
Because of this mechanism, FTP is said to send its control information **out-of-band**. HTTP sends it **in-band**. The FTP default port number is \*_port 21_. For every requested file, FTP open a new data connection, while the control connection stays open throughout the user session.
FTP needs to mantain state about the user, as the server needs to associate the control connection with the user; this reduce the number of possible connections that an FTP server can mantain.

---

# 2.4 Electronic Mail in the Internet

Electronic mail has been around since the beginning of the Internet, being its most popular application in its infancy, and still being one of the most utilized applications.
As with postal mail, e-mail is an asynchrnous communication medium - people send and read messages when its convenient for them, without having to coordinate with the other persone (like with telephonic communication).
The Internet mail system has three major components:

- **user agents**
- **mail server**
- **Simple Mail Transfer Protocol (SMTP)**

Let's examine there components in the context of Alice sending an e-mail message to Bob. User Agents allow user to read, reply to, forwards and compose messages (e.g. Outlook). When Alice finished composing her message, her User Agent sends the message to her mail server, where the message is placed in the server's outgoing messages queue. When Bob wants to read the message , his user agent retrieves it from his mailbox in his server. The mail server has to authenticate Bob (with username and password). If Alices's server cannot deliver mail to Bob's server, it holds the message in a message queue and attemps to transfer the message again later (every 30 minutes or so). If there is no success after several day, the server notifies the sender and removes the message from the queue.
SMTP, the principal application-layer protocol for e-mail, has two side (client and server). Bothe client and server side of SMTP run on every mail server: when a mail server sends a mail it acts as a client; when it receives mail from other mail servers, it acts as a server.

---

## 2.4.1 SMTP

STMP, defined in **RFC 5321**, is the heart of Internet e-mail. It's much older than HTTP (RFC is from 1988, but it was around long before). Although SMTP has a lot of good qualities, it is a legacy technology with certain archaic characteristics. For example, it restricts the body of all messages to simple 7-bit ASCII, which is a bit of a pain as it require binary data to be encoded in ASCII by the sender, and decoded back by the receiver. It is important to note that SMTP doesn't use an intermediate mail server, if the sender's server is in Hong Kong and the receiver's server is in Italy, the TCP connection is a direct one. In particular, if the receiver's server is down, the message remains in the sender's mail server for new attempts and is not stored elsewhere.
For sending an e-mail, first the client SMTP has TCP establish a connection to port 25 at the receriver's mail server; and it retries later if the receiver's server is down. Once the connection is established, the client indicates the email address of the sender and the of the recepient. After this, the client sends the message. The clients repeats this process is it has other message to send to the server, otherwise it tells TCP to clone the connection.

---

## 2.4.2 Comparison with HTTP

Both protocols are used to transfer files from one host to another, they bothe use TCP with persistent connecton. There are important differences:

- HTTP is mainly a **pull protocol** (users use HTTP to pull info from the server), while SMTP is a **push protocol** (the sending server pushes the file into the receiving server).
- As stated before, SMTP needs each message to be in 7-bit ASCII; HTTP does not impose this restriction.
- HTTP encapsulates each object of a Web page on a HTTP response while SMTP places all objects in a simple message.

---

## 2.4.3 Mail Message Format

When Alice writes an e-mail to Bob, whe may include several peripheal headers (`FROM:`, `TO:`, `SUBJECT:`, etc). There headers are defined in **RFC 5322**, and are difference from the SMTP control commands (seen in 2.4.1).

---

## 2.4.4 Mail Access Protocol

Once SMTP delivers the message from Alices's mail server to Bob's mail server, the message is stored in Bob' mailbox. Until now we assumed Bob reads the message by loggin onto the server: this may have been true until the ealy 1990s, but nowadays users read mails from a client that runs on the user's end system.
It would be natural to think of placingalso the server on the user's end system, but this would require the system to be always on and connected to the Internet. Typically, a user runs a User Agent on hi PC and accesses its mailbox stored on an always-on machine. Even the sender doesn't typically interact directly with the server, but with its user agent that sends the message to the server, so that it can try to send the message several times if the recepient's server is not available. But how does Bob's user agent obtain the messages stored in another host? We've seen that SMTP is a push protocol, but this is a pull procedure. Special **mail access protocols** were introduced, such as **Post Office Procol - Version 3** (**POP3**), **Internet Mail Access Protocol** (**IMAP**) and **HTTP**.

---

### POP3

POP3, defined in RFC 1939, is a simple mail access protocol, with limited functionality. It begins when the user agent (U.A.) opens a TCP connection to the mail server at port 110. When the connection is established, it progresses through three phases:

1. **Authorization**: The U.A. sends a username and a password (in the clear) to authenticate the user. It has two principal commands:
   `USER <username>`
   `PASS <password>`
2. **Transaction**: a U.A. can be configured to _download and delete_ or to _download and keep_. The sequence of commands depends on this choiche. In the download-and-delete the U.A. cill issue the `list`, `retr` and `dele` commands ( + `quit` to close the connection). A problem with this mode raises ehn the user wants to read his mail from several clients (work, home, mobile), as the messages will be partitioned through the devices. In the download-and-keep mode, POP3 leaves the messages on the mail server.
3. **Update**: the U.A. can mark messages for delition, remove delition marks, and obtain mail statistics

Server replies with `-OK` and `-ERR`.

---

### IMAP

With POP3, once Bob downloaded his messages, he can create mail folders and organize his messages, but this is again a problem with multiple devices as he should do the same for every end system. A folder hierarchy on a remote server would definetely be better.
IMAP, defined in **RFC 3501** was invented. An IMAP server, associate each message with a folder (by default in the INBOX folder). The user can then move the message to another folder, read it, delete it, etc. Unlike POP3, IMAP has to maintain user state information - e.g. the name of user-created folders.

---

### Web-Based E-Mail

Firstly introduced by Hotmail in the mid 1980s, it is now provided by several others like Google and Yahoo!. The U.A. is the browser and user communicates with the mail server via HTTP.

---

# 2.5 DNS - The Internet's Directory Service

Just as humans can be identified in many ways (birth certificate name, social security number, driver's license number, etc) so too can Internet hosts:

- **by hostname**: hostnames (e.g. cnn.com) are mnemonic and appreciated by humans
- **by IP address**: an IP address consists of four bytes and has a rigid hierachical structure, because as we scan the address from left to right, we obtain more and more specific information about where the host is located

---

## 2.5.1 Services Provided by DNS

IN order to reconcile human's and machine's preferences, we need a directory service that translate hostnames to IP addresses. This is the main task of the Internet's **Domain Name System** (**DNS**). The DNS is :

- a distributed database implemented in DNS servers
- an application-layer protocol that allows host to query the database

When a browser requests the URL `www.school.edu/index.html`, the user's host must obtain the IP address.

1. The user machine runs the client side of DNS
2. The browser extracts the hostname, `www.school.edu`, and passes it to the client side of DNS
3. The DNS client sends a query with the hostname to the DNS server
4. The DNS client receives the reply with the IP Address
5. The browser can start a TCP connection using the IP Address

DNS adds an additional delay to the applications that use it. DNS prodives other important services:

- **host aliasing**: a host with a complicated **canonical hostname** can have one or more host aliases
- **mail server aliasing**: e-mail addresses need to be mnemonical. DNS can be invoked by a mail application to obtain the canonical hostname for a supplied alias hostname. The MX record permits a company mail server and Web server to have identical (aliased) hostnames
- **load distribution**: big and busy sites are replicated to multiple server: each running on different systems with different IP addresses. A set of addresses is thus associated, contained in the DNS database. During a request, DNS responds with the entire set but rotates the orider (client will typically request the first).

DNS is specifies in **RFC 1034** and **RFC 1035**, and updated in several other as it's a very complicated system.

---

## 2.5.2 Overview of How DNS Works

Suppose that some application needs to translate a hostname to an IP address. The application will invoke the DNS client, which will then take over and send a query message into the network. DNS messages are sent within **UDP datagrams** on **port 53**. After a delay, client DNS receives a message with the desired mapping, that is passed to the application. While to the application DNS is a simple black box that translate hostanmes, its implementation is very complex.

A simple design for DNS would have one DNS server that contains all the mappings. The problems are:

- **a single point of failure**: if the DNS server crashes, so does the entire Internet.
- **traffic volumes**: a single DNS server would have to handle all of the DNS queries ( from hundred of million of hosts)
- **maintenance**: the server would have to keep record fo all the Internet hosts, updating them to account for every new host

---

### A Distributed, Hierarchical Database

In order to deal with the issue of scale, DNS uses a large number of server, organized in a hierachical fashion and distributed around the world. There are three classes of DNS server:

- **root servers**: in the Internet there are 13 root server, mostly located in N.A. For security and reliability purposes, each server really is a network of servers.
- **top-level domain** (**TLD**) **server**: responsible for TLD such as com, org, net, edu and all of the country TLD like fr, it, uk, jp.
- _authorative DNS servers_: every organization with publicy accessible hosts must proved DNS records that map those hosts to IP addresses (e.g. universities usually maintain an authoritative server).
  There is another important type: the **local DNS server**, even though it does not belong to the hierarchy. Each ISP - such as an university or a residential ISP - has a local DNS server. Whean a host connects to an ISP, the ISP provides the host with the IP address of its local DNS server(s).
  Let's take a look at a simple example:
- `cis.poly.edu` wants the IP address of `gaia.cs.umass.edu`. Suppose that their respectives local DNS server are `dns.poly.edu` and `dns.umass.edu`.
- the host `cis.poly.edu` first sends a request to its local DNS server, `dns.poly.edu`, with the desired hostname.
- the local DNS server forwards the query to a root server
- the root server takes not of the `edu` suffix and returns to the local server a list of IP addresses for TLF servers responsbile for `edu`.
- the local server resends the query to one of these TLD
- TLD server takes note of the `umass.edu` suffix and responds with the IP address of the authorative server for the University of Massachussetts, namely `dns.umass.edu`
- the local server sends the query to to `dns.umass.edu`, which responds with the IP address of `gaia.cs.umass.edu`

Note that to obtain one IP address four pairs of query/reply were needed. DNS cachin will reduce this traffic.

---

### DNS Caching

DNS extensively exploits DNS caching to improve the delay perfomance and to reduce the number of DNS messages. When a DNS server receives a DNS reply, it caches the mapping in its local memory. If a hostname/IP address is cached and another query arrives for the same hostname, the DNS server can immediately provide the IP address, without relying on the DNS hierarchy. DNS servers dischard information after a period of time (usually 2 days) beacuse mappings aren't permaments.

---

## 2.5.3 DNS Records and Messages

The DNS servers store **resource records** (**RR**s). A resource record is a four tuple that contains the following fields:

> (Name, Value, Type, TTL)
> **TT:** (time-to-live) determines when a resource should be removed from cache. The meanings of **Name** and **Type** depend on Type:

- **Type = A** : Name is a hostname and Value is the IP Address for the hostname
- **Type = NS**: Name is a domain and Value is the hostname of the authorative DNS that know how to obtain the IP Address of that domain
- **Type = CNAME**: Value is the canonical hostname for the alias hostname Name
- **Type = MX**: Value is the canonical name of a mail server with an alias hostname Name

If A DNS server is authorative for a particural hostname it will contain a Type A record for the hostname. If a server is not authorative for a hostname, it will contain a Type NS recordo for the domain that include the domain, and a Type A record that provide an IP Address of the server in the Value field of the NS record. Suppose a DNS is not authorative server for `gaia.cs.umass.edu`. This server will contain a record like `(umass.edu, dns.umass.edu, NS)` and also a `(dns.umass.edu, 128.119.40.111, A)`.

---

### DNS Messages

There are two kinds of DNS Messages: query and reply messages. The two have the same format:

- The first 12 bytes is the header section, which has many fields
  - 16-bit number that identifies the query, allowing the client to match requests and replies
  - 1-bit query (0) or reply (1) flag
  - 1-bit authorative flag
  - 1-bit recursion-desired flag (set by the client)
  - 1-bit recursion available (set by the server)

The are also four number-of fields, indicating the number of occurencies of the four types of data section that follow the header

- The question section contains information about the query:
  1. a name field contains the name that is being queried
  2. a type field indicates the type of record being asked
- The answer section contains the resource records for the name queried
- The authority section contains records of other authorative servers
- The additional section contain other helpful records

---
