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
4. Below are the fdisk utility prompts: [^4]

   *Command (m for help):* `n`

   *Select (default p):* `{ENTER}`

   *Partition number (1-4, default 1):* `{ENTER}`

   *First sector (2048-16777215, default 2048):* `{ENTER} `

   *Last sector, +sectors or +size{K,M,G} (2048-16777215, default 16777215):* `{ENTER}`

   *Command (m for help):* `t`

   *Hex code (type L to list all codes):* `8e`

   *Command (m for help):* `w`

5. *user@host#* `pvcreate /dev/sdb1` [^5]
6. *user@host#* `vgextend system /dev/sdb1` [^6]
> NOTE: Repeat steps 3 – 6 for any additional drives as necessary (sdc, sdd, etc)
7. *user@host#* `lvresize -l  +100%free /dev/system/nsm` [^7]
8. *user@host#* `xfs_growfs /dev/mapper/system-nsm` [^8]
9. *user@host#* `df –h /nsm` [^9]
> NOTE: from this point you can continue with Security Onion setup 
10. *user@host#* `./SecurityOnion/setup/so-setup iso` [^10]


    
## Explanations - These explain the "Why" of each step.
[^1]:Changes user to root (SuperUser)
[^2]:List details about block devices
[^3]:Starts Fdisk utility for disk [sdb]. Sdb is the assumed additional drive.  Fdisk manages disks (drives) and partitions
[^4]:Press M for a full list of options. If you are prompted for Primary or Extended, the partition table needs changed to GPT. 
   N will create a New partition.   The following returns are accepting the defaults for the Partition number and first and last sectors or how big the partition is. 
   T changes the type of the partition. 8e is a hex value for the partition type.  In this case it’s for LVM partition 
   W writes the changes.  (saves) 
[^5]:Initializes physical volume creation for later use with LVM.  This is for the sdb1 partition 
[^6]:Adds a physical volume (sdb1) to a volume group (system) 
[^7]:Resizes a logical volume. –l extends the volume to 100% of the free space on the volume group (system) for the root folder 
[^8]:Expands the existing XFS file system 
[^9]:Shows disk free of nsm folder (/nsm).  –h changes it to human readable (ie: 28TB)
[^10]:Command to re-run the Security Onion setup.  Usually under /home/[username]/ 


