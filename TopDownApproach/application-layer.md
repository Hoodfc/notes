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
