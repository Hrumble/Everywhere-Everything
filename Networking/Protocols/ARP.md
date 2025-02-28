**Address Resolution [[What is a protocol|Protocol]]**
[RFC 826](https://datatracker.ietf.org/doc/html/rfc826)

>[!info] According to the official RFC
>ARP is An Ethernet Address Resolution Protocol or Converting Network Protocol Addresses

The [[IP]] address is used to locate a device on a network, whereas the **MAC** address is used to actually **identify** the device.
The **ARP** is simply the protocol used to convert [[IP]] Addresses into **MAC** Addresses that the [[Ethernet]] frame requires to transmit the data.

>[!info] see [[Ethernet]] for what a **MAC** address is.

*For some reason the official RFC does not seem to have a header for the **ARP**, I **THINK** that this is because, **ARP** is not actually a protocol in and of itself, but rather a concept of a protocol.
In the official RFC the **ARP** is described as what it should do rather than what it is.
**Anyways, it really isn't complicated***

An **ARP** Packet is sent with no [[IP]] Header, but rather only encapsulated by an [[Ethernet]] frame of **source** : the **sending** NIC MAC Address, and destination `ff:ff:ff:ff:ff:ff`: which essentially means **broadcast**, which **ESSENTIALLY** means it's sent to absolutely every single device on the network, **existent or not**.


>[!info] A Typical ARP exchange in amazing theatrical format
>```
> > 192.168.0.3 (0e:0e:0e:3f:ff:2a): Hey **EVERYONE**, who has 192.168.0.4?
> > 192.168.0.4 (0b:0b:0c:dd:ff:2a): Hey (0e:0e:0e:3f:ff:2a), I'm here at (0b:0b:0c:dd:ff:2a)
>```
>


# ARP Request


The **ARP** Request is sent to **every single existing MAC address** by a host on the network that needs to communicate with the host at a particular **[[IP]]** Address, it *essentially* contains:

| Field          | Description                                                                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Protocol Type  | How it has the **IP** Address, either `IPv4` or `IPv6`                                                                                           |
| Sender **MAC** | Where the **request** to get the **MAC** address is coming from                                                                                  |
| Sender **IP**  | What is the **IP** Address of the source host                                                                                                    |
| Target **MAC** | Inside an **ARP** request, this field has a value of 0 or `00:00:00:00:00:00`, since it's requesting for the **MAC** address, it doesn't know it |
| Target **IP**  | The **IP** Address of the host that the source is trying to reach                                                                                |
| OPCode         |                                                                                                                                                  |

# ARP Reply

The ARP Reply is sent by the host who identifies with the **IP** address requested by the **ARP Request**, it contains the exact same fields, but inversed.

| Field          | Description                                                                                    |
| -------------- | ---------------------------------------------------------------------------------------------- |
| Protocol Type  | How it has the **IP** Address, either `IPv4` or `IPv6`                                         |
| Sender **MAC** | The **MAC** Address that the Request was looking for, **MAC** address of the host that replies |
| Sender **IP**  | The **IP** Address of the replying host, used for matching.                                    |
| Target **MAC** | The **MAC** Address of the request host                                                        |
| Target **IP**  | The **IP** Address of the request host                                                         |

# ARP Probe

***

