RAIDs are really good when you have multiple disks, they can help you reach up to 4x read and write speed. HOWEVER, raid 5 and 6 are shit according to [this dude](https://youtu.be/HdEozE2gN9I?t=347), and if you read below you'll understand why, but raid 10 seems to be cool.

RAID stands for Redundant Array of Independent Disks. It's a technology used to combine multiple physical hard drives into a single logical unit for the purposes of data redundancy, performance improvement, or both. RAID configurations offer various levels, each with its own benefits and trade-offs. Here are some common RAID levels:

1. **RAID 0 (Striping)**:
    
    - Data is distributed evenly across multiple disks (striped).
    - Offers improved performance as data can be read from or written to multiple disks simultaneously.
    - No redundancy, so if one drive fails, all data is lost.
    - Used primarily for performance enhancement, not data protection.
2. **RAID 1 (Mirroring)**:
    
    - Data is mirrored across multiple disks.
    - Provides redundancy by maintaining identical copies of data on each disk.
    - Offers improved read performance but potentially decreased write performance.
    - Can withstand the failure of one drive without data loss.
3. **RAID 5**:
    
    - Data is striped across multiple disks like RAID 0, but with distributed parity.
    - Provides both performance improvement and data redundancy.
    - Can withstand the failure of one drive without data loss.
    - Requires a minimum of three disks.
4. **RAID 6**:
    
    - Similar to RAID 5 but with dual parity for increased fault tolerance.
    - Can withstand the failure of two drives without data loss.
    - Offers improved data protection over RAID 5 but may have slightly lower performance.
    - Requires a minimum of four disks.
>[!danger] don't use 5 and 6
> Both RAID 5 and RAID 6 incur a performance penalty for write operations due to the need to calculate parity information, which can impact overall system performance, particularly in environments with high write workloads. 
>With RAID 5, the risk of encountering a second drive failure during the rebuild process (due to the increased load on the remaining drives) is a concern. 
>In RAID 1 and RAID 10, a failed drive can simply be replaced with a spare and the array continues to operate normally, whereas RAID 5 and RAID 6 arrays are in a degraded state until the failed drive is rebuilt.

1. **RAID 10 (RAID 1+0)**:
    
    - Combines RAID 1 mirroring and RAID 0 striping.
    - Provides both redundancy and improved performance.
    - Requires a minimum of four disks.
    - Can withstand multiple drive failures, depending on which drives fail.