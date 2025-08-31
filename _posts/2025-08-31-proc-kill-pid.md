---
title: "0 and 1 processes in Linux"
author: "energyc0"
layout: post
permalink: /0-1-processes/
---

## Introduction
I have been reading source code for **screen** program in Linux and I stumbled upon code:
```c
void Kill(pid_t pid, int sig)
{
	if (pid < 2)
		return;
	(void)kill(pid, sig);
}
```
That raised some questions.

## 0 process
Process with 0 pid is called **swapper** process or **idle task**. It is a special process that starts the Linux kernel and then becomes a waiting process that helps in planning tasks. This process is a part of kernel that is why it is not seen with ``ps`` .

## 1 process
Process with 1 pid is a special process called **init** that manipulates services (deamons) in Linux. It starts right after 0 process. **init** process have different implementations on different systems (systemd, SysVinit, launched, ruinit, OpenRC and etc.)

## About kill()
```c
int kill(pid_t pid, int sig);
```
    The  kill()  system  call  can be used to send any  signal to any process
       group or process.

       If pid is positive, then signal sig is sent to the process with  the  ID
       specified by pid.

       If  pid equals 0, then sig is sent to every process in the process group
       of the calling process.

       If pid equals -1, then sig is sent to every process for which the  call‐
       ing process has permission to send signals, except for process 1 (init),
       but see below.

       If pid is less than -1, then sig is sent to every process in the process
       group whose ID is -pid.

So 
```c
#include <stdio.h>
#include <signal.h>
#include <errno.h>
#include <string.h>

int main(){
    if(kill(1, SIGKILL)){
        perror(strerror(errno));
        return 1;
    }
    return 0;
}
```
prints 
```
Operation not permitted: Operation not permitted
```
And in theory you can't send any signals to the 0 process.