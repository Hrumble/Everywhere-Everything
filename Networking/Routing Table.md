The **routing table** is a table present on every host/gateway on the network.
Your computer has it's own **routing table**, and so does your router. It tells packets trying to reach a particular [[IP]] address where their next "*step*" should be (*basically where is the next gateway to hop to*).

A routing table looks like the following:
```c
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface

default         172.22.16.1     0.0.0.0         UG    0      0        0 wlan0
172.22.16.0     *               255.255.255.0   U     0      0        0 wlan0
```
>[!info] *this is a Linux routing table, obtained with the `route` command*
> it's important to note that not the entire routing table is shown, as linux devs thought it would be "too obvious" to show the rest of them

>[!info] The *Asterix*
>The *asterix* means, to reach this IP, the packet doesn't need to be routed, it can directly be sent to the destination host 


Let's imagine a simple packet **PCK_A** trying to reach the destination address `43.254.45.9`, first,  your **NIC** (*Network Interface Controller*) will look at your routing table (**the one above**) and compare the destination IP address to the **Destination** section of the routing table. 

The `43.254.45.9` IP does not match the `172.22.16.0` entry with the [[Subnetworks|subnet mask]] `255.255.255.0`, as this entry represents all addresses between `172.22.16.0` -> `172.22.16.224` 

So your **NIC** resorts to the **Default** entry, which is... the default entry if none other match.

This default entry tells your **NIC** to send **PCK_A** to the `172.22.16.1` IP Address (*most times, this is your router's IP address*) via your **wlan0** interface (*the `IFace` entry in the table*).

Once **PCK_A** reaches the `172.22.16.1` gateway, the gateway will do the exact same thing your computer just did, except this time with it's own routing table. 

The process will repeat until **PCK_A** reaches it's final destination, `43.254.45.9`.  

>[!info] The windows routing table is kind of ass and doesn't look as nice
>First, compared to linux, no entry is omitted, even if obvious. 
>
>Secondly, there is no `default` entry, but rather a `0.0.0.0` entry with `0.0.0.0` mask which basically means any IP address. 
>
>**Thirdly**, the interface aren't noted as `wlan0`, `eth0` and so on, but rather by their own local IP Addresses
>
>**Fourth**, the `On-link` gateway is the equivalent of the linux *Asterix*.

Here is what a windows **routing table** looks like (*mine*)

```c
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0    192.168.1.254      192.168.1.8     25
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      172.22.16.0    255.255.240.0         On-link       172.22.16.1   5256
      172.22.16.1  255.255.255.255         On-link       172.22.16.1   5256
    172.22.31.255  255.255.255.255         On-link       172.22.16.1   5256
      192.168.1.0    255.255.255.0         On-link       192.168.1.8    281
      192.168.1.8  255.255.255.255         On-link       192.168.1.8    281
    192.168.1.255  255.255.255.255         On-link       192.168.1.8    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link       192.168.1.8    281
        224.0.0.0        240.0.0.0         On-link       172.22.16.1   5256
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link       192.168.1.8    281
  255.255.255.255  255.255.255.255         On-link       172.22.16.1   5256
```

>[!tip] Fun thing I learned by learning all that
>The `255.255.255.255` destination is actually a broadcast, used for [[ARP]]
