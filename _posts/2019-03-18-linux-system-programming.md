---
layout: post
title: "Linux System Programming"
description: ""
category:
tags: [linux,programming]
---
{% include JB/setup %}

## Socket Options
http://man7.org/linux/man-pages/man2/setsockopt.2.html

- Socket level options are in [SOCKET(7)](http://man7.org/linux/man-pages/man7/socket.7.html).
- TCP specific options are in [TCP(7)](http://man7.org/linux/man-pages/man7/tcp.7.html).
- IP specific options are in [IP(7)](http://man7.org/linux/man-pages/man7/ip.7.html).

`SO_RCVLOWAT` and `SO_SNDLOWAT` does not have an effect on blocking read/write.
`SO_RCVLOWAT` and `SO_SNDLOWAT` apply to all non-blocking IO such as select, poll
refer to `UNIX Network Programming Volume 1, Third Edition: The Sockets Networking API`.


- https://stackoverflow.com/a/45105711/431698
- https://my.oschina.net/yangan/blog/185262
- https://www.jianshu.com/p/d3442ff24ba6

https://news.ycombinator.com/item?id=13918141 says:
> [1] Actually, EVFILT_READ and EVFILT_WRITE do support setting a low water mark, so you can theoretically condition readiness signaling on kernel buffer availability. But this is basically a kqueue-based extension to the traditional BSD Sockets API options SO_RCVLOWAT and SO_SNDLOWAT. You could theoretically get the same behavior on Linux, at least on a per socket basis (as opposed to the per event basis for kqueue) by setting those options. Alas, Linux doesn't obey SO_RCVLOWAT or SO_SNDLOWAT when polling. :( Speculating, I bet it's probably a performance optimization netting some single-digit performance improvement for the original expected use cases but, once again, keeping something difficult impossible.

- [What is the relation between the levels in setsockopt(2) man page?](https://stackoverflow.com/questions/55224417/what-is-the-relation-between-the-levels-in-setsockopt2-man-page)
