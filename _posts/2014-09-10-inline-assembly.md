---
layout: post
title: "Inline Assembly"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Resources

- [Brennan's Guide to Inline Assembly](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html)
- [Inline assembly for x86 in Linux](http://www.ibm.com/developerworks/linux/library/l-ia/index.html)
- [GCC-Inline-Assembly-HOWTO](http://www.ibiblio.org/gferg/ldp/GCC-Inline-Assembly-HOWTO.html)

Related parts in GCC manual:

- [Extended Assembly](https://gcc.gnu.org/onlinedocs/gcc-4.8.3/gcc/Extended-Asm.html)
- [Explicit Reg Vars](https://gcc.gnu.org/onlinedocs/gcc-4.8.3/gcc/Explicit-Reg-Vars.html#Explicit-Reg-Vars)
- [Constraints](https://gcc.gnu.org/onlinedocs/gcc-4.8.3/gcc/Constraints.html)

## Tips

`lea` respests AT&T syntax

```
asm ("leal (%1,%1,4), %0"
     : "=r" (x)   /* %0 allocated for it */
     : "0" (x));  /* %1 allocated for it. And use the same register as %0*/
```
Multiply x by 5. 


```
asm volatile("lock; xchgl %0, %1" :
             "+m" (*addr) /* %0 allocated for it */, "=a" (result) /* %1 allocated for it */:
             "1" (newval) /* Use the same register as %1. And no %n token is allocated for it*/:
             "cc");
```
