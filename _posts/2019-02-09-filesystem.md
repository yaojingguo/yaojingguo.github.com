---
layout: post
title: "Filesystem"
description: ""
category:
tags: [OS]
---
{% include JB/setup %}

# Ext4
## Journaling Levels
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

Jidong question `Write latency is actually much longer`:

> In addition, I am actually not very clear why you said that write()
> latency is longer while fsync() latency is shorter, I am trying to
> repeat what you said, please point out if I am incorrect:
> 1. Normally we call write() syscall first and then call fsync() to
> flush the data.
> 2. The write() returns as long as the data is written into page caches
> while the fsync() returns only if the data have been written into a
> stable store.
> 3. Although write() latency for data=journal mode is much longer
> because it requires two writes instead of one, however, since the
> write() means writing to page cache, so the actually cost is not so
> high, compared to the fsync() syscall where we have to write into disk
> and may require disk seeks. So we can mainly focus on the fsync()
> system call.

I agree with Jidong. And I think that by saying `Write latency is actually much
longer: because it requires *two* writes instead of one`, Peter means that
write involves a write of data blocks to the journal and a write of data blocks
to its original location in filesystem.


> commit=nrsec	(\*)	Ext4 can be told to sync all its data and metadata
>			every 'nrsec' seconds. The default value is 5 seconds.

I guess that a `write()` will only write the data blocks and metadata
in the cache without a following `fsync()`. The reason is that `fsync()`
syncs both the data and metatdata to the disk. If `write()` ensures a sync of
metadata, then the sync of metadata by `fsync()` does not make sence.

- [Ext4 Filesystem]: https://www.kernel.org/doc/Documentation/filesystems/ext4.txt
