---
layout: post
title: "Network System"
description: ""
category: 
tags: []
---
{% include JB/setup %}

# Ethernet
## RFC 894 vs RFC 1042
_2.2 Ethernet and IEEE 802 Encapsulation_ of _TCP/IP Illustrated, Vol. 1: The
Protocols (Addison-Wesley Professional Computing Series) 1st Edition_ says:

> RFC 894 encapsulation is most commonly used.

And [Ethernet_frame][1] says:

> Ethernet II frame, or Ethernet Version 2,[g] or DIX frame is the most common
> type in use today, as it is often used directly by the Internet Protocol.

Ethernet II frame means a RFC 894 frame. [Ethernet_frame][1] also says:

> There exists an Internet standard for encapsulating IPv4 traffic in IEEE 802.2
> LLC SAP/SNAP frames.[13] It is almost never implemented on Ethernet, although
> it is used on FDDI, Token Ring, IEEE 802.11 [clarification needed] and other
> IEEE 802 LANs.

IEEE 802.2 LLC SAP/SNAP frames are RFC 1042 frame. So it seems that RFC 894 is
more much widely used than RFC 1042.


  [1]: https://en.wikipedia.org/wiki/Ethernet_frame "Ethernet_frame"

# TCP 

[TCP Flags: PSH and URG](http://packetlife.net/blog/2011/mar/2/tcp-flags-psh-and-urg/)

### Congestion Control
- [TCP Congestion Control RFC](http://tools.ietf.org/html/rfc5681)

TCP backoff:
- [Computing TCP's Retransmission Timer RFC](http://tools.ietf.org/html/rfc6298)
- [Karn's Algorithm](http://en.wikipedia.org/wiki/Karn's_algorithm)
- [Removing exponential backoff from TCP](http://www.sigcomm.org/sites/default/files/ccr/papers/2008/October/1452335-1452338.pdf)
- [TCP Extensions for High Performance RFC](http://tools.ietf.org/html/rfc7323)

One TCP connnection has one RTO timer.

# Checksum

- [Computing the Internet Checksum](https://tools.ietf.org/html/rfc1071)
- [IP checksum definition](http://www.netfor2.com/checksum.html)

For the following `back udp cksum` error in the data captured by tcpdump, [UDP / 
TCP Checksum errors from tcpdump & NIC Hardware 
Offloading](http://sokratisg.net/2012/04/01/udp-tcp-checksum-errors-from-tcpdump-nic-hardware-offloading/) has a good explanation.  

```
13:50:36.947842 IP (tos 0x0, ttl 64, id 17989, offset 0, flags [DF], proto UDP (17), length 56)
    127.0.0.1.4445 > 127.0.0.1.54279: [bad udp cksum 0xfe37 -> 0x7e2b!] UDP, length 28
	0x0000:  4500 0038 4645 4000 4011 f66d 7f00 0001  E..8FE@.@..m....
	0x0010:  7f00 0001 115d d407 0024 fe37 5375 6e20  .....]...$.7Sun.
	0x0020:  4a61 6e20 3034 2031 333a 3530 3a33 3620  Jan.04.13:50:36.
	0x0030:  4353 5420 3230 3135                      CST.2015
```

# MISC
Output form `ifconfig`.
```
lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host


wlan0     Link encap:Ethernet  HWaddr 7c:7a:91:bf:de:3e  
          inet addr:10.62.32.163  Bcast:10.62.47.255  Mask:255.255.240.0
          inet6 addr: fe80::7e7a:91ff:febf:de3e/64 Scope:Link
```
Consider a server and a client which are running on the same machine. Moninoring 
with tcpdump draws the following conclusion. Whatever ip (127.0.0.1 or 
10.62.32.163) the client use to connnect to the server, the lo interface is used. 

# IETF
- [Idnits Tool](https://tools.ietf.org/tools/idnits/about)
- [IETTF Draft Tool xml2rfc](http://xml2rfc.ietf.org/)

# HTTPS
- [HTTP Over TLS RFC](http://tools.ietf.org/html/rfc2818)
- [TLS 1.2 RFC](http://tools.ietf.org/html/rfc5246)
- [TLS Wikipedia](http://en.wikipedia.org/wiki/Transport_Layer_Security)
