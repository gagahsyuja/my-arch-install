# My Arch Install
This is my personal Linux installation. I'll be using `Arch Linux` and `Wayland` for it's display server.

Assuming you've booted the live environment, got the internet working, and did some partitioning, you can continue following this guide. If not, you can do so by reading the [Wiki](https://wiki.archlinux.org/title/installation_guide) beforehand.

## Update the system clock
Use `timedatectl` to ensure the system clock is accurate:
```
# timedatectl set-ntp true
```
## Mount the file systems
Use `lsblk` to list all the disks and its partitions:
```
# lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 223.6G  0 disk 
├─sda1   8:1    0   512M  0 part
├─sda2   8:2    0   7.7G  0 part
└─sda3   8:3    0  92.5G  0 part
```
Mount the root volume to `/mnt`. For example `/dev/sda3` will be used:
```
# mount /dev/sda3 /mnt
```
Mount the boot partition. But before doing so, you have to create the `boot` directory inside the `/mnt` first:
```
# mkdir /mnt/boot
```
Then you can proceed mounting the boot partition to `/mnt/boot`. For example `/dev/sda1` will be used:
```
# mount /dev/sda1 /mnt/boot
```
If you create a `swap` volume, you can also enable it. For example `/dev/sda2` will be used:
```
# swapon /dev/sda2
```
## Install essential packages
Use the `pacstrap` script to install the `base` and `base-devel` package, Linux kernel and its headers, firmware for common hardware, microcode for our processors, and also a text editor. I'll be using `linux-lts` kernel, `intel-ucode` because my processors are Intel, and `vim` as my editor:
```
# pacstrap /mnt base base-devel linux-lts linux-lts-headers linux-firmware intel-ucode vim
```
## Generate an fstab file
```
# genfstab -U >> /mnt/etc/fstab
```
You can see the generated files using `cat`:
```
# cat /mnt/etc/fstab
```
## Change root into the new system
```
# arch-chroot /mnt
```
## Set the time zone:
Replace `Asia` and `Jakarta` with your own location.
```
# ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
Run `hwclock` to generate `/etc/adjtime`:
```
# hwclock --systohc
```
## Localization
Edit `/etc/locale.gen` and uncomment `en_US.UTF-8 UTF-8`, then generate the locales by running:
```
# locale-gen
```
Set `LANG` variable:
```
# echo 'LANG=en_US.UTF-8' > /etc/locale.conf
```
## Network Configuration
Create the hostname file, I'll be using `T480` as my hostname:
```
# echo 'T480' > /etc/hostname
```
Edit the `/etc/hosts` file, and add the following:
```
127.0.0.1    localhost  
::1          localhost  
127.0.1.1    T480.localdomain	  T480
```
Make sure to replace `T480` with your very hostname.
## Root password
Set the root password:
```
# passwd
```
## User and groups
Add a new user, in this case `ssa`:
```
# useradd -mG wheel ssa
```
Set a password for `ssa`:
```
# passwd ssa
```
## Add user to sudoers:
Edit the sudoers using `visudo`:
```
EDITOR=vim visudo
```
To allow members of group `wheel` sudo access, uncomment this line:
```
%wheel ALL=(ALL:ALL) ALL
```
## Enable multilib
To enable multilib repository, uncomment the `[multilib]` section in `/etc/pacman.conf`:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
## Install the bootloader
I'll be using `systemd-boot` for my bootloader, you can also use this if your system is UEFI.
* Installation

    Previously we mount `/dev/sda1` as the boot volume. To install the bootloader:
    ```
    # bootctl --path=/boot install
    ```
* Adding loaders

    Create a new file for example `arch.conf`, then edit the file using `vim`:
    ```
    # vim /boot/loader/entries/arch.conf
    ```
    Previously we use `/dev/sda3` as our root volume, so edit accordingly:
    ```
    title   Arch Linux  
    linux   /vmlinuz-linux-lts
    initrd  /intel-ucode.img
    initrd  /initramfs-linux-lts.img  
    options root=/dev/sda3 rw
    ```
## Reboot
Before rebooting, make sure to install `networkmanager` because you will need them for internet connection:
```
# pacman -Syu networkmanager
```
Also enable `NetworkManager.service`:
```
# systemctl enable NetworkManager
```
Then you can safely reboot to your freshly installed Arch Linux, after exiting the chroot of course.

## [After reboot](./POST.md)
