**Telecommunication Network**

The Telnet [[What is a protocol|Protocol]] is a [[What is a protocol|Protocol]] which allows host to communicate with a remote desktop's terminal. It's the ancestor of [[SSH]].

**Telnet** works by sending every command typed by the client in cleartext over the network to the host, the host executes that command and then returns it's output in cleartext again back to the client.

>[!danger] Telnet does not have encryption and it's packets are sent in cleartext which means anyone can sniff the traffic, further more there is no authentification system apprt from that which is set up on the host, this makes it so that if the host is already up and running, anyone can send and receive from it's terminal.
