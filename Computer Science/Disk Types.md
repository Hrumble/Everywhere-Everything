
>[!info] The names of the following types of disks come from how they each connect to the computer. Each of the following names are **interfaces** <- *important word*

# PATA
**Parallel Advanced Technology Attachment**

These were widely used during **1980**-**2000s** and are becoming obsolete now that **SATA** disks are around.

PATA type disks can have up to `80gb` storage, and transfer data up to `133 mb/s`.
the PATA interface refers to the ability of transferring bits simultaneously using wires that are *parallel* to each other. 
>[!tip] Basically, the disk has a 40 or 44 pin connector, in which data is sent across multiple wires. Each bit in a byte has it's own wire to go trough parallelly

PATA interfaces have clock rates ranging from **33 MHz** to **66 MHz**, or 33 Mil to 66 Mil rounds per second. Knowing PATA sends 8bits/1byte simultaneously, this means that the device sends 1 byte of data every $\frac{1}{33*10^6}$ second, thus **133mbps MAX**

>[!danger] Data being sent across multiple cables all at once greatly limits cable length and speed. It is a necessity for all bits to arrive in order, and each cable can have it's own degradation and relay different from the others.

***
# SATA
**Serial Advanced Technology Attachment**

**SATA** type disks are what are most commonly used nowadays, they send bits one at a time using **serial signaling technology**, (*doesn't really matter what it is tbh*). SATA disks are way faster than PATA even though they only transfer one bit at a time due to their clock rate.
Where PATA had 33-66MHz clock rates, **SATA has 1.5GHz clock speed** meaning it sends data up to 1.5Gbps for **SATA I**

1. **SATA II (3.0 Gbps)**: SATA II doubles the data transfer rate to 3.0 Gbps, maintaining the same clock rate of 1.5 GHz but achieving higher throughput through improvements in encoding and protocol efficiency.

2. **SATA III (6.0 Gbps)**: SATA III further doubles the data transfer rate to 6.0 Gbps, again maintaining the same clock rate of 1.5 GHz. This increase in throughput is achieved through additional enhancements in encoding and protocol efficiency.
***
# SCSI

***
# Floppy Disk

***
# NVMe
**Non-volatile Memory Express**