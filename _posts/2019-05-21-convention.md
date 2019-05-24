---
layout: post
title: "Convention"
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

# Mail
For the line width, there is no hard rule for linux kernel mail list.

Linus:
- [Linux 4.19-rc4 released, an apology, and a maintainership note](https://lkml.org/lkml/2018/9/16/167)
```
having screwed up my calendar, but honestly, I was mostly hopeful that
I
```
```
>>> len("having screwed up my calendar, but honestly, I was mostly hopeful that")
70
```

Width is at most 72.

- [Re: Availability of kdb](https://lkml.org/lkml/2000/9/6/65)
```
>>> len("sources. At the meaning of things. Without a debugger, you basically have")
73
```
```
of reactions to that: you start being careful, or you start whining about
a kernel debugger.
```
```
>>> len("of reactions to that: you start being careful, or you start whining about")
73
```
- [Re: [BREAKAGE] Since 4.18, kernel sets SB_I_NODEV implicitly on userns mounts, breaking systemd-nspawn](https://lkml.org/lkml/2018/12/22/221)
```
>>> len("The fact that you tried to several times claim it was buggy user space")
70
```

G-H:
- [Re: The most insane proposal in regard to the Linux kernel development share](https://lkml.org/lkml/2016/4/7/423)
```
>>> len("operating system's drivers _because_ of our development model.  You wish")
72
```

https://news.ycombinator.com/item?id=7994190
> Many plaintext clients default to 72 to allow room for multiple levels of quoting.

http://kb.mozillazine.org/Plain_text_e-mail_-_Thunderbird says:
> You can use mailnews.wraplength to change the line length for messages you compose (defaults to 72 characters), mail.compose.wrap_to_window_width to wrap to the window width when composing a message (defaults to false) and mail.wrap_long_lines to control the wrapping of long lines (defaults to true).

I use 72 for line width.

Wrap effect of LKML web page:
https://lkml.org/lkml/2019/5/20/1845

# Tense in Issue and PR
# Tense
Us tense as normal English writing.

RocksDB issues reported by Peter Mattis：
- [storage: unable to down-replicate from stores not present in StorePool #34187](https://github.com/cockroachdb/cockroach/issues/34187)
- [gossip: gossiped infos should always take precedence over a previous incarnation's #30031](https://github.com/cockroachdb/cockroach/issues/30031)
- [gossip: local infostore can diverge from remote infostore at startup #29992](https://github.com/cockroachdb/cockroach/issues/29992)

RocksDB issues reported by Ben Darnell：
- [server: Improve Setrlimit error handling #37685](https://github.com/cockroachdb/cockroach/issues/37685)

[RocksDB issues reported by Ben Darnell](https://github.com/facebook/rocksdb/issues?utf8=%E2%9C%93&q=+is%3Aissue+author%3Abdarnell+)
Past sense examples:
- [Data corruption in 5.9 (possibly related to range deletion) #3169](https://github.com/facebook/rocksdb/issues/3169)
- [Clarify warning on `use_fsync` #3410](https://github.com/facebook/rocksdb/issues/3410)

Present perfect sense:
- [Default value for `max_manifest_file_size` #3851](https://github.com/facebook/rocksdb/issues/3851)

[RocksDB issues reported by Peter Mattis](https://github.com/golang/go/issues?utf8=%E2%9C%93&q=+is%3Aissue+author%3Apetermattis):
- [DeleteRange related bug? #1778](https://github.com/facebook/rocksdb/issues/1778)
- [data corruption issue with RocksDB 4.9, maybe fixed by 4.11.2 #1370](https://github.com/facebook/rocksdb/issues/1370)
- [Range tombstones can create excessively large compactions #3977](https://github.com/facebook/rocksdb/issues/3977)

[Golang issues reported by Peter Mattis](https://github.com/golang/go/issues?utf8=%E2%9C%93&q=+is%3Aissue+author%3Apetermattis):
- [runtime: network blips during concurrent GC #20457](https://github.com/golang/go/issues/20457)
- [runtime: gobytes() initializes allocated memory unnecessarily #23634](https://github.com/golang/go/issues/23634)

# Capitalization
`sample-acmlarge.pdf` in [acmart-master.zip](https://www.acm.org/binaries/content/assets/publications/consolidated-tex-template/acmart-master.zip) says:
> The title of your work should use capital letters appropriately - https://capitalizemytitle.com/ has useful rules for
capitalization.

- [Rules for Capitalization in Titles of Articles](https://grammar.yourdictionary.com/capitalization/rules-for-capitalization-in-titles.html)
- [https://capitalizemytitle.com/](https://capitalizemytitle.com/)
