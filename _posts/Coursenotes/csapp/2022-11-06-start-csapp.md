---
layout: post
title: "[CSAPP] Chapter 3: Machine Level Representation"
date: 2022-11-06 00:00:00 +0800
categories: [Course Notes, CSAPP]
tags: [computer architecture]
---
# 1. Machine code
Generate assembly code mstore.s:
```
$ gcc -Og -S mstore.c
```

Generate an object-code file mstore.o:
```
$ gcc -Og -c mstore.c
```

The original 8086 had 16-bit registers, from %ax through %bp; with the extension to IA32, these registers were expanded to 32-bit registers, as labeled %eax through %ebp. For x86-64, they begin with %r.
<img src="/assets/img/x86_64_integer_registers.png" alt="integer_register" width="200"/>

