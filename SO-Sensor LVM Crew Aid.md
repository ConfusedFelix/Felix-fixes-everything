# Security Onion Sensor LVM Crew Aid
## Background
Occasionally we need to combine more than one drive (physical or virtual) into one logical drive.  This increases capacity across multiple drives like a RAID does, but is established at the Operating System (OS) level.  Once created, it’s associated with a system level mount point.  In our situation, we use this the most to add additional drives to the /nsm/ partition in Security Onion (SO) to give us more capacity for Packet Capture (PCAP) retention.  The same fundamentals can be applied to many other applications. Something that may be unique to physical sensors versus virtual is you have the potential to have different types of drives presented to the OS.  For example, the Purple Safari v1.5 Sensors have 4 SATA drives (sda, sdb, sdc, sdd), but the SealingTech physical sensors have one internal 2TB M.2 NVMe (nvme0n1) and four SATA drives (sda, sdb, sdc, sdd).  As the technician, you’ll have to evaluate your hardware and decide what is best.

## Overview
After installing SecurityOnion (SO), the system will reboot and you’ll be presented with the initial setup screen.  At this point, cancel out to return to the CentosOS7 prompt.  From there, you’ll create partitions for any additional drives presented other than the drive the OS is installed and create a physical volume for each of those partitions.   Next, you’ll add those physical volumes to a logical volume and extend the logical volume across all of the partitions.  Finally, you can resume setting up SO.

## Prerequisites

- A server or device, physical or virtual
- An installed version of Linux
- Security Onion OS installed, before setup is run
> Note: This was written with CentOS in mind, but will work for many if not all flavors of linux.

# Steps
1. *user@host#* `sudo su`  [^1] 
2. *user@host#* `lsblk`  [^2]  
> NOTE: Interpretation is required to understand what drives belong to what.
3. *user@host#* `fdisk /dev/sdb` [^3]  
4.
   *Command (m for help):* `n`

   *Select (default p):* `{ENTER} `

   *Partition number (1-4, default 1):* `{ENTER} `

   *First sector (2048-16777215, default 2048):* `{ENTER} `

   *Last sector, +sectors or +size{K,M,G} (2048-16777215, default 16777215):* `{ENTER}`

   *Command (m for help):* `t`

   *Hex code (type L to list all codes):* `8e`

   *Command (m for help):* `w`

5. k
6. k
7. k
8. k
9. k
10. k
    
## Explanations
### These explain the "Why" of each step.

[^1]:Changes user to root (SuperUser)
[^2]:List details about block devices
[^3]:Starts Fdisk utility for disk [sdb]. Sdb is the assumed additional drive.  Fdisk manages disks (drives) and partitions

