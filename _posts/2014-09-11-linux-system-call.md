---
layout: post
title: "Linux System Call"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Add a Syscall

I am using Ubuntu Server 14.04 64 bit. Get the source and build dependencies: 


```
apt-get source linux-image-$(uname -r)
sudo apt-get build-dep linux-image-$(uname -r)
```

### Define the Syscall
hello/hello.c

```C
#include <linux/kernel.h>
asmlinkage long sys_hello(void)
{
  printk("Hello world\n");
  return 0;
}
```
hello/Makefile

```obj-y := hello.o```

Makefile:

```core-y          += kernel/ mm/ fs/ ipc/ security/ crypto/ block/ hello/```

arch/x86/syscalls/syscall_64.tbl
```
314    common    hello    sys_hello
```

include/linux/syscalls.h
```
asmlinkage long sys_hello(void);
```

### Install the Image
```
make
make install
reboot
```

### Use the Syscall
$HOME/sysHello.c

```C
#include <stdio.h>
#include <linux/kernel.h>
#include <sys/syscall.h>
#include <unistd.h>

int main()
{
  long int rtn = syscall(314);
  printf("System call sys_hello return %ld\n", rtn);
}
```

```
gcc sysHello.c
./a.out
```

`tail /var/log/syslog` should show:

```
...
Aug 23 05:53:26 userver kernel: [   79.473407] Hello world
```

### References:
LWN:
- [Anatomy of a system call, part 1](http://lwn.net/Articles/604287/)
- [Anatomy of a system call, part 2](http://lwn.net/Articles/604515/)
