Partition tables are basically **data structures** (somewhat arrays) at the beginning of most disk that basically define where each partition starts and ends. There are two **main** types of partition tables.


## Bootstrap code/Bootloader

Bootstrap code is roughly the code that transfers control to the bootable partition, it's the thing that gets your OS started.


# GPT
**GUID Partition Table**

>[!quote] [GUID Partition Table](https://en.wikipedia.org/wiki/GUID_Partition_Table "wikipedia:GUID Partition Table") (GPT) is a partitioning scheme that is part of the [[Boot Modes#UEFI|Unified Extensible Firmware Interface]] specification; it uses [globally unique identifiers](https://en.wikipedia.org/wiki/Globally_unique_identifier "wikipedia:Globally unique identifier") (GUIDs), or UUIDs in the Linux world, to define partitions and [partition types](https://en.wikipedia.org/wiki/GUID_Partition_Table#Partition_type_GUIDs "wikipedia:GUID Partition Table"). It is designed to succeed the[[#MBR|Master Boot Record]] partitioning scheme method.

**GPT** has no partition limit, and can go up to $94*10^{6}$TB


***
# MBR
**Master Boot Record also known as DOS / MS-DOS**

>[!danger] The **MBR** and **VBR**, although similar in their workings, are not the same thing. MBR operates on an entire disk whereas **VBR** operates on a FAT-compliant partition, both can be Booted from in a BIOS environment, see [[File Systems#VBR]]

The master boot record is a **Boot Sector**: the first 512 bytes of a storage device. It contains a **bootloader** and the device's partition table.
>[!info] The MBR is not located in a partition; it is located at the first sector of the device (physical offset 0), preceding the first partition.

Although the entire **512 bytes** of **MBR** are referred to as the **boot sector**, only the first **446 bytes** contain the **bootloader/bootstrap code**. Leaving `66 bytes` for the partition table

## Partition Table

The next `64 bytes` of the MBR are allocated for the partition table.
>[!faq] What happened to **66 bytes**?
>the `2` remaining `bytes` are reserved for the **MBR Signature**, which is just letting the [[Boot Modes|BIOS or UEFI]] know that that disk contains a valid partition table. 
>`0x55` and `0xAA` to be precise.

In the **MBR** partition table, there are 3 possible **partition types**:
- **PRIMARY**
	- **Primary** partitions can be bootable and are limited to four partitions per disk.
- **EXTENDED**
	- **Extended** partitions can be seen as containers for **logical** partitions, they also count as **primary** partitions except you can **only have one of those per disks**.
	- If the disk has an **extended** partition, then only 3 other primary partitions can be created.
- **LOGICAL**
	- You can have an unlimited number of **logical** partitions inside the **extended** partition, they just help you arrange stuff more cleanly.

**Things to keep in mind:** The MBR partition table uses a 32bit value to represent the start of a partition in sectors. As 32bit numbers can only be as big as $2^{32}$, assuming a sector length of `512 bytes` this represents **2 Terabytes**. 
**TL;DR** if your disk is larger than 2TB you can't use **MBR**

>[!quote] When partitioning a MBR disk consider leaving at least 33 512-byte sectors (16.5 KiB) of free unpartitioned space at the end of the disk in case you ever decide to [convert it to GPT](https://wiki.archlinux.org/title/Gdisk#Convert_between_MBR_and_GPT "Gdisk")
