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

But What if ACK or NAK is corrupted?

- New protocol? not desirable.
- Recovery? but Channel must be reliable, not packet loss.
- Resend duplicated packets? 
  - How to let receiver know the packet is a resend or new data? Introduce state variable to coordinate sender and receiver.
    - The sender will change state if received a non-corrupted ACK message and the ACK message match the state of sender, otherwise it resends the message.
    - The receiver will change state if received a non-corrupted message and the message match the state. otherwise it will resends the ACK message with the last successful state.
      - The sender would know if it received duplicate ACK messages.

#### Reliable Data Transfer over a Lossy Channel with Bit Errors: rdt3.0

Two additional concerns:

- How to detect packet loss.
- What to do when packet loss occurs.

The second question is answered in previous protocol. Handling the first quest needs a new protocol mechanism.

- Wait for some time and no reply ensures the sender that the packet is lost, thus resend the packet.
  - The worst case maximum delay is very difficult to estimate.
  - In practice, Sender choose a well chosen time value.
  - Duplicate data packets would likely to happen, but we luckily can handle this situation.
- A timer is needed for implementation of rdt 3.0
  - This protocol is called **Alternativing-bit protocol**.



### Pipelined Reliable Data Transfer Protocols

rdt 3.0 is not efficient since it is a stop-and-wait protocol.

- Utilization is $ \frac{L / R}{RTT + L / R}$, which is extremely low.
- **Pipelining**, The sender is allowed to send multiple packets without waiting for acknowledgments.
  - The range of sequence numbers must increase, each in-transit packet must have a unique sequence number and there may be multiple in transit un-acknowledge packets.
  - The sender and receiver must have to buffer more than one packet.
  - Two basic approaches toward pipelined error recovery can be identified.
    - Go-Back-N
    - Selective repeat

### Go-Back-N (GBN)

the number of packets allowed to transmit without acknowledgment is limited by $N$.

- Implemented by a moving window of size N which start from the oldest unacknowledged packet.
- GBN protocol is often called sliding-window protocol.
- In practice, a packet's sequence number is carried in a fixed-length field in the packet header. $[0, 2^k - 1]$, and all arithmetic involving sequence numbers must then be done using modulo $2^k$ arithmetic.



The GBN sender

- Invocation from above, 
  - check if the window is full, if not, a packet is created and sent, return the data back otherwise.
- Receipt of an ACK
  - cumulative acknowledgment, indicating that all packets with a sequence number  up to and including $n$ have been correctly received.
- Timeout Event
  - recursively sent packets in the sliding window, until all acknowledged.

The GBN receiver

- receiving sequence number $n$, correctly and in order if not corrupted, sent it to upper layer, and send an ACK for packet $n$. otherwise sends an ACK for the most recently received in-order packet.



### Selective Repeat

When the window size if large, a single packet error can cause GBN to retransmit a large number of packets.

- A window size $N$.
- The sender will have already received ACKs for some of the packets in the window.

SR sender events and actions

- Data received from above, 
  - checks the next available sequence number for the packet. if available , pack data and sent.
  - otherwise it is either buffered or returned to the upper layer for later transmission.
- Timeout,
  - Each packet must now have its own logical timer, since only a single packet will be transmitted on timeout.
- ACK received,
  - Sender marks that packets as having been received, provided it is in the window.
  - The window base is moved forward to the unacknowledged packet with the smallest sequence number.

SR receiver events and actions

- Packet with sequence number in [rcv_base, rcv_base + N - 1] is correctly received.
  - The received packet falls within the receiver's window and a selective ACK packet is returned to the sender.
  - If the packet was not previously received, it is buffered.
  - If the packet has a sequence number equal to the base of the receive window, then this packet and any previously buffered and consecutively numbered packets are delivered to the upper layer. The window is moved forward.
- Packet with sequence number in [rcv_base -N, rcv_base -1] is correctly received.
  - an ACK must be generated, or the sender's window may never move forward.
  - Sender and Receiver will not always have an identical view of what has been received correctly and what has not.
- Otherwise, ignore the packet.



The lack of synchronization between sender and receiver windows have important consequences

- there is no way to distinguish a retransmission of packet no. 0 or a new data from packet no. N.
- The window size should be lower than or equal to half of the size of the sequence number.



With packet reordering, the channel can be thought of as essentially buffering packets and spontaneously emitting these packets at any point in the future.

## Connection-Oriented Transport: TCP

TCP, Internet's transport-layer, connection-oriented, reliable transport protocol.

### The TCP Connection

- Connection-oriented
  - "handshake" process to establish the parameters of the ensuing data transfer. both sides of the connection will initialize many TCP state variables associated with TCP connection.
- Logical "connection" instead of end-to-end TDM or FDM.
- **full-duplex service**, Process A can send and receive data from Process B simultaneously.
- **point-to-point** single sender and single receiver.
  - connection establishment, **"Three-way handshake"**
  - TCP directs application layer data to sendbuffer, set aside during the initial three-way handshake.
  - The maximum amount of data that can be grabbed and placed in a segment is limited by **maximum segment size** (MSS).
  - MSS is determined by the link-layer frame that can be sent by the local sending host MTU. Ethernet and PPP link-layer protocol have MTU of 1500 bytes, thus the typical value of MSS is 1460 bytes.(40 TCP header)
  - a TCP connection consists of buffers, variables and a socket connection to a process in host.

### TCP Segment Structure

- Source and Destination port number
- Checksum field
- 32-bit sequence number field
- 32-bit acknowledge number field
- 16-bit receive window field
- 4-bit header length field
- Optional and variable-length options field
  - sender and reciever negotiate the maximum segment size
  - a window scaling factor for use in high-speed networks.
  - Time-stamping option
- Flag field contains 6 bits
  - **ACK** bit, used to indicate that the value carried in the acknowledgement field is valid.
  - **RST**, **SYN**, **FIN** bits, used for connection setup and teardown.
  - **CWR**, **ECE** bits, used in explicit congestion notification
  - **PSH** bit, receiver should pass the data to upper layer immediately
  - **URG** bit, there is data in this segment that the sending-side upperlayer entity has marked as "urgent".

#### Sequence Numbers and Acknowledgment Numbers

TCP view data as an unstructured, but ordered, stream of bytes.

**The sequence number for a segment** is the byte-stream number of the first byte in the segment.

**The acknowledgment number** is the sequence number of the next byte expecting from other side.

- TCP is said to provide cumulative acknowledgments
- Out of order segments are not discarded, but kept and waiting for missing bytes to fill the gaps.
- In practice, both sides of a TCP connection randomly choose an initial sequence number.
  - minimize the possibility that a segment that is still present in the network from an earlier already terminated connection between two hosts.

### Round-Trip Time Estimation and Timeout

How to estimate RTT? 

- The SampleRTT for a segment is the amount of time between when the segment is sent and when an acknowledgement for the segment is received.

- most TCP implementations take only one SampleRTT measurement at a time.

  - SampleRTT is being estimated for only one of the transmitted but currently unacknowledged segments
  - never computes a SampleRTT for a segment that has been retransmitted.

- SampleRTT will fluctuate from segment to segment due to congestion in the routers and to the varying load on the end systems.

  - TCP maintains an average EstimatedRTT of SampleRTT values:
    $newEstimatedRTT = (1 - \alpha) \cdot EstimatedRTT + \alpha \cdot SampleRTT$

    The recommended value of $\alpha = 0.125$.

  - $DevRTT = (1 - \beta) \cdot DevRTT + \beta \cdot |SampleRTT - EstimatedRTT|$

    The recommended value of $\beta$ is 0.25.

#### Setting and Managing the Retransmission Timeout Interval

$TimeoutInterval = EstimatedRTT + 4 \cdot DevRTT$

where an initial TimeoutInterval value of 1 second is recommended.

### Reliable Data Transfer

TCP creates a reliable data transfer service on top of IP's unreliable best effort service.

- Uncorrupted.
- Without gaps
- Without duplication
- In sequence

The recommended TCP timer management procedures use only a single retransmission timer, even if there are multiple trnasmitted but not yet acknowledge segments.

- Significant overhead of Timer.

#### Doubling the Time Interval

Length of the timeout interval after a timer expiration

- Each time TCP retransmits it sets the next timeout interval to twice the previous value.
- A congestion control approach.

#### Fast Retransmit

Doubling timeout interval may cause delay in transimission.

- If TCP sender receives three duplicate ACKs for the same data, it takes this as an indication that data loss happen.
- TCP sender performs a fast retransmit, overriding the segment's timer.

#### Go-Back-N or Selective Repeat

TCP’s error-recovery mechanism is probably best categorized as a hybrid of GBN and SR protocols.

### Flow Control

Application is relatively slow at reading data. sender would overflow the receive buffer by sending too much data too quickly.

Implementation of flow control

- Sender maintain a variable called the **receiver window**.

- To avoid overflow, we need
   $LastByteRcvd - LastByteRead \leq RcvBuffer$

- The receiver window is defined as 

  $rwnd = RcvBuffer - (LastByteRcvd - LastByteRead)$

- The sender keep track of two variables, LastByteSent and LastByteAcked.

  - Keep 
    $rwnd \geq LastByteSent - LastByteAcked$
  - Sender should continue to send segments with one data byte when rwnd is zero.
    - In order to update rwnd

### TCP Connection Management

How a TCP connection is established and torn down.

Threeway handshake:

1. client side TCP first send a special TCP segment to the server side TCP.
   - SYN bit is set to 1. 
   - Randomly choose an initial sequence number (Client ISN)
   - No application level data
   - This is called  **SYN segment**
2. The server extracts TCP SYN segment, allocates the TCP buffers and variables to the connection, Then send a connection granted segment to the client TCP
   - SYN bit is set to 1.
   - Acknowledgment field of the TCP segment header is set to Client ISN + 1.
   - Server chooses its own initial sequence number (Server ISN) and puts this value in the sequence number field  of the TCP segment header.
   - This segment is called **SYNACK segment**
3. The client received the SYNACK segment, allocates buffers and variables to the connection. Send yet another segment
   - Put the value Server ISN + 1 in the acknowledgment field
   - Put SYN bit to zero
   - May carry client-to-server data in the segment payload.

When a connection ends

1. client TCP send a special TCP segment to the server process.
   - FIN bit is set to 1.
2. server send an acknowledgment segment in return.
3. server send a special TCP segment to client with FIN bit set to 1.
4. client acknowledges the server's shutdown segment. 
   - At this point, the resources in the two hosts are now deallocated.

## Principles of Congestion Control

1. Why congestion is a bad.
2. How network congestion is manifested in the performance received by upper-layer applications.
3. various approaches to avoid, react to, network congestion.

### The Causes and the Costs of Congestion

#### Two senders, a Router with Infinite Buffers

- Two senders's sending rates are limited to the throughput of router.
- The average delay is increasing exponantiallly  when the sending rates approaching the throughput ability. and goes to infinity when exceeds it.

*Optimality in throughput is not Optimality in delay*

- Large queuing delays are experienced as the packet-arrival rate nears the link capacity.

#### Scenario 2: Two Senders and a Router with Finite Buffers

1. Packets would be dropped when arriving to an already full buffer
2. Each connection is reliable, dropped packets would be retransmitted.

Insights:

- *Sender must perform retransmission in order to compensate for dropped packets due to buffer overflow.*

- *Unneeded retransmissions by the sender in the face of large delays may cause a router to use its link bandwidth to forward unneeded copies of a packet.*

#### Scenario 3: Four Senders, Routers with Finite Buffers, and Multihop Paths

Heavy traffic would crowd out throughput of other connections, even to zero.

- A offered load versus throughput tradeoff.

Insights:

- *When a packet is dropped along a path, the transmission capacity that was used at each of the upstream links to forward that packet to the point at which it is dropped ends up having been wasted.*

### Approaches to Congestion Control

- End-to-End congestion control (No Network layer support)
  - network layer provides not explicit support to the transport layer for congestion control.
  - TCP duplicated ACKs or timeouts are indicators of congestion, no IP protocol support.
- Network-assisted congestion control (With Network layer support)
  - Router provides explicit feedback to the sender receiver pair about congestion state of the network.

## TCP Congestion Control

Each sender limit the rate at which it sends traffic into its connection as a function of perceived network congestion.

Three questions to be answered.

- How to limit sending rate?
  - Augmenting Flow Control, cwnd is congestion window.
    $LastByteSent - LastByteAcked \leq \min\{cwnd, rwnd\}$
  - The sender's sending rate is bounded by $cwnd / RTT$.
- How to estimate the congestion?
  - A timeout or three duplicate ACK from the receiver.
  - Direct consequence of a overflow of router.
- What is the optimal relationship between congestion and sending rate?
  - **Self-clocking**: Use acknowledgements to trigger its increase in congestion window size.

How should a TCP sender to determine the rate at which it should send?

- Too fast, congestion collapse
- Too slow, waste of bandwidth

Guiding Principles:

- A lost segment implies congestion, hence sending rate should drop.
- Acknowledged segment implies non-congestion, hence sending rate should increase.
- Bandwidth probing, zig-zag adjusting sending rate to probe bandwidth.

#### Slow Start

1. $cwnd$ is initialized to a small value 1 $MSS$.
2. increases by 1 $MSS$ every time a transmitted segment is first acknowledged. In effect doubling sending rate every $RTT$.
3. When a timeout happen, sender sets the value of $cwnd$ to 1, set $ssthresh$ to $cwnd/2$ and begins the slow start process anew. 
4. When $cwnd$ equals $ssthresh$, TCP transitions into congestion avoidance mode.
5. When three duplicate ACKs are detected, in which case TCP performs a fast retransmit and enters the fast recovery state.

#### Congestion Avoidance

1. Conservative increase in $cwnd$, increase the value of $cwnd$ by just a single $MSS$ every $RTT$. Implemented by increase $cwnd$ by $MSS / cwnd$ for each and every new acknowledgment.
2. When a timeout happen, update $cwnd$ and $ssthresh$, return to slow start state.
3. When three duplicate ACKS happen, update $ssthresh$, 
   $cwnd = ssthreash + 3\cdot MSS$, 
   then enter fast recovery mode.

#### Fast Recovery

1. $cwnd$ is increased by 1 $MSS$ for every duplicate ACK received for the missing segment that caused TCP to enter the fast-recovery state.
2. Fast Recovery is a recommended, but not required component of TCP.

### Fairness

### Explicit Congestion Notification

