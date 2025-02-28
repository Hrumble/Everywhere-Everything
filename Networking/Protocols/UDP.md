**User Datagram [[What is a protocol|Protocol]]**
[RFC 768](https://www.ietf.org/rfc/rfc768.txt)

# UDP RFC

Official UDP RFC can be found [here](https://www.ietf.org/rfc/rfc768.txt)

>[!quote] From the official RFC
>This protocol  provides  a procedure  for application  programs  to send
messages  to other programs  with a minimum  of protocol mechanism.  The
protocol  is transaction oriented, and delivery and duplicate protection
are not guaranteed.  Applications requiring ordered reliable delivery of
streams of data should use the [[TCP|Transmission Control Protocol (TCP)]].

![[UDP-RFC_1.png]]

UDP is fairly simple as it just basically sends the data wherever you ask it to, does not matter if the data is received, does not establish any kind of connection first, just sends the data.

Simply a `source port` (optional), a `destination port` and the data being sent.