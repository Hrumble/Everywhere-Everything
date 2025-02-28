
Bootable Drives/Bootable USBs are made by **burning** an **ISO** onto a USB drive.
Used to boot from and most often times install OS on computer

# ISO?

>[!quote] An ISO file is _an exact copy of an entire optical disk such as a CD, DVD, or Blu-ray archived into a_ single file.

An iso is basically a compacted file in which there's an entire OS. 
>[!tip] Inside of `linux_lite.iso` file
>![[linux_lite_iso_mounted.png | 500]]

To find **ISO** files, just look online for the OS you're trying to install and search for a download tab or directly look for **ISO**.
Here are some common **OS ISO**

- [Ubuntu (any version)](https://ubuntu.com/download/desktop)
- [Fedora](https://fedoraproject.org/workstation/download)
- [Arch](https://archlinux.org/download/) <- Only fun for autistic people so see [[Installing Arch Linux]] :)
- [Kali](https://www.kali.org/get-kali/#kali-installer-images) Pentesting ofc
- [Mint (any edition)](https://linuxmint.com/download.php) 
***
# How to burn ISO on USB 

You can either use programs made for that like [Rufus](https://rufus.ie/en/) or [Balena Etcher](https://etcher.balena.io) but **that's not cool**
PS: *For some reason when I tried manually burning the arch iso, the USB didn't boot, so use rufus for arch*
Let's see how to do it via **terminal** on **Windows**

There's 3 steps to burning any **ISO** on a **USB** 
- [ ] [[#Cleaning and Formatting USB|Clean and format the drive]]
- [ ] [[#Mounting ISO|Mount ISO as virtual drive]]
- [ ] [[#Burning ISO|Copy mounted iso to bootable USB]]

Everything explained here is pulled from that one [SuperUser answer](https://superuser.com/questions/1020654/bootable-usb-flash-drive-from-iso-using-windows-cmd-i-cant-find-the-tutorial)

## Cleaning and Formatting USB

in CMD open **DiskPart**
`diskpart`
List all disks to see which one is your USB
`list disk`
Select **THE CORRECT DISK** don't you select your main disk
`select disk *` (replace * with disk number) e.g `select disk 2`
Clean it
`clean`
Create new primary partition on disk
`create partition primary`
Select partition you just created
`select partition *`
set it as active and format it into [[File Systems#NTFS|NTFS]]
`active`
`format fs=NTFS`
Assign Disk letter to the new disk (let's pretend its `U:`)
`assign`

Then leave disk part using `exit`
Once that is done the USB is ready to boot all that's left is actually burning the ISO onto it, to do so you need to 'Unpack' the iso (mount it on virtual drive)

## Mounting ISO

Mounting the .iso is pretty straightforward, just find it in your files, right click and choose `Mount`
Should create a new drive with the **ISO**'s name in which the entire **OS** is.

## Burning ISO

From here burning the **ISO** actually means just copying the files from the mounted **ISO** to the bootable USB. **However**, to make sure we copy everything, use CMD.

Type in CMD
`XCOPY V:\*.* /s /e /f U:\`  
`V` is the name of the Mounted ISO drive and `U` is the bootable USB

| Option | Detail |
| ---- | ---- |
| `V:\*.*` | Copies every file `*` of every extension `.*` from disk `V` |
| `/s` | Copies directories and subdirectories, unless they're empty. If you omit **/s**, `xcopy` works within a single directory. |
| `/e` | Copies all subdirectories, even if they're empty. Use `/e` with the `/s` and `/t` command-line options. |
| `/f` | Displays source and destination file names while copying. Basically a verbose mode |
| `U:\` | Tells `xcopy` where to copy the files `U:` drive  |

Great job bootable USB is ready, if it's still appearing in explorer, eject it and plug it into the computer on which you want to install the OS.

PS: *This also works for live USB*

