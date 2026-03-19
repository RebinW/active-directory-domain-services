# Creating and configuring a dedicated file share volume

## Overview
In this lab, I'll create a dedicated drive on the server to seperate shared organizational data from the operating system drive. The purpose is to follow a more realistic server design where shared data is stored on a separate volume instead of the C drive. After creating the new volume, an shared folder is created and published through File and Storage Services in Server Manager. 

## Objectives
- Create a dedicated volume/ drive for shared organizational data
- Create a root folder structure for shared resources
- Publish the folder as a network share through Server Manager
- Prepare the environment for later access control and permission configuration
  
## Environment
This lab is focused on the domain controller DC01. The server is running Windows Server and has the **File and Storage Services role** available in Server Manager. A client machine is already domain joined and will be used to validate access to the shared resource.

## Implementation
#### Step 1. Create a dedicated volume for shared data
The first step was to prepare dedicated storage for shared data. Instead of placing shared folders on the C drive directly, free space on the existing disk was reduced and used to create a new volume. On the DC01 Server I went into **Disk Management** where we'll se that we have almost 70 GB of free space on the C-Drive. Lets create a new Z-drive and allocate 10 gb from the C-drive to the new volume:
1. Right click on the C-Drive and choose Shrink volume
2. I'm going to free uip almost 10 GB by specifying 10.000 GB
3. Then simply click shrink.
   ![Free space](screenshots/shrink.png)

If we take a closer look now we can see that we now have almost 10 GB of unallocated space:
![unallocated space for use](screenshots/unallocated.png)

We simply right click on the **unallocated space** and choose **new simple volume**. This takes us to the New Simple Volume Wizard:
1. Specify Volume Size: 9999 MB
2. Asssign drive letter: Z-Drive
3. Format Partition:
  - File system = NTFS
  - Allocation unit size = Default
  - Volume label = Shares'

![specify volume size](screenshots/volumesize.png)
![assign drive](screenshots/driveletter.png)
![format](screenshots/formatpartition.png)

The new drive was formatted using NTFS, since NTFS supports the permission model required for enterprise file sharing, including access control, inheritance, and auditing.

At this point the drive has been successfully created.

#### Step 2. Create the root folder structure.


## Verification

## Results

## Lessons Learned
