---
layout: post
title: "Coding Tool Setup"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Cscope
Create cscope.files for linux kernel source code:
    
    rm cscope.*
    find  .                                                                \
    -path "./arch/*" ! -path "./arch/x86*" -prune -o               \
    -path "./Documentation*" -prune -o                                 \
    -path "./scripts*" -prune -o                                       \
    -path "./drivers*" -prune -o                                       \
          -name "*.[chxsS]" -print >./cscope.files
    cscope -b -q -k
