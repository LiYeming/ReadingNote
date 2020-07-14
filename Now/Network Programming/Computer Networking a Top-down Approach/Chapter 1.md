# Computer Networks and the Internet

- Broad overview of computer networking and the Internet

## What is the Internet

- Nuts and Bolts description
  - A computer network that interconnects billions of computing devices throughout the world.
  - `Hosts` or `End systems`, connected together by a network of `communication links` and `packet switches`.
    - `communicaton link` measured by `transmission rate`, in bits/second.
    - `packets`, data with header bytes added by end system.
    - `packet switch` through `routers` and  `link-layer switches`
  - End systems access the Internet through `Internet Service Providers` (ISP), each is itself a network of communication links and packet switches.
  - Protocol that control the sending and receiving of information within the Internet, (TCP/IP).
    - IP protocal specifies the format of packets that are sent and received.
- Services description
  - Internet is an infrastructure that provides services to applications.
    - How does one program instruct the Internet to deliver data to another one running on another end system?
      - `Socket Interface` specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system.
- What is a Protocol?
  - A `protocol` defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.
  - Mastering the field of computer networking is equivalent to understanding the what why and how of networking protocols.

## The Network Edge

## The Network Core

### Packet Switching

Most packet switches use `store-and-forward transmission`, thus:
$$
d_{end-to-end} = N \frac{L}{R}
$$

- $N$ is the number of links from end to end, for N time copying.
- ${L}/{R}$ is lag of single transmission. $L$ bits at rate of $R$ bits/sec.

Packet switch queue incoming information to `output queue`(`output buffer`), thus suffer `queuing delays`.

If the output buffer is full, `packet loss` will occur, either the arriving one or the queued one.

Each router has a forwarding table that maps destination address to the outbound links. A number of routing protocols are used to automatically set the forwarding tables.

Two fundamental approaches to move data through a network of links and switches: `circuit switching` and `packet switching`.

- Traditional telephone networks are `circuit switching` which path is reserved for end-to-end communication.

  The sender is guaranteed to transfer data at a constant rate to receiver.

- packet switching is more efficient, for more sufficient use of transmission rate, and more users.

### A Network of Networks

The access ISPs themselves must be interconnected. done by creating `network of networks`.

![image-20200712110834245](/home/liyeming/.config/Typora/typora-user-images/image-20200712110834245.png)

## Delay, Loss and Throughput in Packet-Switched Networks

Examine and quantify delay, loss and throughput in computer networks.

### Overview of Delay in Packet-Switched Networks

Packet suffer from several types of delay at each node along the path.

- Processing delay, examine packet's header and determine where to direct the packet.
- Queuing delay, wait to be transmitted onto the link.
- Transmission delay, time required to push all the packet's bits into the link.
- Propagation delay, the time required to move from A to B.

![image-20200712111303690](/home/liyeming/.config/Typora/typora-user-images/image-20200712111303690.png)
$$
d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}
$$

### Queuing Delay and Packet Loss

Characterizing queuing delay usually use statistical measures.

`Traffic intensity`: input bits per second / output bits per second

 Design your system so that the `traffic intensity` is no greater than 1.

![image-20200712115743413](/home/liyeming/.config/Typora/typora-user-images/image-20200712115743413.png)

when `traffic intensity` approaches 1, the `average queuing delay` increase exponentially.

the fraction of packet loss increases as traffic intensity increases.

### End-to-End delay

### Throughput in Computer Networks

The rate (bits / seconds) at which a host receiving the file.

- Instantaneous throughput
- Average throughput

bottle-neck nodes



## Protocol Layers and Their Service Models.

A layered architecture 

- Allows us to discuss a well-defined, specific part of large and complex system.
- Providing modularity for easy implementation.



Network designers organize protocols in layers. 

- Each protocol belongs to one of the layers.
- Each layer provides its service by 
  - Performing certain actions within that layer.
  - Using the services of the layer directly below it.
- A protocol layer can be implemented in software, hardware or in a combination of two.
  - software: HTTP, SMTP
  - hardware: Ethernet or WiFi interface cards



The protocols of various layers are called the `protocol stack`.

- Application Layer
  - Network applications ad their application-layer protocols (HTTP/ SMTP/ FTP)
  - Packet of information at the application layer is called `message`.
- Transport Layer
  - Transport application-layer messages between application endpoints. (TCP / UDP)
  - Packets of transport-layer is called `segment`.
- Network Layer
  - Moving datagrams from one host to another. (IP)
  - Packets of network layer is called `datagrams`.
- Link Layer
  - at each node network layer pass datagrams to link layer, and next node link layer push it to network layer.
  - Packets of link layer is called `frames`.
- Physical Layer
  - Actual transmission medium of the link.
- OSI model 
  - Presentation layer
    - Interpret the meaning of data exchanged
    - Data encryption, data compression and data description
  - Session layer
    - Delimiting and synchronization of data exchange
    - Build checkpoints and recovery scheme

It’s up to the application developer to decide if a service is important and build that functionality into the application.

### Encapsulation

![image-20200712124918845](/home/liyeming/.config/Typora/typora-user-images/image-20200712124918845.png)





