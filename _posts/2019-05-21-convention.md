---
layout: post
title: "Go"
description: ""
category:
tags: [convention]
---
{% include JB/setup %}

# Coding Convention
## Comment
Linux kernel comment (most case, ther is no hard rule):
1. Start with a uppercase letter.
1. Not to end imperative sentences with a period.
1. End the first sentence with a period if multiple sentences.
1. Not to use third-person singular form for the beginning verb.
1. TODO starts with `// TODO(yaojingguo@gmail.com): `. The following sentence should begin with a uppercase letter.

Golang:
1. Start with function name.
1. Always end with a period.

I follow the Linux kernel way.

## Logging
Linux Kernel:
1. Start with a uppercase letter.
1. Not to end imperative sentences with a period.
1. Not to use third-person singular form for the beginning verb.

cockroachDB:
Start with a letter case letter unless the raw word is capitalised.

Imperative form for some short-duration thing to happen.
- `drop schema change mutation: %+v`
- `begin sampling phase of job %s`
- `drop index (%d, %d) at row: %d, span:`

Present continuous tense for for some long-duration thing to happen. `is` is
omitted. If the subject is the running system itself, omit subject.
- `pushing expired txn %s`
- `planner acquiring lease on table '%s'`
- `cleaning up bootstrap addresses`
- `not seeking (key=%s); existing descriptor %s`
- `pgwire writing TEXT datum of type: %T, %#v`

Passive form for something happed in the past. `is` is omitted.
- `optimizer disabled`
- `outbox flushed`
- `node label %s not found`
- `table %d version %d not found`

Past tense for some thing happened in the past. If the subject is the running
system itself, omit subject.
- `found uncommitted table %d`
- `added table '%s' to table collection`
- `failed to read gossip bootstrap info: %s`
- `not pushed; returning to caller`
- `found new resolvers from storage; signaling bootstrap`
- `didn't insert all buffered rows and yet no error was reported. `
- `failed to create config data dir %v`
- `Inbox reader arrived`

The first word of a single sentence is not capitalized.
- `ranges are not adjacent; %s != %s`

`error doing something` form
- `error setting up client session: %v`

## Print
If Print is used as logging, follow logging convention. Otherwise, starts with
a uppercase letter.

## Hexadecimal
CSAPP convention:
1. Use '0x' prefix.
2. Use uppercase letters for `A-F`.

## hello code
The `hello.c` from K&R:
```C
#include <stdio.h>

main()
{
    printf("hello, world\n");
}
```

## Prefix
If a package name is use as prefix for a logging message or commit message
subject line, it is used without any conversion.

## References
- [Linux kernel coding style](https://www.kernel.org/doc/html/v4.10/process/coding-style.html)
