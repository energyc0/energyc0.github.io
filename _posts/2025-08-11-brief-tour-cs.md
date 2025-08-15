---
layout: post
title: A brief tour of computer systems
permalink: /cs-tour/
author: energyc0
---

# What this post is about
+ Compilation process
+ Executables, object files
+ Processes and threads
+ Memory caching
+ SIMD

# Preface
This post is written based on the book "Computer Systems: A Programmer's Perspective, 3/E" by Randal E. Bryant and David R. O'Hallaron, Carnegie Mellon University

# Computer system

A computer system consists of hardware and software. Specific implementations of systems change over time but underlying concepts not. So it it really important to know basics.

Let's start with a simple example:

```c
#include <stdio.h>

int main() {
  printf("hello, world\n");
  return 0;
}
```
When you started your programming journey you wrote something like this, compiled, executed and saw _"hello, world"_ on your display. But what's behind this 'black box' of compiling and executing? How does computer understand your symbols? How does the program displays this title on your screen? 

## Bits and bytes

First of all, you wrote code. It is represented as symbols of ASCII table that are just bytes. You compiled your code and made an executable that represented as bytes. But how does computer understand how to interpret those bytes? Depends on the contex. Every file in your system consists of bytes: texts, folders, images, videos, games etc. This is a fundamental idea.

### Translation process

We can divide translation process into phases:
- **Preprocessing phase** - preprocessor modifies the original C program according to the directives (#define, #include, #ifdef etc). The result is another program typically with **.i** suffix.
- **Compilation phase** - compiler translates the preprocessed program into assembly language (typically generates file with **.s** suffix).
- **Assembly phase** - assembler translates the the **.s** file into machine instructions and store them into **.o** object known as **relocatable object program** (this is just a binary file).
- **Linking phase** - linker handles merging several objects into one. For example, out program uses *printf()* function that contains in precompiled object file called *printf.o*. The result is an **executable object** that is ready to be executed by the system.

### Process execution
When you open your terminal you are greeted by **shell** and prompted to enter commands. Shell is a program that accepts commands from a user and executes them. Shell creates a **process** and transfers control to it.

**Process** is some sort of abstraction in operating system for a running program. Operating system can run multiple processes concurrenty. It means that only a few processor time is given to a process. Traditional systems could only execute one program at a time, while *multicore* systems can execute several processes simultaneously. Operating system performs **context switching** by saving the context of the current process when deciding to pay attention to another process and starts to execute code of another process. 

Shell executes user's commands by invoking **system calls**. System calls are the way applications can communicate with operating system. System calls are managed by **kernel**. Kernel is a program that controls other programs and helps them to get access to hardware. We can say that operating system is a kernel and all the instruments needed by the kernel to work well.

Processes have **threads**. Threads are execution units each running in the context of the current process and sharing its code and data.

Every process is provided its own **virtual memory**. Virtual memory is an abstraction. It makes every process seem to be executed lonely in the system. It is very important aspect for security.

## Memory
When process is executed it is placed in memory. Original program is placed on the disk, then it is placed on the RAM, then some part of the data is placed on the processor registers. There is one fundamental idea: the faster access the smaller amount of memory. Speed matters, so there is **cache** hierarchy:
1.  **Registers**(L0)  
2.  **Cache**/SRAM(L1-L3)
3.  **Main memory**/DRAM(L4)
4.  **Local secondary disks**/local disks(L5)
5.  **Remote secondary storage**/Web servers(L6)

## SIMD
SIMD(Single-instruction, multiple-data) parallelism is used for parrallel calculations. For example, operations with floating numbers or image processing. Processor can execute multiple instructions at a time. It is so called *instruction-level parallelism*.
