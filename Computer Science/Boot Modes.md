In almost every single computer **on earth**, there are one of two **firmwares**: **[[#UEFI]]** or **[[#Legacy BIOS]]**
Those basically tell the computer *how* the system disk is booted
>[!quote] A System Disk Is
>A hard disk, CD-ROM or floppy disk that contains part or all of the operating system or other control program. See [bootable disk](https://www.pcmag.com/encyclopedia/term/bootable-disk), [system file](https://www.pcmag.com/encyclopedia/term/system-file) and [system folder](https://www.pcmag.com/encyclopedia/term/system-folder).

**Really good** blog post explaining clearly what does two are [here](https://www.happyassassin.net/posts/2014/01/25/uefi-boot-how-does-that-actually-work-then/), most quotes are taken from there.
*Actually really good*


# How Does A Computer Boot

1. You press the power button on your laptop/desktop.
2. The CPU starts up, but needs some instructions to work on (remember, the CPU always needs to do something). Since the main memory is empty at this stage, CPU defers to load instructions from the firmware chip on the motherboard and begins executing instructions.
3. The firmware code does a Power On Self Test (POST), initializes the remaining hardware, detects the connected peripherals (mouse, keyboard, pendrive etc.) and checks if all connected devices are healthy. You might remember it as a 'beep' that desktops used to make after POST is successful.
4. Finally, the firmware code cycles through all storage devices and looks for a boot-loader (usually located in first sector of a disk). If the boot-loader is found, then the firmware hands over control of the computer to it.
***
# Legacy BIOS
**Basic Input/Output System**

The **BIOS** used to be the factory standard in the 1980s -> until UEFI was created

The BIOS is a pretty straightforward system.
In your old school computer you have one or multiple disks which have an [[Partition Tables#MBR|**MBR** (Master Boot Record)]].

>[!quote] what is the MBR
>The MBR is another de facto standard; basically, the very start of the disk describes the partitions on the disk in a particular format, and contains a 'boot loader', a very small piece of code that a BIOS firmware knows how to execute, whose job it is to boot the operating system(s)

All the BIOS knows is what disks the system contains, and you the user tell the **BIOS** which disk to boot the system from. All the **BIOS** then does is execute the *bootloader* found in the MBR. 
After that, BIOS is *bye bye*: doesn't do anything anymore.

>[!quote] 
>The firmware layer doesn't really know what a bootloader is, or what an operating system is. Hell, it doesn't know what a partition is. All it can do is run the boot loader from a disk's MBR

***
# UEFI
**Unified Extensible Firmware Interface**

You can read the official **UEFI** Specifications [here](https://uefi.org/specs/UEFI/2.10/01_Introduction.html)

>[!tip]
>Most UEFI firmwares implement some kind of *BIOS-Compatibility feature*, commonly referred to as `CSM` which lets the UEFI act like a BIOS, basically leaving everything to the bootloader like seen [[#Legacy BIOS|above]].

>[!quote] 
>[UEFI is] Completely and utterly different from how BIOS booting works. You cannot apply any of your understanding of BIOS booting to native UEFI booting. You cannot make a little tweak to a system designed for the world of BIOS booting and apply it to _native_ UEFI booting. You need to understand that it is a completely different world. [...] UEFI provides _much_ more infrastructure at the firmware level for handling system boot. It's nowhere near as simple as BIOS. Unlike BIOS, UEFI certainly does understand, to varying degrees, the concepts of 'disk partitions' and 'bootloaders' and 'operating systems'.


*So UEFI seems really complicated for me to grasp rn, but if you want to understand i recommend you to read the blog post linked*

What I have so far is the UEFI lets you boot from other things than MBR partitions which is a good thing? the dude in the blog post seems to really be into it.

## EFI Executables

To fully understand UEFI, It's important to understand EFI, *it's literally $\frac{3}{4}$ of the word.*
And it's taken me some time but here it is:
>[!tip] EFI is a kind of C BASED PROGRAMING LANGUAGE USED TO WRITE BOOTLOADERS FOR UEFI!  ٩( ๑╹ ꇴ╹)۶
>with the `.efi` extension

See [here](https://www.rodsbooks.com/efi-programming/hello.html) to code a **EFI** variant of hello world!

EFI is **THE** code format that the **UEFI** will read and understand. When you write a [[Partition Tables#Bootstrap code/Bootloader|bootloader]] for a **UEFI-compliant** firmware you write it in **EFI**. That's pretty much all there is to it honestly. 
*It genuinely took me so long to grasp that because everything is named the same*

You run **EFI** code from what's called the **EFI Shell**, and it's basically a built-in shell in the UEFI firmware of your motherboard 
*We've come full circle finally!*

## EFI System Partitions
**Known as ESP**

>[!quote] 
>The file system supported by the Extensible Firmware Interface is based on the FAT file system. EFI defines a specific version of FAT that is explicitly documented and testable. Conformance to the EFI specification and its associate reference documents is the only definition of FAT that needs to be implemented to support EFI. To differentiate the EFI file system from pure FAT, a new partition file system type has been defined.

**EFI partitions are partitions which the UEFI firmware recognizes as the partition containing all the bootloaders.** 
They're basically a specific version of [[File Systems#FAT|FAT]] that was agreed upon when making UEFI.

**You should not have more than one ESP on a disk** according to [this discussion](https://news.ycombinator.com/item?id=16261237)
>[!quote] 
>An ESP isn't just a partition mounted to /boot or one that has bootfiles or a bootloader, it's (in practice) a FAT32 partition that has a different filesystem ID, in particular, the magic GUID {C12A7328-F81F-11d2-BA4B-00A0C93EC93B}

This means that the **UEFI** firmware, unlike BIOS which can only boot from disks, can boot from any partition that is EFI. Including USB, CD, Network boot. 

>[!quote] 
>An 'EFI system partition' is really just any partition formatted with one of the UEFI spec-defined variants of FAT and given a specific [[Partition Tables#GPT|GPT partition]] type to help the firmware find it.  And the purpose of this is just as described above: allow everyone to rely on the fact that the firmware layer will definitely be able to read data from a pretty 'normal' disk partition.

# UEFI Boot Manager

So we've seen **UEFI** is already pretty neat because it clearly defines an executable format, clearly defines a partition, and is just overall cooler than **BIOS**. But, we're about to see it gets even cooler.

With **BIOS**, the system boots based on which disk's MBR was located first, and you just **go fuck yourself** if you wanted it to boot from another disk or another partition *<- because you can't*.

But with **UEFI**, you actually get to choose, you can add or remove entries from the boot menu. Even from a running OS and without restarting your computer. On linux you can use `efibootmgr` to view **UEFI Boot Manager**. Here's a sample output
```shell
[root@system directory]# efibootmgr -v
BootCurrent: 0002
Timeout: 3 seconds
BootOrder: 0003,0002,0000,0004
Boot0000* CD/DVD Drive  BIOS(3,0,00)
Boot0001* Hard Drive    HD(2,0,00)
Boot0002* Fedora        HD(1,800,61800,6d98f360-cb3e-4727-8fed-5ce0c040365d)File(\EFI\fedora\grubx64.efi)
Boot0003* opensuse      HD(1,800,61800,6d98f360-cb3e-4727-8fed-5ce0c040365d)File(\EFI\opensuse\grubx64.efi)
Boot0004* Hard Drive    BIOS(2,0,00)P0: ST1500DM003-9YN16G        .
[root@system directory]#
```
>[!quote] The first line tells you which of the 'boot menu' entries you are _currently_ booted from. The second is pretty obvious (if the firmware presents a boot menu-like interface to the UEFI boot manager, that's the timeout before it goes ahead and boots the default entry). The BootOrder is the order in which the entries in the list will be tried. The rest of the output shows the actual boot entries.

A default **UEFI** firmware would try to first boot the entry called `opensuse`, if after 3 seconds it doesn't work, it's going to try booting `Fedora`, and so on...

***
# Coreboot

Just a cool firmware I found online which seems to be the third category [link](https://www.coreboot.org).
Could maybe be fun to look at it someday

