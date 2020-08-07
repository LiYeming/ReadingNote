# Transport Layer

## Introduction and Transport-Layer Services

A transport-layer protocol provides for **logical communication** between application processes running on different hosts. **Logical communication** means from application's perspective, it is as if the hosts running the processes were directly connected.

Transport-layer protocols are implemented in the end systems but not in network routers.

- On the sending side, the transport layer converts the application-layer messages it receives from a sending application process into transport layer **segments**.
  - Breaking the application messages into smaller chunks and adding a transport-layer header to each chunk to create the transport-layer segment.
  - Pass the segment to the network  layer at the sending end system.
- On the receiving side, the network layer extracts the transport-layer segment from the datagram and passes the segment up to the transport layer. The transport layer process the received segment, making the data in the segment available to the receiving application.

### Relationship between Transport and Network Layers

- Transport layer protocol provides logical communication between processes, running on different hosts.
- Network layer protocol provides logical communication between hosts.

The services that a transport protocol can provide are often constrained by the service model of the underlying network-layer protocol. But certain services can be offered by a transport protocol even when the underlying network protocol doesn't offer the corresponding service.

### Overview of the Transport Layer in the Internet

The Internet's network-layer protocol is IP, for Internet Protocol, which provides logical communication between hosts.

IP service model is a **best-effort delivery service**. makes its "best effort", but *no  guarantees*.

- Not guarantee the integrity of the data in the segments.
- Not guarantee the orderly delivery of segments.
- Not guarantee segment delivery.

IP is said to be an **unreliable service**.



The most fundamental responsibility of UDP and TCP is to extend IP's delivery service between two end systems to a delivery service between two processes running on the end systems.

Extending host-to-host delivery to process-to-process delivery is called transport-layer multiplexing and demultiplexing.

- UDP provides process-to-process data delivery and error checking, nothing more.
- TCP offer several additional services to applications.
  - Reliable data transfer, correctly and in order.
  - Congestion control.

## Multiplexing and Demultiplexing

A multiplexing/demultiplexing service is needed for all computer networks.

How does transport layer receives data from the network layer below and direct the received data to one of processes?

 **Demultiplexing**, The job of delivering the data in a transport-layer to the correct socket is called

**Multiplexing**, Gathering data chunks at the source host from different sockets, encapsulating each data chunk with header information to create segments, and passing the segments to the network layer.



Port number is a 16-bit number, ranging from 0 to 65535.  Port numbers ranging from 0 to 1023 are called well-known port numbers and are restricted.

Each socket in the host could be assigned a port number, and when a segment arrives at the host, the transport layer examines the destination prot number in the segment and directs the segment to the corresponding socket.

#### Connectionless Multiplexing and Demultiplexing

`clientSocket = socket(AF_INET, SOCK_DGRAM)`

When a UDP socket is created in this manner, the transport layer automatically assigns a port number to the socket. 

`clientSocket.bind(('', 19157))`

explicitly associate a specific port number to the socket.



## Connectionless Transport:UDP

UDP adds almost nothing to IP, aside from the multiplexing / demultiplexing function and some error checking.

DNS is an example of an application-layer protocol that uses UDP.

Why UDP is preferrable under certain circumstances?

- *Finer application-level control over what data is sent, and when.*
  - Bypass congestion control of TCP.
- *No connection establishment.*
  - No delay to establish a connection, three handshake.
  - QUIC protocol used in Google's Chrome browser uses UDP as its underlying transport protocol and implements reliability in an application-layer on top of UDP.
- *No connection state.*
  - State information is needed for TCP to maintain reliable data transfer.
    - Send and receive buffer.
    - Congestion control parameter.
    - Sequence and acknowledgement number parameters.
  - A server can support more active clients when application runs on UDP rather than TCP.
- *Small packet header overhead.*
  - TCP has 20 bytes, UDP has 8 bytes.

Controversial: bypassing congestion control may crowding out TCP sessions.

It is possible for an application to have reliable data trasnfer when using UDP.

- Can be done if reliability is built into the application itself.

### UDP Segment Structure

![image-20200807085925805](/home/yemingli/.config/Typora/typora-user-images/image-20200807085925805.png)

UDP header has four fields, each consists of two bytes.

- Source port and Destination port
  - allow the destination host to pass the application data to the correct process running on the destination end system.
- Length field
  - specifies the number of bytes in the UDP segment.
- Checksum field
  - used by the receiving host to check whether errors have been introduced into the segment.

### UDP Checksum

UDP checksum provides for error detection.

- Whether bits within the UDP segment have been altered as it moved from source to destination.
- Calculating Checksum, 
  - Partition data into 16 bit numbers 
  - Add 16 bits numbers, wrapping around overflow
  - Take 1s complement.

UDP provides a checksum because there is no guarantee 

- Error may happen link-by-link
  - No guarantee that all the links between source and destination provide error checking.
- Error may happen in-memory
  - No guarantee that router's memory is working correctly.
- End-to-end principle
  - certain functionality must be implemented on an end-to-end basis.

UDP provides error checking, not recovery. discard or warning.

## Principles of Reliable Data Transfer

A generic question of central importance of networking.



It is the responsibility of a reliable data transfer protocol to implement this service abstraction.

- However, the layer below the reliable data transfer protocol may be unreliable. (TCP on IP)
- What protocol mechanism are needed when the underlying channel can corrupt bits or lose entire packets?

![image-20200807094418474](/home/yemingli/.config/Typora/typora-user-images/image-20200807094418474.png)

### Building a Reliable Data Transfer Protocol



#### Reliable Data Transfer over a Perfectly Reliable Channel: rdt1.0

![image-20200807094941859](/home/yemingli/.config/Typora/typora-user-images/image-20200807094941859.png)

#### Reliable Data Transfer over a Channel with Bit Errors: rdt2.0

Assuming all transmitted packets are received in the order in which they were sent, but may be error.



a Message-dictation protocol using both positive acknowledgements and negative acknowledgements.

- Allow the receiver to let the sender know what has bee received correctly, or what has been received in error and thus requires repeating.
- ARQ protocols.

Fundamentally, three additional protocol capabilities in ARQ protocols to handle the presence of bit errors:

- Error detection.
- Receiver feedback.
  - ACK and NAK packets were sent back from the receiver to the sender.
- Retransmission.

![image-20200807104302563](/home/yemingli/.config/Typora/typora-user-images/image-20200807104302563.png)



## Connection-Oriented Transport: TCP

## Principles of Congestion Control

## TCP Congestion Control