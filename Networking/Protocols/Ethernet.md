[RFC 894](https://datatracker.ietf.org/doc/html/rfc894)

>[!warning] The Ethernet frames are read in [[Binary#Big-Endian Little-Endian|Big-Endian]]

In the old network days, computers would be connected to each other via Ethernet. To transmit packets to each other, the computers were referred to by their MAC Addresses, which are hardcoded UIDs inside network devices of each computers. 

Nowadays in most networks, whether WLAN (Wireless Local Area Network) or LAN, each packet is sent enveloped in an ethernet frame, containing the source MAC address and the destination MAC Address.

*Sample Wireshark captured Ethernet Frame*:
```c
Destination: CiscoMer_b8:cf:97 (a8:46:9d:b8:cf:97)
    Address: CiscoMer_b8:cf:97 (a8:46:9d:b8:cf:97)
    .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
Source: IntelCor_4a:74:9b (00:91:9e:4a:74:9b)
    Address: IntelCor_4a:74:9b (00:91:9e:4a:74:9b)
    .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
    .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
Type: IPv4 (0x0800)

```

As you can see, the following packet is sent from an **Intel Corporation** (`IntelCor_4a:74:9b`) network device with a **MAC address** of `00:91:9e:4a:74:9b`, and needs to get to a **Cisco Meraki** network device (`CiscoMer_b8:cf:97`) with a **MAC Address** of `a8:46:9d:b8:cf:97`.
