---
layout: post
title: "Filesystem"
description: ""
category:
tags: [OS]
---
{% include JB/setup %}

# Ext4
- [Ext3#Journaling_levels](https://en.wikipedia.org/wiki/Ext3#Journaling_levels)

[Ext4 Filesystem] has the following text:

> This mode is the slowest except when data
needs to be read from and written to disk at the same time where it
outperforms all others modes.

[Re: Ext3: Why data=journal is better than data=ordered when data needs to be read from and written to disk at the same time by Peter Grandi](https://www.redhat.com/archives/ext3-users/2011-April/msg00000.html)
and [Re: Ext3: Why data=journal is better than data=ordered when data needs to be read from and written to disk at the same time by Jidong Xiao](https://www.redhat.com/archives/ext3-users/2011-April/msg00001.html)
gives the reason.

The reason:

> When executing an fsync(), in data=ordered mode you have to
> write the data data blocks into the journal and wait for the
> data blocks to be written.  This requires generally will
> require extra seeks.  In data=journaled mode, the data blocks
> can be written directly into the sjoujournal without needing
> to seek.
>
> Of course eventually the data and metadata blocks will need
> to be written to their permanent locations before the journal
> space can be reused.  But for short bursty write patterns,
> the fsync() latency will be much smaller in data=journal
> mode.

Peter says:

> Write latency is actually much longer: because it requires *two*
writes instead of one. It is *fsync* latency as mentioned above that is smaller,
because it depends only on the first write to what is in effect a small log
based filesystem. This distinction matters a great deal, because it is the
reason why "short bursty write patterns" is the qualification above. For long
write patterns things are very different as the journal eventually fills up. For
any given size it will also fill up a lot faster for 'data=journal'.

Jidong question 



- [Ext4 Filesystem]:(https://www.kernel.org/doc/Documentation/filesystems/ext4.txt)
