
Arch Linux is the Linux Distro you use to flex your Linux knowledge.
What makes Arch so different is the fact that it is 100% customizable, and that compared to other **distros**, it lacks an installer so
>[!tip] You Have to set up and install the distribution yourself, with nothing but a command line...! :D

Let's see how to actually install and set up Arch, and learn some Linux along the road
*following [this tutorial](https://www.freecodecamp.org/news/how-to-install-arch-linux/#how-to-prepare-your-computer-for-installing-arch-linux:~:text=in%20great%20detail.-,How%20To%20Install%20Arch%20Linux,-Assuming%20that%20you)*
# Boot Arch

First step, burn the [Arch ISO](https://archlinux.org/download/) onto a USB and boot it!
see how to do above [[Bootable Drives#How to burn ISO on USB|here]].

Once you've booted into Arch you'll be greeted by a graphicless command line, where you need to configure everything **manually**.

***

# Setting Up Arch

*I'm writing this while actively learning how to install Arch, I haven't even started I'm already running across issues lmao...*
*First, Manually creating bootable USB doesn't work, need to use Rufus, secondly need to set Rufus to DD mode and not ISO otherwise Arch can't boot it seems*

*it's working now, set the filesystem to fat32 and not NTFS too*

## Making it look and feel nice

### Keyboard Mapping

By default arch assumes you're on a US Keyboard layout.
In case you want to change that:

All keyboard mappings in Linux are stored into `/usr/share/kbd/keymaps` directory as `.map.gz` files.

use the command 

```sh
ls /usr/share/kbd/keymaps
```
To view all the different categories of Keymaps
*Those directories are classifications e.g. `amiga` is an old 80s computer, `i386` is an intel microprocessor, and so on...*

If you genuinely want to waste time use
`ls /usr/share/kbd/keymaps/**/*.map.gz` To list **every single** keyboard mapping

Once you've found they keyboard mapping you want use `loadkey` to set it to the active one e.g. to set `/usr/share/kbd/keymaps/mac/all/mac-us.map.gz` use
`loadkey mac-us.map.gz`

**Honestly, just don't change it. 🦥**

### Console Font

Console fonts like keyboard mappings are stored in `/usr/share/kbd/consolefonts`

to set a particular font just use `setfont`
*Fuck it don't change the font no one cares, if you really want to, see [here](https://www.freecodecamp.org/news/how-to-install-arch-linux/#how-to-prepare-your-computer-for-installing-arch-linux:~:text=You%20can%20also%20change%20the%20console%20font)*

## Verifying [[Boot Modes|Boot Mode]]

>[!warning]
>This part is a little janky, the tutorial I'm following says arch needs to boot in UEFI mode and not BIOS, except I don't really understand why or how it chooses to boot into any of the two modes? also BIOS seems to work just as well?


to verify the boot mode execute 
````bash
ls /sys/firmware/efi/efivars
````

if the directory contains a bunch of files, then you're in **UEFI**.
if the directory `[...]/efi`does not exist you're in **BIOS**

You can also check that using 
```bash
cat /sys/firmware/efi/fw_platform_size
```
if it returns 64 or 32 then you're in **UEFI**

If you are on **BIOS**, toy around with the computer's boot options until you disable BIOS-mode (assuming you didn't buy your computer in the 1770s and actually have UEFI).


***
## Connecting to the **INTERNET**!

>[!info] **Arch** only contains the bare minimum packages to get the OS running, so a lot of other packages must be downloaded to actually make it usable. Thus why you need to connect this early on.

If you're using a wired network you should already be good to go, try to `ping` a random website to check. Otherwise,

Arch comes with the IWD package (iNet Wireless Daemon) which you can use to connect to a wireless network.
Run 
```bash
iwctl
```
to open an interactive prompt that should look like `[iwd]#`

from there you can run
```bash
device list
```
which will list all available wireless interpreters/available wireless adapters e.g. `wlan0`. we'll assume the device chosen is `wlan0` because fuck you that's why.
You can use the chosen device to scan for available networks using:
```bash
station wlan0 scan
```
*don't forget `wlan0` is the one I chose, put your own one there... Be original.*

>[!info] Silly Arch
>so here Arch is being a little silly as above command doesn't actually give you a list of wifi networks, it just scans them

To actually see the networks that were found with the scan, use
```bash
station wlan0 get-networks
```
*don't forget... `wlan0` ...*

once you've found the network `SSID` you want to connect to, let's say `Doom` is it's name
connect with
```bash
station wlan0 connect Doom
```
`iwctl` will prompt you for the password and you'll be connected.

## Updating system clock

Once you're connected to internet you can use **NTP** Network Time Protocol to sync system clocks over network
```bash
timedatectl set-ntp true
```
>[!quote] This command will start outputting some output and after a few seconds. If you do not see the command cursor show up again, try pressing Enter

***
## Partitioning Disks

>[!warning] This is the hard part of the installation as you can easily fuck it up so bad you lose everything: Your family, friends, house, files, self-respect... the list goes on so be careful

The first step to disk partitioning is actually figuring out what disks you have at your disposition
`fdisk -l`
**fdisk** lets you create and modify partitions on a disk... coincidentally enough.
the `-l` just tells it to list the disks
```bash
root@localhost:~# fdisk -l

Disk /dev/sda: 24.5 GiB, 26306674688 bytes, 51380224 sectors
Disk model: QEMU HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdb: 512 MiB, 536870912 bytes, 1048576 sectors
Disk model: QEMU HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```
>[!info] I'm doing this on a vps running ubuntu or something so it has nothing to do with arch linux, just giving sample output

see [[Linux File System#/dev#Block Devices|/dev block devices]] to understand the output

Found a disk you fancy?`/dev/sda`? list the partitions of that disk using
```bash
fdisk /dev/sda -l
```

>[!tip] 
>Here you can actually partition the disks using a **curses** based program (simple GUI) called `cfdisk` by calling
>```bash
>cfdisk /dev/sda
>```
>except that's not really cool nerdy linuxy enough so we'll do it manually!

To Correctly partition A Linux device, you need at least 2 partitions, the **EFI**, and the **Root** partitions.
Honestly, put in a third one called the swap because it's basically a really slow ram, but yk.

I am not explaining the EFI again, so check [[Boot Modes#UEFI]].

According to [official specs](https://wiki.archlinux.org/title/EFI_system_partition#:~:text=The%20partition%20size%20should%20provide%20adequate%20space%20for%20storing%20boot%20loaders%20and%20other%20files%20required%20for%20booting.), **the EFI should be at least 1Gib** EFI partition, basically 1Gb we're not that close to world apocalypse with that.

You can leave up to 4Gb for the swap partition as **Linux Swap**, don't really need to go above that, and you can assign the rest to **Root** as [[File Systems#Ext|ext4]], as it's really good.

>[!info] Some people create a separate ext4 partition for the home directory, this makes it so that you can change OS and still keep all your data, or makes data management easier (*pussy*).

Now for actual commands, start modifying a disk's partition (let's say `/dev/sda`) with fdisk
```shell
fdisk /dev/sda
```

**Now be careful here as it can go wrong real quick**.
You can enter `m` to get a pretty full help window, fdisk is not hard to use, which is what makes it so dangerous for inexperienced people. Note that the only danger is you wiping a disk you should not have wiped, no one is going to come murder you.

If you want a fresh start just enter `d` a bunch of times, it'll **DELETE EVERY PARTITION ONE BY ONE**

Start by setting your disk's Partition Table as [[Partition Tables|GPT]]:
press `g` and enter. Do this multiple times until you get a GUID number you like. <- I'm kidding don't.

if you press `l` you can see the list of every partition, note that **EFI** is `1`, **Linux Swap** `19`, and **Linux File System** `20`.  (*press `q` to leave*)
>[!info] Those are not the final file systems, it's just to prepare them to be `made` using `mkfs`, **Linux File System** is any file system that the linux kernel supports. So includes ext4

Press `n` to create a new partition, the partition number is just to give it a "name", see [[Linux File System#/dev]]. so just leave it to default. 

Leave the first [[File Systems]] to default too so that it starts after the partition table, 
and now to make it 1Gb since you're not actually going to count which sector it should end at, you can enter `+1GiB` or `+1GB` whichever one you prefer (*basically adding 1Gib to first sector*).

Now it'll have created a new partition of type Linux File system, except don't forget this one was supposed to be the **EFI**, so enter `t` to change the partition type, and now enter `1` as it was the EFI partition we noted down earlier. 

Repeat the same process for the 2 following partitions (swap and root), and once you're done enter `w` to write
**Do Not forget to enter `w` otherwise it will discard all your changes.**

You can enter `fdisk -l /dev/sda` now and see all your newly created partitions I'm so proud of you! *Do note which partition name is which tho so you don't mix up everything*

My partitions are
- `/dev/sda1` - **EFI**
- `/dev/sda2` - **Swap**
- `/dev/sda3` - **Root**

### Making File Systems

Now to actually specify the [[File Systems]] of each partition we'll use the `mkfs` and `mkswap`
*make file system | and | make swap*

Do read the [[Boot Modes#EFI System Partitions]] to understand why we're about to format the **EFI** as **FAT32**
```shell
mkfs.fat -F32 /dev/sda1
```
This one will set the `sda1` as FAT32
```shell
mkswap /dev/sda2
```
This one will of course set the `sda2` as swap
```shell
mkfs.ext4 /dev/sda3
```
Finally this one will set `sda3` as [[File Systems#Ext|ext4]]!

***
## Mounting File Systems
Mounting file systems refers to attaching each partition to a particular directory, for instance, our `/dev/sda3` (Root) partition will be mounted on `/`, and if we had had a different home partition `/dev/sda4`, it would have been mounted on `/home/`.

To mount partition use the `mount` command (*not very metaphorical are they*).

>[!info] Our current Linux Environment is temporary, and so is the Root `/`, so for now we'll `mount` our root partition `/dev/sda3` on `/mnt` which is a directory specifically made to temporarily mount disks.

So start by mounting your Root partition onto `/mnt`
```shell
mount /dev/sda3 /mnt
```
The **Swap** partition `/dev/sda2` does not actually get mounted, we just need to tell linux it's a swap partition, to do so
```shell
swapon /dev/sda2
```
We'll see about the EFI partition later, for now these are enough to actually get Arch installed.

***
## Mirrors

>[!info] Mirrors
>Mirrors are basically the network addresses from where your computer is going to download the files. Your **Arch ISO** might be old or outdated so it's important to update the mirrors to ensure you download from 
>1. A server that is not on the opposite side of the world as you
>2. A place that has the latest Linux packages

*I hope you didn't skip the internet connection part because i forgot and I did...*

To list all available mirrors use the `reflector` command util that comes with the Arch installer.
```shell
reflector
```

If you don't encounter the message 
```shell
failed to rate http(s) download (https://arch.jensgutermuth.de/community/os/x86_64/community.db): Download timed out after 5 second(s).
```
which means your internet is too slow and you need to either augment timeout: `--download-timeout 60` (sets a 60sec timeout instead of 5). Or, check to make sure your network operator is not cheating on you with other clients.

The list returned by reflector is terribly long, but thankfully it can sort all of them out for us:
```shell
reflector --country France --age 12 --sort rate
```
This for instance will return me a list of mirrors located in or near France, which have been updated in the last 12 hours sorted by download speed.

Now we have a cool list except we haven't actually told Linux to use it. Knowing Arch uses [[Pacman]], the list of mirrors used by the **packet manager** is going to be in `/etc/pacman.d/mirrorlist`, so we just need to update it with our new mirrors. 
*First back it up just in case `mv /etc/pacman.d/mirrolist /etc/pacman.d/mirrorlist.bak`*\

and now use reflector with the `--save` flag:
```shell
reflector --country France --age 12 --sort rate --save /etc/pacman.d/mirrorlist
```

use `cat` to check the mirror list has been updated.

***
## Installing Arch System

Your disks are ready, your mirrors are ready hell even your Wi-Fi is ready, let's finally make it happen.

To download everything let's use [[Pacman]] to ensure all the libraries are up to date first:
```shell
Pacman -Sy
```
Then use `Pacstrap` to install the Arch System:
```
pacstrap /mnt base base-devel linux linux-firmware sudo nano networkmanager
```
This'll install way more than what we need which is only 
- `base`
- `linux`
- `linux-firmware`
Except you're going to want to edit text files (`nano`), use `sudo` and use the network manager eventually so might as well install them now.

Now this is going to take quite some time (or not), so just wait until it's done and you can type in commands again. Then you'll successfully have installed **Arch**.

*This part was a huge pain in the ass because I kept running into problems with pacman, fuck pacman i fucking hate it why does it have to be so fucking complicated and for what. I ran into problems with databases security keys and so on, I had to disable security check for the shit to install arch*

*Seems this one dude Frederick Shwan has fucked something up and everyone has the same issue*
***
# Configuring Arch

Installing it wasn't that hard, but now we have to configure it, actually get everything looking and feeling right, as well as working right.

## FSTAB

Every time you start your Arch, Linux will automatically `mount` each file system to it's corresponding directory. However, we still haven't told it which one they are. Most Linux distros do it automatically, but arch likes to be cool and different.

the FSTAB file can be found in `/etc/fstab` and looks something like that
```shell
# <device>                                <dir> <type> <options> <dump> <fsck>
UUID=0a3407de-014b-458b-b5c1-848e92a327a3 /     ext4   defaults  0      1
UUID=f9fe0b69-a280-415d-a03a-a32752370dee none  swap   defaults  0      0
UUID=b411dc99-f0a0-4c87-9e05-184977be8539 /home ext4   defaults  0      2
```
Each partition is referred to using it's assigned [[Partition Tables#GPT|GPT GUUID]].

- `<device>` describes the block special device or remote file system to be mounted; see [#Identifying file systems](https://wiki.archlinux.org/title/Fstab#Identifying_file_systems).
- `<dir>` describes the [mount](https://wiki.archlinux.org/title/Mount "Mount") directory.
- `<type>` the [file system](https://wiki.archlinux.org/title/File_system "File system") type.
- `<options>` the associated mount options; see [mount(8) § FILESYSTEM-INDEPENDENT MOUNT OPTIONS](https://man.archlinux.org/man/mount.8#FILESYSTEM-INDEPENDENT_MOUNT_OPTIONS) and [ext4(5) § Mount options for ext4](https://man.archlinux.org/man/ext4.5#Mount_options_for_ext4).
- `<dump>` is checked by the [dump(8)](https://linux.die.net/man/8/dump) utility. This field is usually set to `0`, which disables the check.
- `<fsck>` sets the order for file system checks at boot time; see [fsck(8)](https://man.archlinux.org/man/fsck.8). For the root device it should be `1`. For other partitions it should be `2`, or `0` to disable checking.

We can automatically generate the **fstab** file using the following command
```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

`/mnt being where we mounted /dev/sda3` and where we installed arch acts as our root `/` for now.

## Booting as root

Remember you're still doing all of this on a live USB, However, now that the system is installed we can switch to the physically installed root partition we just created using
```shell
arch-chroot /mnt
```

### Setting Up Locale

The term "locale" refers to language, number, date and currency formats The file `/etc/locale.gen` contains locale settings and system languages and is commented by default. We must open this file using a text editor and uncomment the line which contains the desired locale. This is why `nano` was installed previously using the `pacstrap` command.

Open the `/etc/locale.gen` file and remove the "#" from the start of the line which contains your locale. Then, save the file.

```
[root@archiso /]# nano /etc/locale.gen
```

Since I am in the United States, the following entry has been uncommented prior to saving the file and the locale of `en_US.UTF-8` will be used for the remainder of the steps.

```
# /etc/locale.gen

en_US.UTF-8 UTF-8 
```

Generate the `/etc/locale.conf` file.

```
[root@archiso /]# locale-gen
Generating locales...
  en_US.UTF-8... done
Generation complete.
```

Create and set the `LANG` variable.

```
[root@archiso /]# echo LANG=en_US.UTF-8 > /etc/locale.conf
[root@archiso /]# export LANG=en_US.UTF-8 
```

### Network Configuration

Let's use that text editor once more to give our machine a hostname and proper identity on the network.

Create the `/etc/hostname` file and add the hostname entry. Then, save the file.

```
[root@archiso /]# nano /etc/hostname
```

This entry has been added:

```
# /etc/hostname

ArchLinuxPC
```

Create the /etc/hosts file and add the proper entries. Then, save the file.

```
[root@archiso /]# nano /etc/hosts
```

These entries have been added:

```
# /etc/hosts

127.0.0.1 localhost
::1 localhost
127.0.1.1 ArchLinuxPC
```

### Root Password

Finally, let's give the root user a password for the sake of security.

Use the `passwd` command to set the password for root.

```
[root@archiso /]# passwd
New password:
Retype new password:
passwd: password updated successfully
```

## Installing GRUB

grub is the bootloader letting us start our Arch linux, we need to install it.

Install the `grub` package.

```
[root@archiso /]# pacman -S grub efibootmgr
```

Create the directory where EFI partition will be mounted.

```
[root@archiso /]# mkdir /boot/efi
```

Mount the ESP partition.

```
[root@archiso /]# mount /dev/sda1 /boot/efi
```

Install GRUB to the hard disk.

```
[root@archiso /]# grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
```

Finally, generate the `/boot/grub/grub.cfg` file.

```
[root@archiso /]# grub-mkconfig -o /boot/grub/grub.cfg
```


### Sudo

Install the `sudo` command.

```
[root@archiso /]# pacman -S sudo
```

***
# Installing the Desktop environment

Technically that's it. You have arch, the entire OS is here everything you need is here. It's good to keep it this way if you're here to flex, but let's flex even harder by installing our own fully customed [[Desktop Environment]]

A **DE** typically consists of **a window manager, a file manager, a panel, a menu, a system tray, and various other applications and utilities**

You can install fully made Desktop Environments, or you can install each part individually.

For now we'll install a full fledged desktop environment **Cinnamon**



