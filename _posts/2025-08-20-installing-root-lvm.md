---
layout: post
author: energyc0
permalink: /root-on-lvm/
title: "How to install root directory on logical volume"
---
# Starting
I had to download linux system on my laptop and I chose Debian. I had been playing around LVM on my computer so I decided to try to install Debian on LVM.

I downloaded .iso file on USB, plug it into my laptop and started installation. Everything was fine but after reboot everything I saw was a blinking cursor at the beginning of the black screen. I turned off the laptop and tried to boot from another kernel and everything worked. I started to dig into it...

# nomodeset, splash, quiet
I read in the internet that new linux systems set videomode in kernel. So all the video-drivers are in kernel instead of X-server. It makes possible to show beautiful images during system boot. But not all the machines support this and it leads to errors (black screen and lags). So I needed to set a **nomodeset** option in **/boot/grub/grub.cfg** in **/boot/vmlinuz\*** line. I could also edit my config file in grub menu by pressing 'e'. I entered the system. The problem was that after executing **grub-update** all the options would be restored. I found out about **GRUB_CMDLINE_LINUX_DEFAULT** and **GRUB_CMDLINE_LINUX** variables. I needed to set them in the **/etc/default/grub** file. 

**GRUB_CMDLINE_LINUX_DEFAULT** sets options in **/boot/vmlinuz\*** line for normal boot.
**GRUB_CMDLINE_LINUX** sets options in **/boot/vmlinuz\*** line for recovery boot.

So I changed them to:
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet nomodeset splash"
GRUB_CMDLINE_LINUX=""
```

Also I added **splash** and **quiet** options.

**splash** option makes it possible to show boot images like this:

![Ubuntu splash screen](/assets/ubuntu-splash-screen.png)

**quiet** option supresses verbose output about starting the system.

# Setting root directory
Downloading my system I made three partitions:
1. **/boot** 1GB
2. LVM partition 324GB
3. Temporary root partition 16GB

After it I created **/dev/vg1/lv_home** logical volume for **/home** and **/dev/vg1/lv_root** logical volume for **/root**. I mounted **lv_home** and copied **/home** there. After it I added mount point in the **/etc/fstab**. Then I mounted **lv_root** and made a copy of the system:
```bash
rsync -aHAX / /mnt/new-root -v --one-file-system
```
then I mounted **/dev**, **/run**, **/proc** and **/sys** with **--bind** option there.
```bash
for i in /proc/ /sys/ /dev/ /run/; do mount --bind $i /mnt/new-root/$i; done
chroot /mnt/new-root
```
But the problem was that my kernel needed to be recompiled for LVM to be loaded via grub.
I had **initrd** files in the **/boot** directory (I will write about initrd in the future), so I ran:
```bash
update-grub
cd /boot
for i in `ls initrd.img-*`; do dracut -v $i `echo $i | sed s/initrd.img-//g` --force; done
```
I used this website - <https://casp.ru/?p=55>.

But the problem wasn't solved :(. I rebooted and ran
```
lsblk
```
And my **/** wasn't on **lv_root**. I realized that I needed to mount **/boot** directory because all the newly created grub configuration files was stored on **lv_root**. I mounted **/boot** directory
```bash
mount --bind /boot /mnt/new-root/boot
```
and did the same thing. After reboot everything worked!

# Troubleshooting
I decided to delete that temporary partion and left only /boot and LVM partition. After reboot I ran into problems... Grub didn't know how to find root partition - <https://unix.stackexchange.com/questions/329926/grub-starts-in-command-line-after-reboot>. So I ran:
```bash
ls
```
found lvm partition and
```bash
set prefix=(lvm/vg1-lv_root)/boot/grub
set root=(hd1,2)
insmod linux
insmod normal
normal
boot
```

After it I joined my system and realized that I didn't update **/boot** partition, so I ran (because /boot partition was mounted on **/boot/efi**):
```bash
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=debian --recheck --debug /dev/sdb
update-grub
```
and everything worked!