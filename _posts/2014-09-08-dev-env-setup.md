---
layout: post
title: "Coding Tool Setup"
description: ""
category:
tags: []
---
{% include JB/setup %}

## Cscope


## Linux Source Code on macOS
```
wget https://cdn.kernel.org/pub/linux/kernel/v2.6/linux-2.6.11.tar.xz

tar xf  linux-2.6.11.tar.xz

cd linux-2.6.11

find  .                                                                \
-path "./arch/*" ! -path "./arch/x86*" -prune -o               \
-path "./Documentation*" -prune -o                                 \
-path "./scripts*" -prune -o                                       \
-path "./drivers*" -prune -o                                       \
      -name "*.[chxsS]" -print >./cscope.files

cscope -b -q -k
```

Since macOS's file system is case-insensitive and Linux kernel has some files
whose have the same case-insensitive file names. The following files can't exist
on macOS.

```
include/linux/netfilter_ipv4/ipt_connmark.h
include/linux/netfilter_ipv4/ipt_TOS.h
include/linux/netfilter_ipv4/ipt_TCPMSS.h
include/linux/netfilter_ipv4/ipt_MARK.h
include/linux/netfilter_ipv4/ipt_ECN.h
include/linux/netfilter_ipv4/ipt_DSCP.h
include/linux/netfilter_ipv6/ip6t_MARK.h
net/ipv4/netfilter/ipt_tcpmss.c
net/ipv4/netfilter/ipt_ecn.c
net/ipv4/netfilter/ipt_dscp.c
net/ipv4/netfilter/ipt_connmark.c
net/ipv4/netfilter/ipt_TOS.c
net/ipv4/netfilter/ipt_MARK.c
net/ipv6/netfilter/ip6t_MARK.c
```

## C++ Hacking

```
rm -fr *cscope*;find -name '*.h' -or -name '*.cc' > cscope.files; cscope -b -q;rm cscope.files;
```
