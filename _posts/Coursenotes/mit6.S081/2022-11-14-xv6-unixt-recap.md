---
layout: post
title: "[mit6.S081] A Quick Recap On OS Basics"
date: 2022-11-14 17:00:00 +0800
categories: [Course Notes, MIT6.S081]
tags: [MIT6.s081, XV6, Operating System, Unix]
---

Good lecture references:
- [<span style="color:#3ababa">IIT Bombay, Lectures on OS</span>](https://www.cse.iitb.ac.in/~mythili/os/) and [<span style="color:#3ababa">Youtube</span>](https://www.youtube.com/watch?v=aCJ3YgoolHQ&list=PLDW872573QAb4bj0URobvQTD41IV6gRkx)

- [<span style="color:#3ababa">Operating Systems: Three Easy Pieces</span>](https://pages.cs.wisc.edu/~remzi/OSTEP/)

- [<span style="color:#3ababa">Blog</span>](https://blog.miigon.net/categories/mit6-s081/)

- [<span style="color:#3ababa">Zhihu</span>](https://www.zhihu.com/column/c_1502374640542023680)


## Processes
About Operating System (Virtualization->process+virtual memory + Concurrecncy + Persistence)
- middleware between user programs and system hardware
- shares a computer(CPU) among mutiple programs -> timeshares, scheduling
- manages and abstracts the low-level hardware  
- share the hardware/memory among mutiple programs
- provides controlled ways for programs to interact 
- OS goal: isolation + efficiency + convenience <br />


About Process Abstraction
- process is a running program
- process is composed of __PID__, __Memory Image__(code, data, stack, heap), __CPU context__(registers, current operands, pc, sp), __File descriptors__
- process controll block
    - PID
    - Process state
    - Pointers to other related processes
    - CPU context
    - Pointers to memory locations
    - Pointers to open files


About API
- functions available to write user programs; system calls are special API provided by OS
- system call is a function call into OS kernel that runs at a higher privilege level of the CPU
- POSIX API, a standard set of system calls, ensures program portability
- Libaries hid the details of invoking system calls, eg. C -> libc -> syscall (printf -> write) 
- process related syscall, fork(), exec(), exit(), wait() <br />

About fork()
- it's used to create a new process by making a copy of parent's memory image + parent's file descriptor table
- child process is added to process list and scheduled
- both parent and child start to execute


About wait()
- int wiat(int *status), returns the PID of an exited(or killed) child of the current process, and copies the exit status of the child to the address passted to wait. 
- wait() blocks in parent until child terminates; if the caller has no children, wait immediately returns -1; the parent doesn't care about the exit status of a child when pass a 0 address to wait.
- Terminated process exists as zombie, when a parent calls wait(), zombie child is cleaned up or "reaped" - 收割, 收获.
- If parent terminates before child, _init_ process adopts orphans and reaps it. <br />

About shell
- _init_ process is created after init of hardware
- _init_ spawns a shell like bash
- shell, read command -> fork a child - > exec -> wait for child -> read next command <br />


I/O redirection <br />
<p class="text-justify">
The close system call releases a file descriptor, making it free for resuse by a future open, pipe, or dup system call. An newly allocated file descriptor is always the lowest numbered unused descriptor of the current process. File descriptor and fork interact to make I/O redirection easy to implement. Fork copies the parent's file descriptor table along with its memory, so that the child starts with exactly the same open files as the parent. They system call exec replaces the calling process's memory but perserve its file table. This behavior allows the shell to implement I/O redirection by forking, reopening chosen file descriptors in the child, and the calling calling exec to run the new program.
</p>


Example 
```
/* Simplified version of cat < input.txt */

char *argv[2];

argv[0] = "cat";
argv[1] = 0;
if(fork() == 0) {
    close(0);
    open("input.txt", O_RDONLY);
    exec("cat", argv);
}
```


## Memory

## Concurrency

## I/O and Filesystems