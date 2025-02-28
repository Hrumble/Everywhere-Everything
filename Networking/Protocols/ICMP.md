**Internet Control Message [[What is a protocol|Protocol]]**
[RFC 792](https://datatracker.ietf.org/doc/html/rfc792)

>[!danger] **ICMP** does not qualify for a particular layer of the [[OSI Model]], it is in between the Transport and network layers.


>[!info] The [[IP|Internet Protocol]] was not made to be reliable, the entire purpose of **ICMP** is to provide feedback about possible problems in the communication.

When gateways communicate between themselves via [[IP]], there is no guarantee that a certain packet has reached it's destination, or that a gateway was successful in transferring the data to the next gateway, for that reason **ICMP** comes in.
**ICMP** sends information about what happened to the packet back to the source host.

>[!danger] ICMP messages are only sent about errors in handling fragment zero of fragmented datagrams.

# Average ICMP Header

The **ICMP** encapsulates another **IP** header as well as the first 64 bits of it's data. Here's a **Possible ICMP** Header. For `Type = 4, 11, or 3`
![[ICMP_header.png]]

## Type
`8 Bits`

The **Type** of **ICMP** message is what will determine how the rest of the data is interpreted, there are up to 16 possible types
```shell
0  Echo Reply
3  Destination Unreachable
4  Source Quench
5  Redirect
8  Echo
11  Time Exceeded
12  Parameter Problem
13  Timestamp
14  Timestamp Reply
15  Information Request
16  Information Reply
```

>[!warning] Based on the **Type** of **ICMP** message, the entire header can change.

## Code
`8 Bits`

The **Code** is present in every **ICMP** header. However, it's interpretation depends on the **ICMP** Type.
The code helps the source gateway identify in more details the cause of the issue (*What caused an **ICMP** Message of this particular **type** to be sent*)
## Checksum
`16 Bits`

used to check that the packet arrived correctly, it's computed and should be zero.

## Unused

This is an area that is different for each **ICMP** Type, see below for each different type

## IH + 64 Bits

This is an area reserved for the [[IP]] which was not able to go through or that requested an **ICMP** from the gateway. the `64 Bits` are the first `64 Bits` of the data portion of the **IP** datagram, used to match the right packet.

# Different ICMP Types

## Destination Unreachable - 3

![[ICMP_header.png]]
As seen above, simply returns a **Type 3 ICMP**.

**Code** can be one of the following values:
```shell
0 = net unreachable;

1 = host unreachable;

2 = protocol unreachable;

3 = port unreachable;

4 = fragmentation needed and DF set;

5 = source route failed.
```

## Time Exceeded - 11

![[ICMP_header.png]]
Returns a **Type 11 ICMP**
Same as [[#Destination Unreachable - 3]] Header.

**Code** can be
```shell
0 = time to live exceeded in transit;

1 = fragment reassembly time exceeded.
```

## Parameter Problem - 12

![[ICMP_header_12.png]]
Returns a **Type 12 ICMP**, this time, there is one more field, **Pointer**.
The only possible **Code** is `0`, as information about the issue is stored in the **Pointer** field

### Pointer
`8 Bits`

it's in the name, it points to where the error in the IP header is located.
it's value represents the octet in which the error is, for instance, a value of `1` would represent an error in the [[IP#Type of Service|ToS]].

## Source Quench Message - 4

![[ICMP_header.png]]
**Type 4 ICMP**

The **Code** field is left at `0`, this message is sent if the rate at which you are sending the packet is too high, that the gateway cannot process all of them.

This message is asking you **to chill**.

>[!quote] The gateway or host may send the source quench message when it approaches its capacity limit rather than waiting until the capacity is exceeded.  This means that the data datagram which triggered the source quench message may be delivered.

## Redirect Message - 5

![[ICMP_header_5.png]]
Returns **Type 5 ICMP**, instead of the unused field the **Gateway Internet Address**

Possible **Codes**
```shell
0 = Redirect datagrams for the Network.

1 = Redirect datagrams for the Host.

2 = Redirect datagrams for the Type of Service and Network.

3 = Redirect datagrams for the Type of Service and Host.
```

The Redirect Message is sent to the host when a shorter route to the destination is possible, the packet still gets sent to the destination. However, the host is advised to change it's routing.

### Gateway Internet Address
`32 bits`

This is the address to the gateway that would make the path shorter.


## Echo Request or Echo Reply - 8/0

![[ICMP_header_8.png]]
**ICMP Type 8** or **Type 0**. 

>[!quote] The address of the source in an echo message will be the destination of the echo reply message.  To form an echo reply message, the source and destination addresses are simply reversed, the type code changed to 0, and the checksum recomputed.

This is what is used when using `ping www.example.com`, it sends an **ICMP Request (Type 8)** to the target destination, if the destination host is alive then it will send back an **ICMP Echo Reply (Type 0)** to the source **IP** address.

The **Identifier** serves to identify which **Request** is this **Reply** "replying to", both the request and it's reply should have the same identifier.

The **Sequence Number** is