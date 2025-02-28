Skip to the most common subnets [[Subnetworks#Common Subnets|here]]

Subnetworks are multiple portions of a larger network. Each subnet allows its connected devices to communicate with each other, while routers are used to communicate between subnets.

An IP address is made up of [[Binary|32bits]]
![[Pasted image 20240109194546.png]]

In each network, an IP address is cut up into two parts:
- The *Host ID* as well as the *Network ID* each respectively indicating the part of the IP reserved for the *Host* and *Network*

Which is which is indicated by a **Subnet Mask**
![[Pasted image 20240109195312.png]]
The following subnet mask allows for a subnetwork with a maximum of ==255 hosts== (*254 technically because one IP is reserved for the router*)
# CIDR Notation

**CIDR** Notation is a shorter way to specify subnet masks 
it's defined with a *'/' and a number* at the end of the IP address 
e.g. `192.168.100.10/24`
the `/24` here means that the first **24 bits** are reserved for the *Network* which leaves the last **8 bits** for the *Host* portion
in turns the subnet mask `225.225.225.0`

# Common Subnetworks CIDR
Table of the most common Subnet Masks:

| CIDR | Subnet Mask | Maximum Number of Hosts | "Class" |
| ---- | ----------- | -----------------| ---|
| /24 | 225.225.225.0 |  255 hosts | Class C |
| /16 | 225.225.0.0   |  510 hosts | Class B |
| /8  | 225.0.0.0    |   765 Hosts | Class A |

*The Class is a common way of addressing the above subnetworks*