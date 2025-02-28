**Internet [[What is a protocol|Protocol]]**
[RFC 791](https://datatracker.ietf.org/doc/html/rfc791)
[[OSI Model#Network Layer - 3|Network Layer]]

The Internet protocol is **NOT** what gives your computer an IP address (*this is [[DHCP]]*), rather it is what **USES** your computer's address to send and receive packet, thus the name **Internet Protocol Address**, the address is used in the **IP**.

The **IP** is used to identify which machine is sending what data to which other machine.
It references the two machines using **IP** **Addresses**, and transmits the data via [[OSI Model|higher level]] protocols such as [[TCP]] or [[UDP]].

The following shows the **IP Header** (each tick representing a [[Binary|bit]]) 
![[ip_header.png]]

## Version 
`4 Bits`

The Version of the **IP** protocol (*this literally means Internet Protocol Protocol so I'll stop writing it like that, just know I'm referring to IP protocol*).
It usually represents either $4$ `0100` or $6$ `0110` according to `IPv4` or `IPv6`.

## IHL
`4 Bits`
**Internet Header Length**

The length of the **IP** header, basically where the info on the packet stops and the data starts.
It's noted as [[Binary#Definitions|DWORDs]] (`32 Bits`), and the minimum valid value is 5 (Since each layer in the above image is `32 Bits`, `5 DWORDs` represents the End of **[[#Destination Address]]**).

## Type of Service
`8 Bits`

>[!danger] Not sure I correctly understand this, basically `8 Bits` set to tell the network how important or reliable this packet needs to be treated as?

## Total Length
`16 Bits`

The **Total Length** (TL) of the entire **datagram** in [[Binary#Definitions|octets]], including header + data. `16 Bits` allow for datagrams of up to 65 535 Octets, except that's really impractical for both the host and the network. 
The average value of **TL** is 576 Octets (4608 Bits), so most hosts should be expecting a datagram of that size.

## Identification
`16 Bits`

A 16 Bit value that helps reassembling the packet when the packet has been [[#Fragmentation|Fragmented]].

## Flags
`3 Bits`

```
Bit 0: reserved, must be zero
Bit 1: (DF) 0 = May Fragment,  1 = Don't Fragment.
Bit 2: (MF) 0 = Last Fragment, 1 = More Fragments.
```

Helps during [[#Fragmentation]] to identify if 
1. The packet is fragmented (`Bit 1`)
2. The packet has more fragments on the way (`Bit 2`)

## Fragment Offset
`13 Bits`

If the packet is fragmented, this value helps identify where the fragment is located amongst the other ones, it's measured in [[Binary#Definitions|QWords]].
The first fragment has an offset of `0`.

## Time To Live
`8 Bits`
**TTL**

**Basically** how many hops the packet can make without being destroyed.
The value is reduced by one every hop it makes.

## Protocol
`8 Bits`

This value represents what is the protocol used in the next layer of the [[OSI Model]], they are referenced using assigned decimal values. For instance, [[TCP]] is `6`.

Assigned values can be found [here](https://datatracker.ietf.org/doc/html/rfc790#:~:text=Internet%20Protocol%20Numbers-,ASSIGNED%20INTERNET%20PROTOCOL%20NUMBERS,-In%20the%20Internet)

## Checksum
`16 Bits`

>[!quote] 
>The Header Checksum provides a verification that the information used in processing internet datagram has been transmitted correctly.  The data may contain errors.  If the header checksum fails, the internet datagram is discarded at once by the entity which detects the error.

## Source Address
`32 Bits`

From where the data is coming from.
## Destination Address
`32 Bits`

To where the data is supposed to go/arrive.

## Options
`Variable`

The Options part is a real pain in the ass, I don't understand anything it seems to be used mostly in debugging and security, see [here](https://datatracker.ietf.org/doc/html/rfc791#section-3.2:~:text=Internet%20Protocol%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20Specification-,Options%3A%20%20variable,-The%20options%20may)


# Fragmentation

Blah blah blah basically sends multiple packets instead of one for large files and then puts it all back together and shit idc.