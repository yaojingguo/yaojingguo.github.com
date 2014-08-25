---
layout: post
title: "Windows Setup"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Cgywin Git

Let git show English message and handle Chinese pathname correctly.
Open Cygwin, select `option->text->locale character`,  set` zh_CN, GBK`.
Add `export LC_MESSAGES="en_US.UTF-8"` to .bashrc.

For the meaning of LC_MESSAGES, refer to:

- [Why “LANG=C”? (not D or E or F)](http://superuser.com/questions/219945/why-lang-c-not-d-or-e-or-f)
- [C library function - setlocale()](http://www.tutorialspoint.com/c_standard_library/c_function_setlocale.htm)
- `man 3 setlocale`
