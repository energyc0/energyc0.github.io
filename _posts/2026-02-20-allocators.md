---
layout: default
title: Heap and allocators
permalink: /heap-and-allocators/
author: energyc0
---
# What this post is about
+ Process address space
+ **brk()** and **sbrk()**
+ Examples of usage of allocators

Hello everyone, because of the programming class that took place last Saturday, I decided to do a little research and understand where and how allocators are used. I read an interesting chapter in Michael Kerrisk's book Linux API. A comprehensive guide", which is called "Memory Allocation". To understand what I'm going to talk about next, you need this picture (it's below).
It shows the address space of the process, which is divided into segments: .text (text), .data (initialized data), .bss (uninitialized data), then comes the pile, which grows upwards. There is free space above the heap, and above it is the process stack, which grows downwards (in the x86 architecture). It turns out that the stack and the heap are growing towards each other. Above the stack is a memory location where various program arguments, as well as environment variables, are placed.
So, when a process "starts living", then its heap is empty. Using the brk() and sbrk() system calls, you can move the heap size up or down. The brk() system call takes a pointer to the memory that is above the &end pointer (see the picture), that is, above the .bss segment. At the same time, the size of the allocated memory is rounded up to the page size. The sbrk() function is already a library wrapper over brk(), which can increase or decrease the heap size. It returns a pointer to the previous allocated block, that is, if we call sbrk(size), we will get a pointer to the beginning of the allocated block of size. Accordingly, calling sbrk(0) simply returns a pointer to the top of the heap (the behavior is defined). Thus, the malloc family of functions is just a wrapper over brk system calls. An allocator is implemented under the hood, which controls the number of free blocks, gives them away if necessary, and also requests the required amount of memory using brk().
You can also allocate memory on the stack. The alloca() system call is used for this purpose.  It cannot be used when passing arguments, otherwise the stack frame will float:
``c
func(x, alloca(size), z); /* This is not allowed! */
```
But you can do it this way:
```c
void *y;
y = alloca(size);
func(x, y, z);
```
Using alloca() is convenient because it is faster. Also, you do not need to monitor the release of memory, because it will be released by itself when the function that called alloca() returns, since the base register will return to its previous state, that is, the stack will unwind.
There are also memalign() and posix_memalign() system calls that allow you to allocate memory with alignment. You can also "control" the operation of malloc functions using mallopt() and mallinfo().
Usage examples:
Render/Game engines — https://developer.unigine.com/ru/docs/future/principles/allocator /, https://github.com/id-Software/DOOM-3 . As I understand it, they allow for efficient memory usage when working with the graphics API, allocating all sorts of data structures for the graphics pipeline.
OS Kernels — https://github.com/torvalds/linux/blob/master/mm/slab.h , https://github.com/freebsd/freebsd-src/blob/main/sys/vm/uma_core.c
DBMS — https://reindexer.io/reindexer-docs /, https://www.sqlite.org/src/doc/trunk/README.md
Highly loaded servers - https://github.com/nginx/nginx/blob/master/src/core/ngx_palloc.c
Things for all kinds of difficult calculations (sparse matrices, arena allocators) - https://github.com/trilinos/Trilinos
Media Players and video/audio processing — https://github.com/Ffmpeg/Ffmpeg , https://github.com/videolan/vlc
Translators — https://github.com/llvm/llvm-project , https://github.com/python/cpython
Financial systems - https://github.com/OpenHFT/Chronicle-Queue
Embedded systems — https://github.com/FreeRTOS/FreeRTOS , https://github.com/zephyrproject-rtos/zephyr