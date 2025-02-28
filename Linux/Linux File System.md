
*Todo, refer to [this](https://www.youtube.com/watch?v=42iQKuQodW4&pp=ygURbGludXggZGlyZWN0b3JpZXM%3D) for now*

*Done, thanks gpt because I was never going to do this for every dir*

# /dev

`/dev` is the directory which contains all *Device* files, those are files that represent all devices attached to the system, each file is either a **Character Device** or a **Block Device**
>[!faq] A what device??
>both Character and Block devices are devices which are able to send and receive data
>>[!info] Block Devices
>>Block Devices are devices which send and receive data in fixed-sized blocks or chunks. Data is read from or written to that device in multiples of that fixed-size.
>>Those are mainly [[Disk Types|storage devices]]
>
>>[!info] Character Devices
>>Those are devices which send or receive data one *character at a time* (one byte). 
>>Character devices are devices like keyboards, mouse, or serial ports
## Block Devices

the `/dev` directory is commonly referred to when talking about **Block Devices** with `/dev/sda` or `/dev/sda2` and so on
- `sd` refers to a **Block Device** of type [[Disk Types#SATA|SATA/SCSI]], it could also be `fd` for [[Disk Types#Floppy Disk|Floppy Disk]], or `hd` for [[Disk Types#PATA|Hard Disks]] or `nvme` for [[Disk Types#NVMe|NVMe]] (this one is particular see [here](https://www.quora.com/I-have-a-new-laptop-My-disc-is-not-dev-sda-but-dev-nvmeOn1-Why-is-that#:~:text=If%20all%20went%20the%20same%20way%2C%20you%E2%80%99d%20expect%20something%20like%20ndb%20to%20mean%2C%20second%20nvme%20drive%2C%20right%3F))

- `a` is the letter used to identify in which order the disk was discovered, if two disks were found on the machine, then you'd have `/dev/sda` and `/dev/sdb`. goes `a, b, c, d, ..., z, Aa, Ab, ..., Zz` and so on. (*Try to have so many disks you gotta have a `/dev/sdZz` I dare you*)

- `2` or the number after `sda` is the partition of the disk
e.g. the **3rd** partition of the **5th** disk found of type **SATA** would** be `/dev/sde3`
the **2nd** partition of the **only connected** **Floppy Disk** would be `/dev/fda2`
*you get the idea. 
(floppy disks practically don't exist anymore if you see a `/dev/fda` **kill yourself**)*

***

# /etc

`/etc` is the directory that contains **system-wide configuration files** and **settings**. It's one of the most important directories on a Linux system, as it holds the configurations for almost everything that runs on the machine.
>[!faq] What is a configuration file??
>Configuration files are files that define settings for various programs and system services. They control how programs behave and can be edited to change system functionality.
>>[!info] Key Files in /etc
>>- `/etc/passwd` stores user account information.
>>- `/etc/fstab` defines how disk partitions, devices, and remote file systems are mounted.
>>- `/etc/hostname` holds the system's hostname.
>>- `/etc/network/` holds configuration files for networking.
## Subdirectories and Files

### /etc/passwd
Contains the basic information for user accounts. Each line represents one user and contains several fields (e.g., username, password hash, user ID, group ID, etc.).
- Example: `username:x:1001:1001:Full Name:/home/username:/bin/bash`

### /etc/fstab
Defines how file systems are mounted automatically during boot. It includes partitions, storage devices, and network file systems.
- Example: `/dev/sda1  / ext4  defaults  0  1`

### /etc/hostname
Contains the machine's hostname (the unique name for the system on the network).
- Example: `my-linux-machine`

### /etc/network/
Contains network configuration files (e.g., `/etc/network/interfaces` for static IPs or DHCP configuration).
- Example: `/etc/network/interfaces` might contain:
	- iface 
	- eth0 
	- inet 
	- dhcp

### /etc/apt/ (Debian/Ubuntu)
The directory for configuration files related to APT (Advanced Packaging Tool) in Debian-based systems. It includes repositories and package manager settings.
- Example: `/etc/apt/sources.list` specifies which repositories APT should use.

### /etc/systemd/
Configuration files for **systemd**, which is the default system and service manager for modern Linux distributions.
- Example: `/etc/systemd/system/` holds service unit files to manage services like web servers, databases, etc.

## Editing Files
It’s important to note that changes made to files in `/etc` usually require **root** (admin) privileges, so be cautious when editing these files. Always make backups when possible!

***

# /bin

`/bin` stands for **binary**, and this directory contains essential **binary executables** (programs) that are required for the system to boot and run in single-user mode.
>[!faq] What are binary executables??
>Binary executables are files that contain programs or commands that the operating system can run directly, like `ls`, `cat`, or `cp`.
>>[!info] Common Executables in /bin
>>- `ls` (List directory contents)
>>- `cp` (Copy files)
>>- `mv` (Move files)
>>- `cat` (Concatenate and display file contents)
## Characteristics
- These programs are essential for **system repair**, basic shell functionality, and utilities that users need to operate the system.
- It is used by both the system administrator and regular users, but it is critical for system operations, so it should not be emptied.

***
# /lib

`/lib` contains essential **shared libraries** and kernel modules that the programs in `/bin` and `/sbin` depend on. It’s basically the system’s **library storage**.
>[!faq] What are shared libraries??
>Shared libraries are collections of functions and routines that programs can use without needing to include their own copies of the code.
>>[!info] Common Libraries
>>- `/lib/x86_64-linux-gnu/` contains architecture-specific libraries.
>>- `/lib/modules/` contains kernel modules for hardware drivers.
## Kernel Modules
- Kernel modules are part of the **Linux kernel** and can be loaded or unloaded to control how the kernel interacts with hardware.
- These are stored in subdirectories like `/lib/modules/` and are loaded dynamically as needed.

***
# /home

`/home` is where the **home directories** of regular users are stored. Each user has a subdirectory here, typically named after their username.
>[!faq] What’s a home directory??
>Each user has a unique home directory that holds their personal files, settings, and configuration files.
>>[!info] Typical Files in /home
>>- `/home/username/` contains personal files, configurations (e.g., `.bashrc`), and application data.
>>- `/home/username/Desktop` contains files on the user’s desktop.
## Permissions
- The home directories are owned by individual users, and other users typically don’t have access to them unless permission is granted.
- This is where you’ll find things like user-specific **documents**, **downloads**, and **settings**.

***
# /mnt

`/mnt` is intended for **mounting temporary file systems** and devices like network drives or USB drives.
>[!faq] What is mounting??
>Mounting is the process of making a storage device (like a hard drive or USB stick) available to the system by attaching it to the file system.
>>[!info] Mounting Example
>>- A USB drive might be mounted at `/mnt/usb` or `/mnt/external_drive`.
- It’s a temporary mounting point, so devices will usually be unmounted after use.
- `/mnt` is often used during system setup, maintenance, or when adding devices.

***

# /opt

`/opt` is for **optional add-on software packages** that are installed outside of the distribution’s package management system.
>[!faq] What goes in /opt??
>Large, self-contained applications or third-party software that isn’t part of the default system installation are placed in `/opt`.
>>[!info] Common Software in /opt
>>- `/opt/google/` might contain Google’s software, like Chrome.
>>- `/opt/xyz/` might contain software for a custom application.
- These directories may contain the entire application, including binaries, libraries, and other necessary files.

***

# /usr

`/usr` contains **user-related programs** and files that are accessible to all users. It holds most of the software and utilities that are not required for booting the system but are still essential for normal operation.
>[!faq] What’s the difference between /usr and /bin??
>The main difference is that `/bin` holds essential binaries for booting, while `/usr` contains additional software installed for normal system operation.
>>[!info] Common Files in /usr
>>- `/usr/bin/` holds most user-level programs (e.g., `python`, `git`).
>>- `/usr/lib/` contains libraries for the programs in `/usr/bin/`.
>>- `/usr/share/` contains shared data, like documentation, icons, and more.

***
# /srv

`/srv` contains data for **services provided by the system**. It's used for data that's served by the system like web or FTP servers.
>[!faq] What kind of data is stored in /srv??
>This is where server data is stored, such as a website’s files or FTP server files.
>>[!info] Example Usage
>>- `/srv/http/` for web server files.
>>- `/srv/ftp/` for FTP server files.
- It's not widely used by all systems, but some service-oriented systems place data here.

***

# /tmp

`/tmp` is used for **temporary files** that need to be accessible to all users. Files in `/tmp` are usually deleted on reboot, but they can also be cleared periodically by the system.
>[!faq] Can anyone use /tmp??
>Yes, `/tmp` is generally readable and writable by all users, though files can be secured with permissions if necessary.
>>[!info] Example Files in /tmp
>>- Temporary files created by programs during their execution (e.g., backup files or temporary downloads).
- It should never be used for storing important files because it’s temporary and can be wiped.

***

# /var

`/var` stands for **variable**, and it contains files that are **expected to change** or grow in size while the system is running, such as logs, spool files, or cached data.
>[!faq] What types of files are in /var??
>Logs, spools, and other variable data that changes frequently are stored here.
>>[!info] Common Subdirectories in /var
>>- `/var/log/` holds system and application log files (e.g., `syslog`, `auth.log`).
>>- `/var/spool/` holds files waiting to be processed, such as print jobs or mail queues.
>>- `/var/cache/` contains cached data for applications to speed up future operations.
- As this directory grows, it's a good place to check for system health and troubleshoot issues.

***

# /root

`/root` is the **home directory for the root user** (the superuser), which is different from the regular users' home directories in `/home`.
>[!faq] What is the root user??
>The root user is the system administrator with full control over the entire system.
>>[!info] About /root
>>- It’s the home directory for the **root user**, where their personal files and configuration settings are stored.
- Unlike regular users, the root user can perform any action on the system, so it’s often used for system administration tasks.

***
# /sys

`/sys` is a **virtual filesystem** that provides a way to interact with the **kernel** and devices in real time.
>[!faq] What’s inside /sys??
>It contains information about the kernel’s internal parameters and real-time information about devices.
>>[!info] Common Subdirectories in /sys
>>- `/sys/class/` contains information about various device classes (e.g., network devices, block devices).
>>- `/sys/devices/` contains information about the system's devices (e.g., CPU, USB devices).
- It allows you to interact with kernel parameters and check the status of hardware.

***
# /proc

`/proc` is another **virtual filesystem** that provides runtime information about **system processes** and **kernel parameters**.
>[!faq] What kind of info can I find in /proc??
>It contains dynamic information about the system, like process stats, memory usage, and hardware info.
>>[!info] Common Files in /proc
>>- `/proc/cpuinfo` provides details about the system’s CPU.
>>- `/proc/meminfo` contains information about system memory.
>>- `/proc/[PID]` contains directories for each running process (where [PID] is the process ID).
- This directory is constantly updated and provides insight into the current state of the system.

***
