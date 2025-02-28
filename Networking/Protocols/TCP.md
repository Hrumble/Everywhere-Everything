**TCP (Transmission Control [[What is a protocol|Protocol]])**
[RFC 793](https://www.ietf.org/rfc/rfc793.txt)
# TCP RFC
**RFC** stands for **Requests For Comments** which is a fancy way to say it's the ruleset of how a protocol should behave, and what information is contained in each of the packets sent via said protocol

When sending or receiving a TCP packet, all the information is stored in what's called the **header** comprised of **7 layers**
![[TCP-RFC_1.png]]

### Layer 1

**The layer one contains the `source port` : `16 bits` as well as the `destination port`**  : `16 bits`

TCP is a protocol for ports to communicate between themselves, for that, TCP needs to know which port is communicating with which other port.

### Layer 2

**The layer 2 contains the `Sequence Number seq`** : `32 bits`

The sequence number is a randomly chosen octet (8 bits) when the connection first start, that random sequence is called `ISN` ***Initial Sequence Number***. If the `SYN` flag is set, then the **Sequence Number** is equal to `ISN + 1`
>[!quote] From the official RFC website
>"The TCP must recover from data that is damaged, lost, duplicated, or
delivered out of order by the internet communication system.  This
is achieved by assigning a sequence number to each octet
transmitted, [...]  At the receiver, the sequence
numbers are used to correctly order segments that may be received
out of order and to eliminate duplicates.[...]"

>[!faq] What the fuck does that mean?
>it means the sequence number is just a number to tell you in which order the packets have been sent
### Layer 3

**The layer 3 contains the `Acknowledgement Number ACK : 32 bits`**

if the [[#Layer 4|ACK control bit]] is set, then this field is equal to the next sequence number the sender is expecting to receive.

### Layer 4

**The layer 4 contains the `Control Bits:6bits` (the other things don't really matter)**

There are 6 control bits used to tell the sender/receiver what this packet contains.
```
URG:  Urgent Pointer field significant
ACK:  Acknowledgment field significant
PSH:  Push Function
RST:  Reset the connection
SYN:  Synchronize sequence numbers
FIN:  No more data from sender
```
See [[#TCP Control Bits|below]] for more info on each bit


# Three Way Handshake

TCP is used to send data across the network, to ensure the data can successfully be sent, it allows a way to firstly execute a "**Three Way Handshake**", establishing a solid connection between host and source.

To do so, it uses **flags**
![[Pasted image 20240108144559.png]]
1. The client sends a *SYN* packet to the server with a certain [[#Layer 2|seq]], for now let's say `seq = m`
	- This is the client nudging the server and seeing if the server is alive and available
1. The server receives the *SYN* packet and sends back a new *SYN* paquet with a new sequence number `seq = n` to nudge the client back, **as well as** an *ACK* (Acknowledgment) packet with a sequence number `seq = m+1` 
	-  This is to tell the client it received the right SYN packet and is alive and available
	- the reason for the *ACK* packet `seq = m+1` is for the client to know that this is the correct server to which it sent the previous *SYN* packet to, and not an attacker impersonating it.
2. The client receives the *SYN-ACK* packet, checks that the *ACK* packet has the right sequence number, and sends another *ACK* packet back of `seq = n+1` for the server to verify it's identity and establishes the secure connection

 >[!info] When sending data with **TCP**, the data is only send along with the last **SYN** flag, as there is no use sending the data if the connection hasn't been fully established.