A file system is a **set of rules** that a particular disk/partition follows to store, refer, and locate files inside itself.
Information about a particular file system along with it's required information is stored inside the [[#Boot Sector|Boot Sector]] also known as **Superblock** for [[#Ext|Ext filesystems]]

To correctly understand **File Systems**, it's important to understand exactly what **Files** are.

# What are Files?

**Files are a series of bits.**
A file is basically just an array of [[Binary|binary numbers]], which are each interpreted according to the **File Format**. 
For instance, let's take a look at the following **raw** `.txt` file:
`1000110 1110101 1100011 1101011 100000 1011001 1101111 1110101 101110`
*Doesn't look like much does it?*
This is the inside of a text file, **excluding [[#Metadata]]**

in `.txt` files, each **Byte** represents a character in ascii format, so for instance the first byte `1000110` equals the number `70`, which is in ASCII the character `F`, the following byte `1110101` is `117` and is the character `u`, and you just keep on going until the end of the file.
*Have fun [[Binary#Read Binary Numbers|decoding it]]*
## Metadata
How exactly can the computer know how he should interpret all the binary values given to him? What tells it that the following array of bits is supposed to be interpreted as ASCII and thus text?

This is done using **Metadata**

**Metadata** is Data about the data itself, it's a cluster of bytes of varying size located at the beginning of each file inside the disk, telling the computer everything it needs to know to interpret the binary.

***
# Boot Sector
The boot sector is a small region located at the beginning of a disk or partition that contains multiple information regarding the files of the disk/partition such as:
- Type (Which file system)
- size (size of the partition/disk)
- location of key data structures

***
# FAT
**File Allocation Table**
see [this](https://www.youtube.com/watch?v=7o3qx66uLz8)

>[!danger] All disk data structures on **FAT** are in [[Binary#Big-Endian Little-Endian|Little-endian]]

The following image shows what information the FAT Boot sector contains
![[fat_boot_sector.png|400]]

In the context of a [[Boot Modes#Legacy BIOS|BIOS]] firmware, A **FAT** partition will look like the following
![[fat_regions.png]]

And according to our Above boot sector let's see how many sectors are in each of them.

| Boot Sector/ Reserved Sector | FAT (File Allocation Table) | Root Directory | File Data |
| ---- | ---- | ---- | ---- |
| $$1$$ | Sectors per fat $*$ fat count<br>$$9*2 = 18$$ | Dir Entry Count $*$ Dir entry size[^1]/bytes per sector <br>$$224*32/512=14$$ | $$2880-14+18+1 = 2885$$ |
Knowing that one sector has a size of 512 Bytes, we can calculate the total size of the disk as $$512*2880 = 1474560$$ so around 1.4 MB

## VBR

The **VBR** (Volume Boot Record) is the equivalent of an [[Partition Tables#MBR|MBR]] for FAT partitions, it's what we call the reserved space/boot sector at the beginning of the partition. It is required on all **FAT** partitions to ensure compatibility. 
*See section 3 of the [official FAT Specs](https://academy.cba.mit.edu/classes/networking_communications/SD/FAT.pdf) for a detailed description of what it contains*.

## Locating Files

![[fat_root_dir.png|600]]
The above picture shows the contents of the Root Directory portion of the partition. It contains file types, file names, and a lot more metadata, but most importantly each file/directory's first **cluster**
>[!faq] What are clusters?
>Clusters are what FAT partitions call blocks of data, the boot sector defines how many sectors are in each cluster, in our case it's one thus 512bytes per cluster.

We can see that the `TEST.txt` has it's first cluster value set to 3, which means that **The first 512 Bytes of the `TEST.txt` file are in the third cluster of the Data Region**.

We have the first 512bytes of data from our text file, but what if it's bigger than that? where's the rest of the data? To find it, we refer to the **File Allocation Table**:
![[fat_fat.png|600]]
>[!warning]
>The image has been taken for better understanding. **HOWEVER**, the hex values are represented in Big-endian here whereas FAT usually works in little-endian. it's just for ease of understanding.

As we can see here, the 3rd cluster refers to the 4th entry in the table, and it's value is `0x004`, In **big endian that represents the number 4**, which tells us that the next 512 bytes of data are in the 4th cluster.

The computer repeats that process until it reaches a value inside the **FAT** that is superior to `0xFF8`, these are special values that mark the end of a chain -> **File**.

## FAT Versions

So far we've seen how a FAT12 partition works, but fret not youngin, only one thing changes from a FAT version to another.
**The FAT12 version** is called that because each entry in the **FAT** is of maximum 12bits, thus the `0x004` we saw earlier.
**The FAT16 version** uses 16bits entries, so would've been `0x0004`.
**Finally the FAT32**... you guessed it uses 32bit entries so `0x00000004` would be the 4th cluster.

**FAT32 Can not have files larger than 4GB**, this is due to the fact that it stores the size of each file as a 32bit value in bytes (*kind of stupid*), so $2^{32}$ approximately equals 4gb.

The **ex-Fat**, ex-Fat is just a fancy way of saying **FAT64** so **64bit entries, and 64bit size values, AND clusters able to be as large as 32MB**, so unless you have a disk larger than 64 zettabytes with files being as big as 16 exabytes, you shouldn't be limited in size by exFAT. 

Basically the higher the FAT version the more entries you can have, so the larger your disk can be.

[^1]: Directory Entry Size can be found in the official FAT specs and is usually 32bytes.

***

# Ext
**Extended file system**

>[!quote]- The ext file system family, including ext2, ext3, and ext4, is more feature-rich and complex compared to FAT.
>- ext file systems include features like journaling, which enhances data reliability and recovery in the event of a system crash or power failure. 
>- ext file systems support more advanced features such as extended attributes, access control lists (ACLs), and larger file and partition sizes.
>- Understanding ext file systems may require familiarity with concepts like journaling, block groups, inode structure, and more advanced file system features.
>- While ext file systems provide enhanced capabilities and performance compared to FAT, their complexity may make them more challenging to understand for beginners or those without prior experience with file systems.

*I have not found anything explaining it in less than an hour so wtv*

Compared to the FAT file system, EXT starting from Ext2 up until Ext4 offers journaling.
>[!faq] What?
> Journaling refers to the file system reserving a space on the partition where it stores every changes made to the metadata of each files/directory before they're actually comitted to the main data structure. This basically means that if your system crashes while you're overwriting a certain sector, and then it has no clue what it was doing with that sector, it can just go back to the previous one.

Ext is also very fast.

***
# NTFS

**NTFS (New Technology File System)**:
- Native file system for Windows operating systems, offering advanced features and capabilities.
- Supports features like journaling, access control lists (ACLs), encryption, compression, and disk quotas.
- Well-suited for Windows installations, particularly for system partitions and volumes where reliability, security, and support for large files and volumes are essential.

***
# ZFS

Apparently really good/the best file system, made by oracle but complicated because of licensing.