---
layout: post
author: energyc0
title: "Choosing repositories for package manager(apt) on Debian"
---

# From what I started
I installed a second Linux system(Debian) on my computer in a strange way. I downloaded .ISO file from the Internet and created a new partition on my disk. I tryed to manually download it on my partition with **dd** command.
It worked and I could select my new Debian system in BIOS but the problem was that the system installer searched for boot files in removable media. That strange issue I actually solved by copying .ISO file to my USB.
I chose Debian in BIOS, plugged in the USB and started installation.

I had to wait a lot of time and it seemed that the installation had stucked. So I decided to do something stupid and turned off my computer. After booting again I started Debian and it looked good, but it wasn't. 

I tried to install needed packages but **sudo** wasn't working.

# Trying to fix "sudo"
When I tried to type something like this ``` sudo apt install vim ```, it answered me ``` Command 'sudo' not found ```. I had found that Debian didn't install it initially, so I logged in with root account and tried ``` apt install vim ``` and it asked me to mount my local repository: 

```E: Please use apt-cdrom to make this CD-ROM recognized by APT.```

I checked ```/etc/apt/sources.list``` and it looked like:

```
deb file:/media/cdrom/ stable main
```

# "chroot" helps

I booted from my Ubuntu and mounted Debian on ```/media/debian```. I had to use ```chroot```. This command creates a new shell environment with chosen ```/``` directory. Thanks to this command we can execute other commands from chosen root directory.

For example, I created a new directories and mounted system directories there:\
```sudo mkdir /new_env```\
```cd /new_env```\
```sudo mkdir dev bin lib lib64```\
```sudo mount --bind /lib ./lib```\
```sudo mount --bind /lib64 ./lib64```\
```sudo mount --bind /bin./bin```\
```sudo mount --bind /dev ./dev```\
and executed from ```sudo chroot /new_env/ ls```\
and its output was something like this:

```bin  dev  lib  lib64```

Also we can use an option ```--userspec=USER:GROUP``` to choose user and group. For example, \
```sudo chroot /media/debian whoami``` outputs ```root```,\
while ```sudo chroot --userspec=energyc0 /media/debian whoami``` outputs ```energyc0```.

Back to my problem, I executed ```sudo chroot /media/debian``` 
and this command just helped me to google answers to my question during solving problems on Debian :)

# A little about "mount" 
Also I understood two crucial options in ``mount`` - ``--bind`` and ``--rbind``. These options remount parts of the hierarchy somewhere else. So when I typed:\
```sudo mount --bind /dev /home/my_dir/dev```\
I could access ``/dev`` directory from two places: from ``/dev`` and from ``/home/my_dir/dev``. 

According to the docs: 

    Remount part of the file hierarchy somewhere else. The call is:

          mount --bind olddir newdir

       or by using this fstab entry:

          /olddir /newdir none bind

       After this call the same contents are accessible in two places.

       It is important to understand that "bind" does not create any second-class or special node in the kernel VFS. The
       "bind" is just another operation to attach a filesystem. There is nowhere stored information that the filesystem has
       been attached by a "bind" operation. The olddir and newdir are independent and the olddir may be unmounted.
And if you want to mount all submounts you need to use ``--rbind``.

If I didn't mount ``/dev`` directory to my Debian root directory I got a error

# Solving apt-manager problem
I found in the Internet(https://deb.debian.org/) debian repositories, write them down in the ``sources.list`` and run ``apt update``. That is the way I fixed my apt repositories. Then I downloaded ``sudo`` packages. But there was a problem - speed.
I needed to select optimal repository for downloading packages. I downloaded ``netselect-apt`` package and just run ``
