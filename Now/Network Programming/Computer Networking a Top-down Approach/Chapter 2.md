# Application Layer



## Principles of Network Applications

When developing your new applications, you need to write software that will run on multiple end systems for communication with each other. Don't need and Don't allowed to write software that runs on network-core devices, such as routers or link-layer switches.

### Network Application Architectures

From application developer's perspective, the network architecture is fixed and provides a specific set of services to applications. **Application architecture** is designed by the application developer and dictates how the application is structured over the various end systems.

- **Client-Server architecture**
  - An always-on host called *server*.
    - Uses a data center for large companies.
  - Services requests from many other hosts called *clients*.
- **Peer-to-Peer architecture**
  - Minimal reliance on dedicated servers in data centers.
  - Direct communication between pairs of intermittently connected hosts, called peers.
  - **Self-scalability**, each peer generates workload as well as adds service capacity.

### Processes Communicating

This book focus on how processes running on different hosts communicate. 

Processes on two different end systems communicate with each other by exchanging messages across the computer network. 

- A sending process creates and sends messages into the network.
- A receiving process receives these messages and possibly responds by sending messages back.

#### Client and Server Processes

Usually label one of the two processes as the **client** and **server**.

- In the context of a communication session between a pair of processes, the process that initiates the communication is labeled as the client. The process that waits to be contacted to begin the session is labeled as server.

#### The Interface Between the Process and the Computer Network

A process sends messages into, and receives messages from, the network through a software interface called a **socket**.

- A socket is the interface between the application layer and the transport layer within a host
- Also referred to as the Application Programming Interface (API) between the application and network.
- The only control that the application developer has on the transport-layer side is
  1. The choice of transport protocol
  2. Ability to fix a few transport-layer parameters such as maximum buffer and maximum segment sizes

#### Addressing Processes

To identify the receiving process, two pieces of information needs to be specified

1. **IP address**,  address of the host
2. **Port number**, identifier that specifies the receiving process in the destination host

### Transport Services Available to Applications

Choosing transport-layer protocols depends on its merits and demerits.

- Reliable Data Transfer
  - Guaranteed data delivery service.
- Throughput
  - Guaranteed throughput at some specified rate.
- Timing
  - Guaranteed transfer time.
- Security
  - Encrypt and decrypt.

### Transport Services Provided by the Internet

TCP vs UDP

- TCP Services
  - Connection-oriented service
    - Handshaking procedure allowing server and clients to prepare for packets.
    - A TCP connection, after the handshaking procedure, is a full-duplex connection in that the two processes can send messages to each other over the connection at the same time.
  - Reliable data transfer service.
    - Deliver all data sent without error and in the proper order.
  - A congestion-control system included to ameliorate the entire system.
- UDP Services
  - Lightweight transport protocol providing minimal services.
  - No connection service, No reliable data transfer service, No congestion-control system.



Timing and Throughput guarantees are missing. But still fine with clever design.

### Application-Layer Protocols

Application-layer protocol defines how an application's processes running on different end systems, pass messages to each other.

- The types of messages exchanged.
- The syntax of the various message types.
- The semantics of the fields.
- Rules for determining when and how a process sends messages and responds to messages.

## The Web and HTTP

### Overview of HTTP

HTTP, HyperText Transfer Protocol, the Web's application-layer protocol.

- Defined in RFC 1945 and RFC 2616.
- Implemented in a client program and a server program. They communicate with each other by exchanging HTTP messages.

HTTP uses TCP as its underlying transport protocol. HTTP is said to be a stateless protocol because it will not remember the state of client.

### Non-Persistent and Persistent Connections

Should each request / response pair be sent over separate TCP connections or the same TCP connection.

- Non-persistent connection.
- Persistent connection.

HTTP specifications defines only the communication protocol between the client HTTP program and the server HTTP program.

#### HTTP with Non-Persistent Connections

1. HTTP client process initiates a TCP connection to the server.
2. HTTP client sends an HTTP request message to the server via its socket.
3. HTTP server process receives the request message via its socket, retrieves the object from its storage, encapsulates the object in an HTTP response message, and sends the response message to the client via its socket.
4. HTTP server process tells TCP to close the TCP connection.
5. HTTP client receives the response message. The TCP connection terminates.

Non-persistent connections, where each TCP connections is closed after the server sends the object.

**Round-trip time (RTT)** is the time it takes for a small packet to travel from client to server and then back to the client.

HTTP request/response use "Three way handshake", used roughly two RTT plus the transmission time at the server of the HTML file.

#### HTTP with Persistent Connections

Shortcomings of Non-persistent connections:

- A brand new connection must be established and maintained for each requested object. Heavy burden on the Web server.
- Delay of two RTTs for each connection.

HTTP/2 allows multiple requests and replies to be interleaved in the same connection, and a mechanism for prioritizing HTTP message requests and replies with this connection.

### HTTP Message Format

#### HTTP Request Message

- **request line**
  - Method field, `GET, POST, HEAD, PUT, DELETE`
  - URL field
  - HTTP version field
- **header lines**
  - Server can behave differently according to header lines.

#### HTTP Response Message

- **Status line**
- **Header lines**
- **Entity body**

### User-Server Interction: Cookies

HTTP server is stateless. To identify clients, Cookies were introduced to allow sites to keep track of users.

Cookie technology has four components

- A cookie header line in the HTTP response message.
- A cookie header line in the HTTP request message.
- A cookie file kept on the user's end system and managed by the user's browser.
- A back-end database at the Web site.

### Web Caching

A Web cache is a network entity that satisfies HTTP requests on the behalf of an origin Web server.

Suppose  a browser is requesting the object `http://www.someschool.edu/campus.gif` 

1. The browser establishes a TCP connection to the Web cache and sends an HTTP request for the object to the Web cache.
2. The Web cache checks to see if it has a copy of the object stored locally. If it does, the Web cache returns the object within an HTTP response message to the client browser.
3. If the Web cache does not have the object, the Web cache opens a TCP connection to the origin server, that is, to `www.someschool.edu`. The Web cache then sends an HTTP request for the object into the cache-to-server TCP connection. After receiving this request, the origin server sends the object within an HTTP response to the Web cache.
4. When the Web cache receives the object, it stores a copy in its local storage and sends a copy, within an HTTP response message, to the client browser.

Typically a Web cache is purchased and installed by an ISP.

#### The Conditional GET

When using web caching, the copy of an object residing in the cache may be stale.

```HTTP
GET /fruit/kiwi.gif HTTP/1.1
Host: www.exotiquecuisine.com
If-modified-since: Wed, 9 Sep 2015 09:23:24
```



## Electronic Mail in the Internet

Three major components: **User Agents**, **Mail Servers** and **Simple Mail Transfer Protocol**.

### SMTP

Suppose Alice wants to send Bob a simple ASCII message.

1. Alice invokes her user agent for e-mail, provides Bob's email address, composes a message, and instructs the user agent to send the message.
2. Alice's user agent sends the message to her mail server, where it is placed in a message queue.
3. The client side of SMTP, running on Alice's mail server, sees the message in the message queue. It opens a TCP connection to an SMTP server, running on Bob's mail server.
4. After initial SMTP handshaking, the SMTP client sends Alice's message into the TCP connection.
5. At Bob's mail server, the server side of SMTP receives the message. Bob's mail server then places the message in Bob's mailbox
6. Bob invokes his user agent to read the message at his convenience.

### Comparison with HTTP

1. SMTP is a push protocol, HTTP mainly is a pull protocol.
2. SMTP requires each message including the body of each messae to be in 7-bit ASCII format. HTTP does not impose such restriction.
3. HTTP encapsulates each object in its own HTTP response message. SMTP places all of the message's objects into one message.

### Mail Message Formats

```html
From : addr@server.com
To : addrto@server2.com
Subject : test
```

### Mail Access Protocols

Special mail access protocol that transfers mail from the recipient's mail server to the recipient's PC.

- POP3
  Simple, functionality is limited.
  - Open a TCP connection to mail server on port 110
  - authorization, username and passwd.
  - transaction, retrieves messages, mark messages for deletion, remove deletion marks and obtain mail statistics.
  - update, server delete messages indicated in transaction phase.
- IMAP
  More features than POP3, but also more complex.
- HTTP
  Web-Based email. User communicates with its remote mailbox via HTTP protocol.

## DNS - The Internet Directory Service

Identify a host

- hostname, `www.facebook.com`
  Little information about the location within the Internet of host.
- IP addresses, `121.7.106.83`

The DNS protocol runs over UDP and uses port 53.

### Services Provided by DNS

People prefer hostname, routers prefer IP address. DNS(Domain name system) is used as a translator.

- A distributed database implemented in a hierarchy of DNS servers
- An application-layer protocol that allows hosts to query the distributed database.

In order to be able to send an HTTP request message to the webserver, the user's host must first obtain the IP address of it.

1. The same user machine runs the client side of the DNS application.
2. The browser extracts the hostname from URL and passes the hostname to the client side of the DNS application.
3. The DNS client sends a query containing the hostname to a DNS server
4. The DNS client eventually receives a reply, which includes the IP address for the hostname
5. Once the browser receives the IP address from DNS, it can initiate a TCP connection to the HTTP server process located at port 80 at that IP address.

DNS also provides additional important services:

- Host aliasing, different names for same server.
- Mail server aliasing, same as above.
- Load distribution, different IP addresses for same hostname.

### Overview of How DNS Works

Focus on Hostname-to-IP address translation service.

A simple centralized design, one DNS server handle all requests.

- Unacceptable demerits of Centralized design
  - A single point of failure
  - Traffic volume
  - Distant centralized database
  - Maintenance

#### A Distributed, Hierarchical Database

DNS uses a large number of servers, organized in a hierarchical fashion and distributed around the world.

Visiting `www.amazon.com`

- Contact one of root DNS server, return IP address for TLD servers for top-level domain `com`.
- Contact TLD servers, return the IP address of an authoritative server for `amazon.com`.
- Contact authoritative servers for `amazon.com`, return the IP address of `www.amazon.com`.

Three classes of DNS servers, all belong to the DNS server hierarchy.

- Root DNS servers.
- Top-level Domain DNS servers.
- Authoritative DNS servers.

Local DNS server

- Does not belong to DNS server hierarchy.
- Each ISP has a local DNS server. When a host connect to an ISP, the ISP provides the host with the IP address of one or more of its local DNS servers.
- Local DNS server works as a relay between host and DNS server hierarchy.



In reality, TLD server does not necessarily know the authoritative DNS server for the hostname. TLD server may only know an intermediate DNS server, which in turn knows the DNS server for hostname.

#### DNS Caching

DNS extensively exploits NDS caching in order to improve the delay performance and to reduce the number of DNS messages ricocheting around the internet.

- When a DNS server receives a DNS reply, it can cache the mapping in its local memory.
- DNS servers discard cached information after a period of time.
- In fact, because of caching, root servers are bypassed for all but a very small fraction of DNS queries.

### DNS Records and Messages

The DNS server that together implement the DNS distributed database store **resource records**.

A resource record is a four-tuple that contains the following fields:

`(Name, Value, Type, TTL)`

- `Type = A`, `Name` is a hostname, `Value` is the IP address of the hostname.
- `Type = NS`, `Name` is a domain, `Value` is the hostname of the authoritative server.
- `Type = CNAME`, `Value` is canonical hostname for the alias hostname `Name`.
- `Type = MX`, `Value` is canonical name of a mail server that has an alias hostname `Name`.

If a server contain a `Type NS` Resource Record, it should also contain a `Type A` resource record for IP address of the hostname stored in the `Type NS` Resource Record.

#### DNS Messages

There are only two kinds of DNS messages, query and reply. They share the same format.

- 12 bytes *Header Section*
  - **Identification**, 16-bit number
  - **Flag**, 
    - 1-bit flag to classify query or reply.
    - 1-bit flag authoritative flag, set in a reply message when a DNS server is an authoritative server.
    - 1-bit recursion-desired flag, set when a client desires that the DNS server perform recursion when it doesn't have the record.
    - 1-bit recursion-available flag, set in reply if DNS server supports recursion.
  - **Four number-of fields**, the number of elements in the following four data sections.
- *Question Section*, information about the query that is being made.
  - A name field that contains the name that is being queried.
  - A type field that indicates the type of question being asked about the name.
- *Answer Section*, resource records for the name that was originally queried. It may composed of multiple Resource Records.
- *Authority section*, records of other authoritative servers.
- *Additional Section*,  other helpful records.

#### Inserting Records into the DNS Database

Register a new domain name

- Register the domain name with some registrar.
- Provide the registrar with names and IP addresses of primary and secondary authoritative DNS servers. The registrar will make sure a Type NS and a Type A record are entered into the TLD servers.
- Also a Type A resource record for Web server and Type MX resource record for mail server. are entered into authoritative DNS server.



## Peer-to-Peer File Distribution

A server distribute a large file to lots of clients.

#### Scalability of P2P Architectures

Distribution time for client-server architecture.
$$
D_{cs} = \max \{\frac{NF}{u_s}, \frac{F}{d_{min}}\}
$$
Distribution time for P2P architecture
$$
D_{p2p} = \max\{\frac{F}{u_s},\frac{F}{d_{min}},\frac{NF}{u_s + \sum_{i} u_i}\}
$$

#### BitTorrent

All peers participating in the distribution of a particular file is called a *torrent*.

Each torrent has an infrastructure node called a *tracker*. a peer joins the torrent by registers itself with the tracker and periodically informs the tracker that it is still in the torrent. The tracker keeps track of the peers that are participating in the torrent.

Then a peer request a list of peers and periodically update their chunk information for downloading.  **Rarest first** downloading scheme ensures number of copies of each chunks in the torrent is equalized.

When choosing which request she responds to, BitTorrent uses a clever trading algorithm. She gives priority to the neighbors that are currently supplying her data at the highest rate.

## Socket Programming: Creating Network Applications













