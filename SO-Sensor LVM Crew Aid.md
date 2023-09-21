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
1. *user@host#* `sudo su`[^1]  
2. *user@host#* `lsblk`[^2]  
3. *user@host#* `fdisk /dev/sdb` [^3]  
> NOTE: Interpretation is required to understand what drives belong to what.
4.
Command (m for help): `n`

Select (default p): `{ENTER} `

Partition number (1-4, default 1): `{ENTER} `

First sector (2048-16777215, default 2048): `{ENTER} `

Last sector, +sectors or +size{K,M,G} (2048-16777215, default 16777215): `{ENTER} `

Command (m for help): `t `

Hex code (type L to list all codes): `8e `

Command (m for help): `w `


5. k
6. k
7. k
8. k
9. k
10. k
    
[^1]:1. Changes user to root (SuperUser)
[^2]:2. List details about block devices
[^3]:3. Starts Fdisk utility for disk [sdb]. Sdb is the assumed additional drive.  Fdisk manages disks (drives) and partitions 



A footnote can also have multiple lines[^2].  

You can also use words, to fit your writing style more closely[^note].


[^2]: Every new line should be prefixed with 2 spaces.  
  This allows you to have a footnote with multiple lines.







...

## Lexicon of Terms

To reduce confusion, these are the terms that should be used when referring to items and workflows within GitLab.

- Group - Collection of projects managed by permissions.  Each mission will typically have its own singular group.
- Project – separate lines of effort within a group.  Host, Network, Planning, etc. Will typically have their own projects for tracking their work. 
- Issue – used to assign and track technical hunt tasks, as well as other mission essential objectives that may need to be accomplished.
- Task – a singular item of work to be completed.  These items a binary in nature (complete or not complete) and do not provide any sort of documentation functionality.


# General Usage

This section is for all users of GitLab.


## Creating Issues

Ensure that you are in the desired project or group level before creating an issue.  Creating issues in unexpected areas of the heirarchy can create a host of issues.

- Select “Issues” from the sidebar on the left
- Select “New Issue” at the top of the page
  - Input a title, description, assignees, and select the necessary labels

> NOTE:  If desired, multiple checklist items can be tracked inside an issue by using the "Add a checklist" button when editing the description of an issue.

## Closing Issues

- Select “Issues” from the sidebar on the left
- Select the issue that needs to be closed
- Select "Close Issue" at either the top of the page or the bottom
> NOTE:  The issue can also be closed by adding comments and selecting "Comment & close issue"
> NOTE:  Issues can be reopened by selecting "Reopen Issue" at the top of the issue page

## Deleting Issues

Before deleting an issue, it is recommended that you consult your lead, Mission Commander, or a Senior Operator.  Issues generally should not be deleted.  Even if some information seems meaningless at the time, it may be worth retaining.

- Select the three dots at the top right of the issue page, next to the "Close Issue" or "Reopen Issue" button
- Select "Delete Issue"
- Confirm deletion of issue by selecting "Delete Issue" again

## Board Usage

The board in Gitlab operates similar to any other Kanban board.  The columns of the board are associated with the labels applied to any given issue.

- Ensure you are in the desired group or project and select "Issues" on the left hand side of the page
- Select "Boards"
  - Clicking on the card of an issue will present a pop up of the issue on the right side of the page
  - Clicking the title of a card will navigate to the issue page
  - Cards can be moved between columns as desired by left click dragging
>  NOTE:  Moving cards between columns will modify the labels applied to the issue.


# GitLab Management

This section is for administrators of GitLab, assigned to the  "Maintainers" role within Gitlab.


## Creating Groups

A group is a collection of projects managed by permissions. Each mission will typically have its own singular group.

- Select "Switch to..." > "Groups" > "View all groups" from the  menu next to the Gitlab logo in the top left
- Select "New Group" 
  -  Enter the group name and choose members to invite to the group


## Creating Projects

A project contains separate lines of effort within a group. Host, Network, Planning, etc. will typically have their own projects for tracking their work. 

- Select the Group name from the sidebar on the left, or select  "Switch to..." > "Projects" > "View all projects" from the menu next to the Gitlab logo in the top left
- Select "New Project"
- Select "Import project" or "Create blank project"
  - If creating a new project, enter the project name and choose the group the project will fall under


## Creating Labels

Labels are used to categorize issues

- Ensure that you are in the desired group and select "Group information" > "Labels" from the sidebar on the left
- Select "New label"
  - Enter the title, description, and color for the new label
- Assign labels when creating issues, or by choosing "Edit" on the right side menu when viewing an issue

> NOTE: Labels can also be created under a project by selecting "Create project label" under the dropdown for choosing a label when creating issues. These labels are not visible when viewing the labels for a group.



# Process for One-Way GitLab Sync from R2D2 to Kit

## 1. Create Access Tokens

### Create Access Token for R2D2 GitLab
1. Navigate to https://code.levelup.cce.af.mil and login
1. Click Profile Icon in upper-right
2. Click Preferences
3. Click Access Tokens in the left-hand menu
4. Create a Token called "GitLabSync" or another suitable name
5. Set Expiration Date as needed
6. Check the "read_api" box
7. Click "Create personal access token"
8. IMPORTANT: Copy the access token. You will not be able to access it again, so save it somewhere safe

### Create Access Token for Kit GitLab
1. Navigate to kit GitLab and login
1. Click Profile Icon in upper-right
2. Click Preferences
3. Click Access Tokens in the left-hand menu
4. Create a Token called "GitLabSync" or another suitable name
5. Set Expiration Date as needed
6. Check the "api" box
7. Click "Create personal access token"
8. IMPORTANT: Copy the access token. You will not be able to access it again, so save it somewhere safe

## 2. Download and Load the PEM certificate from code.levelup.cce.af.mil into Kit GitLab

This section of the guide provides step-by-step instructions on how to download the PEM certificate from https://code.levelup.cce.af.mil using different web browsers or OpenSSL.

Due to DoD certificates not being signed by a truested Certificate Authority, this step is necessary to avoid GitLab erroring on "SSL Self-Signed Certificate".


### Downloading from GitLab VM with OpenSSL (Easiest)

If GitLab is installed on a unix system with OpenSSL, this will be the easiest method.

1. Run the following command

```bash
openssl s_client -connect code.levelup.cce.af.mil:443 </dev/null 2>/dev/null | openssl x509 -outform PEM > /etc/gitlab/trusted-certs/levelup3-cloudone-af-mil.pem
```
>Explanations:
>
>`openssl s_client` : Establish a TLS/SSL connection with a given host.
>
>`connect code.levelup.cce.af.mil:443` : Connects to code.levelup.cce.af.>mil on port 443
>
>`< /dev/null` : Redirects input from /dev/null, which is a special file >that provides no data. It's used here because we don't need to send any >data to the server.
>
>`2>/dev/null` : Redirects the standard error stream, discarding all >error messages that might be generated by the command.
>
>`openssl x509` : Used to parse or display X.509 certificates.
>
>`-outform PEM` : Specifies the output certificate should be in PEM format.

2. Verify the file is created and populated using the command: `cat /etc/gitlab/trusted-certs/levelup3-cloudone-af-mil.pem` 
3. Restart GitLab with `gitlab-ctl reconfigure`


### Download via Browser and transfer to GitLab VM

If certificates need to be downloaded off-kit and then transferred, follow one of the below instructions. 

#### Downloading in Firefox

1. Open Firefox and navigate to `https://code.levelup.cce.af.mil`.
2. Click on the lock icon to the left of the URL bar.
3. Click on `Connection not secure` or `Connection secure`. (This will depend if on if your browser trusts DoD certs).
4. Click on `More Information`.
5. In the `Security` tab, click on `View Certificate`.
6. Click on the `levelup.cce.af.mil` certificate tab.
7. In the `Miscellaneous` section next to `Download`, click on `PEM (cert)`.

#### Downloading in Chrome

1. Open Chrome and navigate to `https://code.levelup.cce.af.mil`.
2. Click on the lock icon to the left of the URL bar.
3. Click on `Connection is secure` or `
4. Click on `Connection not secure` or `Connection secure`. (This will depend if on if your browser trusts DoD certs).
5. Click on `Certificate`
6. In the `Certificate viewer`, go to the `Details` tab.
7. Select the `levelup.cce.af.mil` certificate.
8. Click on `Export`.
9. Follow the `Certificate Export Wizard`, choosing `Base-64 encoded X.509, single certificate (.PEM)`.

### Downloading in Edge

1. Open Edge and navigate to `https://code.levelup.cce.af.mil`.
2. Click on the lock icon to the left of the URL bar.
3. Click on `Connection not secure` or `Connection secure`. (This will depend if on if your browser trusts DoD certs).
4. Click on the `Certificate` icon in the upper-right.
5. Click the `Details` tab.
6. Select the `levelup.cce.af.mil` certificate
7. Click on `Export`.
8. Follow the `Certificate Export Wizard`, choosing `Base-64 encoded X.509, single certificate (.PEM)`.


### Transfer the PEM certificate from code.levelup.io

1. Using SCP or another available method, transfer the downloaded PEM file to the `/etc/gitlab/trusted-certs/` directory in the gitlab VM. 
2. Verify the file is created and populated using the command: `cat /etc/gitlab/trusted-certs/levelup3-cloudone-af-mil.pem` 
3. Restart GitLab with `gitlab-ctl reconfigure`

## 3. Transfer a Project

### Identify the Project
1. Navigate to https://code.levelup.cce.af.mil and login
2. Click the project you want to copy
3. Click the "Clone" dropdown 
4. Click the copy icon for the "Clone with HTTPS". (This should be the project url with a .git extension)

### Import Project to Kit
1. Navigate to the Kit's GitLab  instance
2. Click Projects
3. Click New Project
4. Click Import Project
5. Click "Repository by URL"
6. Paste in the Project URL hosted on https://code.levelup.cce.af.mil. This url should end in `.git`. (You might get a warning that it is not valid, that is normal for the https://code.levelup.cce.af.mil projects.)
7. Enter the username for the access token. (This is often *first.last* and can be verified by clicking on the profile image on the R2D2 gitlab)
8. Paste the access token generated created with the "read_api" permission
9. Configure name as needed
10. Select a group or namespace
11. Adjust any other options as needed
12. Click Create Project
13. Wait for the Import to complete
