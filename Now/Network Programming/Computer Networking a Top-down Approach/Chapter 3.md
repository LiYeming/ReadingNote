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

## Principles of Reliable Data Transfer

## Connection-Oriented Transport: TCP

## Principles of Congestion Control

## TCP Congestion Control