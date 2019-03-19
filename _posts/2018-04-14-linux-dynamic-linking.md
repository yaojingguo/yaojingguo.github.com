---
layout: post
title: "Linux Dynamic Linking"
description: ""
category:
tags: [linux, compiler]
---

`15.7 Dynamic Linking` of _Programming Language Pragmatics, Fourth Edition_ gives
a high level description of linux dynamic linking. It is great treatment of the topic
without involving much low-level details. One note is that the `push GOT_main` mains `GOT[1]` which can also be referred as `GOT+8`.

[Linux Internals ~ Dynamic Linking Wizardry](https://0x00sec.org/t/linux-internals-dynamic-linking-wizardry/1082)
gives a good description of linux dynamic linking. But the post talks about i386 platform.
Here I describe some parts of the post applied to x86-64 platform. My post can
be use a complementary reading to the original post when you want to play along on x86-64 platform.
.


name.c:
```
int main(void) {
  puts("Hey!");
  return 0;
}
```

`gcc -o name name.c` produces:
```
name.c: In function ‘main’:
name.c:2:3: warning: implicit declaration of function ‘puts’ [-Wimplicit-function-declaration]
   puts("Hey!");
   ^~~~
```

`./name` produces:
```
Hey!
```

`callq  400400 <puts@plt>` jumps to the first instruct of puts's PLT entry whose address is `0x400400`. A part of `objdump- D name`:
```
0000000000400507 <main>:
  400507: 55                    push   %rbp
  400508: 48 89 e5              mov    %rsp,%rbp
  40050b: bf a4 05 40 00        mov    $0x4005a4,%edi
  400510: e8 eb fe ff ff        callq  400400 <puts@plt>
  400515: b8 00 00 00 00        mov    $0x0,%eax
  40051a: 5d                    pop    %rbp
  40051b: c3                    retq
  40051c: 0f 1f 40 00           nopl   0x0(%rax)
```

The first instruction of puts PLT entry jumps to `GOT[3]` whose address is `0x601008`. A part of `objdump- D name`:
```
Disassembly of section .plt:

00000000004003f0 <puts@plt-0x10>:
  4003f0: ff 35 12 0c 20 00     pushq  0x200c12(%rip)        # 601008 <_GLOBAL_OFFSET_TABLE_+0x8>
  4003f6: ff 25 14 0c 20 00     jmpq   *0x200c14(%rip)        # 601010 <_GLOBAL_OFFSET_TABLE_+0x10>
  4003fc: 0f 1f 40 00           nopl   0x0(%rax)

0000000000400400 <puts@plt>:
  400400: ff 25 12 0c 20 00     jmpq   *0x200c12(%rip)        # 601018 <_GLOBAL_OFFSET_TABLE_+0x18>
  400406: 68 00 00 00 00        pushq  $0x0
  40040b: e9 e0 ff ff ff        jmpq   4003f0 <_init+0x28>
```

`readelf -r name` produces:
```
Relocation section '.rela.dyn' at offset 0x380 contains 1 entries:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
000000600ff8  000300000006 R_X86_64_GLOB_DAT 0000000000000000 __gmon_start__ + 0

Relocation section '.rela.plt' at offset 0x398 contains 2 entries:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
000000601018  000100000007 R_X86_64_JUMP_SLO 0000000000000000 puts@GLIBC_2.2.5 + 0
000000601020  000200000007 R_X86_64_JUMP_SLO 0000000000000000 __libc_start_main@GLIBC_2.2.5 + 0
```

`objdump -R name` produces:
```
name:     file format elf64-x86-64

DYNAMIC RELOCATION RECORDS
OFFSET           TYPE              VALUE
0000000000600ff8 R_X86_64_GLOB_DAT  __gmon_start__
0000000000601018 R_X86_64_JUMP_SLOT  puts@GLIBC_2.2.5
0000000000601020 R_X86_64_JUMP_SLOT  __libc_start_main@GLIBC_2.2.5
```

`puts@GLIBC_2.2.5` is puts's GOT entry. Its type is `R_X86_64_JUMP_SLOT`.

`06 04 40` starting from `601008` is `400406` in the little endian. So the first
instruction of puts' PLT entry jumps to the second instruction of puts' PLT entry.
```
Disassembly of section .got.plt:

0000000000601000 <_GLOBAL_OFFSET_TABLE_>:
  601000: 28 0e                 sub    %cl,(%rsi)
  601002: 60                    (bad)
  ...
  601017: 00 06                 add    %al,(%rsi)
  601019: 04 40                 add    $0x40,%al
  60101b: 00 00                 add    %al,(%rax)
  60101d: 00 00                 add    %al,(%rax)
  60101f: 00 16                 add    %dl,(%rsi)
  601021: 04 40                 add    $0x40,%al
  601023: 00 00                 add    %al,(%rax)
  601025: 00 00                 add    %al,(%rax)
  ...
```
