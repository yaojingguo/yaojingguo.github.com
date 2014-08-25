---
layout: post
title: "Rsync Rolling Checksum"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Tridgell describes it in his paper [Rolling checksum](http://rsync.samba.org/tech_report/node3.html). librsync implements it in [rollsum.h](https://github.com/librsync/librsync/blob/master/rollsum.h) and [rollsum.c](https://github.com/librsync/librsync/blob/master/rollsum.c).
The difference between the paper and librsync is the adding of ROLLSUM_CHAR_OFFSET in librsync.
This post [weak checksum question][1] explains the meaning of ROLLSUM_CHAR_OFFSET:

>librsync adds an offset to each byte before it is fed into the Fletcher 
>checksum.

And I agree with the post's author that `ROLLSUM_CHAR_OFFSET` does not have any impact 
on the rolling checksum algorithm.  If `ROLLSUM_CHAR_OFFSET` is 0, the paper and 
the librsync are the same. Let's use `o` to represent ROLLSUM_CHAR_OFFSET.  
`s1` and `s2` are defineds as follows in librsync:

```
s1 = (x[1]+o) + (x[2]+o) + ... + (x[n]+o)
s2 = n*(x[1]+o) + (n-1)*(x[2]+o) + ... + (x[n]+o) 
```

[bup](https://github.com/bup/bup) also implements a rolling checksum algorithm 
[bupsplit.c](https://github.com/bup/bup/blob/master/lib/bup/bupsplit.c) which is similar to librsync.  
Unlike librsync which has `RollsumRollin`, `RollsumRollout` and `RollsumRotate`	 for 
update `s1` and `s2`, bup only uses one function `rollsum_add`. `s1` and `s2` are defined 
as:

```
s1 = (x[1]+o) + (x[2]+o) + ... + (x[n]+o)
s2 = n*x[1] + (n-1)*x[2] + ... + x[n] + n*(n-1)*o
```

[1]: http://lists.samba.org/archive/rsync/2002-November/004176.html
